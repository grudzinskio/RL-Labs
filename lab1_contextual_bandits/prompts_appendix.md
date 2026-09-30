# Appendix: Generative AI use

The assignment permits AI tools for implementation as long as every prompt is disclosed, and prohibits
using AI to write the report. This appendix documents the former.

**Tool used:** Claude (Anthropic), via Claude Code, in a single session.

**What it was used for:** implementing the code in `Lab 1 Oliver Grudzinski.ipynb`. That includes the
documented simulator class, the contextual bandit agent, Algorithm 1, the three broken environment
variants, the experiment harness, and the plotting functions.

**What it was not used for:** the written report. The experimental design choices, the interpretation
of results, and the analysis are my own.

---

## Prompts, in order, verbatim

**1.** (initial request, with the full assignment handout pasted in)

> help me do this lab, i cloned the repo in here, can take hours to run. make sure to save results in
> csv or something. maybe once you get the code ready ill run manually. for report dont worrry too much
> right now maybe make .md draft written by college student my writing style NO em dashes at all for any
> of this. Here it is there is also pdf and .md of assignment
>
> [full text of the Lab Assignment 1: Contextual Bandits handout]

**2.**

> target score should be like 0.45-5

Meaning the converged agent should reach roughly 0.45 to 0.5 average reward. Used as the sanity check
threshold for the implementation.

**3.**

> dont add the report answers inside the notebook only do what is minimum required etc

**4.**

> also why did it run so fast

**5.**

> i need cell at top for pip installs with %pip

**6.** (pasted traceback)

> FileNotFoundError: [Errno 2] No such file or directory: 'results/sweep_summary.csv'

**7.**

> no i wsa running in notebook

**8.**

> well i dont have to run it inside of notebook? but what does it say on lab manual to do

**9.**

> so yeah get rid of all other py files

**10.**

> is this pip install up to date %pip install -q numpy pandas matplotlib scikit-learn joblib

**11.** (pasted traceback)

> ModuleNotFoundError ... ----> 9 import cb_lab as L

**12.**

> i dont think you edited the notebook right... maybe make new one?

**13.**

> i think it is done. Awesome

**14.**

> did you update md for lab report

## Clarifying questions answered during the session

The assistant asked three configuration questions before writing code. My answers:

1. **Seeds per configuration:** 3.
2. **Run fidelity:** strictly sequential, one `env.step()` and one `env.reward(a)` per turn, using the
   provided class as intended rather than a faster batched variant.
3. **Which broken environment to fix:** non-stationarity, using a sliding-window refit.

A fourth question came up once I read the submission rules, which say only `.pdf`, `.html` and `.ipynb`
are accepted. Because a helper module could not be submitted alongside the notebook, I had the code
restructured so that the notebook is fully self-contained and imports nothing local.

---

## What was specified by me versus surfaced by the assistant

The structure of the solution was set by the assignment and by my answers above. The following were
design decisions the assistant raised, which I reviewed and kept:

- A constant-predictor fallback for arms with zero observations or only one observed reward class,
  which prevents `RandomForestClassifier` from failing at `T = 100` with `epsilon = 0`.
- Random tie-breaking in the `argmax` step, rather than defaulting to the lowest-indexed article.
- Fitting the models once at `t = T` so the first greedy decision has a model to act on.
- CSV checkpointing with resume, so the long run survives interruption.
- The three environment modifications (non-stationary, misleading features, hidden variable) and the
  sliding-window fix.
- Recording a trailing-window reward curve in addition to the cumulative average, because a cumulative
  mean from turn 1 hides the post-drift collapse.

## Verification I performed

The implementation was checked against values derivable independently of the code:

- The random-action baseline returns 0.2201 at 10,000 turns against a theoretical
  `(1/5)(0.9) + (4/5)(0.05) = 0.22`.
- The standard deviation of the random baseline falls 0.119, 0.045, 0.014, 0.004 across
  10 / 100 / 1000 / 10,000 turns, consistent with `1/sqrt(n)`.
- The agent reaches 0.449 mean reward at `T=500, b=100, epsilon=0.05`, within the expected range.
- Each broken environment measurably lowers reward relative to the unmodified one at identical
  hyperparameters.
- The generated user population is identical under scikit-learn 1.8 and 1.9, so the results do not
  depend on the exact library version.
