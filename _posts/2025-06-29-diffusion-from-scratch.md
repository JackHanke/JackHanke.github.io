---
layout: post
title:  "[MLCS] Diffusion from Scratch"
date:   2025-06-28 19:58:31 -0600
categories: jekyll update
---

| [GitHub Repo 👾](https://github.com/JackHanke/nets) | **Scope:** ⭐⭐⭐ |

## Wait computers can draw now?

Diffusion models, popularized in the famous paper [Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2006.11239) (DDPM), are the current state of the art for image generation. Implementing one from scratch proved difficult, especially for the limited tools I had at already implemented from scratch. 

## What is diffusion doing?

In the DDPM paper, **TODO**


I had to make a proper diffusion model with no convolution layers (or skip connections for UNET-like architectures) and no GPU-enabled training. In hindsight, I went into this project underprepared, but I did finally accomplish an implementation.

I chose to train my network on the [EMNIST](https://www.nist.gov/itl/products-and-services/emnist-dataset) dataset (as opposed to the "default" MNIST dataset) so that I could generate sentences. I initially tried to train a diffusion model with the base neural network class I had written before on the raw pixels. This proved to be unfeasible. I'm unsure why the performance using a base neural network was so poor, but it did show me that residual connections and convolution layers were critical.

At the time, however, I decided that my best option was to use my previously implemented VAE to reduce the dimensionality of the input, and then conduct diffusion on the latent representation. This means that I technically implemented a LDDPM as opposed to a vanilla DDPM model. This allowed for much better conditional image generation to be learned just on my CPU. 

## Some fun supporting code

I also wrote some code to structure the generations into short sentences, shown below. I added a debug feature that showed which of the $t<50$ generation steps the animation was on, then held the final generated image for another $20$ time steps.

{:refdef: style="text-align: center;"}
![]({{ site.baseimg }}/assets/imagegen.gif)
{: refdef}

As you can see, the blurring artifacts from the VAE decoder are present in the final animation, and some of the generation lacks variety (such as the letter 'I'). Regardless, I am very proud of the final result. I had a lot of fun sending messages to my friends with the final trained network. 

With my neural architectures completed for now, I wanted to focus on the third type of learning -- the algorithms that originally got me into ML -- reinforcement learning. I explore these techniques in a future post!

Thanks for reading!

## Sources

- [Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2006.11239)
