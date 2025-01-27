---
layout: post
title:  "Neural Networks from Scratch"
date:   2025-01-23 19:58:33 -0600
categories: jekyll update
---

[NN GitHub Repo 👾](https://github.com/JackHanke/nets) | [RL GitHub Repo 👾](https://github.com/JackHanke/2048rl)

## Wow Machine Learning is Cool

Even before I studied the subject, I found machine learning fascinating. To me it was some confusing, amorphous new technique blazing past traditional programming. I remember my feeling of true amazement hearing the results of a chess match between two computer programs, [AlphaZero vs Stockfish](https://www.chess.com/news/view/updated-alphazero-crushes-stockfish-in-new-1-000-game-match). Stockfish was the long-time champion, being coded by Grandmaster chess players and expert programmers. AlphaZero was the challenger, only playing chess *against itself* for four hours. AlphaZero's performance improved using techniques from *deep reinforcement learning*, a topic so dense I thought I had no hope of ever understanding it. 

AlphaZero's crushing victory marked a similar paradigm shift in the world of chess to the [Kasparov vs DeepBlue](https://en.wikipedia.org/wiki/Deep_Blue_versus_Garry_Kasparov) match some two decades prior. The prior shift was that now software could beat the "learned software" of the human brain in a complicated competetive domain. With the AlphaZero vs Stockfish match, we saw our ability to write software being surpassed. Our understanding of what even makes a good chess move was now second to what the machine "thought". And even worse, it surpassed the collective understanding of our hundreds of years of experience in *four hours*. I knew I had to learn how these programs worked. But how?

## Machine Learning's Pedagogy Problem

I was lucky enough to accidentally study all the prerequisites for machine learning before I even started. The math was not a haze on my understanding, but instead my strength. In fact, I was happy to dive into the computation! But when I finally felt I was able to study the subject, I faced a **major** disappointment. I took multiple courses, and yet none would explain how the learning was actually happening!

Each time I would work through a Jupyter notebook, I would inevitabely get to a line that looked like this:

{% highlight python %}
    ...
    model.train()
    ...
{% endhighlight %}

I would almost scream "What is `.train()` doing!?" The thing I wanted from the course most was reduced to a single line. Worst of all, my instructors often downplayed the details as "laborious" and "uninteresting applications of the chain rule". Well they weren't uninteresting to me!

To clarify, I understand that hiding complexity is what programming is all about. It is necessary for ease of use, and I use frameworks like `sklearn` and `PyTorch` all the time. But my intellectual curiosity felt like it was slamming into a brick wall. So I decided I would have to go out on my own. I was going to do it from scratch.

## Neural Networks from Scratch

My interest in deep learning led me to the *excellent* textbook [Neural Networks and Deep Learning](http://neuralnetworksanddeeplearning.com/) by Michael Nielsen. I follow the notation laid out in this text, namely that a feed forward neural network is defined for layers $2 \leq \ell \leq L$ as 

$$z^{\ell} = w^{\ell}a^{\ell-1} + b^{\ell}$$

$$a^{\ell} = \sigma^{\ell}(z^{\ell})$$

for activation functions $\sigma^{\ell}$, weights $w^{\ell}$ and biases $b^{\ell}$. Note that the weights and biases are almost always matrices.

>  NOTE: Superscripts are not exponentiation unless stated. 

TODO add diagram

Even this textbook lists the backpropagation chapter as optional reading, but the chapter was beautifully written all the same. After reading through it in its entirety, intentionally avoiding the provided implementations, I returned to the backpropagation chapter. Nielsen defines the algorithm as follows. Define the error $\delta_j^{\ell}$ of neuron $j$ at layer $\ell$ be 

$$\delta_j^{\ell} = \frac{\partial C}{\partial z_j^\ell}$$ 

Then backpropagating the error can be conducted using the following equations

$$\delta^L = \nabla_a C \cdot \sigma'^{L}(z^L)$$

$$\delta^{\ell} = ((w^{\ell+1})^{T}\delta^{\ell+1}) \cdot \sigma'^{\ell}(z^{\ell})$$

$$\frac{\partial C}{\partial b_j^\ell} = \delta_j^{\ell}$$

$$\frac{\partial C}{\partial w_{jk}^\ell} = a_k^{\ell-1}\delta_j^{\ell}$$

For a while I struggled with the intuition as to why the error would be defined as a gradient. Though Nielsen describes this well, it just seemed too perfect, and consequently took a long time to "sit right" with me.

But after doing my own derivations for a small network, I felt as if I really could write a neural network from scratch. And that is what I did. After a series of stressful nights, I did complete the project. I encountered a series of difficult bugs, including after my network was running but failing to learn. The bug that took longest to find was an erroneous summation of the weight gradients in a batch, as opposed to an average. But after I weeded all these issues out, I'm happy to say I completed the project. Below is an interactive demo for MNIST using a network written and trained entirely from scratch. 

TODO demo, credit person that wrote canvas

This level of understanding not only grounded my understanding of the topic, but it also further motivated my study of deep learning. Now that I had a neural network from scratch, what else could I do?

## (Variational) AutoEncoders from Scratch

One of the first things one can do with a neural network is to create an [autoencoder](https://en.wikipedia.org/wiki/Autoencoder). An autoencoder is a neural network that is trained to predict its input, but is structured so that the network "funnels" information through a smaller space before being expanded out. This smaller space is often called the *latent space*, and the task of predicting input is often called *reconstruction*.

The portion of the network that maps the input to the latent space is called the *encoder*, and the part that maps the latent space to the prediction is the *decoder*. 

TODO add diagram of AE

If we denote $x$ as the input and $x'$ as the predicted input, an autoencoder might use the sum of squared errors loss function to train on, like so.

$$\mathcal{L} = \frac{1}{2}\sum_{i}(x_i - x_i')^2$$

This loss solely prioritizes the task of reconstruction for the network, so the latent space is often unorganized. This means that points in the latent space that aren't derived directly from data often decode into a blurry mess. 

This is bad if one wants not just to compress the data, but generate plausible samples from the original data distribution. It turns out there is a way to train a network that also prioritizes latent space organization! These networks are called [variational autoencoders](https://en.wikipedia.org/wiki/Variational_autoencoder), or VAEs for short. After I watched [this excellent YouTube](https://www.youtube.com/watch?v=qJeaCHQ1k2w&t=826s) video by Deepia, VAEs became the next network architecture I chose to implement from scratch. 

The loss function for VAEs has two components, the usual reconstruction loss, written $$\mathcal{L}_{rec}$$, and the *regularization loss*, written $\mathcal{L}_{reg}$. The regularization loss is a mathematical expression for how far away the distribution created by the encoder $P$ differs from some given distribution $Q$. VAEs choose this $Q$ to be a standard normal distribution of some latent dimension. 

More specifically, the regularization loss is the [Kullback-Leibler divergence](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence) between the created normal distribution to the standard normal. KL divergence is usually complicated to compute, but the divergence between two gaussians can be symbollically written. This gives the full loss function for VAEs.

$$\mathcal{L} = \mathcal{L}_{rec} + \mathcal{L}_{reg} = \frac{1}{2}\sum_{i}(x_i - x_i')^2 - \frac{1}{2}\sum_{i}(1+2\log(\sigma_i)-\mu_i^2-\sigma_i^2).$$

Because I have yet to implement an automatic differentiation engine, I had to compute the gradients for a VAE by hand. I also had to come up with a clever way to write a VAE given how I chose to implement the neural network class. This resulted

For VAE's that consist of encoder $E$ and decoder $D$, we have the following loss function

$$\mathcal{L}_{reg} = \frac{1}{2}\exp(2\log\sigma) + \frac{1}{2}\mu^2 - \log\sigma - \frac{1}{2}$$


If we let $z$ be the input to

$$z = \mu + \epsilon \exp(\log \sigma)$$


$\frac{\partial \mathcal{L}}{\partial z}$ is computable after a VAE forward pass and a $D$ backprop. 

Let 

$$\frac{\partial \mathcal{L}_{rec}}{\partial \mu} = \frac{\partial \mathcal{L}_{rec}}{\partial z}\frac{\partial z}{\partial \mu} = \frac{\partial \mathcal{L}_{rec}}{\partial z}$$

$$\frac{\partial \mathcal{L}_{rec}}{\partial \log{\sigma}} = \frac{\partial \mathcal{L}_{rec}}{\partial z}\frac{\partial z}{\partial \log{\sigma}} = \frac{\partial \mathcal{L}_{rec}}{\partial z} \epsilon \exp(\log\sigma)$$

$$\frac{\partial \mathcal{L}_{reg}}{\partial \mu} = \mu$$

$$\frac{\partial \mathcal{L}_{reg}}{\partial \log{\sigma}} = \exp(2\log \sigma) - \vec{1}$$


The below animation shows how traversal between the latent space of an AE compares to a VAE. This animation transitions between a latent representation for a `2` and a `7`.

{:refdef: style="text-align: center;"}
![]({{ site.baseimg }}/assets/extrap-anim.gif)
{: refdef}

Notice that the AE only prioritizes reconstruction, and so has a much sharper image than the VAE. However, the VAE's transition between the two is smoother, and more often looks like some type of digit. 

As VAEs served as the beginning of the generative AI boom, I set my sights next on diffusion models, the current state of the art for image generation. 

## Diffusion from Scratch

Diffusion models, popularized in the famous paper [Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2006.11239), are the current state of the art for image generation. 

Implementing one from scratch proved difficult. There were a series of reasons for this, including no CNN, no UNET, no GPU, TODO

TODO

I chose to train my network on the EMNIST dataset (as opposed to the MNIST dataset) for more striking final generation. This allows for "sentence generation", like below. 

{:refdef: style="text-align: center;"}
![]({{ site.baseimg }}/assets/imagegen.gif)
{: refdef}

I had a lot of fun sending messages to my friends with the trained network. 

TODO There was a fun bit of implementation as to how I would structure the letters to create the best-looking `.gif`.

## Reinforcement Learning from Scratch

I have been playing [2048](https://www.2048.org/) since its release in 2014. I always knew it was a perfect choice for a beginner reinforcement learning project, as it has a large state space, small action space, and a clearly defined reward function. The game has non-trivial strategy and is [entirely open source](https://github.com/gabrielecirulli/2048). It's a great game, and an even better game to learn RL with. 

I decided I would make the bot after I completed the [Sutton and Barto](http://www.incompleteideas.net/book/the-book.html) textbook. It was an intense summer read, but when I completed it I got to work. The first challenge the project posed was the game itself. I initially used a pre-built Python implementation called [Macht](https://github.com/rolfmorel/macht), but later chose to implement the game from scratch for speed of training. 

As for the learning algorithm, I initially wanted to do policy optimization, as opposed to TD or Q learning. I really liked the idea of having a single neural network that just takes in a board state and plays a good move. However, the performance was really poor, and I abandoned the technique quickly. I decided to see if other people had written 2048 RL bots to see what learning algorithm I should pursue. 

I was surprised to find that not only have people written 2048 bots, but there is serious academic research on the subject. I looked at a series of papers, the most important being [Temporal Difference Learning of N-Tuple Networks for the Game 2048](https://www.cs.put.poznan.pl/wjaskowski/pub/papers/Szubert2014_2048.pdf) by Szubert and colleagues. They showed that TD learning with a [RAMnet](https://en.wikipedia.org/wiki/RAMnets) approximator worked well, and so I chose to pursue this. 

There was a gruelling episode of debugging my algorithm, so finally seeing the algorithm finally learning was one of the most satisfying moments of my programming journey. After training for 10,000 games, the agent achieves the 2048 tile 49.1% of the time, and achieves 4196 tile 1.5% of the time.

Now that the bot was trained, the final step was hooking up the history of the game to play on the real game's visualizer. I cloned the original repo and changed the internal Javascript to play a JSON history to make the final animation. I showcased one of my agent's bests games in the following animation.

{:refdef: style="text-align: center;"}
![]({{ site.baseimg }}/assets/2048viz.gif)
{: refdef}

## Citations

TODO
