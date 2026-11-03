# Appendix: Generative AI use

Every prompt is listed below.

Tool: Claude (Anthropic) through Claude Code.

## Prompts, in order, verbatim

**1.**

> i want you to use markitdown on lab4 rl learning representations.pdf. also i added two pdfs for my final
> lab submisisons for lab 2 and 3 to have copies off. for future lab reports i got a really good grade on
> lab 2 so it is good to note. maybe for like agent.md or something. also commit everytjhing

**2.**

> make sure to use starter file. make code as simple as possible and use the md inside of the lab4 folder as
> ground truth of what needs to be done.Act as an expert Reinforcement Learning engineer  but college student and
> also write like me and make realistic and help me complete my Python code for Lab 4: Representation Learning via
> Modern RL. The objective is to evaluate when Deep Q-Networks (DQN) provide meaningful advantages over classical
> Q-learning with hand-engineered features.   We are training agents to play a simplified, partially observable
> version of Battleship. The environment rules are:   Single-player version where only the agent selects targets on
> a hidden board.   The fleet contains exactly three ships with lengths of 2, 3, and 4.   The board is a 6x6 grid.
> Using the starter code provided below, please generate simple, clean, and well-commented Python code to fulfill
> the following lab requirements:1. Random Agent BaselineSimulate 1,000 games with randomly generated actions.
> Plot and save the distribution over game lengths.   2. Classical Q-Learning ImplementationImplement a classical
> Q-learning agent with its respective Q-values, policy, and learning update rule.   Define an effective reward
> function, $r(s',a,s)$, to incentivize the agent to win quickly.   Train Agent 1 using the raw grid board as the
> state (recording hits, misses, and untargeted cells).   Train Agent 2 using a creatively hand-engineered state
> representation of your own design that aligns with the agent's action granularity.   3. Deep Q-Network (DQN)
> ImplementationFill in the TODO sections of the provided Agent class.   Implement the replay buffer, target
> network updates (update_Q_target), forward pass (forward_pass), action selection (pi), batch creation
> (make_batch), and network optimization step (update_Q_value).   Use the current board as the state for this
> agent and define an appropriate reward function.   4. Training & CheckpointingWrite a training loop for all
> three agents.   Record the average reward and average turns to completion during training.   Save the models at
> specific checkpoints: 500; 5,000; 10,000; and 20,000 training episodes.   Generate visualizations of the learned
> policies for each agent (e.g., target-selection heatmaps, action distributions, or estimated value functions).
> 5. Ablation StudiesFor the engineered representation in classical Q-learning (Agent 2), remove one feature at a
> time and measure the effect on sample efficiency and final performance.   For the DQN agent, remove or modify
> one major component (such as the target network, replay buffer, or a specific CNN layer) and measure the effect
> on sample efficiency and final performance.   Keep the code as simple as possible. Please output the code
> necessary to complete these tasks based on this starter code:

Claude then asked three multiple choice questions. My answers: only let agents shoot cells that haven't been shot
yet (mask targeted cells), the random baseline picks a random untargeted cell, and the DQN ablation removes the
target network.

**3.**

> is it still running?

**4.**

> is it still running?

**5.**

> ok now using my writing style and last reports, draft the report for me. look at md doc and make sure everything
> is covered.

**6.**

> do q9 for me and keep going till done

For question 9, Claude Code sent this prompt to a separate Claude agent that hadn't seen my analysis or report
draft:

> I'm a student in a reinforcement learning class. For my lab report I need to ask an AI assistant to analyze one
> of my experiments, and then I'll critically evaluate its explanation. Please give me your own analysis.
>
> The experiment is in `C:\Users\grudzinskio\School Repos\RL-Labs\lab4_learning_representations\`. Use only
> `Lab_4_Oliver_Grudzinski.ipynb` (the code and its outputs) and the CSVs in `results/` (`checkpoint_eval.csv`,
> `training_curves.csv`, `final_games.csv`, `ablation_summary.csv`, `ablation_curves.csv`, `random_games.csv`).
> Do NOT open `report_draft.md`, `prompts_appendix.md`, or anything in other folders. Don't modify any files.
>
> Setup in short: single-player Battleship on a 6x6 board with ships of length 2, 3 and 4. Agents can only shoot
> untargeted cells. Reward is +1 per new hit, -1 per miss, and +10 for the winning shot. There are three agents,
> each trained for 20,000 episodes. Agent 1 is tabular Q-learning on the raw board. Agent 2 is tabular Q-learning
> on hand-engineered per-cell features. The third is a DQN with a CNN over the board.
>
> Question: why did the DQN agent outperform agent 2 (the hand-engineered tabular agent)? Explain the mechanism
> you think is responsible and back it up with numbers from the results.
>
> Reply with your explanation as you'd give it to me, around 250 to 400 words. That's the text I'll be quoting
> and evaluating, so write it as the answer itself, not as notes about what you did.

Its answer, verbatim:

> Most of the gap isn't the CNN learning a better representation. It comes from agent 2's final greedy policy
> ending up in a bad spot. Under that there's a smaller real gap, and I'd blame state aliasing for it.
>
> The headline numbers are 23.24 turns for DQN and 25.30 for agent 2 over 1000 greedy games, a difference of 2.07
> (SE about 0.2). The checkpoints tell a different story. Agent 2's greedy evals were 23.09, 23.40 and 22.98 at
> 500, 5000 and 10000 episodes, so it beat or tied DQN (23.88, 22.88, 23.16). Then it jumped to 24.91 at 20000.
> Meanwhile agent 2's training turns, which include epsilon = 0.1 random shots, stayed flat at about 23.7 from
> episode 1000 to 20000. So the final agent 2 plays worse greedily (25.3) than it did while exploring (23.7). DQN
> doesn't do this: it trains at about 23.0 to 23.3 and plays greedily at 23.24.
>
> Here's the mechanism. Agent 2 maps every cell to one of 30 feature types (it saw 25), so lots of very different
> boards share one Q-table entry. Each Q(type) becomes an average over the true states behind it, weighted by how
> often the epsilon-greedy behavior policy produces them. With a constant alpha = 0.1, those averages keep
> drifting with recent games. Several types have close values, so their ranking keeps flipping. Greedy play
> commits completely to whatever ranking the table had at episode 20000, and that one was bad. Random exploration
> hid the problem during training.
>
> The real advantage is smaller and shows up in the training curves, where both agents use the same epsilon. After
> episode 1000, DQN averages 23.0 to 23.3 turns and agent 2 averages 23.6 to 23.7, so DQN is about half a turn
> faster. Agent 2 hit its ceiling by episode 500, since 25 states don't leave much to learn. DQN started worse
> (24.15 over its first 500 episodes) and kept improving. The CNN sees the actual 5x5 pattern of hits and misses
> around each cell, so it can separate situations that agent 2's three features lump together, like a pair of
> hits already capped by misses versus one that's still open. DQN also has fewer long games: 8.4% took 30+ turns,
> versus 17.1% for agent 2.
>
> Caveats: each agent got one seed, and the checkpoint evals are 500 games each (SE about 0.2 turns), so the small
> differences at the early checkpoints are noise. The jump from 22.98 to 24.91 is about 6 SE, so it isn't.

**7.**

> give me just prompts for agent for question 9

**8.**

> do i need to provide its answer

**9.**

> commit everything and push
