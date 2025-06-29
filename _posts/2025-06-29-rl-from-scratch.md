---
layout: post
title:  "[MLCS] Reinforcement Learning from Scratch"
date:   2025-06-28 19:58:34 -0600
categories: jekyll update
---

| [GitHub Repo 👾](https://github.com/JackHanke/2048rl) | **Scope:** ⭐⭐⭐ |

## Pavlov's program

I've often heard reinforcement learning referred to as the closest to how humans and animals really learn, that being associating actions with positive and negative rewards. Pavlov's experiments with ringing a bell before feeding a dog come to mind, where a dog learns that the ringing of a bell is associated with dinner time. Incredibly, one of the core concepts in RL, the temporal difference error (to be discussed later), has even been [shown to be present in dopamine firing rates](https://en.wikipedia.org/wiki/Temporal_difference_learning#In_neuroscience) in humans. This seems to support the idea that our brains are "RL-first". 

So what project should you learn reinforcement learning with? I suggest picking a simple game! 

## A game designed to learn RL

I had been playing [2048](https://www.2048.org/) since it was released. It is the perfect choice for a beginner reinforcement learning project. It has a large state space, small action space, non-trivial strategies, and a clearly defined reward function. The game is also [entirely open source](https://github.com/gabrielecirulli/2048). I decided I would make the bot after I completed the [Sutton and Barto](http://www.incompleteideas.net/book/the-book.html) textbook. It was an intense summer read, and by the time I completed it I was itching to get started. 

The first challenge the project posed was the game itself. I initially used a pre-built Python implementation called [Macht](https://github.com/rolfmorel/macht), but later chose to implement the game from scratch to speed up training as much as possible. 

## Finding an approach

**TODO explain td learning and policy optimization**

As for the learning algorithm, I initially wanted to do policy optimization, as opposed to TD or Q learning. I really liked the idea of having a single neural network that does everything. However, the performance was really poor (I learned later that the reason was that I didn't implement a replay buffer of past experiences, and was just training the network on a single board, reward pair. This was so noisy that the network failed to learn anything). I abandoned the technique quickly, and the (self-imposed) setback led me to see if other people had written 2048 RL bots.

I was surprised to find that not only have people written 2048 RL bots, but there is serious academic research on the subject. I looked at a series of papers, the most important being [Temporal Difference Learning of N-Tuple Networks for the Game 2048](https://www.cs.put.poznan.pl/wjaskowski/pub/papers/Szubert2014_2048.pdf) by Szubert and colleagues. They showed that TD learning with a [RAMnet](https://en.wikipedia.org/wiki/RAMnets) approximator worked well for 2048, and so I chose to pursue this. 

**TODO explain RAMnet and chosen format**

## Implementing everything

There was a grueling episode of debugging this implementation. Like the many other RL-from-scratch projects I had read about, the "code runs but learning fails" part of the ML development pipeline was arduous. 

It is hard to describe the feeling of elation of finally seeing my algorithm learning to play one of my favorite games. After training for ~10,000 games, the agent achieves the 2048 tile 49.1% of the time, and achieves the 4196 tile 1.5% of the time.

Now that the bot was trained, the final step was hooking up the history of the game to play on the real game's visualizer. I cloned the original repo and changed the internal Javascript to play a JSON history to make the final animation. I decided to showcase one of my agent's best games, shown below.

{:refdef: style="text-align: center;"}
![]({{ site.baseimg }}/assets/2048viz.gif)
{: refdef}

This was one of my favorite animations to create, and I still enjoy watching it to this day. I look forward to writing more RL bots in the future.


Thanks for reading!

## Sources

- [Reinforcement Learning: An Introduction](http://incompleteideas.net/book/the-book-2nd.html)
- [Temporal Difference Learning of N-Tuple Networks for the Game 2048](https://www.cs.put.poznan.pl/wjaskowski/pub/papers/Szubert2014_2048.pdf)
