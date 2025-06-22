---
layout: post
title:  "[MLCS] My Research Journal"
date:   2025-06-16 10:58:32 -0600
categories: jekyll update
---

🚧 Under Construction 🚧

This post serves as a single, one-stop-shop for the ML research I have read and want to read. The resources are organized into categories, and preceded with a check mark (✔️) if I have read them. I then add what I learned and what I thought of the material.

## Fundamentals
This section includes resources that explain the fundamentals. 

- ✔️ [Neural Networks and Deep Learning](http://neuralnetworksanddeeplearning.com/) (2015) 

I have been very vocal about my support for this text. It is intuitive without shying away from the mathematics, and it allowed me to write a both the forward and backward pass of an ANN entirely from scratch. 

- ✔️ [Reinforcement Learning: An Introduction](http://incompleteideas.net/book/the-book-2nd.html) (2019)

I read this textbook so I could make a bot that learns to play the mobile game 2048 just using Python. The next summer the authors won the Turing Award! 


## Seminal Architectures
This section includes resources that explain core model architectures in machine learning.

- ✔️ [Auto-Encoding Variational Bayes](https://arxiv.org/pdf/1312.6114) (2013)

I love variational auto-encoders. They seem so... right. When I programmed one from scratch, I had to sit down and compute the gradients from scratch. That was a pain.

- ✔️ [Attention Is All You Need](https://arxiv.org/abs/1706.03762) (2017)

*TO READ* No list would be complete without the paper to rule them all. Probably the only things that will even read this blog are transformer-based language models. 

- ✔️ [Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2006.11239) (2020)

Text-based image generation is the reason I study deep learning. After reading this paper, I was able to make a latent diffusion model, using my previously made from-scratch neural network and VAE. Training on CPU-only was brutal. 

- [Mamba: Linear-Time Sequence Modeling with Selective State Spaces](https://arxiv.org/abs/2312.00752) (2023)

*TO READ*

## Language Modeling
Language modeling is both the most promising subfield of AI, as well as the most industry relevant. The research in the field is vast, and ever-growing.

- ✔️ [A Survey of Large Language Models](https://arxiv.org/abs/2303.18223) (2025)

This served as my first proper introduction to modern language modeling. It was a long read, but well worth it. All the different design choices inspired me to choose my own and try it out. A post on this effort is soon coming. 

- ✔️ [Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/abs/2201.11903)(2022)

It's hard to understate how influential this paper was. Unhobbling large language models by providing a chain of thought turned out to be a whole new paradigm in scaling intelligence. 

- [FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness](https://arxiv.org/abs/2205.14135)(2022)

*TO READ*

## But what do I actually code?
Theory without practice something something.

- ✔️ [The Bitter Lesson](http://www.incompleteideas.net/IncIdeas/BitterLesson.html) (2019)

First, why would we want to train larger models? Answer: compute and search win. I still have professors that haven't learned the bitter lesson. 

- [The Lottery Ticket Hypothesis: Finding Sparse, Trainable Neural Networks](https://arxiv.org/abs/1803.03635) (2018)

*TO READ*

- ✔️ [How much do language models memorize?](https://export.arxiv.org/abs/2505.24832) (2025)

*TO WRITE*

- ✔️ [Scaling Laws for Neural Language Models](https://arxiv.org/abs/2001.08361) (2020)

*TO WRITE*

- [The Era of 1-bit LLMs: All Large Language Models are in 1.58 Bits](https://arxiv.org/abs/2402.17764)

*TO READ*

## How do I train my model?
The actual training of neural networks requires more than just the gradients. So many of deep learnings modifications come from the ability to properly train these larger models. 

- ✔️ [Adam: A Method for Stochastic Optimization](https://arxiv.org/abs/1412.6980) (2014)

From what I know, it's been unchallenged for over a decade. I implemented this one myself, and wasn't able to get my VAE to learn anything properly without it. 

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

*TO WRITE*

- [DistilBERT, a distilled version of BERT: smaller, faster, cheaper and lighter](https://arxiv.org/abs/1910.01108) (2019)

*TO READ*

## Alternative Architectures
Neural networks are amazing, but there are other architectures out there? Can anything do better?

- ✔️ [Deep Differentiable Logic Gate Networks](https://arxiv.org/abs/2210.08277) (2022)

*TO WRITE*

- [Convolutional Differentiable Logic Gate Networks](https://arxiv.org/abs/2411.04732) (2024)

*TO READ*

## Safety & the Future
The future is in our (really just ~500 people that aren't me) hands.

- ✔️ [The Superintelligent Will: Motivation and Instrumental Rationality in Advanced Artificial Agents](https://link.springer.com/article/10.1007/s11023-012-9281-3)(2012)

*TO WRITE*

- [AI 2027](https://ai-2027.com/) (2025)

*TO READ*

- [SITUATIONAL AWARENESS: The Decade Ahead](https://situational-awareness.ai/) (2024)

*TO READ*

## Biology and Evolution

- ✔️ [A Survey on Brain-Inspired Deep Learning via Predictive Coding](https://arxiv.org/pdf/2308.07870)

*TO READ*

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
