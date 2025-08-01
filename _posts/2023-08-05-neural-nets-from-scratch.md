---
layout: post
title:  "Neural Networks from Scratch"
date:   2023-08-05 19:58:33 -0600
categories: [Machine Learning]
tags: [Neural Networks, From Scratch]
math: true
image:
  path: assets/img/soreal.jpg
---

| [GitHub Repo 👾](https://github.com/JackHanke/nets) | **Scope:** ⭐⭐⭐ |

## Wow Machine Learning is Cool

Even before I studied the subject, I found machine learning fascinating. To me it was some confusing, amorphous new technique blazing past traditional programming. I remember my feeling of true amazement hearing the results of a chess match between two computer programs, [AlphaZero vs Stockfish](https://www.chess.com/news/view/updated-alphazero-crushes-stockfish-in-new-1-000-game-match). Stockfish was the long-time champion, being coded with the help of Grandmaster chess players. AlphaZero was the challenger, only playing chess *against itself* for four hours. AlphaZero's performance improved over those four hours using techniques from *deep reinforcement learning*, a topic so dense I thought I had no hope of ever understanding it. 

AlphaZero's crushing victory marked a similar paradigm shift in the world of chess to the [Kasparov vs DeepBlue](https://en.wikipedia.org/wiki/Deep_Blue_versus_Garry_Kasparov) match some two decades prior. The prior shift was that now software could beat the "learned software" of the human brain in a complicated competitive domain. With the AlphaZero vs Stockfish match, we saw our ability to write software being surpassed. Our understanding of what even makes a good chess move was now second to what the machine "thought". And even worse, it surpassed the collective understanding of our hundreds of years of experience in *four hours*. I knew I had to learn how these programs worked. But how?

## Machine Learning's Pedagogy Problem

I was lucky enough to accidentally study all the prerequisites for machine learning before I even started. The math was not a haze on my understanding, but instead my strength. In fact, I was happy to dive into the computation! But when I finally felt I was able to study the subject, I faced a major disappointment. I took multiple courses, and yet none would explain how the learning was actually happening!

Each time I would work through a Jupyter notebook, I would inevitably get to a line that looked like this:

{% highlight python %}
    ...
    model.train()
    ...
{% endhighlight %}

I would almost scream "What is `.train()` doing!?" The thing I wanted from the course most was reduced to a single line. Worst of all, my instructors often downplayed the details as "laborious" and "uninteresting applications of the chain rule". Well they weren't uninteresting to me!

To clarify, I understand that hiding complexity is what programming is all about. It is necessary for ease of use, and I now use frameworks like `sklearn` and `PyTorch` all the time. But at the time my intellectual curiosity felt like it was slamming into a brick wall. So I decided I would have to go out on my own. I was going to do it from scratch.

## Neural Networks from Scratch

My interest in deep learning led me to the *excellent* textbook [Neural Networks and Deep Learning](http://neuralnetworksanddeeplearning.com/) by Michael Nielsen. I follow the notation laid out in this text, namely that a feed forward neural network is defined for layers $2 \leq \ell \leq L$ as 

$$z^{\ell} = w^{\ell}a^{\ell-1} + b^{\ell}$$

$$a^{\ell} = \sigma^{\ell}(z^{\ell})$$

for activation functions $\sigma^{\ell}$, weights $w^{\ell}$ and biases $b^{\ell}$. Note that the weights and biases are almost always matrices.

>  NOTE: Superscripts are layer indexes and not exponentiation, unless otherwise stated. 

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\lablnode}[3]{\node[shape=circle,draw=white,fill=white, inner sep=0pt,minimum size=2pt] (A) at ( #1 , #2 ) {#3};}
    % actual image
    \begin{tikzpicture}
        % Define the nodes (neurons)
        \node (I1) [circle, draw, minimum size=0.6cm] {};
        \node (I2) [circle, draw, below of=I1, minimum size=0.6cm] {};
        \node (I3) [circle, draw, below of=I2, minimum size=0.6cm] {};

        \node (H1) [circle, draw, right of=I1, xshift=2cm, yshift=0.5cm, minimum size=0.6cm] {};
        \node (H2) [circle, draw, below of=H1, minimum size=0.6cm] {};
        \node (H3) [circle, draw, below of=H2, minimum size=0.6cm] {};
        \node (H4) [circle, draw, below of=H3, minimum size=0.6cm] {};

        \node (O1) [circle, draw, right of=H1, xshift=2cm, yshift=-1cm, minimum size=0.6cm] {};
        \node (O2) [circle, draw, below of=O1, minimum size=0.6cm] {};

        % Connect the layers
        \foreach \i in {1,2,3} {
            \foreach \j in {1,2,3,4} {
                \draw[->] (I\i) -- (H\j);
            }
        }

        \foreach \i in {1,2,3,4} {
            \foreach \j in {1,2} {
                \draw[->] (H\i) -- (O\j);
            }
        }

        \draw[->, color=red] (H4) -- (O2);

        % weight label
        \( \lablnode{4.5}{-2.5}{$w^{3}_{2 , 4}$} \)
    \end{tikzpicture}
    </script>
</div>

Above is a replication of a diagram from Nielsen's textbook detailing how his notation describes a "ball and stick" diagram of a neural network. The red "stick" denotes the weight $w^3_{2,4}$. This is the notation I use throughout the project and this post. 

Even this textbook lists the back propagation chapter as optional reading, but it was beautifully written all the same. After reading through it in its entirety, intentionally avoiding the provided implementations, I returned to the back propagation chapter. Nielsen defines the algorithm as follows. Define the error $\delta_j^{\ell}$ of neuron $j$ at layer $\ell$ be 

$$\delta_j^{\ell} = \frac{\partial C}{\partial z_j^\ell}$$ 

Then back propagating the error can be conducted using the following equations

$$\begin{eqnarray*}
\delta^L & = & \nabla_a C \cdot \sigma'^{L}(z^L) \\
\delta^{\ell} & = & ((w^{\ell+1})^{T}\delta^{\ell+1}) \cdot \sigma'^{\ell}(z^{\ell}) \\
\frac{\partial C}{\partial b_j^\ell} & = & \delta_j^{\ell} \\
\frac{\partial C}{\partial w_{jk}^\ell} & = & a_k^{\ell-1}\delta_j^{\ell}
\end{eqnarray*}$$

<!-- 
$$\delta^L = \nabla_a C \cdot \sigma'^{L}(z^L)$$

$$\delta^{\ell} = ((w^{\ell+1})^{T}\delta^{\ell+1}) \cdot \sigma'^{\ell}(z^{\ell})$$

$$\frac{\partial C}{\partial b_j^\ell} = \delta_j^{\ell}$$

$$\frac{\partial C}{\partial w_{jk}^\ell} = a_k^{\ell-1}\delta_j^{\ell}$$ -->

For a while I struggled with the intuition as to why the error would be defined as a gradient. Though Nielsen describes this well, it just seemed too perfect, and consequently took a long time to "sit right" with me.

But after doing my own derivations for a small network, I felt as if I really could write a neural network from scratch. And that is what I did. After a series of stressful nights, I did complete the project. I encountered a series of difficult bugs, including after my network was running but failing to learn. The bug that took longest to find was an erroneous summation of the weight gradients in a batch, as opposed to an average. But after I weeded all these issues out, I'm happy to say I completed the project. Here is the backward pass of my network written out:

```python
def _backward(self, activation, label, N=None, epsilon=None):
    # forward pass
    activation, weighted_inputs, activations = self._forward(activation, include=True)
    # compute cost of forward pass for verbose output
    cost = self.loss.cost(activation, label)
    # backward pass, starting with final layer
    delta = np.multiply(self.loss.loss_prime(activation, label, epsilon=epsilon), self.activation_funcs[-1].function_prime(weighted_inputs[-1]))
    #remaining layers
    for layer_index in range(self.num_layers, 1, -1):
        # compute product before weights change
        product = np.dot(self.weights[layer_index].transpose(), delta)
        m = activations[layer_index-1].shape[1] # batch_size
        weight_gradient = (np.dot(delta, activations[layer_index-1].transpose()))*(1/m)
        bias_gradient = (delta).mean(axis=1, keepdims=True)
        # add computed gradients
        self.weights_gradients[layer_index] = weight_gradient
        self.biases_gradients[layer_index] = bias_gradient
        # computes (layer_index - 1) delta vector
        # NOTE this computes first layer delta if the ann is pipelines from another model
        delta = np.multiply(product, self.activation_funcs[layer_index-1].function_prime(weighted_inputs[layer_index-1]))
    return cost, delta
```

Below is an interactive demo for MNIST using a network written and trained entirely from scratch. 

**TODO** demo, [credit](https://www.nathom.dev/blog/mnist/)

This level of understanding not only grounded my understanding of the topic, but it also further motivated my study of deep learning.

Thanks for reading!

## Sources

**TODO**
