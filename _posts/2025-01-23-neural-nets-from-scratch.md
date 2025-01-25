---
layout: post
title:  "Neural Networks from Scratch"
date:   2025-01-23 19:58:33 -0600
categories: jekyll update
---

[GitHub Repo 👾](https://github.com/JackHanke/nets)

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

To clarify, I understand that hiding complexity is what programming is all about. It is necessary for ease of use, and I use frameworks like `sklearn` and `PyTorch` all the time. But my intellectual curiosity felt like it was slamming into a brick wall. 

So I decided I would have to go out on my own. I was going to do it from scratch.

## Neural Networks from Scratch

My interest in deep learning led me to the *excellent* textbook [Neural Networks and Deep Learning](http://neuralnetworksanddeeplearning.com/) by Michael Nielsen. I follow the notation laid out in this course, namely that a feed forward neural network is defined by, for layers $2 \leq \ell \leq L$, 

$$z^{\ell} = w^{\ell}a^{\ell-1} + b^{\ell}$$

$$a^{\ell} = \sigma^{\ell}(z^{\ell})$$

for activation functions $\sigma^{\ell}$ and weights $w^{\ell}$ and biases $b^{\ell}$. Note that the weights and biases are almost always matrices.

>  NOTE: Superscripts are not exponentiation unless stated. 

TODO add diagram

Even this textbook lists the backpropagation chapter as optional reading, but it was beautifully written all the same. After reading through it in its entirety, intentionally avoiding the provided implementations, I returned to the backpropagation chapter. Nielsen defines the algorithm as follows. Define the error $\delta_j^{\ell}$ of neuron $j$ at layer $\ell$ be 

$$\delta_j^{\ell} = \frac{\partial C}{\partial z_j^\ell}$$ 

Then backpropagating the error can be conducted using the following equations

$$\delta^L = \nabla_a C \cdot \sigma'^{L}(z^L)$$

$$\delta^{\ell} = ((w^{\ell+1})^{T}\delta^{\ell+1}) \cdot \sigma'^{\ell}(z^{\ell})$$

$$\frac{\partial C}{\partial b_j^\ell} = \delta_j^{\ell}$$

$$\frac{\partial C}{\partial w_{jk}^\ell} = a_k^{\ell-1}\delta_j^{\ell}$$

I sturggled with the intuition as to why the error would be defined as a gradient for a while. Though Nielsen describes this well, it just seemed too perfect, and consequently took a long time to "sit right".

But after doing my own derivations for a small network, I felt as if I really could write a neural network from scratch. And that is what I did. After a series of stressful nights, I did complete the project. I encountered a series of difficult bugs, including after my network was running but failing to learn. The bug that took longest to find was an erroneous summation of the weight gradients in a batch, as opposed to the average. But after I weeded all these issues out, I'm happy to say I completed the project. Below is an interactive demo for MNIST using a network written and trained entirely from scratch. 

TODO demo, credit person that wrote canvas

This level of understanding not only grounded my understanding of the topic, but it also further motivated my study of deep learning. Now that I had a neural network from scratch, what else could I do?

## (Variational) AutoEncoders from Scratch

One of the first thing one can do with a neural network is to create an [autoencoder](https://en.wikipedia.org/wiki/Autoencoder). 

TODO

This motivates the study of [variational autoencoders](https://en.wikipedia.org/wiki/Variational_autoencoder).

$$\mathcal{L} = \mathcal{L}_{rec} + \mathcal{L}_{rec} = \frac{1}{2}\sum_{i}(x_i - x_i')^2 - \frac{1}{2}\sum_{i}(1+2\log(\sigma_i)-\mu_i^2-\sigma_i^2).$$

Because I have yet to implement an automatic differentiation engine, I had to compute the gradients for a VAE by hand. 

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
