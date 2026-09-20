# Contextual-Bandits

Lab Assignment 1: a contextual bandit agent for article recommendation, with five per-article random
forest function approximators and an epsilon-greedy policy.

Everything lives in `Lab_1_Oliver_Grudzinski.ipynb`. The notebook is self-contained: every class and
function it uses is defined inside it, with no imports from local `.py` files. That is deliberate,
because the assignment accepts only three file types and a helper module could not be submitted
alongside it.

## How to run

Open `Lab_1_Oliver_Grudzinski.ipynb` and run all cells.

Section 6.1 runs the full experiment schedule, 198 runs at 5000 turns each, which takes about 80
minutes the first time. It caches every finished run to `results/*.csv` and skips anything already on
disk, so it is safe to interrupt and re-running the notebook later is fast.

Two knobs in that cell if you need them:

- `JOBS` defaults to `cpu_count() - 2`, capped at 12. Lower it on a smaller machine, or set it to 1 if
  the parallel backend causes trouble.
- `SEEDS` defaults to 3. Dropping to 1 cuts the runtime to about a third but leaves the error bars too
  noisy to separate close configurations.

To force a full re-run, delete `results/sweep_summary.csv` and `results/sweep_curves.csv`.

## Files

| File | What it is |
|---|---|
| `Lab_1_Oliver_Grudzinski.ipynb` | The whole thing. Rename to spaces before submitting. |
| `report_draft.md` | Scaffold for the written report, NOT finished prose |
| `prompts_appendix.md` | AI use disclosure required by the assignment |
| `results/` | Cached experiment output, written by the notebook |
| `figures/` | Plots, written by the notebook |

## Output

- `results/random_baseline.csv` one row per (n_turns, seed)
- `results/sweep_summary.csv` one row per run: mean reward, last-1000-turn reward, how often the agent
  picked the truly best article, refit count, wall time
- `results/sweep_curves.csv` reward over time, downsampled every 25 turns, as both a cumulative average
  and a trailing 500-turn average
- `figures/*.png` the five plots the report needs

## Submission

The assignment names exactly three files and states that no other submission types will be accepted:

- `Lab 1 Oliver Grudzinski.pdf` from the written report
- `Lab 1 Oliver Grudzinski.ipynb`
- `Lab 1 Oliver Grudzinski.html` via
  `jupyter nbconvert --to html Lab_1_Oliver_Grudzinski.ipynb`

Run the notebook fully and save it before exporting, so the stored outputs and figures appear in the
HTML. Rename all three to use spaces, matching the stated convention.

The assignment also prohibits using generative AI to write the report and requires all prompts to be
disclosed for code. See `prompts_appendix.md`.
