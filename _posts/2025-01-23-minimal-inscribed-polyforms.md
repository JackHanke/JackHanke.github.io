---
layout: post
title:  "[MATH] Minimal Inscribed Polyforms"
date:   2025-01-23 19:58:32 -0600
categories: jekyll update
---

| [GitHub Repo 👾](https://github.com/JackHanke/minimal-inscribed-polyforms) | [YouTube Video 📺](https://www.youtube.com/watch?v=8N80EbXVUU0) | **Scope:** ⭐⭐⭐⭐ |

## A Global Pandemic

When I was sent home in the middle of my undergraduate degree, I was faced with a lot of time on my hands. I had done all that I thought I could on the [Mosaic Problem](https://jackhanke.github.io/jekyll/update/2025/01/23/mosaics.html), and so I turned my attention to a similar problem I also enjoyed. Predictably, it takes place on the square lattice. Consider an $n \times m$ grid. How many ways can one shade the cells so that they make a single connected shape, the shape touches all four sides, and that shape contains the least amount of squares that can do so? For example, here is an example shading in a $7 \times 5$ grid.

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

*Case 1.* We begin with $S_2$. Split $S_2$ into two subsets, $S_{T,2}$ and $S_{T,2}^c$ so that $S_{T,2} \cup S_{T,2}^c = S_2$. Let $S_{T,2}$ represent all polyominos that are "T-shaped", while $S_{T,2}^c$ will represent all polyominos that are not. Formally, the "T-shaped" polyominos are all polyominos that contain two adjacent corner cells and a perpendicular bar, as in the left polyomino in the figure below.

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

*Case 2.* For $S_1$, suppose we choose corner $(1,1)$ and start a path to some cell $(i,j)$. Extending a path from $(i,j)$ to $(i,\ell)$ and from $(i,j)$ to $(w,j)$ creates a minimally inscribed polyomino that touches only one corner. In the case image above, $(i,j)=(3,1)$. Notice that $(i,j)$ must have $i \in [2,w-1]$, and $j \in [2,\ell-1]$, as otherwise the polyomino formed would have more than $1$ corner. Therefore, the total number of polyominos that touch only corner $(1,1)$ in $S_1$ is 
$$\sum_{i=2}^{w-1}\sum_{j=2}^{\ell-1} \binom{i+j-2}{i-1} = \binom{w+\ell-2}{w-1}-w-\ell+2.$$

As we can make the same argument starting with any corner, we have $\|S_1\| = 4\binom{w+\ell-2}{w-1}-4w-4\ell+8$

*Case 3.* Finally, consider $S_0$. Suppose we have two cells $(i,j)$ and $(i',j')$, with $i,i' \in [2,w-1]$ and $j,j' \in [2,\ell-1]$, $i \leq i'$ and $j \leq j'$. 

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\cell}[4]{\filldraw[gray!40] ( #1 , #2 ) rectangle ( #3 , #4 ); \draw[thick] ( #1 , #2 ) rectangle ( #3 , #4 );}
    \newcommand{\cellred}[4]{\filldraw[red!60] ( #1 , #2 ) rectangle ( #3 , #4 ); \draw[thick] ( #1 , #2 ) rectangle ( #3 , #4 );}
    % actual image
    \begin{tikzpicture}[scale=2]
        \draw[step=0.5cm,color=black,thick] (0,0) grid (3.5,2.5);
        \( \cell{0}{0.5}{0.5}{1} \)
        \( \cell{0.5}{0}{1}{0.5} \)
        \( \cellred{0.5}{0.5}{1}{1} \)
        \( \cell{1}{0.5}{1.5}{1} \)
        \( \cell{1.5}{0.5}{2}{1} \)
        \( \cell{1.5}{1}{2}{1.5} \)
        \( \cell{2}{1}{2.5}{1.5} \)
        \( \cellred{2}{1.5}{2.5}{2} \)
        \( \cell{2}{2}{2.5}{2.5} \)
        \( \cell{2.5}{1.5}{3}{2} \)
        \( \cell{3}{1.5}{3.5}{2} \)
    \end{tikzpicture}
    </script>
</div>

One can construct a path from $(i,j)$ to $(i',j')$. Next, make a path from $(i,j)$ to $(1,j)$, and a path from $(i,j)$ to $(i,1)$. Similarly, make a path from $(i',j')$ to $(w,j')$ and a path from $(i',j')$ to $(i',\ell)$. This constructs a minimal inscribed polyomino that touches no corners, as shown above. 

These polyominos can easily be enumerated by the size of the rectangle made with corners $(i,j)$, $(i,j')$, $(i',j)$, and $(i',j')$. If we suppose that $\Delta i = i'-i$, and $\Delta j = j'-j$, we can construct the following sum.

$$\sum_{\Delta i=1}^{w-2}\sum_{\Delta j=1}^{\ell-2}\binom{\Delta i+\Delta j-2}{\Delta i-1}(\ell-1- \Delta j)(w-1- \Delta i)= \binom{w+\ell-2}{w-1} -w\ell + w+\ell-2.$$

As $(i,j')$ and $(i',j)$ define the same rectangle but different polyominos, we multiply the above result by two. However, this double counts the polyominos that are built with $i=i'$ and $j=j'$. There are $(w-2)(\ell-2)$ of these polyominos, so we subtract this quantity once. Therefore $\|S_0\| = 2\binom{w+\ell-2}{w-1} -2w\ell + 2w+2\ell-4 - (w-2)(\ell-2) = 2\binom{w+\ell-2}{w-1} -3w\ell +4w +4\ell -8$

Combining the three subsets, we get the result.

$$s_{n,m} = |S| = |S_2|+|S_1|+|S_0|=8\binom{w+\ell-2}{w-1} -3w\ell + 2w +2\ell-8.$$

What a nice formula! I was now ready to contact the professor.

## A Rediscovery

I wrote up a cursory proof of the above result and emailed the combinatorics professor (at the time I hardly knew LaTex, so in hindsight the writeup looks terrible). In that email I asked if the work was enough for an *honors thesis*, a degree requirement of mine. 

After a while I received a response. The professor, who from now on I will call Tom, said that though the work wasn't enough for an honors thesis, he would be happy to supervise one where I continue work in this area. How excited I was to hear that! We had a few meetings over Zoom throughout the summer, one of which turned out to be the most important Zoom meeting I've ever had. 

We were discussing my work when my professor typed into Google some search terms, and opened up a paper called [Enumeration of Inscribed Polyominos](https://www.researchgate.net/publication/281299307_Enumeration_of_inscribed_polyominos) by Goupil, Cloutier, and Noubound. To both of our shock, there on page 2, was not only the problem we were studying, but the formula I had found! 

Though Tom was happy, I admit my heart sunk. I thought I had found something new. I wanted the formula I found to be, well, mine. But I would later learn that this discovery lent me a huge amount of credibility to Tom. I had rediscovered a result, published only ten years prior. To him, I was now a student worthy to dedicate time to advising. 

The paper turned out to be very useful. I learned that my $4$-paths were a special case of a well known mathematical object called a *polyomino*. A polyominos of area $n$ is a shape in the plane constructed by joining $n$ unit squares along their edges. A famous example is the $4$-ominos are the [TETRIS](https://en.wikipedia.org/wiki/Tetris) pieces. The authors had called these special cases *minimal inscribed polyominos*, so that is what we will call them from now on. As we continued researching, we found that the authors had also answered the analagous question in $3$ dimensions. What else was there to do for my thesis?

## An Honors Thesis

The direction I chose to go in for my thesis was to generalize not to higher dimensions (polycubes), but instead to other shapes. I chose to study minimal inscribed *polyforms*, where a polyform of area $n$ is a shape in the plane constructed by joining $n$ unit side length polygons along their edges. For example,below is a polyform composed of $9$ unit triangles.

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\createtri}[6]{\filldraw[gray!40] ( #1 , #2 ) -- ( #3 , #4 ) -- ( #5 , #6 ) -- cycle; \draw[thick] ( #1 , #2 ) -- ( #3 , #4 ) -- ( #5 , #6 ) -- cycle;}
    % actual image
    \begin{tikzpicture}[scale=1.5]
        \( \createtri{1}{0}{2}{0}{1.5}{0.866} \);
        \( \createtri{1.5}{0.866}{2.5}{0.866}{2}{1.732} \);
        \( \createtri{2}{1.732}{3}{1.732}{2.5}{2.598} \);
        \( \createtri{1}{1.732}{2}{1.732}{1.5}{2.598} \);
        \( \createtri{1.5}{0.866}{2.5}{0.866}{2}{0} \);
        \( \createtri{1.5}{0.866}{1}{1.732}{2}{1.732} \);
        \( \createtri{2.5}{0.866}{3}{1.732}{2}{1.732} \);
        \( \createtri{3}{1.732}{3.5}{2.598}{2.5}{2.598} \);
        \( \createtri{2}{0}{3}{0}{2.5}{0.866} \);
    \end{tikzpicture}
    </script>
</div>

However, to generalize to polyforms, we also need to generalize the grid (or lattice) that we are inscribing these polyforms. For minimal inscribed polyforms it was clear what cells were considered touching a side of the rectangular lattice. In other lattices is may be less clear. For example, consider the figure below. 

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\createtri}[6]{\filldraw[gray!40] ( #1 , #2 ) -- ( #3 , #4 ) -- ( #5 , #6 ) -- cycle; \draw[thick] ( #1 , #2 ) -- ( #3 , #4 ) -- ( #5 , #6 ) -- cycle;}
    % actual image
    \begin{tikzpicture}[scale=1.5]
            %row 1
            \draw[thick] (0,0.25) -- (0.433,0) -- (0.866,0.25) -- (0.866,0.75) -- (0.433,1) -- (0,0.75) -- (0,0.25);
            \draw[thick] (0.866,0.25) -- (1.299,0) -- (1.732,0.25) -- (1.732,0.75) -- (1.299,1) -- (0.866,0.75) -- (0.866,0.25);
            \draw[thick] (1.732,0.25) -- (2.165,0) -- (2.598,0.25) -- (2.598,0.75) -- (2.165,1) -- (1.732,0.75) -- (1.732,0.25);
            \draw[thick] (2.598,0.25) -- (3.031,0) -- (3.464,0.25) -- (3.464,0.75) -- (3.031,1) -- (2.598,0.75) -- (2.598,0.25);
            %row 2
            \draw[thick] (0.433,1) -- (0.866,0.75) -- (1.299,1) -- (1.299,1.5) -- (0.866,1.75) -- (0.433,1.5) -- (0.433,1);
            \draw[thick] (1.299,1) -- (1.732,0.75) -- (2.165,1) -- (2.165,1.5) -- (1.732,1.75) -- (1.299,1.5) -- (1.299,1);
            \draw[thick] (2.165,1) -- (2.598,0.75) -- (3.031,1) -- (3.031,1.5) -- (2.598,1.75) -- (2.165,1.5) -- (2.165,1);
            %row 3
            \draw[thick] (0.866,1.75) -- (1.299,1.5) -- (1.732,1.75) -- (1.732,2.25) -- (1.299,2.5) -- (0.866,2.25) -- (0.866,1.75);
            \draw[thick] (1.732,1.75) -- (2.165,1.5) -- (2.598,1.75) -- (2.598,2.25) -- (2.165,2.5) -- (1.732,2.25) -- (1.732,1.75);
            %row 4
            \draw[thick] (1.299,2.5) -- (1.732,2.25) -- (2.165,2.5) -- (2.165,3) -- (1.732,3.25) -- (1.299,3) -- (1.299,2.5);
    \end{tikzpicture}
    </script>
</div>

One of the lessons I learned during this thesis was just how important it is to make it clear exactly what we are talking about. The rest of this section is a formalization of exactly what I mean by minimal inscribed polyforms. 

To make it clear which cells in a given lattice belong to a side, construct the dual graph $G$ of the lattice. Then let $k$ be the number of sides chosen. Minimal inscribed polyominos have $k=4$. In the above figure, the most obvious choice is $k=3$. After a $k$ is decided, label each vertex of $G$ with a subset of $[k]=\{1,2,\dots,k \}$, so that $\bigcup_{j \in V(G)} j = [k]$, where $V(G)$ is the vertex set of $G$. Let us call these graphs the *labelled dual*}* of $G$.

The lattice in the above figure has many possible labelled duals, one of which is shown below.

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\lablnode}[3]{\node[shape=circle,draw=white,fill=white, inner sep=0pt,minimum size=2pt] (A) at ( #1 , #2 ) {(#3)};}
    % actual image
    \begin{tikzpicture}[scale=1.5]
        \draw[thick] (0,0) -- (3,0) -- (1.5,2.598) -- (0,0);
        %rows
        \draw[thick] (0.5,0.866) -- (2.5,0.866);
        \draw[thick] (1,1.732) -- (2,1.732);
        %big diagonals
        \draw[thick] (1,1.732) -- (2,0);
        \draw[thick] (2,1.732) -- (1,0);
        %small diagonals
        \draw[thick] (1,0) -- (0.5,0.866);
        \draw[thick] (2,0) -- (2.5,0.866);
        
        %nodes
        \( \lablnode{0}{0}{1,3} \)
        \( \lablnode{1}{0}{3} \)
        \( \lablnode{2}{0}{3} \)
        \( \lablnode{3}{0}{2,3} \)
        \( \lablnode{2}{1.732}{2} \)
        \( \lablnode{2.5}{0.866}{2} \)
        \( \lablnode{1.5}{2.598}{1,2} \)
        \( \lablnode{1}{1.732}{1} \)
        \( \lablnode{0.5}{0.866}{1} \)
        
        \( \lablnode{1.5}{0.866}{} \)
    \end{tikzpicture}
    </script>
</div>

Therefore, polyform inscription can be defined as so. Suppose we have a labelled dual $G$. A given subgraph $G'$ is an *inscribed polyform* in $G$ if the following condition holds.

$$\bigcup_{j \in V(G')} j = [k]$$

Informally, the condition above says that the subgraph $G'$ touches all $k$ sides. Notice that the number of vertices in an inscribed polyform of $G$ can vary. If $A$ is an inscribed polyform, then $\|V(A)\| \in [m(G),\|V(G)\|]$.  Here $m(G)$ represents the minimum number of vertices for which the above holds, and can range from $1$ to $\|V(G)\|$, depending on the structure and labelling of $G$. Our focus is on the minimal inscribed polyforms, and so an inscribed polyform $A$ is *minimal* if $\|V(A)\| = m(G)$.  

Finally, we are concerned with the number of polyforms that are minimal. For a labelled graph $G$, let $\rho(G)$ denote the number of minimal inscribed polyforms. My thesis was then constructing families of labelled graphs $G$ and computing $\rho(G)$ for these families. 

## Results

My thesis was essentially a catalogue of $8$ of these families, and a few patterns I found among these families. This post will only hilight my favorite family from that catalogue. If you are interested in the other families for any of these results, please find my formal writeup of my results [here](https://github.com/JackHanke/minimal-inscribed-polyforms/blob/main/minnesota-sub/mjumsubmission.pdf).

My favorite family I enumerated was a triangle made of hexagons, where inscription meant touching all corner hexagons of the triangle. Let $\triangle_n$ represent a triangle with a side made up of $n$ hexagons. Below is an animation of all $41$ minimal inscribed polyforms in $\triangle_5$.

{:refdef: style="text-align: center;"}
![]({{ site.baseimg }}/assets/hex.gif)
{: refdef}

*Theorem 2.* The number of minimal inscribed polyforms $\rho(\triangle_n)$ for $n\geq 2$ is given by the following formula.

$$\rho(\triangle_n) = \binom{2(n-1)}{n-1} - \sum_{k=0}^{n-2}\binom{2k}{k}.$$

The proof of Theorem was very difficult for me to arrive at. I tried the methods I had used for the other families before it, and yet I couldn't identify the right approach. Only after months of thinking about the problem did I come to another one of those realizations that I live for. The path for this proof, namely the realization in the first sentence, came to me all at once when I was sitting on the couch. I am very happy to give the following argument.

*Proof.* Each unit hexagon in the triangle corresponds with a unique weak integer $3$-composition of $n-1$. The figure below shows the visual interpretation of the integer composition for the dark grey hexagon. The lengths of the $3$ light grey "spokes" are the components of the composition. Suppose the components of the composition are labelled $k_1$, $k_2$, and $k_3$. Then the figure below represents the cell for $k_1 = 2$, $k_2 = 1$, and $k_3 = 1$, read counter-clockwise around the dark grey hexagon. 

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\lablnode}[3]{\node[shape=circle,draw=white,fill=white, inner sep=0pt,minimum size=2pt] (A) at ( #1 , #2 ) {(#3)};}
    % actual image
    \begin{tikzpicture}[scale=1.5]
        \filldraw[gray!40] (1.732,0.25) -- (2.165,0) -- (2.598,0.25) -- (2.598,0.75) -- (2.165,1) -- (1.732,0.75) -- (1.732,0.25);

        \filldraw[gray!95] (2.165,1) -- (2.598,0.75) -- (3.031,1) -- (3.031,1.5) -- (2.598,1.75) -- (2.165,1.5) -- (2.165,1);
        \filldraw[gray!40] (3.031,1) -- (3.464,0.75) -- (3.897,1) -- (3.897,1.5) -- (3.464,1.75) -- (3.031,1.5) -- (3.031,1);

        \filldraw[gray!40] (1.732,1.75) -- (2.165,1.5) -- (2.598,1.75) -- (2.598,2.25) -- (2.165,2.5) -- (1.732,2.25) -- (1.732,1.75);

        \filldraw[gray!40] (1.299,2.5) -- (1.732,2.25) -- (2.165,2.5) -- (2.165,3) -- (1.732,3.25) -- (1.299,3) -- (1.299,2.5);

        %row 1
        \draw[thick] (0,0.25) -- (0.433,0) -- (0.866,0.25) -- (0.866,0.75) -- (0.433,1) -- (0,0.75) -- (0,0.25);
        \draw[thick] (0.866,0.25) -- (1.299,0) -- (1.732,0.25) -- (1.732,0.75) -- (1.299,1) -- (0.866,0.75) -- (0.866,0.25);
        \draw[thick] (1.732,0.25) -- (2.165,0) -- (2.598,0.25) -- (2.598,0.75) -- (2.165,1) -- (1.732,0.75) -- (1.732,0.25);

        \draw[thick] (2.598,0.25) -- (3.031,0) -- (3.464,0.25) -- (3.464,0.75) -- (3.031,1) -- (2.598,0.75) -- (2.598,0.25);
        \draw[thick] (3.464,0.25) -- (3.897,0) -- (4.33,0.25) -- (4.33,0.75) -- (3.897,1) -- (3.464,0.75) -- (3.464,0.25);

        %row 2
        \draw[thick] (0.433,1) -- (0.866,0.75) -- (1.299,1) -- (1.299,1.5) -- (0.866,1.75) -- (0.433,1.5) -- (0.433,1);
        \draw[thick] (1.299,1) -- (1.732,0.75) -- (2.165,1) -- (2.165,1.5) -- (1.732,1.75) -- (1.299,1.5) -- (1.299,1);
        \draw[thick] (2.165,1) -- (2.598,0.75) -- (3.031,1) -- (3.031,1.5) -- (2.598,1.75) -- (2.165,1.5) -- (2.165,1);
        \draw[thick] (3.031,1) -- (3.464,0.75) -- (3.897,1) -- (3.897,1.5) -- (3.464,1.75) -- (3.031,1.5) -- (3.031,1);

        %row 3
        \draw[thick] (0.866,1.75) -- (1.299,1.5) -- (1.732,1.75) -- (1.732,2.25) -- (1.299,2.5) -- (0.866,2.25) -- (0.866,1.75);
        \draw[thick] (1.732,1.75) -- (2.165,1.5) -- (2.598,1.75) -- (2.598,2.25) -- (2.165,2.5) -- (1.732,2.25) -- (1.732,1.75);
        \draw[thick] (2.598,1.75) -- (3.031,1.5) -- (3.464,1.75) -- (3.464,2.25) -- (3.031,2.5) -- (2.598,2.25) -- (2.598,1.75);

        %row 4
        \draw[thick] (1.299,2.5) -- (1.732,2.25) -- (2.165,2.5) -- (2.165,3) -- (1.732,3.25) -- (1.299,3) -- (1.299,2.5);
        \draw[thick] (2.165,2.5) -- (2.598,2.25) -- (3.031,2.5) -- (3.031,3) -- (2.598,3.25) -- (2.165,3) -- (2.165,2.5);

        %row 5
        \draw[thick] (1.732,3.25) -- (2.165,3) -- (2.598,3.25) -- (2.598,3.75) -- (2.165,4) -- (1.732,3.75) -- (1.732,3.25);
    \end{tikzpicture}
    </script>
</div>

Notice that $k_i$ can be $0$, as there are cells in which one cannot extend a spoke in a certain direction (the dark grey hexagon being located on a side or corner). 

Using the spokes as guides, we can group paths from each corner to the dark grey hexagon. An example of this grouping is shown in the figure below.

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\lablnode}[3]{\node[shape=circle,draw=white,fill=white, inner sep=0pt,minimum size=2pt] (A) at ( #1 , #2 ) {(#3)};}
    % actual image
    \begin{tikzpicture}[scale=1.5]
        %polyform
        \filldraw[gray!60] (0,0.25) -- (0.433,0) -- (0.866,0.25) -- (1.299,0) -- (1.732,0.25) -- (2.165,0) -- (2.598,0.25)-- (2.598,0.75) -- (3.031,1) -- (3.031,1.5) -- (2.598,1.75) -- (2.165,1.5) -- (1.732,1.75) -- (1.299,1.5)  -- (0.866,1.75)-- (0.433,1.5) -- (0.433,1) -- (0.433,1) -- (0,0.75)  -- (0,0.25);

        \filldraw[gray!20] (2.165,1.5) -- (2.165,1) -- (2.598,0.75) -- (2.598,0.25) -- (3.031,0) -- (3.464,0.25) -- (3.897,0) -- (4.33,0.25) -- (4.33,0.75) -- (3.897,1) -- (3.897, 1.5) -- (3.464,1.75) -- (3.031,1.5) -- (3.031,1);

        \filldraw[gray!40] (3.464,1.75) -- (3.464,2.25) -- (3.031,2.5) -- (3.031,3) -- (2.598,3.25) -- (2.598,3.25) -- (2.598,3.75) -- (2.165,4) -- (1.732,3.75) -- (1.732,3.25) -- (1.732,3.25) -- (1.299,3) -- (1.299,2.5) -- (1.732, 2.25) -- (1.732, 1.75) -- (2.165,1.5) -- (2.598,1.75) -- (3.031,1.5) -- (3.464,1.75);

        \filldraw[gray!95] (2.165,1) -- (2.598,0.75) -- (3.031,1) -- (3.031,1.5) -- (2.598,1.75) -- (2.165,1.5) -- (2.165,1);

        %row 1
        \draw[thick] (0,0.25) -- (0.433,0) -- (0.866,0.25) -- (0.866,0.75) -- (0.433,1) -- (0,0.75) -- (0,0.25);
        \draw[thick] (0.866,0.25) -- (1.299,0) -- (1.732,0.25) -- (1.732,0.75) -- (1.299,1) -- (0.866,0.75) -- (0.866,0.25);
        \draw[thick] (1.732,0.25) -- (2.165,0) -- (2.598,0.25) -- (2.598,0.75) -- (2.165,1) -- (1.732,0.75) -- (1.732,0.25);
        \draw[thick] (2.598,0.25) -- (3.031,0) -- (3.464,0.25) -- (3.464,0.75) -- (3.031,1) -- (2.598,0.75) -- (2.598,0.25);
        \draw[thick] (3.464,0.25) -- (3.897,0) -- (4.33,0.25) -- (4.33,0.75) -- (3.897,1) -- (3.464,0.75) -- (3.464,0.25);

        %row 2
        \draw[thick] (0.433,1) -- (0.866,0.75) -- (1.299,1) -- (1.299,1.5) -- (0.866,1.75) -- (0.433,1.5) -- (0.433,1);
        \draw[thick] (1.299,1) -- (1.732,0.75) -- (2.165,1) -- (2.165,1.5) -- (1.732,1.75) -- (1.299,1.5) -- (1.299,1);
        \draw[thick] (2.165,1) -- (2.598,0.75) -- (3.031,1) -- (3.031,1.5) -- (2.598,1.75) -- (2.165,1.5) -- (2.165,1);
        \draw[thick] (3.031,1) -- (3.464,0.75) -- (3.897,1) -- (3.897,1.5) -- (3.464,1.75) -- (3.031,1.5) -- (3.031,1);

        %row 3
        \draw[thick] (0.866,1.75) -- (1.299,1.5) -- (1.732,1.75) -- (1.732,2.25) -- (1.299,2.5) -- (0.866,2.25) -- (0.866,1.75);
        \draw[thick] (1.732,1.75) -- (2.165,1.5) -- (2.598,1.75) -- (2.598,2.25) -- (2.165,2.5) -- (1.732,2.25) -- (1.732,1.75);
        \draw[thick] (2.598,1.75) -- (3.031,1.5) -- (3.464,1.75) -- (3.464,2.25) -- (3.031,2.5) -- (2.598,2.25) -- (2.598,1.75);

        %row 4
        \draw[thick] (1.299,2.5) -- (1.732,2.25) -- (2.165,2.5) -- (2.165,3) -- (1.732,3.25) -- (1.299,3) -- (1.299,2.5);
        \draw[thick] (2.165,2.5) -- (2.598,2.25) -- (3.031,2.5) -- (3.031,3) -- (2.598,3.25) -- (2.165,3) -- (2.165,2.5);

        %row 5
        \draw[thick] (1.732,3.25) -- (2.165,3) -- (2.598,3.25) -- (2.598,3.75) -- (2.165,4) -- (1.732,3.75) -- (1.732,3.25);

        %hi light
        \draw[line width = 0.4mm, red] (0,0.25) -- (0.433,0) -- (0.866,0.25) -- (1.299,0) -- (1.732,0.25) -- (2.165,0) -- (2.598,0.25)-- (2.598,0.75) -- (3.031,1) -- (3.031,1.5) -- (2.598,1.75) -- (2.165,1.5) -- (1.732,1.75) -- (1.299,1.5)  -- (0.866,1.75)-- (0.433,1.5) -- (0.433,1) -- (0.433,1) -- (0,0.75)  -- (0,0.25);

        \draw[line width = 0.4mm, red] (2.165,1.5) -- (2.165,1) -- (2.598,0.75) -- (2.598,0.25) -- (3.031,0) -- (3.464,0.25) -- (3.897,0) -- (4.33,0.25) -- (4.33,0.75) -- (3.897,1) -- (3.897, 1.5) -- (3.464,1.75) -- (3.031,1.5) -- (3.031,1);

        \draw[line width = 0.4mm, red] (3.464,1.75) -- (3.464,2.25) -- (3.031,2.5) -- (3.031,3) -- (2.598,3.25) -- (2.598,3.25) -- (2.598,3.75) -- (2.165,4) -- (1.732,3.75) -- (1.732,3.25) -- (1.732,3.25) -- (1.299,3) -- (1.299,2.5) -- (1.732, 2.25) -- (1.732, 1.75);
    \end{tikzpicture}
    </script>
</div>

Using this grouping we can construct the following sum for the number of possible sets of paths from each corner to a given vertex.

$$s(n) = \sum_{k_1 + k_2 + k_3 = n-1} \binom{k_1 + k_2}{k_1}\binom{k_2+k_3}{k_2}\binom{k_3+k_1}{k_3}$$

Notice that this grouping, and consequently $s(n)$, will count certain polyforms multiple times. An example of a polyform that is counted multiple times is shown in the figure below.

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\lablnode}[3]{\node[shape=circle,draw=white,fill=white, inner sep=0pt,minimum size=2pt] (A) at ( #1 , #2 ) {(#3)};}
    % actual image
    \begin{tikzpicture}[scale=1.5]
        %polyform
        \filldraw[gray!40] (0,0.25) -- (0.433,0) -- (0.866,0.25) -- (0.866,0.75) -- (0.433,1) -- (0,0.75) -- (0,0.25);

        \filldraw[gray!40] (0.866,0.25) -- (1.299,0) -- (1.732,0.25) -- (1.732,0.75) -- (1.299,1) -- (0.866,0.75) -- (0.866,0.25);

        \filldraw[gray!40] (3.464,0.25) -- (3.897,0) -- (4.33,0.25) -- (4.33,0.75) -- (3.897,1) -- (3.464,0.75) -- (3.464,0.25);

        \filldraw[gray!40] (1.299,1) -- (1.732,0.75) -- (2.165,1) -- (2.165,1.5) -- (1.732,1.75) -- (1.299,1.5) -- (1.299,1);
        \filldraw[gray!40] (2.165,1) -- (2.598,0.75) -- (3.031,1) -- (3.031,1.5) -- (2.598,1.75) -- (2.165,1.5) -- (2.165,1);
        \filldraw[gray!40] (3.031,1) -- (3.464,0.75) -- (3.897,1) -- (3.897,1.5) -- (3.464,1.75) -- (3.031,1.5) -- (3.031,1);
        \filldraw[gray!40] (1.732,1.75) -- (2.165,1.5) -- (2.598,1.75) -- (2.598,2.25) -- (2.165,2.5) -- (1.732,2.25) -- (1.732,1.75);
        \filldraw[gray!40] (1.299,2.5) -- (1.732,2.25) -- (2.165,2.5) -- (2.165,3) -- (1.732,3.25) -- (1.299,3) -- (1.299,2.5);
        \filldraw[gray!40] (1.732,3.25) -- (2.165,3) -- (2.598,3.25) -- (2.598,3.75) -- (2.165,4) -- (1.732,3.75) -- (1.732,3.25);

        %row 1
        \draw[thick] (0,0.25) -- (0.433,0) -- (0.866,0.25) -- (0.866,0.75) -- (0.433,1) -- (0,0.75) -- (0,0.25);
        \draw[thick] (0.866,0.25) -- (1.299,0) -- (1.732,0.25) -- (1.732,0.75) -- (1.299,1) -- (0.866,0.75) -- (0.866,0.25);
        \draw[thick] (1.732,0.25) -- (2.165,0) -- (2.598,0.25) -- (2.598,0.75) -- (2.165,1) -- (1.732,0.75) -- (1.732,0.25);
        \draw[thick] (2.598,0.25) -- (3.031,0) -- (3.464,0.25) -- (3.464,0.75) -- (3.031,1) -- (2.598,0.75) -- (2.598,0.25);
        \draw[thick] (3.464,0.25) -- (3.897,0) -- (4.33,0.25) -- (4.33,0.75) -- (3.897,1) -- (3.464,0.75) -- (3.464,0.25);

        %row 2
        \draw[thick] (0.433,1) -- (0.866,0.75) -- (1.299,1) -- (1.299,1.5) -- (0.866,1.75) -- (0.433,1.5) -- (0.433,1);
        \draw[thick] (1.299,1) -- (1.732,0.75) -- (2.165,1) -- (2.165,1.5) -- (1.732,1.75) -- (1.299,1.5) -- (1.299,1);
        \draw[thick] (2.165,1) -- (2.598,0.75) -- (3.031,1) -- (3.031,1.5) -- (2.598,1.75) -- (2.165,1.5) -- (2.165,1);
        \draw[thick] (3.031,1) -- (3.464,0.75) -- (3.897,1) -- (3.897,1.5) -- (3.464,1.75) -- (3.031,1.5) -- (3.031,1);

        %row 3
        \draw[thick] (0.866,1.75) -- (1.299,1.5) -- (1.732,1.75) -- (1.732,2.25) -- (1.299,2.5) -- (0.866,2.25) -- (0.866,1.75);
        \draw[thick] (1.732,1.75) -- (2.165,1.5) -- (2.598,1.75) -- (2.598,2.25) -- (2.165,2.5) -- (1.732,2.25) -- (1.732,1.75);
        \draw[thick] (2.598,1.75) -- (3.031,1.5) -- (3.464,1.75) -- (3.464,2.25) -- (3.031,2.5) -- (2.598,2.25) -- (2.598,1.75);

        %row 4
        \draw[thick] (1.299,2.5) -- (1.732,2.25) -- (2.165,2.5) -- (2.165,3) -- (1.732,3.25) -- (1.299,3) -- (1.299,2.5);
        \draw[thick] (2.165,2.5) -- (2.598,2.25) -- (3.031,2.5) -- (3.031,3) -- (2.598,3.25) -- (2.165,3) -- (2.165,2.5);

        %row 5
        \draw[thick] (1.732,3.25) -- (2.165,3) -- (2.598,3.25) -- (2.598,3.75) -- (2.165,4) -- (1.732,3.75) -- (1.732,3.25);
    \end{tikzpicture}
    </script>
</div>

All polyforms that are grouped multiple ways contains the following substructure.

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\lablnode}[3]{\node[shape=circle,draw=white,fill=white, inner sep=0pt,minimum size=2pt] (A) at ( #1 , #2 ) {(#3)};}
    % actual image
    \begin{tikzpicture}[scale=1.5]
        %polyform
        \filldraw[gray!40] (0,0.25) -- (0.433,0) -- (0.866,0.25) -- (0.866,0.75) -- (0.433,1) -- (0,0.75) -- (0,0.25);
        \filldraw[gray!40]  (0.866,0.25) -- (1.299,0) -- (1.732,0.25) -- (1.732,0.75) -- (1.299,1) -- (0.866,0.75) -- (0.866,0.25);
        \filldraw[gray!40] (0.433,1) -- (0.866,0.75) -- (1.299,1) -- (1.299,1.5) -- (0.866,1.75) -- (0.433,1.5) -- (0.433,1);

        %row 1
        \draw[thick] (0,0.25) -- (0.433,0) -- (0.866,0.25) -- (0.866,0.75) -- (0.433,1) -- (0,0.75) -- (0,0.25);
        \draw[thick] (0.866,0.25) -- (1.299,0) -- (1.732,0.25) -- (1.732,0.75) -- (1.299,1) -- (0.866,0.75) -- (0.866,0.25);

        %row 2
        \draw[thick] (0.433,1) -- (0.866,0.75) -- (1.299,1) -- (1.299,1.5) -- (0.866,1.75) -- (0.433,1.5) -- (0.433,1);

    \end{tikzpicture}
    </script>
</div>

Polyforms with these substructures are counted $3$ separate times. Fortunately, the number of polyforms that contain this substructure are easy to count, after the following transformation is made.

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\lablnode}[3]{\node[shape=circle,draw=white,fill=white, inner sep=0pt,minimum size=2pt] (A) at ( #1 , #2 ) {#3};}
    % actual image
    \begin{tikzpicture}[scale=1.5]
        \filldraw[gray!40] (0,0.25) -- (0.433,0) -- (0.866,0.25) -- (0.866,0.75) -- (0.433,1) -- (0,0.75) -- (0,0.25);

        \filldraw[gray!40] (0.866,0.25) -- (1.299,0) -- (1.732,0.25) -- (1.732,0.75) -- (1.299,1) -- (0.866,0.75) -- (0.866,0.25);

        \filldraw[gray!40] (3.464,0.25) -- (3.897,0) -- (4.33,0.25) -- (4.33,0.75) -- (3.897,1) -- (3.464,0.75) -- (3.464,0.25);

        \filldraw[gray!95] (1.299,1) -- (1.732,0.75) -- (2.165,1) -- (2.165,1.5) -- (1.732,1.75) -- (1.299,1.5) -- (1.299,1);
        \filldraw[gray!95] (2.165,1) -- (2.598,0.75) -- (3.031,1) -- (3.031,1.5) -- (2.598,1.75) -- (2.165,1.5) -- (2.165,1);
        \filldraw[gray!40] (3.031,1) -- (3.464,0.75) -- (3.897,1) -- (3.897,1.5) -- (3.464,1.75) -- (3.031,1.5) -- (3.031,1);
        \filldraw[gray!95] (1.732,1.75) -- (2.165,1.5) -- (2.598,1.75) -- (2.598,2.25) -- (2.165,2.5) -- (1.732,2.25) -- (1.732,1.75);
        \filldraw[gray!40] (1.299,2.5) -- (1.732,2.25) -- (2.165,2.5) -- (2.165,3) -- (1.732,3.25) -- (1.299,3) -- (1.299,2.5);
        \filldraw[gray!40] (1.732,3.25) -- (2.165,3) -- (2.598,3.25) -- (2.598,3.75) -- (2.165,4) -- (1.732,3.75) -- (1.732,3.25);
        
        %row 1
        \draw[thick] (0,0.25) -- (0.433,0) -- (0.866,0.25) -- (0.866,0.75) -- (0.433,1) -- (0,0.75) -- (0,0.25);
        \draw[thick] (0.866,0.25) -- (1.299,0) -- (1.732,0.25) -- (1.732,0.75) -- (1.299,1) -- (0.866,0.75) -- (0.866,0.25);
        \draw[thick] (1.732,0.25) -- (2.165,0) -- (2.598,0.25) -- (2.598,0.75) -- (2.165,1) -- (1.732,0.75) -- (1.732,0.25);
        
        \draw[thick] (2.598,0.25) -- (3.031,0) -- (3.464,0.25) -- (3.464,0.75) -- (3.031,1) -- (2.598,0.75) -- (2.598,0.25);
        \draw[thick] (3.464,0.25) -- (3.897,0) -- (4.33,0.25) -- (4.33,0.75) -- (3.897,1) -- (3.464,0.75) -- (3.464,0.25);
        
        %row 2
        \draw[thick] (0.433,1) -- (0.866,0.75) -- (1.299,1) -- (1.299,1.5) -- (0.866,1.75) -- (0.433,1.5) -- (0.433,1);
        \draw[thick] (1.299,1) -- (1.732,0.75) -- (2.165,1) -- (2.165,1.5) -- (1.732,1.75) -- (1.299,1.5) -- (1.299,1);
        \draw[thick] (2.165,1) -- (2.598,0.75) -- (3.031,1) -- (3.031,1.5) -- (2.598,1.75) -- (2.165,1.5) -- (2.165,1);
        \draw[thick] (3.031,1) -- (3.464,0.75) -- (3.897,1) -- (3.897,1.5) -- (3.464,1.75) -- (3.031,1.5) -- (3.031,1);
        
        %row 3
        \draw[thick] (0.866,1.75) -- (1.299,1.5) -- (1.732,1.75) -- (1.732,2.25) -- (1.299,2.5) -- (0.866,2.25) -- (0.866,1.75);
        \draw[thick] (1.732,1.75) -- (2.165,1.5) -- (2.598,1.75) -- (2.598,2.25) -- (2.165,2.5) -- (1.732,2.25) -- (1.732,1.75);
        \draw[thick] (2.598,1.75) -- (3.031,1.5) -- (3.464,1.75) -- (3.464,2.25) -- (3.031,2.5) -- (2.598,2.25) -- (2.598,1.75);
        
        %row 4
        \draw[thick] (1.299,2.5) -- (1.732,2.25) -- (2.165,2.5) -- (2.165,3) -- (1.732,3.25) -- (1.299,3) -- (1.299,2.5);
        \draw[thick] (2.165,2.5) -- (2.598,2.25) -- (3.031,2.5) -- (3.031,3) -- (2.598,3.25) -- (2.165,3) -- (2.165,2.5);
        
        %row 5
        \draw[thick] (1.732,3.25) -- (2.165,3) -- (2.598,3.25) -- (2.598,3.75) -- (2.165,4) -- (1.732,3.75) -- (1.732,3.25);

        % arrow
        \( \lablnode{5.2}{2}{$\pmb{\to}$} \)

        %polyform
        \filldraw[gray!40] (6,0.25) -- (6.433,0) -- (6.866,0.25) -- (6.866,0.75) -- (6.433,1) -- (6,0.75) -- (6,0.25);
        \filldraw[gray!40] (6.866,0.25) -- (7.299,0) -- (7.732,0.25) -- (7.732,0.75) -- (7.299,1) -- (6.866,0.75) -- (6.866,0.25);
        \filldraw[gray!40] (8.598,0.25) -- (9.031,0) -- (9.464,0.25) -- (9.464,0.75) -- (9.031,1) -- (8.598,0.75) -- (8.598,0.25);
        \filldraw[gray!95] (7.299,1) -- (7.732,0.75) -- (8.165,1) -- (8.165,1.5) -- (7.732,1.75) -- (7.299,1.5) -- (7.299,1);
        \filldraw[gray!40] (8.165,1) -- (8.598,0.75) -- (9.031,1) -- (9.031,1.5) -- (8.598,1.75) -- (8.165,1.5) -- (8.165,1);
        \filldraw[gray!40] (6.866,1.75) -- (7.299,1.5) -- (7.732,1.75) -- (7.732,2.25) -- (7.299,2.5) -- (6.866,2.25) -- (6.866,1.75);
        \filldraw[gray!40] (7.299,2.5) -- (7.732,2.25) -- (8.165,2.5) -- (8.165,3) -- (7.732,3.25) -- (7.299,3) -- (7.299,2.5);

        %row 1
        \draw[thick] (6,0.25) -- (6.433,0) -- (6.866,0.25) -- (6.866,0.75) -- (6.433,1) -- (6,0.75) -- (6,0.25);
        \draw[thick] (6.866,0.25) -- (7.299,0) -- (7.732,0.25) -- (7.732,0.75) -- (7.299,1) -- (6.866,0.75) -- (6.866,0.25);
        \draw[thick] (7.732,0.25) -- (8.165,0) -- (8.598,0.25) -- (8.598,0.75) -- (8.165,1) -- (7.732,0.75) -- (7.732,0.25);
        \draw[thick] (8.598,0.25) -- (9.031,0) -- (9.464,0.25) -- (9.464,0.75) -- (9.031,1) -- (8.598,0.75) -- (8.598,0.25);
        
        %row 2
        \draw[thick] (6.433,1) -- (6.866,0.75) -- (7.299,1) -- (7.299,1.5) -- (6.866,1.75) -- (6.433,1.5) -- (6.433,1);
        \draw[thick] (7.299,1) -- (7.732,0.75) -- (8.165,1) -- (8.165,1.5) -- (7.732,1.75) -- (7.299,1.5) -- (7.299,1);
        \draw[thick] (8.165,1) -- (8.598,0.75) -- (9.031,1) -- (9.031,1.5) -- (8.598,1.75) -- (8.165,1.5) -- (8.165,1);
        
        %row 3
        \draw[thick] (6.866,1.75) -- (7.299,1.5) -- (7.732,1.75) -- (7.732,2.25) -- (7.299,2.5) -- (6.866,2.25) -- (6.866,1.75);
        \draw[thick] (7.732,1.75) -- (8.165,1.5) -- (8.598,1.75) -- (8.598,2.25) -- (8.165,2.5) -- (7.732,2.25) -- (7.732,1.75);
        
        %row 4
        \draw[thick] (7.299,2.5) -- (7.732,2.25) -- (8.165,2.5) -- (8.165,3) -- (7.732,3.25) -- (7.299,3) -- (7.299,2.5);
    \end{tikzpicture}
    </script>
</div>

Therefore the number of polyforms that contain this substructure is $s(n-1)$.  This, and the following identity

$$\sum_{k_1 + k_2 + k_3 = n} \binom{k_1 + k_2}{k_1}\binom{k_2+k_3}{k_2}\binom{k_3+k_1}{k_3} = \sum_{k=0}^n \binom{2k}{k}$$

completes the proof, as 

$$\rho(\triangle^{H*}_n) =s(n) - 2s(n-1) = \sum_{k=0}^{n-1} \binom{2k}{k} - 2\sum_{k=0}^{n-2} \binom{2k}{k} = \binom{2(n-1)}{n-1} - \sum_{k=0}^{n-2}\binom{2k}{k}.$$

## Trivial Minimal Inscribed Polyforms

Strangely, some families do not have a strictly increasing number of minimal inscribed polyforms in $n$. A family of labelled duals with this property is referred to as a *trivial* family. Inspired by the Article Circle Theorem, I really wanted to enumerate minimal inscribed polyforms inscribed in the [Aztec diamond](https://en.wikipedia.org/wiki/Aztec_diamond). An Aztec diamond can have a length $n$ and a width $m$, where the length and height are the number of squares on the respective sides. An example of an $n,m=3$ Aztec diamond is shown below.

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\cell}[4]{\filldraw[gray!40] ( #1 , #2 ) rectangle ( #3 , #4 ); \draw[thick] ( #1 , #2 ) rectangle ( #3 , #4 );}    
    \newcommand{\cellw}[4]{\draw[thick] ( #1 , #2 ) rectangle ( #3 , #4 );}
    \newcommand{\lablnode}[3]{\node[shape=circle,draw=white,fill=white, inner sep=0pt,minimum size=2pt] (A) at ( #1 , #2 ) {#3};}
    % actual image
    \begin{tikzpicture}[scale=1.5]
        \( \cellw{1}{0}{1.5}{0.5} \);
        \( \cellw{1.5}{0}{2}{0.5} \);
        
        \( \cellw{0.5}{0.5}{1}{1} \);
        \( \cellw{1}{0.5}{1.5}{1} \);
        \( \cellw{1.5}{0.5}{2}{1} \);
        \( \cellw{2}{0.5}{2.5}{1} \);
        
        \( \cellw{0}{1}{0.5}{1.5} \);
        \( \cellw{0.5}{1}{1}{1.5} \);
        \( \cellw{1}{1}{1.5}{1.5} \);
        \( \cellw{1.5}{1}{2}{1.5} \);
        \( \cellw{2}{1}{2.5}{1.5} \);
        \( \cellw{2.5}{1}{3}{1.5} \);
        
        \( \cellw{0}{1.5}{0.5}{2} \);
        \( \cellw{0.5}{1.5}{1}{2} \);
        \( \cellw{1}{1.5}{1.5}{2} \);
        \( \cellw{1.5}{1.5}{2}{2} \);
        \( \cellw{2}{1.5}{2.5}{2} \);
        \( \cellw{2.5}{1.5}{3}{2} \);
        
        \( \cellw{0.5}{2}{1}{2.5} \);
        \( \cellw{1}{2}{1.5}{2.5} \);
        \( \cellw{1.5}{2}{2}{2.5} \);
        \( \cellw{2}{2}{2.5}{2.5} \);
        
        \( \cellw{1}{2.5}{1.5}{3} \);
        \( \cellw{1.5}{2.5}{2}{3} \);
    \end{tikzpicture}
    </script>
</div>

I was surprised that the Aztec diamond was a trivial family, only having $4$ minimal inscribed polyforms when $n=m$ for any $n \geq 2$, namely the below polyform and its rotations. 

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\cell}[4]{\filldraw[gray!40] ( #1 , #2 ) rectangle ( #3 , #4 ); \draw[thick] ( #1 , #2 ) rectangle ( #3 , #4 );}    
    \newcommand{\cellw}[4]{\draw[thick] ( #1 , #2 ) rectangle ( #3 , #4 );}
    \newcommand{\lablnode}[3]{\node[shape=circle,draw=white,fill=white, inner sep=0pt,minimum size=2pt] (A) at ( #1 , #2 ) {#3};}
    % actual image
    \begin{tikzpicture}[scale=1.5]
        \( \cellw{1}{0}{1.5}{0.5} \);
        \( \cellw{1.5}{0}{2}{0.5} \);
        
        \( \cellw{0.5}{0.5}{1}{1} \);
        \( \cellw{1}{0.5}{1.5}{1} \);
        \( \cellw{1.5}{0.5}{2}{1} \);
        \( \cellw{2}{0.5}{2.5}{1} \);
        
        \( \cell{0}{1}{0.5}{1.5} \);
        \( \cell{0.5}{1}{1}{1.5} \);
        \( \cell{1}{1}{1.5}{1.5} \);
        \( \cell{1.5}{1}{2}{1.5} \);
        \( \cell{2}{1}{2.5}{1.5} \);
        \( \cell{2.5}{1}{3}{1.5} \);
        
        \( \cell{0}{1.5}{0.5}{2} \);
        \( \cellw{0.5}{1.5}{1}{2} \);
        \( \cellw{1}{1.5}{1.5}{2} \);
        \( \cellw{1.5}{1.5}{2}{2} \);
        \( \cellw{2}{1.5}{2.5}{2} \);
        \( \cell{2.5}{1.5}{3}{2} \);
        
        \( \cellw{0.5}{2}{1}{2.5} \);
        \( \cellw{1}{2}{1.5}{2.5} \);
        \( \cellw{1.5}{2}{2}{2.5} \);
        \( \cellw{2}{2}{2.5}{2.5} \);
        
        \( \cellw{1}{2.5}{1.5}{3} \);
        \( \cellw{1.5}{2.5}{2}{3} \);
    \end{tikzpicture}
    </script>
</div>

I was really disappointed that this family was trivial, so I thought about what I could do to get around this. Then I realized: what if I just made the corners count as adjacent too! 

## My White Whale Problem

Because we were dealing with unit squares connected by adjacent edges and adjacent corners, I decided to call these objects *minimal inscribed pseudopolyominos*. Here is an example of one of these polyominos.

<div align=center>
    <script type="text/tikz">
    % setup
    \newcommand{\cell}[4]{\filldraw[gray!40] ( #1 , #2 ) rectangle ( #3 , #4 ); \draw[thick] ( #1 , #2 ) rectangle ( #3 , #4 );}    
    \newcommand{\cellw}[4]{\draw[thick] ( #1 , #2 ) rectangle ( #3 , #4 );}
    \newcommand{\lablnode}[3]{\node[shape=circle,draw=white,fill=white, inner sep=0pt,minimum size=2pt] (A) at ( #1 , #2 ) {#3};}
    % actual image
    \begin{tikzpicture}[scale=1.5]
        \( \cellw{1}{0}{1.5}{0.5} \);
        \( \cellw{1.5}{0}{2}{0.5} \);
        
        \( \cell{0.5}{0.5}{1}{1} \);
        \( \cellw{1}{0.5}{1.5}{1} \);
        \( \cellw{1.5}{0.5}{2}{1} \);
        \( \cellw{2}{0.5}{2.5}{1} \);
        
        \( \cellw{0}{1}{0.5}{1.5} \);
        \( \cell{0.5}{1}{1}{1.5} \);
        \( \cell{1}{1}{1.5}{1.5} \);
        \( \cellw{1.5}{1}{2}{1.5} \);
        \( \cell{2}{1}{2.5}{1.5} \);
        \( \cell{2.5}{1}{3}{1.5} \);
        
        \( \cell{0}{1.5}{0.5}{2} \);
        \( \cellw{0.5}{1.5}{1}{2} \);
        \( \cellw{1}{1.5}{1.5}{2} \);
        \( \cell{1.5}{1.5}{2}{2} \);
        \( \cellw{2}{1.5}{2.5}{2} \);
        \( \cellw{2.5}{1.5}{3}{2} \);
        
        \( \cellw{0.5}{2}{1}{2.5} \);
        \( \cellw{1}{2}{1.5}{2.5} \);
        \( \cellw{1.5}{2}{2}{2.5} \);
        \( \cell{2}{2}{2.5}{2.5} \);
        
        \( \cellw{1}{2.5}{1.5}{3} \);
        \( \cellw{1.5}{2.5}{2}{3} \);
    \end{tikzpicture}
    </script>
</div>

I knew this enumeration would be brutal. And boy was I right. I didn't get anywhere close before my thesis deadline, and it so this problem sat in my notebook for over a year. Over that year, the problem  achieved a sort of white whale status in my mind, being the problem I knew existed but couldn't conquer. 

That was until, I submitted my applications for graduate school. I had this brief period of reprieve between applications and restarting my studies that I knew I needed to use for this problem. I dedicated *many* nights to working on the myriad cases these pseudopolyforms exhibit. I wrote hundreds of lines of SageMath to confirm my enumeration results, and finally (finally) solved it. If we denote $\rho(\diamond_{n,m})$ as the number of minimal inscribed pseudopolyforms in the generalized aztec diamond, then we have the following generating function. 

$$\begin{eqnarray*}
    \sum_{n,m\geq 0} \rho(\diamond_{n,m})x^n y^m & = &  xy(2 \, x^{5} y - 11 \, x^{4} y^{2} + 9 \, x^{3} y^{3} - 11 \, x^{2} y^{4} + 2 \, x y^{5} \\
    & - & 10 \, x^{4} y + 16 \, x^{3} y^{2} + 16 \, x^{2} y^{3} - 10 \, x y^{4}\\
    & + & x^{4} + 15 \, x^{3} y - 21 \, x^{2} y^{2} + 15 \, x y^{3} + y^{4} - 8 \, x^{2} y - 8 \, x y^{2} \\
    & - & 4 \, x^{2} + 5 \, x y - 4 \, y^{2} + 2 \, x + 2 \, y + 1 ) \\
    & / & {\left(1 - 2 \, x - 2 \, y + x^{2} + x y + y^{2} \right)} {\left(1-x\right)}^{4} {\left(1-y\right)}^{4}
\end{eqnarray*}$$

It's hard to describe how much time this equation took to arrive at. All I can think of when I see this equation is:

"Look on my works, ye Mighty, and despair!"

After this I decided I would not be enumerating any more minimal inscribed polyforms. 

## Further Questions

1. Is there an overarching theorem for enumerating the minimal inscribed polyforms in a given labelled graph $G$?

Thank you for reading!
