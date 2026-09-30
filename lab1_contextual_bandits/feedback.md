# Lab 1 feedback

From Jeremy Kedziora, Sep 30 2026. Score: 95/100.

## General comment

> Well done -- I totally support using Claude, but I will make a couple of suggestions. First, I don't
> think you need to/should task Claude as you did to recreate an already provided environment. That
> seems like a great opportunity for the token generation process to create differences between my
> intended lab and what you end up working with. Second, if you are working with Claude as a coding
> agent it will have learned implicit preferences over things and may try to add unnecessary stuff. I'm
> not sure if the multiple seeds idea came from you or from Claude, but no need for that here. If you
> want to publish research in RL then for sure you'd want multiple seeds. In class this will just extend
> training runs for no gain. You'll need to be on your guard so that these things don't creep in and
> change the lab in material ways.

## Rubric

| Criterion | Points | Comment |
|---|---|---|
| Q1: define MAB + example | 3/5 | Good description, but this lab isn't actually an example of a MAB precisely because of the user features. |
| Q2: when is pure greedy bad | 5/5 | Another problem case is a non-stationary process. Depending on implementation a greedy agent might not adapt to changes in the reward distributions. |
| Q3: A/B/n and T | 5/5 | With large enough T, A/B/n almost surely finds the optimal action (Law of Large Numbers), at the cost of possibly many suboptimal actions. |
| Q4: how the generator class works | 5/5 | |
| Set up CB environment with the provided class | 5/5 | |
| Four experiments (100, 1k, 10k, 100k turns) | 4/4 | |
| Q5: plots + theoretical average reward | 5/5 | Excellent. Not everybody uses the actual reward probabilities in the code to compute this. |
| CB algorithm implemented correctly | 10/10 | Looks right but probably more verbose than needed. "Maybe an example of where Claude isn't as good as it could be!" |
| Correct T and function approximators | 4/4 | |
| 3 experiment sets (as is, broken, fixed) over T, b, e | 30/30 | Multiple seeds not required here. Good practice if developing a new algorithm, not for class. |
| Q6: explain agent + function approximators | 5/5 | Good description. |
| Q7: how b and e affect the agent | 4/5 | b discussion was off. Without purging old data, the model fit at t=1000 with b=100 should be the same in expectation as with b=1000. The only difference is that fitting more often makes better decisions, which changes the class balance of the data. |
| Q8: advantages over multiclass supervised ML | 3/5 | Does it really avoid the expense of random data collection? T and epsilon *are* random data collection. Better point: managing explore/exploit lets the agent control the data generating process, which plain supervised learning can't. |
| Q9: breaking the environment | 7/7 | Good modifications. Nonstationarity is the easiest way to break it. Mine was limited (a single intervention). With periodic drift, picking a window size would be very hard, it depends on drift period vs b, and with low b any fixed window mixes in lots of old-distribution data. |

## Takeaways for next labs

- Use the instructor's provided code as-is. Don't have AI rewrite or re-implement it.
- One seed. No robustness extras unless the lab asks.
- Keep the agent code short.
- Be precise with definitions (contextual bandit is not a MAB).
- Think through what a parameter actually changes in expectation before claiming an effect.
