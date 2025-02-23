---
layout: post
title:  "Minimal Inscribed Polyforms"
date:   2025-01-23 19:58:32 -0600
categories: jekyll update
---

[GitHub Repo 👾](https://github.com/JackHanke/minimal-inscribed-polyforms) | [YouTube Video 📺](https://www.youtube.com/watch?v=8N80EbXVUU0)

## A Global Pandemic

When I was sent home in the middle of my undergraduate degree, I was faced with a lot of time on my hands. I had done all that I thought I could on the [Mosaic Problem](https://jackhanke.github.io/jekyll/update/2025/01/23/mosaics.html), and so I turned my attention to a similar problem I also enjoyed. Predictably, it takes place on the square lattice. Consider an $n \times m$ grid. How many ways can one shade the cells so that they make a single connected shape, and the shape touches all four sides? For example, here is an example shading in a $7 \times 5$ grid.

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\cell}[4]{\filldraw[gray!40] ( #1 , #2 ) rectangle ( #3 , #4 ); \draw[thick] ( #1 , #2 ) rectangle ( #3 , #4 );}
    % actual image
    \begin{tikzpicture}
        \draw[step=1cm,color=black,thick] (0,0) grid (7,5);
        \( \cell{0}{0}{1}{1} \)
        \( \cell{1}{0}{2}{1} \)
        \( \cell{1}{1}{2}{2} \)
        \( \cell{2}{1}{3}{2} \)
        \( \cell{3}{1}{4}{2} \)
        \( \cell{4}{1}{5}{2} \)
        \( \cell{4}{2}{5}{3} \)
        \( \cell{4}{3}{5}{4} \)
        \( \cell{4}{4}{5}{5} \)
        \( \cell{5}{2}{6}{3} \)
        \( \cell{6}{2}{7}{3} \)
    \end{tikzpicture}
    </script>
</div>

Let's call the number of these shapes $s_{n,m}$. At the time, I called these shapes $4$*-paths*, as the shapes typically had four spokes that were joined by a single path. As usual, I began by collecting some empirical data by counting all the $4$-paths in a small grid. At this point I didn't know how to code that well, and I worked out examples on grid paper. I remember a lot of double and triple checking to make sure my diagrams didn't miss or double count any cases. 

After some painstaking and boring work, I enumerated a series of cases that I will not get into (as they turned out to be wrong). Still, I was happy with my (non-working) formula. But when I decided I wanted to speak with my universities combinatorics professor, I thought I should take a look at the formula again to see if I could make it better. 

That night I had one of the realizations that most mathematicians live for, that simplification that opens the whole problem up. I realized that these shapes can nicely be split into three classes, shown below.

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\cell}[4]{\filldraw[gray!40] ( #1 , #2 ) rectangle ( #3 , #4 ); \draw[thick] ( #1 , #2 ) rectangle ( #3 , #4 );}
    % actual image
    \begin{tikzpicture}[scale=2]
        \draw[step=0.5cm,color=black,thick] (0,0) grid (2,2);
        \( \cell{0}{0}{0.5}{0.5} \);
        \( \cell{0.5}{0}{1}{0.5} \);
        \( \cell{0.5}{0.5}{1}{1} \);
        \( \cell{1}{0.5}{1.5}{1} \);
        \( \cell{1}{1}{1.5}{1.5} \);
        \( \cell{1.5}{1}{2}{1.5} \);
        \( \cell{1.5}{1.5}{2}{2} \);
        
        \draw[step=0.5cm,color=black,thick] (3.5,0) grid (5.5,2);
        \draw[thick] (3.5,0) -- (3.5,2);
        \( \cell{3.5}{0}{4}{0.5} \);
        \( \cell{4}{0}{4.5}{0.5} \);
        \( \cell{4}{0.5}{4.5}{1} \);
        \( \cell{4.5}{0.5}{5}{1} \);
        \( \cell{5}{0.5}{5.5}{1} \);
        \( \cell{4.5}{1}{5}{2} \);
        \( \cell{4.5}{1.5}{5}{2} \);
        
        \draw[step=0.5cm,color=black,thick] (7,0) grid (9,2);
        \draw[thick] (7,0) -- (7,2);
        \( \cell{7.5}{0.5}{8}{1} \);
        \( \cell{7}{0.5}{7.5}{1} \);
        \( \cell{7.5}{0}{8}{0.5} \);
        \( \cell{8}{0.5}{8.5}{1} \);
        \( \cell{8}{1}{8.5}{1.5} \);
        \( \cell{8}{1.5}{8.5}{2} \);
        \( \cell{8.5}{1}{9}{1.5} \);
    \end{tikzpicture}
    </script>
</div>

These classes, from left to right, are
- The shapes that contain $2$ or $3$ corners $S_2$
- The shapes that contain $1$ corner $S_1$
- The shapes that contain $0$ corners $S_0$

*Case 1.* We begin with $S_2$. Split $S_2$ into two subsets, $S_{T,2}$ and $S_{T,2}^c$ so that $S_{T,2} \cupdot S_{T,2}^c = S_2$. Let $S_{T,2}$ represent all polyominos that are "T-shaped", while $S_{T,2}^c$ will represent all polyominos that are not. Formally, the "T-shaped" polyominos are all polyominos that contain two adjacent corner cells and a perpendicular bar, as in the left polyomino in the figure below.

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\cell}[4]{\filldraw[gray!40] ( #1 , #2 ) rectangle ( #3 , #4 ); \draw[thick] ( #1 , #2 ) rectangle ( #3 , #4 );}
    % actual image
    \begin{tikzpicture}[scale=2]
        \draw[step=0.5cm,color=black,thick] (0,0) grid (2,2);
        \( \cell{0.5}{0}{1}{0.5} \);
        \( \cell{0.5}{0.5}{1}{1} \);
        \( \cell{0.5}{1}{1}{1.5} \);
        \( \cell{0}{1.5}{0.5}{2} \);
        \( \cell{0.5}{1.5}{1}{2} \);
        \( \cell{1}{1.5}{1.5}{2} \);
        \( \cell{1.5}{1.5}{2}{2} \);
        
        \draw[step=0.5cm,color=black,thick] (3.5,0) grid (5.5,2);
        \draw[thick] (3.5,0) -- (3.5,2);
        \( \cell{3.5}{0}{4}{0.5} \);
        \( \cell{3.5}{0.5}{4}{1} \);
        \( \cell{3.5}{1}{4}{1.5} \);
        \( \cell{3.5}{1.5}{4}{2} \);
        \( \cell{4}{1.5}{4.5}{2} \);
        \( \cell{4.5}{1.5}{5}{2} \);
        \( \cell{5}{1.5}{5.5}{2} \);
    \end{tikzpicture}
    </script>
</div>

For $\|S_{T,2}\|$ it is easy to see that $\|S_{T,2}\| = 2(w-2) + 2(\ell-2) = 2w+2\ell-8$, as there is a "T-shape" polyomino for every edge cell.

For $\|S_{T,2}^c\|$, suppose the bottom left cell of the $w \times \ell$ lattice is called $(1,1)$, and the top right cell $(w,\ell)$. The number of paths from $(1,1)$ to $(w,\ell)$ is the number of ways one can make $w-1$ unit steps right and $\ell -1$ unit steps up among $w + \ell -2$ total unit steps. This gives a total of $\binom{w+\ell-2}{w-1}$ paths from $(1,1)$ to $(w,\ell)$. As we can make the same argument for corners $(1,\ell)$ and $(w,1)$, we can conclude $\|S_{T,2}^c\| = 2\binom{w+\ell-2}{w-1}$. This gives us $\|S_2\| = \|S_{T,2}\| + \|S_{T,2}^c\| = 2\binom{w+\ell-2}{w-1} +2w+2\ell-8$.

I would later learn 

Amazingly, 



TODO

## An Honors Thesis

TODO

## 

TODO
