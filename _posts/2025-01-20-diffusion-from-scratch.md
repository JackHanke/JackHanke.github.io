---
layout: post
title:  "Diffusion from Scratch"
date:   2025-01-20 19:58:31 -0600
categories: [Machine Learning]
tags: [Neural Networks, Diffusion, From Scratch]
math: true
image:
  path: assets/img/imagegen.gif
---

| [GitHub Repo](https://github.com/JackHanke/nets) 👾 | **Scope:** ⭐⭐⭐ | 🚧 Under Construction 🚧 |

## Wait... computers can draw now?

Diffusion models, popularized in the famous [Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2006.11239) (DDPM) paper, are the current state of the art for image generation. Implementing one from scratch proved difficult, especially for my limited "from scratch" toolkit. At the time, I had a neural network, a VAE, and the Adam optimizer. This turned out to just barely be enough.

## What is diffusion doing?

Denoising diffusion models work by learning to reverse a gaussian noising process to a source image $x_0$ over many images. This noising process $q$ involves progressively adding Gaussian noise for $T$ steps to $x_0$, with $0$ mean and a *variance schedule* $\beta_1, \dots, \beta_T$. Through some math tricks with Gaussian distributions, we can instead sample the distribution of the noising process at an arbitrary point $t$ without calculating the previous $t-1$ noising steps. This looks like, for $\bar{\alpha} = \prod_{s=1}^t (1-\beta_t)$,

$$q(x_t|x_0) = \cal{N}(\mathbf{x}_t;\sqrt{\bar{\alpha_t}}\mathbf{x}_0, (1-\bar{\alpha_t})\mathbf{I}).$$

Diffusion models optimize the **TODO**

$$\mathbb{E}\left[-\log(p_\theta(\mathbf{x}_0))\right]$$

**TODO**

During training, the denoising network $\epsilon_\theta$ is then learning a vector field in data space. Then while sampling, we move our random seed in data space to higher density regions in such a way that we don't just converge to the original datapoints, which are the most likely points in data space.

## My approach

I was limited by the architecture of $\epsilon_\theta$, as I couldn't use convolution layers (or skip connections for UNET-like architectures), as well as no GPU-assisted training. In hindsight, I went into this project underprepared, but I did finally accomplish what I wanted.

I chose to train on the [EMNIST](https://www.nist.gov/itl/products-and-services/emnist-dataset) dataset so that I could generate sentences. I initially tried to train the denoiser using the vanilla feed-forward neural network class I had written before on the raw pixels, as described in the paper. This proved to be unfeasible, as the network struggled to learn anything meaningful. This was really disappointing, but the poor results did show me how important residual connections and convolution layers are to learning for denoising spatial data.

At the time, however, I decided that my best option was to encode all datapoints using my previously-implemented VAE. This reduced the data dimensionality and organized data space better. This is a trick lifted from modern image generators like Stable Diffusion. This means that I technically implemented a LDDPM as opposed to a vanilla DDPM model. This fix allowed for much better conditional image generation to be learned just on my CPU. 

## Making sentences with my letters

After training was complete, my model could draw letters. But I wanted it to write sentences!

I mocked up some code to structure a list of letters into short sentences, shown above. I also added a debug feature that showed which of the $t<T=50$ generation steps the animation was on, then held the final generated image for another $20$ time steps.

As you can see, the blurring artifacts from the VAE decoder are present in the final animation, and some of the generation lacks variety (such as the letter 'I'). Regardless, I am very proud of the final result. I had a lot of fun sending messages to my friends with the final trained network. 

Thanks for reading!

## Sources

- [Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2006.11239)
