# Lab 5: Policy-Based Reinforcement Learning Algorithms

Cleaned up from `RL_policy_based_algorithms.pdf` (markitdown raw output is in
`RL_policy_based_algorithms_raw.md`). Text is the instructor's, only the formatting was fixed. The
`make_env` snippet was an image in the PDF and was transcribed by hand.

## 1 Introduction

In this lab we will train an Advantage Actor-Critic (A2C) agent to play the Atari game Space Invaders
using the Gymnasium implementation of the environment. Unlike our previous work with value-based methods,
A2C is a modern policy-gradient method that learns both a policy model (the "actor") and a value function
(the "critic"). Space Invaders has high-dimensional visual state inputs, requiring convolutional neural
networks to learn useful representations. Your task is to set up the environment, preprocess
observations, implement an A2C agent, train it, and evaluate its performance.

## 2 Set up and learn about the environment

Space Invaders is a classic Atari arcade game in which the player controls a laser cannon moving
horizontally along the bottom of the screen, firing at alien invaders descending from above. The
Gymnasium environment provides:

- A 210×160×3 RGB frame at each step.
- A discrete action space including NOOP, FIRE, LEFT, RIGHT, LEFT/FIRE and RIGHT/FIRE.
- Rewards are relatively dense. See [here](https://ale.farama.org/environments/space_invaders/) for more
  documentation.

To begin (you will not need to repeat this if you already installed gymnasium for a previous lab):

- Install Gymnasium and the ALE Atari integration. The following commands entered at the command line
  enabled me to set up a virtual environment on Rosie in which I could run the environment and train an
  agent, with a kernel recognized by jupyter notebooks:

  ```
  conda create -n py_Gym python=3.10
  conda activate py_Gym
  pip install ale-py
  pip install autorom[accept-rom-license]
  AutoROM --accept-license
  pip install "gymnasium[atari,accept-rom-license]" --force-reinstall
  pip install torch
  conda install matplotlib
  conda install ipykernel
  python -m ipykernel install --user --name=py_Gym --display-name="Python py_Gym"
  ```

- Open the starter code, and then create and initialize the environment. I suggest using the helper
  function provided in the starter code (see figure 2) for this purpose. This function sets up the
  environment to output grayscaled and resized screens.
- Inspect what the environment returns:
  - Print the observation shape.
  - Print the action space.
  - Generate 100 episodes with random actions. Record the total reward for each and plot the
    distribution over rewards.

Figure 2 (helper function from the starter code):

```python
#@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
#A function to set up the environment with automated preprocessing
#@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
def make_env(env_name = "ALE/SpaceInvaders-v5"):
    """A function to set up the environment
        Takes:
            env_name -- string, the name of the environment in ALE
        Returns:
            an environment set up with standard DQN atari preprocessing (grayscale 84x84, frame_skip=4)
    """
    env = gym.make(env_name, render_mode="rgb_array",frameskip=1)    #make the environment
    env = gym.wrappers.AtariPreprocessing(
        env,
        screen_size=84,
        grayscale_obs=True,
        frame_skip=4,
        scale_obs=True, #note these could also be normalized in the network
        terminal_on_life_loss=False)    #set up the preprocessing choices
    return env
```

## 3 Training Agents to play Space Invaders with A2C

We will practice using a modified version of the A2C algorithm. Our version will be an episodic version
which updates after every episode. We will also incorporate entropy regularization. Given a policy
π(s_t), entropy is defined as:

$$H(\pi(s_t)) = -\sum_a \pi(a|s_t) \ln \pi(a|s_t).$$

Larger entropy means that the policy spreads out the probability more.

1. **You will need to assemble a history of 4 screens to use as the state.** Recall from lecture that
   with a single screen the environment is partially observable since the agent cannot infer movement.
   In the original DQN paper, the authors stacked 4 frames together to deal with this. I have provided
   three helper methods to do this: `init_history()`, `reshape_history()`, and `update_history()`.
2. **Use the default environment reward function.** Reward information will be returned by the
   environment as it evolves.
3. **Implement A2C.** Build an agent class and a training loop that implements the episodic A2C from
   lecture. This algorithm is configured for the actor and critic to save on parameters by sharing
   convolutional input layers that process raw screen input. This is why only a single gradient step is
   required. Thus, you should use the `CNNActorCritic` architecture (or analogous) provided in the
   starter code.
4. **Train agents.** Train an A2C agent for an appropriate number of episodes (e.g., 2-4 million
   frames). Atari learning takes time; adjust if computational resources are limited. You will need to
   choose the discount factor γ, the learning rate α, and the entropy regularization weight β.
5. **Evaluate agents.** Run your trained agent for 100 evaluation episodes with NO parameter updates.
   Record:
   - Average return across the episodes.
   - The variance of the return across the episodes.

## Lab Assignment Report

Write up your analysis in a separate report. Your report should answer the following questions:

1. Describe how the Space Invaders environment works and what information it returns.
2. Provide and interpret the plot of reward distribution you generated in section 2.
3. Describe the full state preprocessing pipeline and the role played by each method involved. Explain
   how partial observability motivates frame stacking and discuss how preprocessing changes the learning
   problem.
4. Describe the A2C architecture you implemented. Why does it have separate actor and critic heads?
   Explain why shared representations are commonly used in modern reinforcement learning.
5. Assess the performance of your trained agent using the data you collected about reward during the
   evaluation episodes.
6. Provide a visualization of the learned policy, showing what the agent chooses to do in representative
   states.
7. Design and conduct an experiment to determine what impact, if any, entropy has on agent performance.
   What is the purpose of entropy regularization? How does it impact parameter updates? Explain how
   entropy-based exploration differs from e-greedy exploration and analyze its impact on exploration,
   convergence, and final policy quality.
8. Investigate the effect of state preprocessing by comparing different observation histories (for
   example, one frame, two frames, and four stacked frames).

## 4 Submission Instructions

- Write up the answers to the questions in a short word document; aim for no more than 2 pages of
  written text and include all graphics generated. Add footnotes identifying which sentence addresses
  which questions. Write in complete sentences organized into paragraphs. Your goal is to explain what
  you've done and what you've learned to your audience (me!). Include the appropriate plots you've
  generated as mentioned above. Convert this to pdf and submit it. Submit your .ipynb file and an .html
  of it as well.
- Please use the following naming conventions:
  - Lab_5_[YOUR NAME].pdf
  - Lab_5_[YOUR NAME].html
  - Lab_5_[YOUR NAME].ipynb
- The grading rubric for this assignment will be available in Canvas.
- NO OTHER SUBMISSION TYPES WILL BE ACCEPTED.
