|     | Lab           | Assignment | #5: Policy-Based |            |
| --- | ------------- | ---------- | ---------------- | ---------- |
|     | Reinforcement |            | Learning         | Algorithms |
1 Introduction
In this lab we will train an Advantage Actor–Critic (A2C) agent to play the Atari game
Space Invaders using the Gymnasium implementation of the environment. Unlike our
previous work with value-based methods, A2C is a modern policy-gradient method that
learns both a policy model (the “actor”) and a value function (the “critic”). Space In-
vadershashigh-dimensionalvisualstateinputs,requiringconvolutionalneuralnetworks
to learn useful representations. Your task is to set up the environment, preprocess obser-
vations,implementanA2Cagent,trainit,andevaluateitsperformance.
| 2 Set | up and | learn about | the environment |     |
| ----- | ------ | ----------- | --------------- | --- |
Space Invaders is a classic Atari arcade game in which the player controls a laser cannon
moving horizontally along the bottom of the screen, firing at alien invaders descending
| fromabove. | TheGymnasiumenvironmentprovides: |     |     |     |
| ---------- | -------------------------------- | --- | --- | --- |
• A210×160×3RGBframeateachstep.
1

• AdiscreteactionspaceincludingNOOP,FIRE,LEFT,RIGHT,LEFT/FIREandRIGHT/FIRE.
• Rewardsarerelativelydense–seehereformoredocumentation.
Tobegin(youwillnotneedtorepeatthisifyoualreadyinstalledgynamsiumforaprevi-
ouslab):
• Install Gymnasium and the ALE Atari integration. The following commands en-
tered at the command line enabled me to set up a virtual environment on Rosie in
which I could run the environment and train an agent, with a kernel recognized by
jupyternotebooks:
1. condacreate-npy Gympython=3.10
2. condaactivatepy Gym
3. pipinstallale-py
4. pipinstallautorom[accept-rom-license]
5. AutoROM--accept-license
6. pipinstall“gymnasium[atari,accept-rom-license]”--force-reinstall
7. pipinstalltorch
8. condainstallmatplotlib
9. condainstallipykernel
10. python -m ipykernel install –user –name=py Gym - -display-name=“Python
py Gym”
• Open the starter code, and then create and initialize the environment. I suggest
usingthehelperfunctionprovidedinthestartercode(seefigure2)forthispurpose.
Thisfunctionsetsuptheenvironmenttooutputgrayscaledandresizedscreens.
• Inspectwhattheenvironmentreturns:
2

– Printtheobservationshape.
– Printtheactionspace.
– Generate 100 episodes with random actions. Record the total reward for each
andplotthedistributionoverrewards.
| 3 Training | Agents | to  | play Space | Invaders | with A2C |
| ---------- | ------ | --- | ---------- | -------- | -------- |
We will practice using a modified version of the A2C algorithm. Our version will be an
episodic version which updates after every episode. We will also incorporate entropy
| regularization. | Givenapolicy | π(s | ),entropyisdefinedas: |     |     |
| --------------- | ------------ | --- | --------------------- | --- | --- |
t
∑
|     |     | H(π(s | )) = − π(a|s | )lnπ(a|s | ).  |
| --- | --- | ----- | ------------ | -------- | --- |
|     |     |       | t            | t        | t   |
a
Largerentropymeansthatthepolicyspreadsouttheprobabilitymore.
1. You will need to assemble a history of 4 screens to use as the state. Recall from
lecture that with a single screen the environment is partially observable since the
agent cannot infer movement. In the original DQN paper, the authors stacked 4
frames together to deal with this. I have provided three helper methods to do this:
| init history(),reshape |     | history(),andupdate |     | history(). |     |
| ---------------------- | --- | ------------------- | --- | ---------- | --- |
2. Use the default environment reward function. Reward information will be re-
turnedbytheenvironmentasitevolves.
3. ImplementA2C.Buildanagentclassandatrainingloopthatimplementstheepisodic
A2Cfromlecture. Thisalgorithmisconfiguredfortheactorandcritictosaveonpa-
rameters by sharing convolutional input layers that process raw screen input. This
iswhyonlyasinglegradientstepisrequired. Thus,youshouldusetheCNNActor-
Criticarchitecture(oranalogous)providedinthestartercode.
4. Train agents. Train an A2C agent for an appropriate number of episodes (e.g., 2–4
million frames). Atari learning takes time; adjust if computational resources are
limited. You will need to choose the discount factor γ, the learning rate, α, and the
| entropyregularizationweight |     |     | β.  |     |     |
| --------------------------- | --- | --- | --- | --- | --- |
5. Evaluate agents. Run your trained agent for 100 evaluation episodes with NO pa-
| rameterupdates. | Record: |     |     |     |     |
| --------------- | ------- | --- | --- | --- | --- |
• Averagereturnacrosstheepisodes.
• Thevarianceofthereturnacrosstheepisodes.
3

Lab Assignment Report
Write up your analysis in a separate report. Your report should answer the following
questions:
1. Describe how the Space Invaders environment works and what information it re-
turns.
2. Provideandinterprettheplotofrewarddistributionyougeneratedinsection2.
3. Describe the full state preprocessing pipeline and the role played by each method
involved. Explain how partial observability motivates frame stacking and discuss
howpreprocessingchangesthelearningproblem.
4. Describe the A2C architecture you implemented. Why does it have separate actor
and critic heads? Explain why shared representations are commonly used in mod-
ernreinforcementlearning.
5. Assess the performance of your trained agent using the data you collected about
rewardduringtheevaluationepisodes.
6. Provideavisualizationofthelearnedpolicy,showingwhattheagentchoosestodo
inrepresentativestates.
7. Design and conduct an experiment to determine what impact, if any, entropy has
on agent performance. What is the purpose of entropy regularization? How does
it impact parameter updates? Explain how entropy-based exploration differs from
e-greedy exploration and analyze its impact on exploration, convergence, and final
policyquality.
8. Investigatetheeffectofstatepreprocessingbycomparingdifferentobservationhis-
tories(forexample,oneframe,twoframes,andfourstackedframes).
4 Submission Instructions
• Write up the answers to the questions in a short word document; aim for no more
than2pagesofwrittentextandincludeallgraphicsgenerated. Addfootnotesiden-
tifying which sentence addresses which questions. Write in complete sentences
organized into paragraphs – your goal is to explain what you’ve done and what
you’ve learned to your audience (me!). Include the appropriate plots you’ve gener-
atedasmentionedabove. Convertthistopdfandsubmitit. Submityour.ipynbfile
andan.htmlofitaswell.
• Pleaseusethefollowingnamingconventions:
– Lab 5 [YOURNAME].pdf
– Lab 5 [YOURNAME].html
4

– Lab 5 [YOURNAME].ipynb
• ThegradingrubricforthisassignmentwillbeavailableinCanvas.
• NOOTHERSUBMISSIONTYPESWILLBEACCEPTED.
• You are welcome to use AI tools for implementation as you code up your solution,
buttheemphasisisonexperimentaldesign,analysis,andscientificreasoning. Ifyou
dothisyoumustincludeallpromptsusedasanappendixinyourwrittenreportor
provideashareablelinktothem.
5