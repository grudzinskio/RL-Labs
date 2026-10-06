# Lab 2 report outline

The questions, the figures, and the numbers to cite for each one.

Format reminders from the handout:
- Word doc, then PDF, named `Lab_2_Oliver_Grudzinski.pdf` (same as the notebook and HTML).
- No more than 2 pages of written text. Figures are on top of that, so about 700 to 900 words.
- Complete sentences in paragraphs, not bullets.
- **Footnotes saying which sentence answers which question.** Easy to forget.
- Prompts appendix at the end (copy `prompts_appendix.md`).

## Figures and tables to include

| What | File | Goes with |
|---|---|---|
| CartPole reward per time step + episodes still running | `figures/cartpole_reward_by_step.png` | Q1 |
| CartPole state summary table (mean, std, min, max is enough) | `results/cartpole_state_summary.csv` | Q1 |
| Racetrack heatmap | `figures/racetrack_heatmap.png` | Q2 |
| Chase heatmap | `figures/chase_heatmap.png` | Q2 |
| Original vs obstacles comparison table | `results/racetrack_comparison.csv` | Q5 |

The video can't go in a Word doc. It's in the notebook/HTML, so you don't need it here.

---

## Q1. For the gymnasium environment describe the task the agent must do. What does success mean? Report an appropriate summary of the state across the 1000 episodes. Also, report the average reward across episodes at each time step. How often does it succeed at its task with random actions?

Facts from the notebook:
- Env: CartPole-v1. Actions are push left (0) or push right (1). State is cart position, cart
  velocity, pole angle, pole angular velocity.
- Reward is +1 every step the episode is still going. The episode is terminated when the pole tips
  past about 12 degrees (0.21 rad) or the cart leaves the track (about ±2.4). It's truncated at 500
  steps.
- Success as defined in the notebook: reaching the 500 step limit.
- State summary over 23,546 visited states:
  - cart position mean -0.002, sd 0.093, range -0.83 to 1.20
  - cart velocity mean -0.008, sd 0.54
  - pole angle mean 0.001, sd 0.10, range -0.256 to 0.262 (right at the failure threshold, which is
    how almost every episode ends)
  - pole angular velocity mean 0.011, sd 0.83
  - All means are about 0, so random pushes don't favor a side. The cart barely moves, so the pole
    falling is what ends episodes, not the cart leaving the track.
- Average reward per time step: exactly 1.0 at every step, averaged over episodes still running. It's
  flat because CartPole always gives +1. The "episodes still running" panel is where the information is:
  fewer than 500 still running by step 20, fewer than 100 by step 38, the longest made it to 102.
- Episode length: mean 22.5, median 19, sd 12.1, min 8, max 102.
- Success rate: 0 of 1000 (0%).

Your call: is "reach 500 steps" the right definition of success? Say why. He liked precise definitions
in lab 1.

## Q2. For each custom environment, plot a heatmap of the agent's position. How often does it succeed at its task with random actions (crossing the finish line for the track, reaching base for the chase)?

Racetrack:
- Success: 3 of 1000 (0.3%).
- Episodes are short: mean 6.2 steps, median 5, max 26.
- Of 6,248 logged positions, 2,525 (40%) are on the start row, only 250 are in the top 10 rows, and only
  76 are in the horizontal section past x = 9.
- Mention your layout assumption: the horizontal section is the top 10 rows, sticking out 15 cells to
  the right.

Chase (n = 20):
- Success: 0 of 1000 (0%). All 1000 were caught.
- Episodes: mean 4.1 steps, max 8. 3.3% were caught on the very first step (predator spawned within 4).
- Of 4,071 logged positions, 3,813 (94%) are in the 5x5 corner by the start. Zero are in the 5x5 corner
  by the base.
- Visits run along the bottom and left edges, because the player gets clamped to the wall when its
  random velocity points out of the field.
- Worth saying that n isn't given in the lab and you picked 20. A smaller n would change the success
  rate.

## Q3. Describe how you would seek to reward the agent in each custom environment. Can you identify any challenges the agent might face in learning given your suggested approach to rewards? How difficult was reward design?

What the code uses (you picked it, now you justify it):
- Racetrack: -1 per step, +100 for crossing the finish, -100 for a crash.
- Chase: -1 per step, +100 for reaching base, -100 for getting caught.

Data that might help:
- Random agents almost never see the +100 (3 times on the racetrack, never in the chase), so nearly
  every episode ends with -100. Think about what that means for what the agent can learn from
  random-ish early exploration.
- Rubric: 3 pts for the reward and 2 pts for a learning challenge, for **each** environment. Do both
  for both.
- "How difficult was reward design" is just your honest opinion.

## Q4. Describe how you would conceptualize the state in each custom environment. What information do you think the agent would need to manage its task?

What the code returns from `step` (you explain why it's needed, or what else you'd add):
- Racetrack: (x, y, vx, vy). Integer cells, vx from -4 to 4, vy from 0 to 4.
- Chase: player x, player y, player vx, player vy, predator x, predator y. All continuous.
- Both are listed in each class's `state_space` in the notebook.

## Q5. Compare the original and modified environments over your 1,000 episode runs. Compare success rate, average reward, and episode length, and summarize your findings. Do you think your modification would make it more difficult for an agent to learn? Why or why not?

The modification: 40 blocked cells placed at random on the track at every reset (never on the start
row or the finish column). Hitting one counts as a crash.

| | Success rate | Avg episode reward | Avg episode length |
|---|---|---|---|
| Racetrack | 0.3% | -104.6 | 6.2 |
| Racetrack + obstacles | 0.0% | -103.7 | 4.7 |

Things to be careful about:
- Avg reward is *higher* (less negative) with obstacles even though it's harder. Episodes end sooner,
  so there are fewer -1 step penalties before the -100. Don't read the higher reward as "easier".
- A random agent ignores the state, so these numbers only show how the dynamics changed. They don't show
  how hard it is to *learn*. That part (7 of the 10 points) is your reasoning.
- His lab 1 comments liked framing things in terms of exploration and the data generating process.
  Think about what the agent's data looks like when the obstacles move every episode, compared with a
  fixed track.

## Q6. How would the ideas in this lab apply to autonomous vehicles, robotics, or recommender systems?

Not in the Canvas rubric, but the handout asks it, so give it a short paragraph. Nothing from the data
for this one, it's all you. The lab intro talks about simulators letting you train where real-world
testing is "costly, dangerous, or slow".
