---
layout: post
title:  "Multi-Head Attention from Scratch"
date:   2025-06-28 19:58:33 -0600
categories: [Machine Learning]
tags: [Neural Networks, Attention, Transformers, From Scratch]
math: true
image:
  path: /assets/img/oopsallfromscratch.jpg
---

| [GitHub Repo](https://github.com/JackHanke/nets) 👾 | **Scope:** ⭐⭐⭐ |

For my final "from scratch, `NumPy`-only" neural network architecture, I decided to tackle *multi-head attention*. Multi-head attention, originally introduced in 2017 in the famous [Attention Is All You Need](https://arxiv.org/abs/1706.03762) paper, now runs the world. It is the core component in any state-of-the-art architecture, and is essential to have in your machine learning toolkit. The forward pass is:

$$\begin{eqnarray*}
\text{Multihead}(Q,K,V) & = & \text{Concat}(\text{head}_1, \dots, \text{head}_h)W^O \\
\text{head}_i & = & \text{Attention}(QW_i^Q, KW_i^K, VW_i^V) \\
\text{Attention}(Q, K, V) & = & \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V \\
\end{eqnarray*}$$

where $Q,K,V$ are the input matrices and $W_i^Q, W_i^K, W_i^V, W^O$ are the weight matrices. 

In contrast to my other from scratch projects, I decided to just check my work with the established [PyTorch](https://docs.pytorch.org/docs/stable/generated/torch.nn.MultiheadAttention.html) module. 

## The Forward Pass

The forward pass is simple enough with NumPy operations. I decided, somewhat unprofessionally, to not vectorize by head, and instead throw the arrays in lists `q_bar_list`, `k_bar_list`, `v_bar_list`.

```python
def _scaled_dot_attention(
        self,
        activation_q: np.array,
        activation_k: np.array,
        activation_v: np.array,
        q_bar_list: list,
        k_bar_list: list,
        v_bar_list: list,
        t_list: list,
        s_list: list,
        i: int,
        include: bool,
    ):
    # bar functions
    q_bar = np.matmul(activation_q, self.query_weights_list[i])
    k_bar = np.matmul(activation_k, self.key_weights_list[i])
    v_bar = np.matmul(activation_v, self.value_weights_list[i])
    if include:
        q_bar_list.append(q_bar)
        k_bar_list.append(k_bar)
        v_bar_list.append(v_bar)

    # intermediate t value
    k_t_bar = np.transpose(k_bar, axes=(0,2,1))
    t = np.matmul(q_bar, k_t_bar)
    if include:
        t_list.append(t)
    
    # softmaxed matrix s
    s = self.softmax.function(x=t/np.sqrt(self.d_k))

    if include:
        s_list.append(s)

    # attention value head
    head = np.matmul(s, v_bar)
    return head
```

I found the hardest part of this was properly vectorizing the row-wise softmax, which is below:

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

With these implemented, I checked that a forward pass from the Pytorch's `MultiheadAttention` class (with the same initialization) matched. Once they did, I moved on to the backward pass.

## The Backward Pass

Like with my "VAE from scratch" project, I needed to compute the gradients for the backward pass manually. As multi-head attention has a lot of intertwined matrix operations, this was an involved process. I first defined the following intermediate identities:

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

We also use the multivariate chain rule to get the following useful identity.

**Identity 1.** If $\cal{L}$ is a scalar valued function that involves summing over entries of some $n \times m$ matrix $A$, and $A=BC$, then

$$\frac{\partial \cal{L}}{\partial B} = \frac{\partial \cal{L}}{\partial A} C^T$$

and 

$$\frac{\partial \cal{L}}{\partial C} = B^T \frac{\partial \cal{L}}{\partial A}.$$

With this identity and the above intermediate quantities, I was able to manually [calculate](https://github.com/JackHanke/nets/blob/main/models/attention/grad.md) the gradients for the backward pass. The results are summarized below:

$$\begin{eqnarray*}
\frac{\partial \cal{L}}{\partial W^O} & = & C^T (M - \hat{M}) \\
\frac{\partial \cal{L}}{\partial W^V_i} & = & V^T S^T_i A [d_k (i-1): d_{k} i] \\
\frac{\partial \cal{L}}{\partial W^Q_i} & = & Q^T A [d_k (i-1): d_{k} i] \bar{V}^T_i B_i \bar{K}_i \\
\frac{\partial \cal{L}}{\partial W^K_i} & = & K^T \left(\bar{V}^T_i A [d_k (i-1): d_{k} i] \bar{V}^T_i B_i\right)^T \\
\end{eqnarray*}$$

Here I use the [Python slicing](https://docs.python.org/3.9/reference/expressions.html) notation of square brackets and colons for array indexing. Implemented in Python, the backward pass looks like:

```python
# backward pass for multi head attention module
def _backward(self, 
        activation_q: np.array, 
        activation_k: np.array, 
        activation_v: np.array, 
        label: np.array,
    ):
    
    # forward pass for activations
    multi_head_attention_vals, q_bar_list, k_bar_list, v_bar_list, t_list, s_list, C = self._forward(
        activation_q = activation_q, 
        activation_k = activation_k, 
        activation_v = activation_v, 
        include=True
    )
    
    ## compute gradients 
    grad_output_weights = np.matmul(np.transpose(C, axes=(0,2,1)), (multi_head_attention_vals-label))

    mean_grad_weights_k_list = []
    mean_grad_weights_v_list = []
    mean_grad_weights_q_list = []

    # define intermediate matrix A 
    A = np.matmul((multi_head_attention_vals-label), np.transpose(self.output_weights))

    # for each head i
    for i in range(self.h):
        # define intermediate matrix B and slice of A
        B = self.softmax.function_prime(t_list[i]/np.sqrt(self.d_k))/np.sqrt(self.d_k)
        A_slice = A[:,:,self.d_k*i:self.d_k*(i+1)]

        ## NOTE had to do this bc only two args at a time

        # compute gradients for v
        grad_weights_v = np.transpose(activation_v, axes=(0,2,1))
        for term in [
                np.transpose(s_list[i], axes=(0,2,1)),
                A_slice,
            ]:
            grad_weights_v = np.matmul(grad_weights_v, term)
            
        # compute gradients for q
        grad_weights_q = np.transpose(activation_q, axes=(0,2,1))
        for term in [
                A_slice,
                np.transpose(v_bar_list[i], axes=(0,2,1)),
                B,
                k_bar_list[i],
            ]:
            grad_weights_q = np.matmul(grad_weights_q, term)

        # compute gradients for k
        temp_prod = np.transpose(v_bar_list[i], axes=(0,2,1))
        for term in [
                A_slice,
                np.transpose(v_bar_list[i], axes=(0,2,1)),
                B,
            ]:
            temp_prod = np.matmul(temp_prod, term)
        grad_weights_k = np.matmul(np.transpose(activation_k, axes=(0,2,1)), np.transpose(temp_prod, axes=(0,2,1)))

        # dimension checks
        assert grad_weights_v.shape == (activation_q.shape[0], self.d_model, self.d_k)
        assert grad_weights_q.shape == (activation_q.shape[0], self.d_model, self.d_k)
        assert grad_weights_k.shape == (activation_q.shape[0], self.d_model, self.d_k)
        
        # mean grads over batch
        mean_grad_weights_v = np.mean(grad_weights_v, axis=0)
        mean_grad_weights_q = np.mean(grad_weights_q, axis=0)
        mean_grad_weights_k = np.mean(grad_weights_k, axis=0)

        # append to grads list
        mean_grad_weights_v_list.append(mean_grad_weights_v)
        mean_grad_weights_q_list.append(mean_grad_weights_q)
        mean_grad_weights_k_list.append(mean_grad_weights_k)

    # mean output weights grads 
    mean_grad_output_weights = np.mean(grad_output_weights, axis=0)
    assert mean_grad_output_weights.shape == (self.d_model, self.d_model)

    return mean_grad_output_weights, mean_grad_weights_q_list, mean_grad_weights_k_list, mean_grad_weights_v_list
```

## The Sanity Check

To complete the project, I needed to verify the backward passes matched. I defined an MSE loss $\cal{L}$ between some randomly generated "label" attention pattern. After verifying the forward passes matched again, I then would compute the loss for a batch of $2$, then perform a single update of stochastic gradient descent to $W_i^V$, $W_i^K$, $W_i^V$, and $W^O$. I then compare that the final weights from both modules match (within floating point tolerance). 

After some debugging, the tests passed! 

## The Finale of the "From Scratch" Journey

With the last test passing, I completed my "from scratch" journey. I learned a lot about programming and machine learning. I understand the core machine learning architectures so much better now that I've seen every part of them. I think I'm ready to do some "real" projects now...

Thanks for reading!
