# Rules for agents working in this repo

This is my repo for my reinforcement learning class. One folder per lab. Read this before doing anything.

## Layout

- `labN_short_name/` holds everything for lab N: the assignment (PDF plus a markdown copy), the
  notebook, results, figures, and any notes.
- The assignment markdown in each lab folder is the source of truth for what's being asked. Read it
  first.
- Graded labs have a `feedback.md` with the professor's comments. Read the feedback from every earlier
  lab before starting a new one, including the per-question conceptual comments, not just the code
  ones. They show what he's looking for: precise definitions, reasoning about what a parameter changes
  in expectation, and framing ideas in terms of exploration and the data generating process. Use that
  when working on the report, and point out if one of my claims repeats a mistake he already
  marked.
- New lab? Make a new `labN_short_name/` folder, drop the PDF in, convert it with markitdown
  (`python -m markitdown file.pdf -o file.md`), and clean up the output since it mangles tables and
  spacing.

## Academic integrity (don't skip this)

AI use is allowed as long as every prompt is disclosed.

- Keep a `prompts_appendix.md` in each lab folder and log every prompt used.

## Code

Keep it minimal. This is a class assignment, not a production codebase. The professor flagged this on
lab 1 (see `lab1_contextual_bandits/feedback.md`): AI adds unnecessary stuff that changes the lab in
material ways. Watch for it.

- **Use provided code as-is.** If the lab gives an environment, class, or starter notebook, import or
  copy it exactly. Never recreate or "clean up" it, that introduces differences from what the
  professor intended.
- **Do exactly what the assignment asks, nothing more.** No multiple seeds, extra sweeps, error bars,
  resume/skip logic, or parallelism unless the lab asks for it. It just makes runs longer for no gain.
- If you think something extra is worth adding, ask me first instead of adding it.
- Shortest thing that correctly does what the assignment asks. No extra features, config systems,
  abstraction layers, or defensive checks for cases that can't happen.
- Everything goes in one Jupyter notebook per lab, for now. Self contained, no helper .py files
  (submission only takes .pdf, .ipynb, .html).
- Put a `%pip install` cell at the top for anything not already installed (lab 2 needed
  `gymnasium[classic-control]` and `moviepy` for video recording).
- Always save experiment results. Write the numbers to CSV in `results/` and every plot to PNG in
  `figures/` inside the lab folder, so they can be looked at without rerunning the notebook.
- Plain numpy / matplotlib / gymnasium. Don't pull in a library for something a few lines can do.
- Comments only where something isn't obvious. No docstrings on every function.
- Short variable names are fine if they match the assignment's notation.
- Plots need a title and labeled axes, that's it.

## Writing (READMEs, notes, comments, commit messages, anything I'd read)

Keep it short and make it sound like a college student wrote it, not a chatbot.

- Say it in fewer words. If a sentence can go, cut it.
- Plain, casual-but-correct language. Contractions are fine. First person is fine.
- No filler openers or closers ("Great question", "In summary", "It's worth noting", "Let's dive in").
- No buzzwords: delve, leverage, robust, seamless, crucial, comprehensive, utilize, showcase, pivotal,
  intricate, landscape, realm, tapestry, underscore, furthermore, moreover.
- No em dashes, ever. Use a comma, period, or parentheses instead.
- Don't overuse bold or bullet lists. A short paragraph is often better than five bullets.
- Don't hedge everything. State what the data shows.
- No emojis.

## Submission

Each lab wants exactly three files: .pdf, .ipynb, .html. Use whatever naming the handout gives (lab 2
was `Lab_2_[YOUR NAME]`, so `Lab_2_Oliver_Grudzinski.*`). Export the HTML with
`jupyter nbconvert --to html` after running the notebook top to bottom.

The report draft lives in `report_draft.md`. Convert it to Word with pandoc (installed), putting the
prompts appendix after a page break, then save as PDF from Word. Pandoc drops raw HTML, so side by side
images need `![](a.png){width=49%} ![](b.png){width=49%}` in the copy that gets converted.
