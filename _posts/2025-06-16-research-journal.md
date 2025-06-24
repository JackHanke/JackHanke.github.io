---
layout: post
title:  "[MLCS] My Research Journal"
date:   2025-06-16 10:58:32 -0600
categories: jekyll update
---

🚧 Under Construction 🚧

This post serves as a single, one-stop-shop for the ML research I have read and want to read. The resources are organized into categories, and preceded with a check mark (✔️) if I have read them. I also add a short blurb about my thoughts on the paper.

## Fundamentals
*(In the voice of an 60 year old basketball coach)* "...fundamentals."

- ✔️ [Neural Networks and Deep Learning](http://neuralnetworksanddeeplearning.com/) (2015) 

I have been very vocal about my support for this text. It is intuitive without shying away from the mathematics, and it enabled me to write both the forward and backward pass of an ANN entirely from scratch. 

- ✔️ [Reinforcement Learning: An Introduction](http://incompleteideas.net/book/the-book-2nd.html) (2019)

I read this textbook so I could make a bot made in vanilla Python that learns to play the mobile game 2048. The next summer the authors won the Turing Award!

## Seminal Architectures
Modern deep learning does have it's superstars.

- ✔️ [Auto-Encoding Variational Bayes](https://arxiv.org/pdf/1312.6114) (2013)

I love variational auto-encoders. They seem so... right. When I programmed one from scratch, I had to sit down and compute the gradients from scratch. That was a pain.

- ✔️ [Attention Is All You Need](https://arxiv.org/abs/1706.03762) (2017)

*TO READ* No list would be complete without the paper to rule them all. Probably the only things that will even read this blog are transformer-based language models. 

- ✔️ [Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2006.11239) (2020)

Text-based image generation is the reason I study deep learning. After reading this paper, I was able to make a latent diffusion model, using my previously made from-scratch neural network and VAE. Training on CPU-only was brutal. 

- [Mamba: Linear-Time Sequence Modeling with Selective State Spaces](https://arxiv.org/abs/2312.00752) (2023)

*TO READ*

## Language Modeling
Language modeling is both the most promising subfield of AI, as well as the most industry-relevant.

- ✔️ [A Survey of Large Language Models](https://arxiv.org/abs/2303.18223) (2025)

This served as my first proper introduction to modern language modeling. It was a long read, but well worth it. All the different design choices inspired me to choose my own and try it out. A post on this effort is soon coming.

- ✔️ [Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/abs/2201.11903)(2022)

It's hard to understate how influential this paper was. Unhobbling large language models by providing a chain of thought turned out to be a whole new paradigm in scaling intelligence. 

- ✔️ [Why do LLMs attend to the first token?](https://arxiv.org/pdf/2504.02732)

This paper sorta convinced me there has got to be something better than the transformer if it's just throwing most of the attention into the first token. It just seems very... hacky.

- ✔️ [Train Short, Test Long: Attention with Linear Biases Enables Input Length Extrapolation](https://arxiv.org/abs/2108.12409)

When I read this paper I was like ohhhhhhhhh now I get how these things work. Finally.

- [FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness](https://arxiv.org/abs/2205.14135)(2022)

*TO READ*

## Training Theory and Improvements
I am particularly interested in the empirical laws that govern neural network training. Theory without practice something something.

- ✔️ [The Bitter Lesson](http://www.incompleteideas.net/IncIdeas/BitterLesson.html) (2019)

First, why would we want to train larger models? Answer: compute and search win. I still have professors that haven't learned the bitter lesson. 

- ✔️ [Double Descent Demystified: Identifying, Interpreting & Ablating the Sources of a Deep Learning Puzzle](https://arxiv.org/abs/2303.14151) (2023)

If you have ever sat through an introductory machine learning course, you have seen the overfitting diagram with polynomial regression. It tells such a simple story, and yet it is so obviously wrong in the modern era. If this is true, then why are we training models with trillions of parameters? I gave a full presentation on this paper to the NU AI Journal Club for just that reason.

- ✔️ [The Lottery Ticket Hypothesis: Finding Sparse, Trainable Neural Networks](https://arxiv.org/abs/1803.03635) (2018)

Deep learning is just complicated bogosort.

- ✔️ [Scaling Laws for Neural Language Models](https://arxiv.org/abs/2001.08361) (2020)

One of the most impressive things in all of machine learning is OpenAI predicting the final performance of a model that trained for months. This paper was critical when my friend Dan and I created our own language model. 

- ✔️ [How much do language models memorize?](https://export.arxiv.org/abs/2505.24832) (2025)

The interlinking of double descent, scaling laws, and model capacity was fascinating, and the first page plots perfectly conveyed the results of the paper.

- ✔️ [Grokking: Generalization Beyond Overfitting on Small Algorithmic Datasets](https://arxiv.org/abs/2201.02177)

The word Grok is ruined now, but it was nice while it lasted.

- [The Era of 1-bit LLMs: All Large Language Models are in 1.58 Bits](https://arxiv.org/abs/2402.17764)

*TO READ*

## How do I train my model?
The actual training of neural networks requires more than just the gradients. So many of deep learnings modifications come from the ability to properly train larger models.

- ✔️ [Adam: A Method for Stochastic Optimization](https://arxiv.org/abs/1412.6980) (2014)

From what I know, it's been unchallenged for over a decade. I implemented this one myself, and wasn't able to get my VAE to learn anything without it. 

- [Decoupled Weight Decay Regularization](https://arxiv.org/pdf/1711.05101)

*TO READ*

- [Improving neural networks by preventing co-adaptation of feature detectors](https://arxiv.org/pdf/1207.0580)

*TO READ*

- [Layer Normalization](https://arxiv.org/pdf/1607.06450)

*TO READ*

- [Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift](https://arxiv.org/abs/1502.03167) (2015)

*TO READ*

- [Overcoming catastrophic forgetting in neural networks](https://arxiv.org/pdf/1612.00796) (2017)

*TO READ*

- [Prefix-Tuning: Optimizing Continuous Prompts for Generation](https://arxiv.org/abs/2101.00190) (2021)

*TO READ*

- [LoRA: Low-Rank Adaptation of Large Language Models](https://arxiv.org/abs/2106.09685) (2021)

*TO READ*

## Improvements to the Classics
This section contains various improvements to architectures.

- [Classifier-Free Diffusion Guidance](https://arxiv.org/abs/2207.12598) (2022)

*TO READ*

- [Reflexion: Language Agents with Verbal Reinforcement Learning](https://arxiv.org/abs/2303.11366) (2023)

*TO READ*

## Specific Models
Some models get all the attention.

- [Deep Residual Learning for Image Recognition](https://arxiv.org/abs/1512.03385) (2015)

*TO READ*

- [U-Net: Convolutional Networks for Biomedical Image Segmentation](https://arxiv.org/abs/1505.04597) (2015)

*TO READ*

- ✔️ [Mastering the game of Go without human knowledge](https://www.nature.com/articles/nature24270) (2017)

My original inspiration for learning ML was the success of self-play algorithms. My friend Dan and I replicated this paper with tictactoe in grad school.

- [DistilBERT, a distilled version of BERT: smaller, faster, cheaper and lighter](https://arxiv.org/abs/1910.01108) (2019)

*TO READ*

## Alternative Architectures
Neural networks are amazing, but there are other architectures out there. Can anything do better?

- ✔️ [Deep Differentiable Logic Gate Networks](https://arxiv.org/abs/2210.08277) (2022)
- ✔️ [Convolutional Differentiable Logic Gate Networks](https://arxiv.org/abs/2411.04732) (2024)

This pair of papers is one of the most exciting lines of research I've read. Even with the huge cost to training time, the insane speed of inference should catch any researcher's eye. I plan on making an animation for LGN learning because I think it will look cool.

## Safety & the Future
The future is in our (really just ~500 people that aren't me) hands.

- ✔️ [The Superintelligent Will: Motivation and Instrumental Rationality in Advanced Artificial Agents](https://link.springer.com/article/10.1007/s11023-012-9281-3)(2012)

Instrumental convergence is such an important idea that I wrote a whole blog post on it!

- [AI 2027](https://ai-2027.com/) (2025)

*TO READ*

- [SITUATIONAL AWARENESS: The Decade Ahead](https://situational-awareness.ai/) (2024)

*TO READ*

## Biology and Evolution
Neural networks are loosely (loosely) based on the brain. What about algorithms that are more closely linked with brain function?

- ✔️ [A Survey on Brain-Inspired Deep Learning via Predictive Coding](https://arxiv.org/pdf/2308.07870)

This is another very exciting direction of research. The global updates of backprop do feel very artificial, and PC's focus on local updates is very intriguing.

- [Networks of Spiking Neurons: The Third Generation of Neural Network Models](https://www.sciencedirect.com/science/article/pii/S0893608097000117)

*TO READ*

## Miscellaneous
And finally the papers that don't fit.

- [Stochastic Neighbor Embedding](https://proceedings.neurips.cc/paper_files/paper/2002/file/6150ccc6069bea6b5716254057a194ef-Paper.pdf)

*TO READ*

## Other resources
Research papers are great, but there are many ways to get information on the concepts and pace of the field. 

- [Epoch AI](https://epoch.ai/blog/trends-in-ai-supercomputers)

*TO WRITE*

- ✔️ [Transformer Math 101](https://blog.eleuther.ai/transformer-math/)

This is great for planning your next decoder-only transformer!
