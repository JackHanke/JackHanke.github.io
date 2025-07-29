---
layout: post
title:  "[MLCS] Multi Head Attention from Scratch"
date:   2025-06-28 19:58:33 -0600
categories: jekyll update
---

| [GitHub Repo 👾](https://github.com/JackHanke/nets) | **Scope:** ⭐⭐⭐ |

Multi head attention runs the world. It is the core component in any state-of-the-art architecture, and yet is surprisingly simple for how successful it is:

$$\begin{eqnarray*}
\text{Multihead}(Q,K,V) & = & \text{Concat}(\text{head}_1, \dots, \text{head}_h)W^O \\
\text{head}_i & = & \text{Attention}(QW_i^Q, KW_i^K, VW_i^V) \\
\text{Attention}(Q, K, V) & = & \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V \\
\end{eqnarray*}$$

For my final "from scratch" project, I decided to implement a multi head attention module just in `NumPy`.

In contrast to my other from scratch projects, I decided to just check my module with an established module, namely from [PyTorch](https://docs.pytorch.org/docs/stable/generated/torch.nn.MultiheadAttention.html). 

## The forward pass

This is simple enough with NumPy operations. I found the hardest part was properly vectorizing the row-wise softmax, the implementation is below:

```python
class SoftMax:
    def __init__(self):
        self.name = 'softmax'

    # expects x to be of dimension (batch, row, axis to be summed over)
    def function(self, x: np.array):
        # numerical stability
        m = np.max(x, axis=2, keepdims=True)

        numerator = np.exp(x - m)
        denominator = np.sum(numerator, axis=2, keepdims=True)
        # numerator
        val = numerator / denominator
        return val

    def function_prime(self, x: np.array):
        val = self.function(x) * (1 - self.function(x))
        return val
```

## The backward pass

Like with my "VAE from scratch" project, I needed to compute the gradients for the backward pass manually. As multi head attention has a lot of intertwined matrix operations, this was an involved process. I first defined the following intermediate identities:

$$\begin{eqnarray*}
M & = & \text{Multihead}(Q,K,V) = CW^O \\
head_i & = & S_i \bar{V}_i \\
T_i & = & \bar{Q}_i \bar{K}_i^T \\
S_i & = & \text{softmax}\left(\frac{T_i}{\sqrt{d_k}}\right) \\
\bar{Q}_i & = & QW_i^Q  \\
\bar{K}_i & = & KW_i^K \\
\bar{V}_i & = & VW_i^V \\
A & = & \frac{\partial \cal{L}}{\partial C} = (M - \hat{M}) (W^O)^T \\
B_i & = & \frac{\partial S_i}{\partial T_i} = \frac{1}{\sqrt{d_k}}\text{softmax}'\left(\frac{T_i}{\sqrt{d_k}} \right) \\
\end{eqnarray*}$$

I also derived the following identity, which is simply a consequence of the multivariate chain rule.

If $\cal{L}$ is a scalar valued function that involves summing over entries of some $n \times m$ matrix $A$, and $A=BC$, then

$$\frac{\partial \cal{L}}{\partial B} = \frac{\partial \cal{L}}{\partial A} C^T$$

and 

$$\frac{\partial \cal{L}}{\partial C} = B^T \frac{\partial \cal{L}}{\partial A}$$

With this identity and the above intermediate quantities, I was able to [calculate](https://github.com/JackHanke/nets/blob/main/models/attention/grad.md) the following relations for the backward pass. I use square brackets and colons in the style of [Python slicing](https://docs.python.org/3.9/reference/expressions.html).

$$\begin{eqnarray*}
\frac{\partial \cal{L}}{\partial W^O} & = & C^T (M - \hat{M}) \\
\frac{\partial \cal{L}}{\partial W^V_i} & = & V^T S^T_i A [d_k (i-1): d_{k} i] \\
\frac{\partial \cal{L}}{\partial W^Q_i} & = & Q^T A [d_k (i-1): d_{k} i] \bar{V}^T_i B_i \bar{K}_i \\
\frac{\partial \cal{L}}{\partial W^K_i} & = & K^T \left(\bar{V}^T_i A [d_k (i-1): d_{k} i] \bar{V}^T_i B_i\right)^T \\
\end{eqnarray*}$$

## Checking my work

I first verify that passing the same data through my custom module matches the PyTorch module. Though simple in theory, a hiccup is that we need to initialize both modules with the same $W^O, W^V, W^K,$ and $W^V$. This caused quite the headache, as the PyTorch implementation does a number of efficiencies that confound which matrix is which. For example, they combine $W^V, W^K,$ and $W^V$ into a single "in-projection" matrix for parallelism. This is obviously the better way to do it, but I stuck with my implementation. This meant that I needed to translate these two matrices (and transpose them!) to match. 

After the forward passes agreed, I then verified the backward pass as follows. I defined an MSE loss $\cal{L}$ between some random labels in the shape of the final attention pattern. After verifying the forward passes matched, I then would compute the loss for a batch of $2$, then preform a single update of SGD to $W^V, W^K,$ and $W^V$. I then compare that the final weights from both modules match (within floating point tolerance). 

After some debugging, the tests passed! This completed my "from scratch" journey. I now feel I understand all of the core machine learning architectures. 

Thanks for reading!

## Sources

- [Attention Is All You Need](https://arxiv.org/abs/1706.03762)
