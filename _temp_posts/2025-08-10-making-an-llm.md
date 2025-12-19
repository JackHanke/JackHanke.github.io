---
layout: post
title:  "Making an LLM"
date:   2025-08-10 09:58:30 -0600
categories: [Machine Learning]
tags: [Neural Networks, Attention, Transformers, Ground Up]
math: true
image:
  path: /assets/img/bmo_reading.jpg
---

<p style="text-align:center;"><i> Art by <a href="https://www.deviantart.com/ughh-nyet">tapiokurii</a> </i></p>



| [GitHub Repo](https://github.com/danplotkin/qt) 👾 | **Scope:** ⭐⭐⭐⭐⭐ | Collaborator: [Dan Plotkin](https://github.com/danplotkin) | 🚧 Under Construction 🚧 |

Large language models, or LLMs, are undoubtedly the technology of our time. The fact that you can talk to a piece of software like you would a person boggles my mind. These programs are so incredible that I just had to make one. There is only one slight problem: I don't have millions of dollars to spend on this project. The creation of a modern chatbot of the quality of ChatGPT, Gemini, or Claude is therefore completely infeasible for any one person. 

So my friend Dan and I set out to make an LLM, from the ground up, on a hobby budget. But first we would need to create our plan of attack.

## A Transformer Calculator

Suppose you want to make a large language model on a fixed budget. Also suppose that we want model inference to fit on a 16GB GPU, the size of my current graphics card. Finally, assume that the architecture is a decoder-only Transformer [in the style of GPT-2](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf). 

Fortunately, there is a mountain of research on how to make these models as efficiently as possible which we will use to design ours. 

First, some notation. Our budget $B$ puts a constraint on the amount of time we can rent a cloud service provider's GPUs. This time then constrains the number of floating point operations we can do, as a GPU has a maximum number of floating point operations per second (FLOPS) it can compute. 

```python

```

**TODO**

## Architecture



**TODO**

## Data Profile

- For instruction tuning: [Instruction Tuning for Large Language Models: A Survey](https://arxiv.org/abs/2308.10792)

**TODO**

## Pretraining Performance

**TODO**

## Finetuning Performance

**TODO**

## Example Generations

**TODO**

## Sources

**TODO**
