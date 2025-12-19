---
layout: post
title:  "The Mosaic Problem - Solved!"
date:   2025-12-19 01:00:00 -0600
categories: [Math]
tags: [Mosaics, SOME3, Combinatorics]
math: true
image:
  path: assets/img/mosaics_solved.jpg
---

| [GitHub Repo](https://github.com/JackHanke/mosaics) 👾 | [YouTube Video](https://www.youtube.com/watch?v=D3dp5RBmPcs) 📺 | **Scope:** ⭐⭐⭐⭐ | 🚧 Under Construction 🚧 |

This is a followup to [this post]({% post_url 2023-07-23-mosaics %}).

## Reintroducing the Mosaic Problem

Take the following six tiles

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\cellA}[4]{\draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #2) -- (#3, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellB}[4]{\draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #2) -- (#1, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellC}[4]{\draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #4) -- (#1, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellD}[4]{\draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #4) -- (#3, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellE}[4]{\draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #2) -- (#3 * 0.5 + #1 * 0.5 , #4); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellF}[4]{\draw[red, thick] (#3, #4 * 0.5 + #2 * 0.5) -- (#1, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    % actual image
    \begin{tikzpicture}
        \cellA{0}{0}{1}{1}
        \cellB{2}{0}{3}{1}
        \cellC{4}{0}{5}{1}
        \cellD{6}{0}{7}{1}
        \cellE{8}{0}{9}{1}
        \cellF{10}{0}{11}{1}
    \end{tikzpicture}
    </script>
</div>

and create a $n \times m$ rectangular grid using them. Let's call these grids *mosaics*. For example, below is a $(7,5)-$mosaic.

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\cellA}[4]{\draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #2) -- (#3, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellB}[4]{\draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #2) -- (#1, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellC}[4]{\draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #4) -- (#1, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellD}[4]{\draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #4) -- (#3, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellE}[4]{\draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #2) -- (#3 * 0.5 + #1 * 0.5 , #4); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellF}[4]{\draw[red, thick] (#3, #4 * 0.5 + #2 * 0.5) -- (#1, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    % actual image
    \begin{tikzpicture}
        % row1
        \cellA{0}{0}{1}{1}
        \cellB{1}{0}{2}{1}
        \cellF{2}{0}{3}{1}
        \cellA{3}{0}{4}{1}
        \cellC{4}{0}{5}{1}
        \cellE{5}{0}{6}{1}
        \cellD{6}{0}{7}{1}
        % row2
        \cellA{0}{1}{1}{2}
        \cellE{1}{1}{2}{2}
        \cellF{2}{1}{3}{2}
        \cellE{3}{1}{4}{2}
        \cellE{4}{1}{5}{2}
        \cellB{5}{1}{6}{2}
        \cellD{6}{1}{7}{2}
        % row3
        \cellA{0}{2}{1}{3}
        \cellB{1}{2}{2}{3}
        \cellA{2}{2}{3}{3}
        \cellB{3}{2}{4}{3}
        \cellA{4}{2}{5}{3}
        \cellD{5}{2}{6}{3}
        \cellF{6}{2}{7}{3}
        % row4
        \cellF{0}{3}{1}{4}
        \cellE{1}{3}{2}{4}
        \cellA{2}{3}{3}{4}
        \cellA{3}{3}{4}{4}
        \cellE{4}{3}{5}{4}
        \cellD{5}{3}{6}{4}
        \cellC{6}{3}{7}{4}
        % row5
        \cellA{0}{4}{1}{5}
        \cellD{1}{4}{2}{5}
        \cellE{2}{4}{3}{5}
        \cellD{3}{4}{4}{5}
        \cellD{4}{4}{5}{5}
        \cellC{5}{4}{6}{5}
        \cellE{6}{4}{7}{5}    
    \end{tikzpicture}
    </script>
</div>

Clearly there are $6^{nm}$ possible mosaics. The question is for a given $n$ and $m$, how many mosaics contain at least one connected "loop", like below?

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\cellA}[4]{\draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #2) -- (#3, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellB}[4]{\draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #2) -- (#1, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellC}[4]{\draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #4) -- (#1, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellD}[4]{\draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #4) -- (#3, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellE}[4]{\draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #2) -- (#3 * 0.5 + #1 * 0.5 , #4); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellF}[4]{\draw[red, thick] (#3, #4 * 0.5 + #2 * 0.5) -- (#1, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }

    \newcommand{\cellAf}[4]{\filldraw[gray!40] ( #1 , #2 ) rectangle ( #3 , #4 ); \draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #2) -- (#3, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellBf}[4]{\filldraw[gray!40] ( #1 , #2 ) rectangle ( #3 , #4 ); \draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #2) -- (#1, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellCf}[4]{\filldraw[gray!40] ( #1 , #2 ) rectangle ( #3 , #4 ); \draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #4) -- (#1, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellDf}[4]{\filldraw[gray!40] ( #1 , #2 ) rectangle ( #3 , #4 ); \draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #4) -- (#3, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellEf}[4]{\filldraw[gray!40] ( #1 , #2 ) rectangle ( #3 , #4 ); \draw[red, thick] (#3 * 0.5 + #1 * 0.5 , #2) -- (#3 * 0.5 + #1 * 0.5 , #4); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }
    \newcommand{\cellFf}[4]{\filldraw[gray!40] ( #1 , #2 ) rectangle ( #3 , #4 ); \draw[red, thick] (#3, #4 * 0.5 + #2 * 0.5) -- (#1, #4 * 0.5 + #2 * 0.5); \draw[gray, thick] ( #1 , #2 ) rectangle ( #3 , #4 ); }

    % actual image
    \begin{tikzpicture}
        % row1
        \cellA{0}{0}{1}{1}
        \cellB{1}{0}{2}{1}
        \cellDf{2}{0}{3}{1}
        \cellCf{3}{0}{4}{1}
        \cellC{4}{0}{5}{1}
        \cellE{5}{0}{6}{1}
        \cellD{6}{0}{7}{1}
        % row2
        \cellA{0}{1}{1}{2}
        \cellE{1}{1}{2}{2}
        \cellEf{2}{1}{3}{2}
        \cellEf{3}{1}{4}{2}
        \cellE{4}{1}{5}{2}
        \cellB{5}{1}{6}{2}
        \cellD{6}{1}{7}{2}
        % row3
        \cellA{0}{2}{1}{3}
        \cellB{1}{2}{2}{3}
        \cellAf{2}{2}{3}{3}
        \cellBf{3}{2}{4}{3}
        \cellA{4}{2}{5}{3}
        \cellD{5}{2}{6}{3}
        \cellF{6}{2}{7}{3}
        % row4
        \cellF{0}{3}{1}{4}
        \cellE{1}{3}{2}{4}
        \cellA{2}{3}{3}{4}
        \cellA{3}{3}{4}{4}
        \cellE{4}{3}{5}{4}
        \cellD{5}{3}{6}{4}
        \cellC{6}{3}{7}{4}
        % row5
        \cellA{0}{4}{1}{5}
        \cellD{1}{4}{2}{5}
        \cellE{2}{4}{3}{5}
        \cellD{3}{4}{4}{5}
        \cellD{4}{4}{5}{5}
        \cellC{5}{4}{6}{5}
        \cellE{6}{4}{7}{5}   
    \end{tikzpicture}
    </script>
</div>

I would later learn that these loops are called *self-avoiding polygons*, but I just refer to them as polygons in the paper and this post.

## A Complete Solution

About a year after I published the video, I received a comment claiming to have a method for computing $t_{n,m}$ for small fixed $m$. The comment came from a user called Farstar31, who suggested a new method that we were able to generalize into a final solution. 

After some discussions over twitter and email, we worked out a method to compute $6^{nm} - t_{n,m}$ for any $n$ and $m$! Later work refined and improved the solution to the following result.

**Definition.** For integers $k \geq 1$ let $A_k, B_k, C_k, D_k$ be $2^{k-1} \times 2^{k-1}$ matrices with integer entries, where $A_{1}=\begin{pmatrix}6\end{pmatrix}, B_{1}=\begin{pmatrix}-1\end{pmatrix}, C_{1}=\begin{pmatrix}1\end{pmatrix}, D_{1}=\begin{pmatrix}1\end{pmatrix}$, and

$$
\begin{aligned}
A_{k+1} = \begin{pmatrix} 6A_{k} & B_{k} \\ C_{k} & D_{k} \end{pmatrix} & B_{k+1} = \begin{pmatrix} -A_{k} & B_{k} \\ 0C_{k} & D_{k} \end{pmatrix} \\
C_{k+1} = \begin{pmatrix} A_{k} & 0B_{k} \\ C_{k} & D_{k} \end{pmatrix} & D_{k+1} = \begin{pmatrix} A_{k} & -B_{k} \\ C_{k} & 6D_{k} \end{pmatrix}.
\end{aligned}
$$

**Theorem.** The number of $(m,n)$ mosaics that do not contain a polygon $\|S^{(m,n)}\|$ is the $(0,0)$ entry of $A_{n}^m$.

**Proof.** The proof is lengthy, so the full writeup can be found [here](https://github.com/JackHanke/mosaics/blob/main/writeup/mosaics.pdf).

Thank you for reading!
