---
layout: post
title:  "The Math behind Ride the Bus"
date:   2026-03-20 01:00:00 -0600
categories: [Math]
tags: [Math]
math: true
image:
  path: assets/img/ridethebus/ridethebus.webp
---

| [GitHub Repo](https://github.com/JackHanke/ridethebus) 👾 | **Scope:** ⭐⭐⭐ | 🚧 Under Construction 🚧 |

*Ride the Bus* is a game played with a group of friends around a deck of cards. This post focused on *Phase II* of the game.

## Rules

This phase begins with a group of players around a face-down deck of 52 cards. The top card in the deck is drawn and flipped over. The first player's task is to guess whether the next card drawn will be higher or lower in value. To break ties, assume that every card is labeled $1$ through $52$ for comparison. If the player is right, nothing happens. If the player is wrong, he is removed from the game (or faces some other punishment). Then it is the next player's turn. The game ends when the last card is drawn. 

Let's go through an example round! The game begins when we flip over the first card, in this case the "12" card. As we know the 1,...,11 and 13,...,52 cards are still in the deck, so it's smart to pick "higher".

![](assets/img/ridethebus/cards.jpg)

We flip the next card, and see it is "41". We were right! If the card had been, say, a "4", we would have lost. For the next turn, we guess "lower", and the game continues. 

Importantly, all players know the identity of all cards drawn, and so there is always a "correct" guess at any point (unless there are the same number of cards below in the deck that there are above, in which case we choose "higher" by convention). The question we examine in this post is:

What is the probability the optimal strategy is correct through the entire deck of $n$ cards? Note the traditional game is for $n=52$. 

## Solution

Let's let the number of games that make it to the end $a_n$. Clearly, the probability is then $\frac{a_n}{n!}$, as there are $n!$ shuffles. The first couple values of $a_n$ for $n \geq 0$ are:

$$1, 1, 2, 5, 16, 62, 286, 1519, 9184, \dots$$

Interestingly, this has already been solved! Jan Kristian Haugland added a formula for $a_n$ on the OEIS: [A144188](https://oeis.org/search?q=5%2C+16%2C+62%2C+286&language=english&go=Search). He showed the following. Let $f(0, 0) = 1$ and 

$\begin{equation}
f(n, k) = \text{max}\left(\sum_{i=0}^{k-1}f(n - 1, i), \sum_{i=k}^{n-1}f(n - 1, i) \right).
\end{equation}$

Then $a(n) = f(n, 0)$. Here $n$ represents the number of cards in the deck, and $k$ represents the number of cards that are higher than the current card in the deck. $f(n,k)$ is then the number of games that win from this situation. The optimal strategy is choosing the action with more corresponding cards remaining, and $f(n,k)$ is the maximum over drawing each possible card $i$ and continuing with $n-1$ cards. Therefore, we sum over $f(n,i)$ ranging between $0,\dots,k-1$ for the lower option and $k,\dots,n-1$ for the higher option.

## We can improve this formula! 

Instead of the seperate function $f(n,k)$, I wanted $a_n$ just in terms of itself. That is to write all values of $f(n,k)$ in terms of $f(m,0)$ for $m \leq n$. We can find a formula with these contraints, beginning with viewing the values of $f(n,k)$ in a triangle (as in Pascal's Triangle), where the rows start $n=0,1,\dots$ and columns $0 \leq k \leq n$.

$$\begin{array}
&&&&&&&&1&&&&&&\\
&&&&&&1&&1&&&&&\\
&&&&&2&&1&&2&&&&\\
&&&&5&&3&&3&&5&&&\\
&&&16&&11&&8&&11&&16&&\\
&&62&&46&&35&&35&&46&&62&\\
&286&&224&&178&&143&&178&&224&&286\\
1519&&f(7,1)&
\end{array}$$

Notice that our sequence is the left-most and right-most side of this triangle, as $a_n = f(n,0)$ by definition. Also notice that $f(n,k) = f(n,n-k)$, as having $k$ cards higher than the current card is the same as having $n-k$ cards lower than the current card. 

Let's examine the triangle we have constructed closely. We can derive new values applying Haugland's formula in a simple way. Suppose we want to determine the value to the right of $1519$, that being $f(7,1)$. To apply the formula, imagine placing a vertical bar in the row directly above the number we want to compute, in our case between the $286$ and the $224$. Haugland's formula sums all numbers to the left and right of this bar within the row and returns the larger of the two. The left-most edge values $f(n,0)$ are then just the sums of the previous row, as there is no "left" sum. 

We previously saw that the triangle was symmetric, so let's focus on the left-hand side of the triangle. 

$$\begin{array}
&&&&&&&&1\\
&&&&&&1&&\\
&&&&&2&&1&\\
&&&&5&&3&&\\
&&&16&&11&&8&\\
&&62&&46&&35&\\
&286&&224&&178&&143\\
1519&&f(7,1)&&f(7,2)&&f(7,3)
\end{array}$$

Here we can see that for even $n \geq 2$, we have $f(n,\lfloor\frac{n}{2}\rfloor) = \frac{1}{2}f(n,0)$. This is because for even $n$, the sum for $f(n,\lfloor\frac{n}{2}\rfloor)$ splits the previous row perfectly in two. As $f(n,0)$ is the sum of the previous row, and all rows are symmetric, this middle value must be half $f(n,0)$. Therefore, we can write

$\begin{equation} f(n,0) = 2\sum_{k=0}^{\lfloor\frac{n}{2}\rfloor-1}f(n-1,k) + \frac{1}{2}f(n-1,0) \mathbb{I}_{\text{odd } n \geq 3}\end{equation}$

Next notice that within this half-triangle we have $f(n,k)-f(n,k+1) = f(n-1,k)$. That is, in the above diagram the larger sum will always be the "right" sum. Therefore, the sum of the previous row for both $f(n,k)$ and $f(n,k+1)$ differ only by the element in the previous row between the two, that being $f(n-1,k)$. We rewrite this to be 

$\begin{equation}f(n,k) = f(n,k-1)-f(n-1,k-1),\end{equation}$

for $0 \leq k \leq \lfloor\frac{n}{2}\rfloor-1$. For example, we can compute $f(7,1)$ another way. We know that $f(7,0)=1519$ is the sum of the previous row, and we know $f(n,1)$ is the sum of the previous row, excluding $286$. So we have $f(7,1) = 1519-286$. 

We use Equation (3) to rewrite Equation (2). We know that we can write for any $n>1$ that $f(n,1) = f(n,0)-f(n-1,0)$, by the same argument. For $f(n,2)$, we have 

$$f(n,2) = f(n,1)-f(n-1,1) = (f(n,0)-f(n-1,0)) - (f(n-1,0)-f(n-2,0)).$$

We can then simply continue to chain the main identity for $k < \lfloor\frac{n}{2}\rfloor$, as we know $f(n,\lfloor\frac{n}{2}\rfloor)$ for even $n$, so

$\begin{equation}f(n,k) = \sum_{i=0}^{k}(-1)^{i}\binom{k}{i}f(n-i,0).\end{equation}$

Substituing Equation (4) into Equation (2), and writing $a_n = f(n,0)$ gives

$$a_n = 2\sum_{k=0}^{\lfloor\frac{n}{2}\rfloor-1} \sum_{i=0}^{k} (-1)^{i} \binom{k}{i} a_{n-1-i} + \frac{1}{2}f(n-1,0) \mathbb{I}_{\text{odd } n \geq 3}$$

Let's focus on the double sum. Switching the indexing order gives

$$\sum_{i=0}^{\lfloor\frac{n}{2}\rfloor-1} (-1)^{i}a_{n-1-i} \sum_{k=i}^{\lfloor\frac{n}{2}\rfloor-1} \binom{k}{i}.$$

The inner sum nicely simplifies with the [hockey stick identity](https://en.wikipedia.org/wiki/Hockey-stick_identity), so our sum becomes

$$\sum_{i=0}^{\lfloor\frac{n}{2}\rfloor-1} (-1)^{i} a_{n-1-i} \binom{\lfloor\frac{n}{2}\rfloor}{i+1}.$$

Setting $i+1 \to i$ gives that

$\begin{equation}a_n = 2\sum_{i=1}^{\lfloor \frac{n}{2} \rfloor} (-1)^{i+1} \binom{\lfloor \frac{n}{2} \rfloor}{i} a_{n-i} + \frac{1}{2}f(n-1,0) \mathbb{I}_{\text{odd } n \geq 3}\end{equation}$

This is a pretty neat formula, and a lot faster to compute than the original $f$!

## Extensions

We can extend Haugland's function $f$ to to the number of $n$-card games with $k$ cards less than the current card in the deck that end at move $j$. This function, which we call $g(n, k, j)$, is defined as

$$g(n, k, j) = \begin{cases} ? & \text{ for } j = 0 \\ \text{max}\left(\sum_{i=0}^{k-1} g(n - 1, i, j-1), \sum_{i=k}^{n-1} g(n - 1, i, j-1) \right) & \text{ else.} \end{cases}$$

Clearly we have $g(n, k, n) = f(n,k)$. We have an analog to Equation (2),

$\begin{equation} g(n,0,j) = 2\sum_{k=0}^{\lfloor\frac{n}{2}\rfloor-1}g(n-1,k,j-1) + \frac{1}{2}g(n-1,0,j-1)\mathbb{I}_{\text{odd } n \geq 3}, \end{equation}$

as well as to Equation (3),

$\begin{equation} g(n,k,j) = g(n,k-1,j) - g(n-1,k-1,j-1), \end{equation}$

for $0 \leq k < \lfloor \frac{n}{2} \rfloor$ and $j > 0$. Similarly to the goal for Haugland's $f$, we want to write $g(n,k,j)$ in terms of $(g,0,j)$. To get an analog to Equation (4), 

**TODO review above**

$$g(n,k,j) = \sum_{i=0}^{j-1} (-1)^{i} \binom{k}{i} g(n-i,0,j-i) + (-1)^{j} \sum_{i=0}^{j-1}\binom{k-1-i}{j-1}g(n-j,i,0)$$

**TODO review above**

Finally, an interesting identity is

$$\sum_{j=1}^{n} b_{n,j} = n!,$$

as the total number of games that end at a specific $j$ summed over all $j$ should be the number of shuffles.

## Further Questions

- Can we compute the generating function $A(x) = \sum_{n \geq 0} a_n x^n$?
- What is the growth rate of this sequence?
- For a deck of $n$ cards, what is the expected number of correct guesses before being wrong?

Thank you for reading!
