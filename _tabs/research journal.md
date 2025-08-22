---
# layout: tags
icon: fa fa-paperclip
order: 2
math: true
---

A collection of machine learning research I have read and want to read. The resources are organized into categories, and preceded with a check mark (✔️) if I have read them. I also add a short blurb about my thoughts on the work.

---

## Fundamentals
*(In the voice of an 60 year old basketball coach)* "...fundamentals."

- ✔️ [Neural Networks and Deep Learning](http://neuralnetworksanddeeplearning.com/) (2015) 

This is the best introduction to deep learning I know of, and it's free. I have been very vocal about my support for this text. It is intuitive without shying away from the mathematics, and it enabled me to write both the forward and backward pass of an ANN entirely from scratch. Michael Nielsen deserves serious praise for this resource.

- ✔️ [Reinforcement Learning: An Introduction](http://incompleteideas.net/book/the-book-2nd.html) (2019)

This is the foundational text in reinforcement learning. It is the best possible starting resource for learning RL. I read this over the summer so I could make a bot made in vanilla Python that learns to play the mobile game 2048. The next summer the authors won the Turing Award!

---

## Seminal Architectures
Modern deep learning does have its superstars.

- ✔️ [Auto-Encoding Variational Bayes](https://arxiv.org/pdf/1312.6114) (2013)

This work introduces variational autoencoders, or VAEs. I love VAEs. From a mathematical sense, they seem so... right. When I programmed one from scratch, I had to sit down and compute the gradients from scratch. That was a pain, but seeing it to work was very rewarding.

- ✔️ [Attention Is All You Need](https://arxiv.org/abs/1706.03762) (2017)

This work introduces the Transformer, easily one of the most influential papers in deep learning. No research list would be complete without it. I also made multi-head attention entirely from scratch, which also had pretty annoying gradients to compute. Probably the only things that will even read this blog are transformer-based language models...

- ✔️ [Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2006.11239) (2020)

This work popularized denoising diffusion for image generation. Text-based image generation is one of the the reasons I study deep learning. The fact that computers can draw now is amazing to me. After reading this paper, I was able to make a latent diffusion model from scratch, using my previously made neural network and VAE. I had to train it on CPU-only, which was so slow even for MNIST. 

- [Mamba: Linear-Time Sequence Modeling with Selective State Spaces](https://arxiv.org/abs/2312.00752) (2023) *TO READ*

---

## Language Modeling
Language modeling is both the most promising subfield of AI, as well as the most industry-relevant.

- ✔️ [A Survey of Large Language Models](https://arxiv.org/abs/2303.18223) (2025)

This is a comprehensive survey of the creation of a large language model. I didn't fully understand LLMs until I read this paper. It's a long read, but well worth it. All the different design choices inspired me to create my own LLM.

- ✔️ [Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/abs/2201.11903 ) (2022)

This paper is an observation that providing example chains of thought in the prompt can yield better reasoning for few-shot prompted LLMs. For example, typing out a thought process for a math problem in a prompt improved performance on a follow up math problem. It's hard to understate how influential this paper was. Unhobbling large language models by providing a chain of thought pushed the field into test-time scaling. Now every real LLM is a reasoning model. 

- ✔️ [Why do LLMs attend to the first token?](https://arxiv.org/pdf/2504.02732) (2025)

This paper studies the phenomena where trained language models often allocate a significant amount of attention to the first token in the sequence. They provide reasoning as to why this might be, and why it is useful for an LLM to do this. These results give me the suspicion that there is a better than the transformer. If it's just throwing most of the attention into the first token, it seems very... hacky. I want to give this some more thought, maybe there is something better just around the corner!

- ✔️ [Train Short, Test Long: Attention with Linear Biases Enables Input Length Extrapolation](https://arxiv.org/abs/2108.12409) (2021)

This paper introduces ALiBi, a method of position encoding tokens in the attention mechanism that generalize well to longer sequences than seen in training. When I read this paper I was like oh now I get how these things work. Finally. I think RoPE is what the new OpenAI FOSS models, so maybe this is no longer the best for length extrapolation?

- [FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness](https://arxiv.org/abs/2205.14135) (2022) *TO READ*

- [DiffuSeq: Sequence to Sequence Text Generation with Diffusion Models](https://arxiv.org/abs/2210.08933)(2023) *TO READ*

---

## Image Generation
Image generation is just the coolest. Computers can draw!

- [Learning Transferable Visual Models From Natural Language Supervision](https://arxiv.org/abs/2103.00020) (2021) *TO READ*

- [Denoising Diffusion Implicit Models](https://arxiv.org/abs/2010.02502) (2020) *TO READ*

- [Classifier-Free Diffusion Guidance](https://arxiv.org/abs/2207.12598) (2022) *TO READ*

- [Score-Based Generative Modeling through Stochastic Differential Equations](https://arxiv.org/abs/2011.13456) (2021) *TO READ*

- [Hierarchical Text-Conditional Image Generation with CLIP Latents](https://cdn.openai.com/papers/dall-e-2.pdf) (2022) *TO READ*

- [Flow Matching for Generative Modeling](https://arxiv.org/abs/2210.02747) (2023) *TO READ*

---

## Training Theory and Improvements
I am particularly interested in the empirical laws that govern neural network training. Theory without practice something something.

- ✔️ [The Bitter Lesson](http://www.incompleteideas.net/IncIdeas/BitterLesson.html) (2019)

This is a short essay about how the field of AI is moving toward scaling simple algorithms to massive compute. The lesson is bitter because all of the specialized human research into language, computer vision, etc. underperforms algorithms that require a data center to train. It is pretty bitter. It leaves so much of the contributions to enormous corporations. 

- ✔️ [Double Descent Demystified: Identifying, Interpreting & Ablating the Sources of a Deep Learning Puzzle](https://arxiv.org/abs/2303.14151) (2023)

This paper studies the phenomena of double descent, the idea that neural networks initially lose performance when scaling the number of parameters, but as you continue the test loss "descends again". If you have ever sat through an introductory machine learning course, you have seen the overfitting diagram with polynomial regression. It tells such a simple story, and yet it is so obviously wrong in the modern era. I gave a 45 minute talk on this paper to the NU AI Journal Club because I found this paper so interesting.

- ✔️ [The Lottery Ticket Hypothesis: Finding Sparse, Trainable Neural Networks](https://arxiv.org/abs/1803.03635) (2018)

This paper proposes the lottery ticket hypothesis, which claims that when training a large neural network, the network learns many concurrent sparse networks. The fastest learning of which, that being the subnetwork that won the "initialization lottery", dominates the gradients and consequently performance. Essentially, deep learning is bogosort.

- ✔️ [Scaling Laws for Neural Language Models](https://arxiv.org/abs/2001.08361) (2020)

This paper details the empirical trends of training large language models, with a focus on a fixed compute budget. It sets out to answer questions like how big of a model should I have, how much data should I have, and how big of a batch should I have for some fixed amount of time on a GPU cluster. One of the most impressive things in all of machine learning is OpenAI predicting the final performance of GPT-4, which trained for months. This paper was critical when my friend Dan and I created our own language model. 

- ✔️ [How much do language models memorize?](https://export.arxiv.org/abs/2505.24832) (2025)

This paper studies how well large language models generalize with increasing scale. They show that GPT models memorize until their capacity is reached, at which point understanding of the true data-generation process begins. The interlinking of double descent, scaling laws, and model capacity was fascinating, and the first page plots perfectly conveyed the results of the paper.

- ✔️ [Grokking: Generalization Beyond Overfitting on Small Algorithmic Datasets](https://arxiv.org/abs/2201.02177) (2022)

This paper discusses the phenomena where significant improvements in test loss can come rapidly, long after training loss has bottomed out. This rapid decrease in test loss is called grokking. The cause of grokking was deeply studied in a [blogpost by Neel Nanda](https://www.alignmentforum.org/posts/N6WM6hs7RQMKDhYjB/a-mechanistic-interpretability-analysis-of-grokking), where he determined the underlying anlgorithm learned by a transformer to do mod $113$ arithmetic. He showed that grokking is affected by how much data is available to train on compared with the total dataset (in this case $113^2$ datapoints), among other results.  Nowadays the word grok is ruined, but it was nice while it lasted.

- [The Era of 1-bit LLMs: All Large Language Models are in 1.58 Bits](https://arxiv.org/abs/2402.17764) (2024) *TO READ*

- ✔️ [Deep Reinforcement Learning with Double Q-learning](https://arxiv.org/pdf/1509.06461) (2015) 

This paper introduces a modification to deep Q-learning towards double Q-learning that improves performance on Atari. The idea is to have a time-lagged version of the online policy predict the value of the action in the update equation, while the online policy is used to select actions. This acts to decouple the choice of estimating Q values and selecting actions (in the style of double Q-learning), but without splitting training among two networks as traditional double Q-learning would imply.

- ✔️ [The Difficulty of Passive Learning in Deep Reinforcement Learning](https://arxiv.org/abs/2110.14020) (2021)

This paper shows that offline RL learning exhibits a performance decrease from online RL algorithms even when the offline learner trains on data from the actions and experiences of the online learner. They compare this to the 1963 psychology experiment by Held and Hein called the [Kitten carosel](https://www.simplypsychology.org/held-and-hein-1963.html), and do a thorough ablation study of what causes the tandem effect. They conclude that the source of the tandem effect is in the combination of deep function approximation and insufficient data of non-greedy actions. Overall, this paper emphasizes the role of interactivity in the learning process and provides a framework for understanding the differences in online and offline learning.

- ✔️ [The Platonic Representation Hypothesis](https://arxiv.org/abs/2405.07987) (2024)

This paper introduces the hypothesis that the representations large models arrive at are essentially the same, even for models across multiple data domains (text, images, etc). They summarize the hypothesis as "all strong models are alike, each weak model is weak in its own way". As someone who is very interested in learning why scaling works, this was a fascinating read.

---

## How do I train my model?
The actual training of neural networks requires more than just the gradients. Much of deep learning's success comes from the ability to properly train larger models.

- ✔️ [Understanding the difficulty of training deep feedforward neural networks](https://proceedings.mlr.press/v9/glorot10a/glorot10a.pdf) (2010)

This paper introduces a better way to initialize weights in a neural network than the usual samples from the standard normal. I was digging around some of the PyTorch source code and found a reference to this paper in the comments, so this paper is still informing current models!

- ✔️ [Adam: A Method for Stochastic Optimization](https://arxiv.org/abs/1412.6980) (2014)

This paper introduces the Adam optimizer, a variant of SGD that computes estimates of the first and second moments of the gradients. From what I know, its been unchallenged for over a decade. I implemented this one myself, and wasn't able to get my VAE to learn anything without it. 

- [Decoupled Weight Decay Regularization](https://arxiv.org/pdf/1711.05101) (2019) *TO READ*

- [Improving neural networks by preventing co-adaptation of feature detectors](https://arxiv.org/pdf/1207.0580) (2012) *TO READ*

- [Dropout: A Simple Way to Prevent Neural Networks from Overfitting](https://www.cs.toronto.edu/~rsalakhu/papers/srivastava14a.pdf) (2014) *TO READ*

- ✔️ [Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift](https://arxiv.org/abs/1502.03167) (2015)

This paper introduces a technique to remedy a problem in training deep neural networks called internal covariance shift, which is the distribution of each layer's input changes during training. This technique, called batch normalization, normalizes each element of each layer activation using estimates of the mean and variances computed over the minibatch. It then adds two learnable parameters to linearly scale each normalized activation. This is another favorite paper of mine. I love how dramatic the improvement is for how simple the solution is. A paper that makes you go: why didn't I think of that?

- ✔️ [Layer Normalization](https://arxiv.org/pdf/1607.06450)

This paper is a followup to the Batch Normalization paper. Batch normalization, though effective, does slow down for large batch sizes. Layer normalization instead normalizes each element of each layer activation using the same estimate of the mean and variance over the hidden layers of the network, but has different normalization across the batch. This allows for normalization with a batch size of one.

- [Overcoming catastrophic forgetting in neural networks](https://arxiv.org/pdf/1612.00796) (2017) *TO READ*

- [Prefix-Tuning: Optimizing Continuous Prompts for Generation](https://arxiv.org/abs/2101.00190) (2021) *TO READ*

- [LoRA: Low-Rank Adaptation of Large Language Models](https://arxiv.org/abs/2106.09685) (2021) *TO READ*

---

## Improvements to the Classics
This section contains various improvements to classic architectures.

- [Reflexion: Language Agents with Verbal Reinforcement Learning](https://arxiv.org/abs/2303.11366) (2023) *TO READ*

- [Decision Transformer: Reinforcement Learning via Sequence Modeling](https://arxiv.org/abs/2106.01345) (2021) *TO READ*

- [Multi-Game Decision Transformers](https://arxiv.org/abs/2205.15241) (2022) *TO READ*

- [Scaling Laws For Diffusion Transformers](https://arxiv.org/pdf/2410.08184) (2024) *TO READ*

- [Hierarchical Reasoning Model](https://arxiv.org/pdf/2506.21734) (2025) *TO READ*

## Specific Models
Some models get all the attention.

- [Deep Residual Learning for Image Recognition](https://arxiv.org/abs/1512.03385) (2015) *TO READ*

- [U-Net: Convolutional Networks for Biomedical Image Segmentation](https://arxiv.org/abs/1505.04597) (2015) *TO READ*

- ✔️ [Mastering the game of Go without human knowledge](https://www.nature.com/articles/nature24270) (2017)

This paper covers Google Deepmind's AlphaGo Zero self-play training pipeline. Incredibly, they showed that (at least at the time) that pure-RL learns better representations than supervised learning. The news of this result was my original inspiration for learning ML. My friend Dan and I replicated this paper with tictactoe in grad school. 

---

## Alternative Architectures
Neural networks are amazing, but there are other architectures out there. Can anything do better?

- ✔️ [Deep Differentiable Logic Gate Networks](https://arxiv.org/abs/2210.08277) (2022)
- ✔️ [Convolutional Differentiable Logic Gate Networks](https://arxiv.org/abs/2411.04732) (2024)

This pair of papers introduces networks where instead of learning floating point parameters, they learn the choice of a specific logic gate. Training these networks is slower, but the inference speed is insanely fast. Even better, one can print a tiny ASIC with the learned network to get inference blazingly fast.

- [Deep Stochastic Logic Gate Networks](https://ieeexplore.ieee.org/abstract/document/10301592) (2024) *TO READ*

- [The Forward-Forward Algorithm: Some Preliminary Investigations](https://arxiv.org/abs/2212.13345) (2022) *TO READ*

- [Liquid Time-constant Networks](https://arxiv.org/pdf/2006.04439) (2020) *TO READ*

---

## Safety & the Future
The future is in our (really just ~500 people that aren't me) hands.

- ✔️ [The Superintelligent Will: Motivation and Instrumental Rationality in Advanced Artificial Agents](https://link.springer.com/article/10.1007/s11023-012-9281-3) (2012)

This essay introduces a series of fundamental concepts in AI safety, namely instrumental convergence. Instrumental convergence is the idea that any significantly capable agent with almost any terminal goal would necessarily create certain sub-goals in its pursuit. These instrumental sub-goals include access to resources and self preservation. I never know how useful old AI Safety work is because of the transition away from RL-based systems, but it is a useful idea. I even wrote a blog post about it!

- [AI 2027](https://ai-2027.com/) (2025) *TO READ*

- [SITUATIONAL AWARENESS: The Decade Ahead](https://situational-awareness.ai/) (2024) *TO READ*

---

## Biology and Evolution
Neural networks are loosely (loosely) based on the brain. What about algorithms that are more closely linked with brain function?

- [A Survey on Brain-Inspired Deep Learning via Predictive Coding](https://arxiv.org/pdf/2308.07870) (2025) *TO READ*

- [Networks of Spiking Neurons: The Third Generation of Neural Network Models](https://www.sciencedirect.com/science/article/pii/S0893608097000117) (1996) *TO READ*

- [Continuous Thought Machines](https://arxiv.org/pdf/2505.05522) (2025) *TO READ*

---

## Interpretability
Wait what is the program I made doing?

- [Stochastic Neighbor Embedding](https://proceedings.neurips.cc/paper_files/paper/2002/file/6150ccc6069bea6b5716254057a194ef-Paper.pdf) (2002) *TO READ*

- [A Survey on Sparse Autoencoders: Interpreting the Internal Mechanisms of Large Language Models](https://arxiv.org/abs/2503.05613) (2025) *TO READ*

---

## Other resources
Research papers are great, but there are many ways to get information on the concepts and pace of the field. 

- [Epoch AI](https://epoch.ai/blog/trends-in-ai-supercomputers)

*TO WRITE*

- ✔️ [Transformer Math 101](https://blog.eleuther.ai/transformer-math/)

This blog post is great for planning your next decoder-only transformer!

- ✔️ [Spinning Up on Deep RL](https://spinningup.openai.com/en/latest/)

This blog post is great for getting a quick understanding of algorithms like PO and PPO.

- [On the Biology of a Large Language Model](https://transformer-circuits.pub/2025/attribution-graphs/biology.html)

*TO READ*

