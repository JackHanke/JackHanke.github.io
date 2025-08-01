---
layout: post
title:  "Reinforcement Learning from Scratch"
date:   2024-09-28 19:58:34 -0600
categories: [Machine Learning]
tags: [Neural Networks, RL, RAMNets, From Scratch]
math: true
image:
  path: assets/img/viz.gif
---

| [GitHub Repo 👾](https://github.com/JackHanke/2048rl) | **Scope:** ⭐⭐⭐ |

## Pavlov's program

I've often heard reinforcement learning referred to as the closest to how humans and animals really learn, that being associating actions with positive and negative rewards. Pavlov's experiments with ringing a bell before feeding a dog come to mind, where a dog learns that the ringing of a bell is associated with dinner time. Incredibly, one of the core concepts in RL, the temporal difference error (to be discussed later), has even been [shown to be present in dopamine firing rates](https://en.wikipedia.org/wiki/Temporal_difference_learning#In_neuroscience) in humans. This seems to support the idea that our brains are doing RL, at least in part. 

So what project should you learn reinforcement learning with? I suggest picking a simple game! 

## A game designed to learn RL

I had been playing the mobile game [2048](https://www.2048.org/) since it was released. It's a highly addictive puzzle game that involves swiping blocks that combine if they are the same number.

It also happens to be the perfect choice for a beginner reinforcement learning project. It has a large state space, a small action space (swiping directions), non-trivial strategies, and a clearly defined reward function (the score). The game is also [entirely open source](https://github.com/gabrielecirulli/2048). I decided I would make the bot after I completed reading the [Sutton and Barto](http://www.incompleteideas.net/book/the-book.html) textbook. The textbook covers the three core ideas in reinforcement learning, *Temporal Difference Learning*, *Q Learning*, and *Policy Optimization*. It was an intense summer read, and by the time I completed it I was itching to get started.

The first challenge the project posed was the game itself. I initially used a pre-built Python implementation called [Macht](https://github.com/rolfmorel/macht), but later chose to implement the game from scratch to speed up training as much as possible. 

## Finding an approach

For the learning algorithm, I initially wanted to do policy optimization, as opposed to TD or Q learning. I will cover policy optimization in a later post, but it essentially encompasses all of the RL pipeline into a single function, usually a neural network. I really liked the idea of having a single neural network that knows how to play 2048. However, when I wrote this up, the performance was really poor. I later learned that the reason was that I didn't implement a replay buffer of past experiences, and was just training the network on a single board, reward pair. This was so noisy that the network failed to learn anything. I abandoned the technique because at the time I didn't knot this, and the (self-imposed) setback led me to see if other people had written 2048 RL bots.

I was surprised to find that not only have people written 2048 RL bots, but there is serious academic research on the subject. I looked at a series of papers, the most important being [Temporal Difference Learning of N-Tuple Networks for the Game 2048](https://www.cs.put.poznan.pl/wjaskowski/pub/papers/Szubert2014_2048.pdf) by Szubert and colleagues. They showed that TD learning with a [RAMnet](https://en.wikipedia.org/wiki/RAMnets) approximator worked well for 2048, and so I chose to pursue this. 

TD learning sets out to learn the value, or the expected sum of all future rewards, from being in a specific state $s$, if the current policy $\pi$ is followed. We true value of a state $s$, denoted $V^{\pi}(s)$, is computed via the reward $R_1$ received using the following *update rule:*

$$V(s) \leftarrow (1-\alpha)V(s) + \alpha\left(R_{t+1} + \gamma V(S_{t+1})\right)$$

Following the Szubert paper, our $V$ function is an N-tuple network, AKA a RAMNet. A RAMNet is essentially a table of values like with traditional RL, except it is only a table for all possible *portions* of the state vector $s$. In our example, instead of the entire $4 \times 4$ 2048 board, it is, for example, the top-right $2 \times 2$ board, the top-left $2 \times 2$ board, etc. If $s_1, s_n$ are the portions of $s$ that we record, we treat each $s_i$ as its own state, updating each $V(s_i)$ with the above update rule. The final value for $s$ is computed by averaging over all $V(s_i)$.

## Implementing everything

There was a grueling episode of debugging this implementation. Like the many other RL-from-scratch projects I had read about, the "code runs but learning fails" part of the ML development pipeline was arduous. 

Eventually, I did find all the errors. It is hard to describe the feeling of elation of finally seeing my algorithm learning to play one of my favorite games. After training for ~10,000 games, the agent achieves the 2048 tile 49.1% of the time, and achieves the 4196 tile 1.5% of the time.

Now that the bot was trained, the final step was hooking up the history of the game to play on the real game's visualizer. I cloned the original repo and changed the internal Javascript to play a JSON history to make the final animation. I decided to showcase one of my agent's best games, shown above.

This was one of my favorite animations to create, and I still enjoy watching it to this day. I look forward to writing more RL bots in the future.


Thanks for reading!

## Sources

- [Reinforcement Learning: An Introduction](http://incompleteideas.net/book/the-book-2nd.html)
- [Temporal Difference Learning of N-Tuple Networks for the Game 2048](https://www.cs.put.poznan.pl/wjaskowski/pub/papers/Szubert2014_2048.pdf)
