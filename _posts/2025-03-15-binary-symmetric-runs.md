---
layout: post
title:  "[MATH ⭐] Symmetric Runs in Binary Sequences"
date:   2025-03-15 12:58:32 -0600
categories: jekyll update
---

| [GitHub Repo 👾](https://github.com/JackHanke/binaryruns) | 

## LeetCode for Fun

There is a website used for studying for programming interview questions called [LeetCode](https://leetcode.com/). These problems tend to look like contrived word problems that can usually be solved with a clever algorithm. Here is the starter question called *Two Sum* that has become a meme on it's own.

{% highlight text %}
Given an array of integers nums and an integer target, return indices of 
the two numbers such that they add up to target.
{% endhighlight %}

Why these questions are used to assess industry software engineers is beyond me. All the same, I find myself drawn to these questions. I often think about the mathematics that might surround such problems, and occasionally make up a Leetcode-type problem myself. This post is about one of these problems that turned out to have an interesting solution, the *Symmmetric Runs Problem*.

## Symmetric Runs

The problem I came up with is as follows.

Suppose you have a sequence of $n$ numbers, where each number ranges between $0$ to some number $b-1$. We can write this sequence as $s = (s_1, \dots, s_n)$, with $s_i \in [0,b-1]$. An example for $n=7, b=3$ would be 

$$(0,0,2,1,2,0,1).$$

For a given sequence, perform the following process. Find the largest $i$ so that $(s_1, s_2, \dots, s_i)$ is symmetric, meaning $s_1 = s_i$, $s_2 = s_{i-1}$, and so on. Let us call this subsequence a *symmetric subsequence*. Continue this process by finding the largest $j$ so that $(s_{i+1}, s_{i+2}, \dots, s_{j})$ is symmetric. Repeat until $s_n$ is part of a symmetric subsequence. We are interested in the number of symmetric subsequences for a given $s$.

Let's call the function that takes a sequence $s$ of base $b$ and returns the number of symmetric subsequences $f_b$. If this were a Leetcode question, one might be expected to code $f_b$. But I was more interested with the maximum number one could get for a given $b$ over all sequences of given length $n$, namely

$$m_{b,n} = \max_{s} f_b(s).$$

## A Quick Answer

Interestingly, we can say right away that $m_{b,n} = n$ for $b > 2$. 

**Theorem 1.** $m_{b,n} = n$ for $b > 2$.

**Proof.** The sequence where $s_i = i \text{ mod } b$ has $n$ symmetric subsequences. Notice that the values equal to $s_1$ are $s_{bk+1}$ for integer $k$ so that $bk+1 \leq n$. These are the only candidates to check for possible symmetric subsequences for $s_1$. However, the subsequence between $s_{bk+1}$ and $s_{b(k+1)+1}$ cannot be symmetric for $b>2$, as $(i+1) \mod b \neq (i-1) \mod b$ unless $b=2$. As the $(i+1) \mod b \neq (i-1) \mod b$ holds for all $i$, every $s_i$ must be a member of a length $1$ symmetric subsequence.

Wow what a simple proof that takes care of so many cases! But there was still one more case to go, $b=2$.

## Binary Symmetric Subsequences

The binary case is noticeably more interesting. Let's first look at an example. 

$$s' = (1,0,0,1,1,0,1,1,0,1,1,1,1)$$

We have $f(s') = 3$. This doesn't appear to be the best we can do for $n=13$, but what is? 

We can write code to run over all sequences $s$ for small $n$ to collect values of $m_{2,n}$. Here is a table of the first couple values. 

| $n$ |1|2|3|4|5|6|7|8|9|
|  -  |-|-|-|-|-|-|-|-|-|
| $m_{2,n}$ |1|2|2|3|3|4|4|4|5|

We can enumerate this sequence exactly making the following two main ideas that are only true for $b=2$. 

**Lemma 1.** A length $1$ symmetric subsequence in position $i$ implies $s_{i+1}=s_{i+2}=\dots=s_n$.

**Proof.** Suppose there exists a length $1$ symmetric subsequence in position $i$. Then suppose there exists a value $j$ greater than $i$ so that $s_{j}=s_i$. Therefore $(s_{i+1},\dots,s_{j-1})$ must all be the opposite value to $s_i$. However, this implies that $(s_{i},\dots,s_{j})$ form a symmetric subsequence. Therefore there can exist no such $j$. 

**Lemma 2.** There cannot exist three consecutive pairs of length $2$ symmetric sequences.

**Proof.** Suppose there exists a subsequence $s_{i}, s_{i+1}, s_{i+2}, s_{i+3}, s_{i+4}, s_{i+5}$ where the three consecutive pairs form symmetric subsequences.  This implies that $s_{i}= s_{i+1}$, $s_{i+2}= s_{i+3}$, and $s_{i+4}= s_{i+5}$. As $(s_{i}, s_{i+1})$ form a length $2$ symmetric subsequence, $s_{i}\neq s_{i+2}$, as otherwise $(s_{i}, s_{i+1} s_{i+2}, s_{i+3})$ would form a larger symmetric subsequence. Similarly, $s_{i+2}\neq s_{i+4}$. However, this gives $s_{i}=s_{i+4}$, which implies $(s_{i}, s_{i+1} s_{i+2}, s_{i+3}, s_{i+4}, s_{i+5})$ forms a symmetric sequence.

The above Lemmas can be summarizes as loosely saying that for large $n$, having $3$ symmetric subsequences in $6$ numbers is impossible. But what about $3$ in every $7$? It turns out that with these two ideas, and the following subsequence

$$(0,0,1,1,0,1,0)$$

one can prove the following theorem.

**Theorem 2.** For binary sequences we have

$$m_{2,n} = \left\lfloor\dfrac{3n+10}{7}\right\rfloor.$$

**Proof.** The full details can be found [here](https://github.com/JackHanke/binaryruns/blob/main/writeup/runs.pdf).

Notice that the number is actually more than just $\frac{3n}{7}$, as the very end of the sequence allows for more dense sequences (using the ideas from Lemma 1.).

That actually concludes my problem! I hope you enjoyed my cute LeetCode-style math problem as much as I did working on it.

## Further Questions

1. What can be said about the average value of $f$, namely

$$a_{b,n} = b^{-n} \sum_{s}f_b(s).$$

Cursory experiments show that $a_{b,n} \approx \frac{1}{2}m_{b,n}$ for all $b$, but I was unable to prove this.

Thank you for reading!
