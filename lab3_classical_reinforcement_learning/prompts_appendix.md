# Appendix: Generative AI use

Every prompt is listed below.

Tool: Claude (Anthropic) through Claude Code.

## Prompts, in order, verbatim

**1.**

> use markitdon to convert rl_classical alorgithms pdf to md

**2.**

> remove that from agents.md ai can write

**3.**

> include this rubric too
>
> *(followed by the Canvas "Monte Carlo Rubric" pasted as text, now copied into
> `RL_Classical_Algorithms.md` under "Grading Rubric")*

**4.**

> help me look at lab_3 and help me do the lab look at the markdown for the lab guidance, also look at lab 1
> with the feedback and how he graded and use that to guide you. write everything as simple as possible, and
> inside of one jupter notebook. (make sure agent.md) and make everything sound like my college level writig
> and my wiritng style, really simple code commetns etc put it in the point of view as being me.
>
> Act as an expert in Reinforcement Learning and Python. I need to complete a lab assignment focusing on
> Classical Reinforcement Learning in a custom Racetrack gridworld.Please write the complete code for this
> assignment in a single, well-commented Jupyter Notebook structure (using code blocks for cells). To avoid
> indexing errors and state-space complexity, you must use Python dictionaries to store Q-values and state
> visit counts (e.g., Q[state][action]) rather than pre-allocated NumPy arrays. Keep the code as simple,
> readable, and standard as possible. Do not add unnecessary features.Here are the exact requirements to
> implement in order:1. Environment Setup (The Modified Racetrack)Grid: Vertical section 10 wide x 30 high.
> Right turn into horizontal section 15 wide x 10 high (Total bounding box is 25 wide x 30 high).Start line:
> Bottom row of the first section.Finish line: Rightmost column of the horizontal section.State: (x, y, v_x,
> v_y).Actions: 9 possible combinations of changing velocities by +1, -1, or 0.Constraints: $0 \le v_y < 5$
> and $-5 < v_x < 5$. Both velocities cannot be 0 except at the starting line.Dynamics: If the car attempts
> to go through the track boundary (crashes), it is automatically returned to a randomly selected position on
> the starting line, its velocity is set to 0, and the episode continues (do not terminate the episode on a
> crash). Crossing the finish line ends the episode.2. Random SimulationCreate a simulation function and run
> 100 races with random valid actions. Print the average number of turns it takes to finish.3. Reward
> FunctionsImplement reward_1 (Professor's reward): $r(s', a, s) = 10$ if crossing the finish line, else $r =
> 1 / \max(1, ((x - \text{track\_width})^2 + (\text{horizontal\_track\_width} - y)^2)^{1/2})$.Implement
> reward_2 (Custom): Design a very simple, standard alternative reward function (e.g., -1 for every step, -10
> for a crash, +100 for finishing).4. RL AlgorithmsImplement the following algorithms to train agents. Train
> each for a reasonable amount of episodes (e.g., 2,000 - 5,000). For each, record episode length, average
> return, and success rate over training.On-policy, first-visit Monte Carlo Control with $\varepsilon$-soft
> policies.SARSA with $\varepsilon$-greedy policies.Expected SARSA with $\varepsilon$-greedy policies.5.
> "Wreck the Learning" ExperimentsCreate a function or configuration to purposefully ruin the learning (e.g.,
> setting $\varepsilon=1.0$ for pure random exploration, or $\gamma=0$ for zero foresight).Run this broken
> configuration on SARSA to prove it fails (record metrics).Run a "repaired" version showing recovery.6.
> Visualizations (Matplotlib)Plot learning curves comparing the algorithms (Episode vs. Episode Length,
> Average Return, and Success Rate).Plot a visualization of the learned policy for two different agents on
> the track (e.g., drawing arrows for the greedy action's velocity change at each track cell, assuming
> $v_x=0, v_y=0$).Please generate the complete, self-contained Python code. Prioritize robust
> boundary-checking in the environment step() function and use dictionary-based Q-tables to guarantee no
> array dimension mismatch errors.
>
> *(Claude used the provided example `race_environment` class and reward as-is instead of rebuilding the
> environment from the spec above, per the lab 1 feedback.)*

**5.**

> is it done

**6.**

> is it done now

**7.**

> ok a few things on the report, it should only be max 2 pages worht of text, and dont say my professor it is
> being submited to the preffesor just say example and write more like my wirint style

**8.**

> well it is okay to have more pages with the figures but the text should only be so much
