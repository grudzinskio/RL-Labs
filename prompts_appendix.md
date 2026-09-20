# Appendix: Generative AI use

The assignment permits AI tools for implementation as long as every prompt is disclosed, and prohibits
using AI to write the report. This appendix documents the former.

**Tool used:** Claude (Anthropic), via Claude Code, in a single session.

**What it was used for:** implementing the simulator documentation, the agent, the experiment driver,
the plotting code, and the notebook scaffolding. Specifically these files:

- `cb_lab.py`
- `run_experiments.py`
- `make_plots.py`
- `build_notebook.py`

**What it was not used for:** the written report. The experimental design choices, the interpretation
of results, and the analysis are my own.

---

## Prompts

### Prompt 1 (initial, verbatim)

> help me do this lab, i cloned the repo in here, can take hours to run. make sure to save results in
> csv or something. maybe once you get the code ready ill run manually. for report dont worrry too much
> right now maybe make .md draft written by college student my writing style NO em dashes at all for any
> of this. Here it is there is also pdf and .md of assignment
>
> [full text of the Lab Assignment 1: Contextual Bandits handout pasted here]

### Prompt 2 (follow-up, verbatim)

> target score should be like 0.45-5

Meaning: the converged agent should reach roughly 0.45 to 0.5 average reward, used as the sanity check
threshold for the implementation.

### Clarifying questions answered during the session

The assistant asked three configuration questions before writing code. My answers:

1. **Seeds per configuration:** 3 (126 runs total for the sweep).
2. **Run fidelity:** strictly sequential, one `env.step()` and one `env.reward(a)` per turn, using the
   provided class exactly as intended rather than a faster batched variant.
3. **Which broken environment to fix:** non-stationarity, using a sliding-window refit.

---

## Notes on what was generated versus specified

The structure of the solution was specified by me through the answers above and through the assignment
itself. The following were design decisions surfaced and implemented by the assistant, which I reviewed
and kept:

- A constant-predictor fallback for arms with zero observations or only one observed reward class,
  which prevents `RandomForestClassifier` from failing at `T = 100` with `epsilon = 0`.
- Random tie-breaking in the `argmax` step, rather than defaulting to the lowest-indexed article.
- Fitting the models once at `t = T` so the first greedy decision has a model to act on.
- CSV checkpointing with resume, so the long sweep survives interruption.
- The three environment modifications (non-stationary, misleading features, hidden variable) and the
  sliding-window fix.

## Verification I performed

The implementation was checked against values derivable independently of the code:

- The random-action baseline returns 0.2201 at 10,000 turns against a theoretical
  `(1/5)(0.9) + (4/5)(0.05) = 0.22`.
- The standard deviation of the random baseline falls 0.119, 0.045, 0.014, 0.004 across
  10 / 100 / 1000 / 10,000 turns, consistent with `1/sqrt(n)`.
- The agent reaches 0.449 mean reward at `T=500, b=100, epsilon=0.05`, within the expected range.
- Each broken environment measurably lowers reward relative to the unmodified one at identical
  hyperparameters.
