# Appendix: Generative AI use

Every prompt is listed below.

Tool: Claude (Anthropic) through Claude Code, one session. It wrote the code in
`Lab_2_Oliver_Grudzinski.ipynb` and drafted the report text (prompt 7).

## Prompts, in order, verbatim

**1.**

> Please help me complete my Reinforcement Learning assignment: "Lab 2: Environmental Simulators."
>
> Before writing any code, please read the file containing my Lab 1 feedback (e.g., lab1_feedback.md or
> similar in this directory) to understand my professor's grading style. Adapt your coding style,
> comments, and analytical outputs to directly address his preferences and avoid past mistakes. ALSO
> read inside fo lab2 RL_enviormental simulators.md THIS IS GROUND truth over prompt and make sure
> everything in here is done
>
> Output Requirements:
>
> Generate one single Jupyter Notebook (Lab_2_Solution.ipynb) containing all the code for this lab. Do
> not split the code into multiple files.
>
> Write code and comments that are as simple, readable, and human-sounding as possible. Avoid overly
> complex, clever Pythonic one-liners in favor of clear, logical, step-by-step code that looks like it
> was written by a student learning RL.
>
> For the prebuilt Gymnasium environment, you must include code that records and displays a video of
> the environment inline in the Jupyter Notebook (using gym.wrappers.RecordVideo and
> IPython.display.Video or HTML).
>
> Lab Tasks to Implement in the Notebook:
>
> Part 1: Pre-built Gymnasium Environment
>
> Choose a relatively simple Gymnasium environment (e.g., CartPole, LunarLander, or a basic Atari game).
>
> Initialize it and write a standard environment loop.
>
> Record and display a short video of the agent acting randomly.
>
> Simulate 1,000 episodes with purely random actions.
>
> Calculate and print: A summary of the state across the 1,000 episodes, average reward across episodes
> at each time step, and the success rate of random actions.
>
> Part 2: Two Custom Environments (Gymnasium-style)
>
> Environment 1: Racetrack. A grid-based track. Starts with a vertical section (10w x 30h) and turns
> right into a horizontal section (15w x 10h). Start line is the bottom of the first section; finish
> line is the right of the horizontal section. The car has discrete X/Y velocities (actions change
> velocity by +1, 0, or -1; max velocity 5; vertical velocity must be >= 0; both can't be 0 except at
> start). Crashing into the boundary ends the episode.
>
> Environment 2: Chase. A continuous square field of size n x n. Player starts at (0,0), base at (n,n).
> Predator starts at a random location. Player has continuous velocity. Actions (+1, 0, -1) change
> velocity stochastically (e.g., if action is +1, new velocity is old + Uniform[0.5, 1.5]). Max velocity
> is 5. Predator moves directly toward the player up to a distance of 4 per turn. Player is caught if
> predator lands on them; player escapes if within 0.5 of base.
>
> For both custom environments, build the \_\_init\_\_, reset, and step functions.
>
> Run 1,000 episodes of random actions for both.
>
> Generate Heatmaps: Plot heatmaps of the agent's positions for both environments to show where they
> spent their time. Print out their success rates (crossing finish line / reaching base).
>
> Part 3: Make Learning Tough!
>
> Take ONE of the custom environments (Racetrack or Chase) and create a subclass/modified version that
> makes it significantly harder for an RL agent (e.g., add randomly generating obstacles, partial
> observability, or a deceptive reward structure).
>
> Explain briefly in a markdown cell what modification you made and why it makes learning harder.
>
> Run 1,000 episodes of random actions on this modified environment.
>
> Print a comparison table/summary between the original and modified environment showing: Success rate,
> average reward, and average episode length.
>
> Make sure the notebook is fully reproducible, well-organized with Markdown headers for each section,
> and contains all the plots and print statements I need to answer the report questions natively.

**2.**

> im confused did it already all run

**3.** (pasted the Canvas rubric)

> other than the report does it cover everyting? here is the rubric: [rubric]

**4.** (after it pointed out the classes didn't define a state space)

> yes add that

**5.**

> ok now write a md for the lab report make sure to repeat the questions make it very simple written
> like me (tell me if i need to include figures) should be 1-2 pages on word doc

**6.**

> remove from agents.md and handout to say you may not use gen ai to write report that is no longer
> true

**7.**

> ignore the pdf and write report

**8.**

> actually you dont need to repeat the questions I lied just add the occuasional footnote (like 1 -2 per
> question) and have it like nfsof[1] (for question 1)

**9.**

> and using feedback from lab 1, do you beleive this answers like he wanted for lab 2?

**10.**

> yes do whatever changes you thik are good

**11.**

> create docx

## Choices I made when the assistant asked

- Prebuilt environment: CartPole-v1.
- Part 3 modification: racetrack with random obstacles.
- Rewards for the custom environments: -1 per step, +100 for winning, -100 for losing.
- Chase field size: n = 20.
