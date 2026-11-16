# Appendix: Generative AI use

Every prompt is listed below.

Tool: Claude (Anthropic) through Claude Code.

## Prompts, in order, verbatim

**1.**

> use md it down on lab 5 rl polciy based algortithms.pdf

**2.**

> commit everything

**3.**

> Please complete the reinforcement learning assignment located in the lab5 folder of this repository.
> Reference RL_policy_based_alg.md as the absolute ground truth for all requirements and steps that need to be
> completed. Additionally, there is pdf alongside of it.  Here are the specific implementation steps you must
> complete in the Jupyter Notebook:Environment Setup: Initialize the ALE/SpaceInvaders-v5 environment using
> Gymnasium with standard Atari preprocessing, including 84x84 grayscale resizing, a frame skip of 4, and setting
> terminal_on_life_loss=False.   Initial Exploration: Print the observation shape and the discrete action space.
> Generate 100 episodes using random actions, record the total reward for each, and plot the distribution of
> these rewards.   A2C Implementation: Build an episodic Advantage Actor-Critic (A2C) agent that updates after
> every episode. Incorporate entropy regularization into the policy to encourage exploration.   Network
> Architecture: Use a shared convolutional neural network (such as the CNNActorCritic architecture from the
> starter code) for both the actor and critic heads to process the visual inputs efficiently with a single
> gradient step.   Frame Stacking: Implement a state history of 4 stacked screens using the provided
> init_history(), reshape_history(), and update_history() helper methods to overcome the environment's partial
> observability.   Training Loop: Train the agent for 2 to 4 million frames. You will need to select and tune the
> discount factor ($\gamma$), learning rate ($\alpha$), and entropy regularization weight ($\beta$).
> Evaluation: Run the trained agent for 100 evaluation episodes with parameter updates turned off. Calculate and
> output the average return and the variance of the return across these episodes. Generate a visualization
> showing the agent's chosen actions in representative states.   Experiments: Write code to run two specific
> ablation experiments:Test the impact of entropy regularization on agent performance (exploration vs.
> convergence).   Investigate the effect of state preprocessing by comparing models trained on 1-frame, 2-frame,
> and 4-frame stacked observation histories.   Ensure all outputs, charts, and metrics run successfully in the
> notebook so it is fully prepared for the final .ipynb and .html exports required for submission.

**4.**

> should I run on rosie super computer?

(Claude then asked whether to run the full notebook on my laptop or on Rosie, and I picked running it on my
laptop.)

**5.**

> is it done?

**6.**

> can you make the report for me in docx  file in here, write concise and my writing style. Here is a convo I
> had with a classmate I want this to be menionted.
>
> Did your A2C agents just learn to fire a bunch no matter the state?
>
> Yeah pretty much
>
> Yeah because occasionally it can randomly hit an alien
>
> So if it just spams it then it will slowly get points
>
> That's what I figured, but at one point my agent would just go in the corner and hide and shoot the whole
> time, being capped out at 105 or 115 points, so I wanted to make sure.
>
> You need to turn the Entropy weight
>
> Tune
