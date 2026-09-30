# Lab 1 report: DRAFT SCAFFOLD, NOT A FINISHED REPORT

> **Read this first.**
>
> The assignment says plainly: *"You may not use generative AI to write your report."*
> It also says any AI use for the *code* has to be cited with all prompts included as an appendix.
>
> So this file is deliberately **not** finished prose. It is a scaffold: the structure, the claims that
> the data actually supports, and the numbers to cite. You need to write the actual sentences yourself.
> Do not paste this in. Use `prompts_appendix.md` to satisfy the citation requirement for the code.
>
> Numbers marked `[FILL]` come from the full sweep and need to be read off
> `results/sweep_summary.csv` once it finishes.

Target length is 2 pages of written text plus figures. Footnote each sentence with the question number
it answers, as the assignment asks.

---

## Q1. Define the MAB problem, give an example

Points to hit:

- An agent repeatedly picks one of K actions (arms). Each arm returns a stochastic reward from a fixed
  but unknown distribution. The agent only sees the reward for the arm it actually pulled, never the
  others. This is the key difference from supervised learning and it is called bandit feedback.
- The goal is maximizing cumulative reward over some horizon, which is equivalent to minimizing regret
  against always playing the single best arm.
- The tension: every turn spent learning about an arm is a turn not spent exploiting the best known arm.
  That is the explore/exploit tradeoff.
- Example to use: clinical trial arm assignment, ad selection, or the one from this lab, choosing which
  of five articles to show. Pick one and describe it in a sentence or two.
- One line connecting to this lab: the plain MAB version would ignore the user features entirely and
  just learn which single article is best on average. The contextual version gets to condition on who
  the user is.

## Q2. When does purely greedy fail?

Points to hit:

- Greedy commits to whatever looks best from a small sample, and because it then stops sampling the
  other arms, it never collects the evidence that would correct the mistake. The bad estimate is
  self-sealing.
- Concrete scenario: arm A truly pays 0.3, arm B truly pays 0.6. You try B twice early and get unlucky
  with two zeros, so B's estimate is 0.0. Greedy now plays A forever and never revisits B. The error is
  permanent, not just slow to fix.
- Tie this to actual observed behavior in this lab, do not just assert it. At `epsilon = 0` the only
  exploration the agent ever gets is the forced random phase of length T, and an arm can end up starved.
  This is exactly why the agent needs the `_ConstantPredictor` fallback for arms with no data or only
  one observed class.
- The honest complication worth a sentence: in *this* environment `epsilon = 0` still does fine, because
  T turns of forced uniform exploration already give every arm a decent sample and the contexts are
  i.i.d. Greedy is dangerous when it is the *only* thing controlling exploration. Cite the
  `epsilon = 0` column from the sweep table.
- The other failure mode, which the non-stationary experiment demonstrates directly: greedy cannot
  recover from drift, because once the world changes, nothing is sampling the arm that is now correct.

## Q3. A/B/n testing (explore then commit) and the role of T

Points to hit:

- Explore-then-commit spends T turns sampling uniformly, then picks the empirical winner and commits
  to it for the rest of the horizon.
- Each arm gets about T/K samples. The estimate for each arm has standard error roughly
  `sqrt(p(1-p) / (T/K))`, so it shrinks like `1/sqrt(T)`.
- The probability of correctly identifying the best arm rises with T and approaches 1, but it does so
  slowly, and how fast depends on the gap between the best arm and the runner up. A small gap needs a
  much larger T to resolve.
- The cost is symmetric and this is the actual tradeoff to write about: larger T buys a more reliable
  choice but burns more turns at the uninformed rate of 0.22. If you commit wrong, you are stuck at a
  bad arm for all remaining turns, so the regret is linear in the horizon.
- You can make this concrete cheaply from the Section 2 data you already have. At 10 turns the spread
  of the estimate is sd 0.119, at 100 turns 0.045, at 1000 turns 0.014. An arm-selection decision made
  off 10 samples is basically a coin flip, and off 1000 samples it is reliable.
- Connect it to the lab: the pure exploration phase of length T in Algorithm 1 *is* the A/B/n phase.
  The difference is that Algorithm 1 does not hard commit afterward, it keeps exploring at rate epsilon
  and keeps refitting, which is what protects it from committing wrong.

## Q4. How does the Contextual_Bandit_generator class work?

Docstrings and comments are in the notebook, in Section 2. For the report, describe:

- `__init__` calls `make_classification` **once** to build a fixed pool of 100,000 synthetic users.
  Each user is a 5-feature vector paired with a hidden label giving which of five articles they want.
  Building the pool once up front is exactly what makes the environment stationary.
- The generator settings that matter: `n_informative=5` with no redundant or repeated features, so all
  five columns carry signal; `n_classes=5` for the five articles; `shift=[10,4,0,0,0]` and
  `scale=[7,1,1,1,1]` to move the columns into realistic ranges; `random_state=8` so everyone gets the
  same population.
- `flip_y=0.2` randomly reassigns 20 percent of labels after the clusters are built. Call this out
  explicitly, it is the single most important line in the class for interpreting your results.
- `feature_engineer` discretizes the raw gaussians into a plausible user profile: two binary flags, one
  continuous value, one count clipped to 0 through 5, and one rounded integer that reads like an age.
  Note that this is **lossy** and further lowers the achievable ceiling.
- `step()` draws one user uniformly at random with replacement and puts them in `current_context`.
  Contexts are i.i.d. and there is no internal clock, which is precisely the property the
  non-stationary modification breaks.
- `reward(a)` returns a Bernoulli draw: p = 0.9 if the action matches the hidden label, p = 0.05
  otherwise. The hidden label is never exposed to the agent, it is only used here for scoring.
- Worth one sentence: labels are 0 through 4 internally while the assignment writes A = {1,...,5}.

## Q5. Bar chart of the random action rewards, and the theory

The figure is `figures/q5_random_baseline_bar.png`.

Theory, show the arithmetic:

    E[r] = (1/5)(0.9) + (4/5)(0.05) = 0.18 + 0.04 = 0.22

Measured, 30 repetitions per bar:

| turns | mean | sd | min | max |
|---|---|---|---|---|
| 10 | 0.223 | 0.119 | 0.000 | 0.400 |
| 100 | 0.235 | 0.045 | 0.080 | 0.320 |
| 1000 | 0.222 | 0.014 | 0.201 | 0.270 |
| 10000 | 0.220 | 0.004 | 0.211 | 0.228 |

The point to make, and the thing that is easy to get wrong: **the expected value does not depend on
the number of turns, only the variance does.** All four bars sit on 0.22. What changes is the spread,
which falls by roughly a factor of 3 for each 10x increase in turns. That is the `1/sqrt(n)` standard
error of a Bernoulli mean, and going from 10 to 10,000 turns is a 1000x increase so the spread should
shrink about `sqrt(1000) = 32`x, which matches the observed 0.119 to 0.004.

Worth one sentence: a single 10-turn run returned anywhere from 0.0 to 0.4 across repetitions. This is
the concrete argument for why any single short run tells you nothing, and it ties directly back to Q3.

## Q6. How the agent was set up

Points to hit:

- Five separate binary `RandomForestClassifier` models, one per article, left at sklearn defaults
  (100 trees, unlimited depth) since the assignment asks for untuned forests.
- Model `f_a` estimates P(click | X, a) and is trained **only on the turns where article a was actually
  shown**, with the observed click as the binary label. Emphasize this: it converts one bandit problem
  into five independent supervised problems, and it is what lets the agent score articles it did not
  show to this user.
- Scores come from `predict_proba(x)[:, 1]`. The policy is epsilon-greedy over those five scores.
- Two implementation decisions worth defending in the report, because they are not cosmetic:
  1. **Degenerate arms.** An arm can have zero observations, or observations that are all zeros, in
     which case a forest will not fit or returns a one-column `predict_proba`. Those arms fall back to a
     constant predictor returning the observed click rate, or the global prior if never played. This is
     a real case at `T = 100` with `epsilon = 0`.
  2. **Random tie-breaking.** Early on, several arms share the identical constant fallback value, and a
     plain `argmax` would always return article 0. Breaking ties uniformly at random removes that bias.
- The fit schedule: fit once at `t = T` so the first greedy decision at `t = T+1` has a model, then
  refit every b turns. Say why, otherwise the first b greedy turns are arbitrary.

## Q7. Comparing T, b and epsilon

The figures are `figures/q7_sweep_curves.png` and `figures/q7_sweep_final_bars.png`.

Confirmed numbers from single validation runs at 5000 turns:

| T | b | epsilon | mean reward (all) | mean reward (last 1000) | picked best article (last 1000) |
|---|---|---|---|---|---|
| 500 | 100 | 0.05 | 0.449 | 0.486 | 52.9% |
| 250 | 100 | 0.10 | 0.443 | 0.453 | 50.3% |
| 100 | 1000 | 0.00 | 0.426 | 0.519 | 55.8% |

`[FILL]` the full 3 x 2 x 7 table from `results/sweep_summary.csv`.

What to argue, and check each against the actual table before you commit to it:

- **Epsilon.** There is a cost/benefit crossover. Some exploration protects against arm starvation and
  against a bad early model, but every exploratory turn pays the random rate of about 0.22 instead of
  the greedy rate. At large epsilon the agent is deliberately throwing away turns, so reward should fall
  roughly linearly as epsilon rises. Sanity check on the arithmetic: at epsilon = 0.5 the agent plays
  randomly half the time, so its ceiling is about `0.5(greedy rate) + 0.5(0.22)`.
- **b.** Smaller b means refitting more often, so the agent acts on a fresher model and should climb
  faster early. The interesting observation from the validation runs is that b = 1000 finished *higher*
  on the last 1000 turns (0.519 vs 0.486) despite only 5 refits. Do not paper over that. Likely
  explanation: with a stationary environment the model is mostly converged either way, and what matters
  more is total data, so the refit frequency stops mattering once there is enough history. Check
  whether this holds across seeds before claiming it.
- **Interaction with T.** Larger T means more forced exploration up front, so a better model at the
  moment the agent switches to greedy, paid for with more turns at 0.22. Expect large T to look worse
  on mean-over-all-turns but better or equal on mean-over-last-1000. The dependence claim to make: b and
  epsilon matter *most* when T is small, because that is when the initial model is weakest and the agent
  is most dependent on continued exploration to fix it. At T = 500 the initial model is already decent
  and the agent is less sensitive to both.
- **Ceiling.** The agent reaches roughly 0.45 to 0.52 while the oracle is 0.9. Explain the gap rather
  than treating it as underperformance: `flip_y=0.2` corrupts a fifth of the labels outright, and
  `feature_engineer` binarizes two informative columns. Picking the right article about 53 percent of
  the time against a 20 percent random rate is the agent working, not failing.

## Q8. CB agent vs multiclass supervised learning

Points to hit:

- The supervised framing needs a labeled dataset that says which article was correct for each user.
  You do not have that. You only observe a click or no click for the **one** article you actually
  showed. That is bandit feedback, and it is the core reason this is not a classification problem.
- Getting a supervised dataset would require showing random articles to real users for a long time,
  which is expensive in exactly the currency you are trying to maximize. The CB agent instead earns
  while it learns and shifts its own data collection toward promising actions.
- The reward is noisy, not a clean label. Even a correct article only clicks 90 percent of the time and
  a wrong one still clicks 5 percent, so a single observation is a weak signal. A multiclass model
  trained on clicks as labels would be learning from corrupted targets.
- The CB agent controls its own data distribution and can keep adapting. A supervised model trained
  once is frozen, which the non-stationary experiment shows is fatal. This connects Q8 to Q9 directly.
- Honest counterpoint worth a sentence: if you genuinely had the labels and the world were stationary,
  supervised learning would win, because it uses every sample for every class instead of splitting the
  data five ways. The CB advantage is about feedback structure and adaptivity, not raw accuracy.

## Q9. What was broken, why it is realistic, what assumption it violated

The figure is `figures/q9_broken_environments.png`. `[FILL]` the reward drops from
`results/sweep_summary.csv`. Preliminary short-run values at T=200, b=200, epsilon=0.05, 1200 turns:
baseline 0.432, misleading 0.294, hidden 0.304. The non-stationary env matched baseline exactly at that
length, which is correct since the drift is at turn 2500, so quote the 5000-turn numbers instead.

### Modification 1: non-stationarity

- **What changed.** At turn 2500 a fixed derangement is applied to the hidden labels, so the audience
  that wanted article 0 now wants article 3, and so on. The user population is unchanged, only the
  right answer for them moves.
- **Why realistic.** Article inventory rotates, editors swap headlines, a seasonal event or news cycle
  changes what people want. This is the normal condition of a real recommender, not an edge case.
- **Assumption violated.** Stationarity. Algorithm 1 assumes P(reward | context, action) is fixed,
  which is the licence it uses to pool the entire history into one fit. After the drift, old and new
  data actively disagree and the stale majority wins.

### Modification 2: misleading features

- **What changed.** The one genuinely continuous informative column is overwritten with noise, and five
  junk noise columns are appended. The reward still depends on the original hidden label.
- **Why realistic.** A logging change breaks a field so it writes garbage, while someone else dumps a
  pile of unrelated columns into the feature store because storage is cheap. Nothing errors, the
  pipeline keeps running, and nobody notices for months.
- **Assumption violated.** That the context is informative about the reward. A contextual bandit only
  beats a plain MAB if X predicts which action pays. Strip that out and the forests fit noise. Note the
  extra insult: the agent is now *worse* than a plain MAB would be, because it pays estimation variance
  for context it cannot use.

### Modification 3: hidden variable

- **What changed.** With probability 0.35 a user belongs to a latent segment whose best article is
  shifted by two positions. Segment membership never appears in X.
- **Why realistic.** Logged-out or cookie-less traffic, a device type that never made it into the
  feature pipeline, or user intent the platform simply cannot observe. This is the most common reason
  real recommenders miss their offline metrics.
- **Assumption violated.** That X is a sufficient statistic for the reward. Two users with identical
  contexts now want different articles, so P(click | x, a) is genuinely ambiguous and no function of X
  can resolve it. The best available play collapses to whatever the larger segment prefers, which caps
  performance structurally rather than just slowing learning down.

### The fix, for non-stationarity

The figure is `figures/q9_nonstationary_fix.png`. `[FILL]` the before/after numbers.

- **Diagnosis.** After the drift the agent has 2500 turns of history that is not merely stale but
  wrong. Because Algorithm 1 refits on the entire log, the contaminated majority keeps outvoting fresh
  observations. Make the point that refitting *more often* does not help at all, because it just
  re-derives the same wrong model from the same poisoned data. That is worth saying because it is the
  obvious first thing to try and it fails.
- **The change.** Refit on a sliding window of the most recent W observations instead of all history,
  tested at W = 500 and W = 1000. Stale data then ages out automatically after W turns, and the ongoing
  epsilon-greedy exploration supplies the fresh observations needed to relearn.
- **Why this and not something else.** The failure is about *which data* the model is fit on, so the
  fix belongs in the data, not the model class or the exploration rate. Raising epsilon would collect
  fresh data faster but the stale data would still outvote it. Swapping the forest for another
  classifier changes nothing, since any model fit on contaminated data gives a contaminated answer.
- **The tradeoff, state it honestly.** Windowing is not free. On a stationary environment it strictly
  discards usable data and does slightly worse, which the short tests confirmed (0.383 windowed vs
  0.404 unwindowed at 1200 turns, before any drift had occurred). It only pays off when the environment
  actually drifts, and W trades recovery speed against estimator variance. Report this rather than
  presenting the window as a free win.

---

## Checklist before submitting

- [ ] Rewrite every section above in your own prose. Do not submit this file.
- [ ] Fill every `[FILL]` from `results/sweep_summary.csv`.
- [ ] Verify each claim in Q7 against the real sweep table instead of trusting the predictions above.
- [ ] Under 2 pages of written text, figures on top of that.
- [ ] Footnote each sentence with its question number.
- [ ] Include: q5 bar chart, q7 curves, q7 final bars, q9 broken comparison, q9 fix.
- [ ] Attach `prompts_appendix.md` content as an appendix, per the AI use policy.
- [ ] Export as `Lab 1 Oliver Grudzinski.pdf`, `.html` and `.ipynb`.
