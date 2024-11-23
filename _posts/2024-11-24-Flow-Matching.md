---
tags: notes cp math
---

## A Collection of Flow and Matching Results from Scratch

I was reading Ramsey Theory $[1]$. In chapter 1.6, they had a section on corrolary of Ramsey's Theorem. 

Let $D(a+1,b+1)$ denote the minimium size of a poset such that there it is guaranteed that we can either find a chain of size $a+1$ and an anti-chain of size $b+1$.

Let $ES(a+1,b+1)$ denote the minimum size of a distinct array of **distinct** integers such that it is guaranteed that we can either LIS is at least $a+1$ or LDS is at least $b+1$.

But using Ramsey's Theorem, we can show that $D(a+1,b+1), ES(a+1,b+1) \leq R(a+1,b+1)$, which is a very silly corollary of Ramsey's Theorem. We know that from Dilworth's Theorem and Erdős–Szekeres Theorem that $ES(a+1,b+1) = D(a+1,b+1) = ab+1$.

Dilworth's theorem implies $D(a+1,b+1) \leq ab+1$. Suppose that there the maximum chain and anti-chain sizes are $a$ and $b$ respectively. Then, by Dilwroth's theorem, there is a chain decomposition of $b$ disjoint chains. Each chain has $a$ elements. So there are at most $ab$ elements in the poset. 

We also have $ES(a+1,b+1) \leq D(a+1,b+1)$. For the array $A$, the poset $(S,\leq)$ is defined as $u \leq_S v \iff (u \leq v \land A_u \leq A_v)$. A chain is a increasing subsequence. An anti-chain is a decreasing subsequence.

Actually, these inequalities are tight, because it is trivial to find a example to show that $ab < ES(a+1,b+1)$. Geometrically, take a $a \times b$ rectangle grid of points and rotate it slightly.

Anyways, until now, the only proof I knew of Erdős–Szekeres was by the pigeonhole argument given in $[2]$. But now I knew 2 more proofs. One was given above by using Dilworth and the other one was given when I read the original paper where it was proven in $[3]$. I read $[3]$ because it was the paper where Erdős–Szekeres showed the generalization of Happy Ending Problem had a bound of $\binom{2n-4}{n-2}+1$.

Anyways, I realized I didn't know how to prove Dilworth so here is what I learnt from going down this rabbit hole... Here are some references and the appropriate things to read.

### Ford-Fulkerson

See $[4]$.

### Max-Flow Min-Cut Duality

See $[5]$, one should understand the constructive algorithm of generating the minimum cut from the residual graph.

We will now discuss some common graph reductions used in the competitive programming context. I have been told by a lot of old people that in the heydays of Topcoder it was flow meta. Those old people will obviously be really good at flow modelling since it was necessary but now I feel like a complete noob at flow compared to them. Unfortunately those heydays are over and flow barely appears in competitive programming. Unfortunately I don't know how to use Topcoder to access their wealth of flow problems...

### Project Selection Problem

The project selection problem described in $[6]$.

One should note that we can allow for negative costs in the project selection. If we have a negative profit project, there is no case where we would to do the project. If we have a negative cost machine, we should always buy the machine. So it is trivially handled with some if cases.

Note that putting negative weights in the flows don't work (negative weights means if $u \to v$ has cost $-c$, then we instead add $v \to u$ with cost $c$). Suppose we have a project with profit of $1$ and a machine with a cost of $-1$ (we get money from buying it). Then our maximum flow is $0$ since there are no edges pointing to the sink. So we would think our maximum profit is $1$ instead of $2$.

#### [ARC085C](https://atcoder.jp/contests/arc085/tasks/arc085_c)

Using project selection, projects are the negative $a_i$ (we get profit from smashing) and the machines are the positive $a_i$ (we get loss from smashing). Then the dependency is that if project $i$ is smashed, then machine $ik$ must also be smashed.

Now, this correlation isn't really one-to-one as *technically* our set up doesn't always smash all gems that are multiples of $i$ when we smash gem $i$.

Take the array $[-1,2,-1,1]$.

If we only take project $1$, then our only constraint is that we must buy machine $2$ and $4$. But I guess this can be fixed by saying that we can always get project $3$ for free.

#### [ABC225G](https://atcoder.jp/contests/abc225/tasks/abc225_g)

Using project selection, the projects are that we can save $C$ cost by placing `X` on both machines $(i,j)$ and $(i+1,j+1)$ or on both machines $(i,j)$ and $(i+1,j-1)$. 

So the projects all have cost $C$ and the machine $(i,j)$ have cost $2C-A_{i,j}$ instead. Note that $2C-A_{i,j}$ might be negative, which we can handle by the above discussion about project selection with negative weights.

####  [ABC259](https://atcoder.jp/contests/abc259/tasks/abc259_g)

We can actually think about this as a sort of generalization of the project selection problem. We can transform the project selection problem into this grid card game as follows.

Let there be $n$ projects and $m$ machines. Then we will have a grid of size $(n+1) \times (m+1)$. Put $A_{n+1,m+1} = -\infty$.

The profit of the $i$-th project $p_i$ will be in cell $A_{i,m+1}$ and the cost of the $j$-th machine $c_j$ will be in cell $A_{n+1,j}$.

Then for $1 \leq i \leq n$ and $1 \leq j \leq m$, we have $A_{i,j} = -\varepsilon$ if the $i$-th project depends on the $j$-th machine and $A_{i,j}=0$ otherwise.

Then the correlation is as such:

- If we do the $i$-th project, Takahashi places a red token on the $i$-th row.
- If we do **not** buy the $j$-th machine, Aoki places a blue token on the $j$-th row.

Now if the answer of this grid card game $A$, then the maximum profit from project selection is $A-\sum c_j$. In particular, we can obtain $\sum c_j$ cost by Aoki playing blue token on all rows except row $m+1$.

Now, if we want to make this analogy more general, we will want to account for those $1 \leq i \leq n$ and $1 \leq j \leq m$ such that $0 \leq A_{i,j}$. Given a grid game of size $(n+1) \times (m+1)$ where $A_{n+1,m+1} = -\infty$, this is how we can inteprete this grid game into a general form of the project selection problem.

There $n$ projects with the $i$-th project giving you profit of $p_i = \sum A_{i,\star}$. There are $m$ machines with the $j$-th machine has a cost of $c_j = \sum A_{\star,j}$. Then for a pair of projects and machines $(i,j)$ there are two cases:

- If $A_{i,j}<0$, The project $i$ must buy machine $j$
- If $A_{i,j} \geq 0$, For the project $i$, if we don't buy machine $j$, we lose $A_{i,j}$ money

We note that the flow network constructed for this problem is very similar to the project selection problem. In future problems, if we are able to convert problems either to generalized project selection or this grid game problem, that means we can attack it using min cuts!

### Binary String with Constraints

Consider the following problem. You need to construct a binary string $S$ of length $n$, the cost of the binary string is defined as followed:

- If $S_i = \texttt{0}$, you get $A_i$ penalty
- If $S_i = \texttt{1}$, you get $B_i$ penalty
- If $S_i = \texttt{0}$ and $S_j = \texttt{1}$, you get $C_{i,j}$ penalty

Here the problem does not change if we add all a constant to $A_i$ and $B_i$. So we can safely assume that $A_i$ and $B_i$ are positive. But unfortunately, we need to force that our problem has $C_{i,j}$ being positive as I don't know how to handle it when it is negative.

Then we construct the following flow graph:

- $s \to i$ with cost $A_i$
- $i \to j$ with cost $C_{i,j}$
- $i \to t$ with cost $B_i$

Then our min cut corresponds to the answer to our problem. I was wondering why it is never the case that our min cut cuts both edges $s \to i$ and $i \to t$ for the same $i$. I was stuff thinking about min-cut in terms of the edges that we cut. Instead, we should think of the min-cut in terms of partitioning of the flow graph into $S$ and $T$ where we only take the edges directed from $S \to T$. So from this way, it is clear that we only cut one of $s \to i$ and $i \to t$ depending on whether $i \in T$ or $i \in S$.

#### [ABC225G](https://atcoder.jp/contests/abc225/tasks/abc225_g) (Again)

Our original problem transfers in the following way: $S_{(i,j)} = \texttt{0}$ if we do not place $\texttt{X}$ on the tile $(i,j)$. Then $A$ in the defined is the definition we want alongside with $B_{(i,j)}=0$.

For convenience, let the grid be expanded for convinience and fill the other tiles with $A = 0$ and $B = \infty$ so that we are forced to not place $\texttt{X}$ on those tiles.

We also have the penalty $C_{(i+1,j+1),(i,j)} = C$ and $C_{(i+1,j-1),(i,j)} = C$.

#### [ABC193F](https://atcoder.jp/contests/abc193/tasks/abc193_f)

We want to change this problem into one where we are minimising penalty. We will have penalty when two adjacent cells are the same color.

Unfortunately, this does not really correspond with our general condition of "If $S_i = \texttt{0}$ and $S_j = \texttt{1}$, you get $C_{i,j}$ penalty". The trick here is to flip cells $(i,j)$ where $i+j$ has odd parity, then we have have penalty when two adjacent cells are different color.

If we want to force $S_{(i,j)}$ to be $\texttt{0}$, then we set $B_{(i,j)} = \infty$ and if we want to force $S_{(i,j)}$ to be $\texttt{1}$, then we set $A_{(i,j)} = \infty$. Then the penalty on adjacent cells being different color becomes $C_{(i,j),(i \pm 1, j \pm 1)} = 1$. 

#### [ARC142E](https://atcoder.jp/contests/arc142/tasks/arc142_e)

First, an obvious constraint is that for all pairs $(x,y)$, we have $a_x,a_y \geq \min(b_x,b_y)$. Let $mn_i$ denote the minimum value of $a_i$ under this constraint.

Now notice that $a_i$ and $b_i$ are small. For the indices of our binary string above, we can have $S_{(i,v)} = \texttt{1}$ iff $a_i \geq v$.

This is reasonable but now the constraints are of the form $\max(a_x,a_y) \geq \max(b_x,b_y)$. So again, it does not correspond with our general condition of  "If $S_i = \texttt{0}$ and $S_j = \texttt{1}$, you get $C_{i,j}$ penalty". Luckily, we can do the same trick of flipping the meaning of $S$ for some $(i,v)$.

Let $X$ be the set of wizards such that for all pairs $(x,y)$, we have $b_y < b_x$. Let $Y$ be the other wizard. For all wizards in $Y$, we can deduce that $a_i \geq b_i$, since there exists some pair $(x,y)$ such that $b_x \geq b_y$ so that $a_y \geq \min(b_x,b_y) = b_y$.

Now, the extra constraint that $\max(a_x,a_y) \geq \max(b_x,b_y)$ ends up being needed only for pairs where one is in $X$ and the other is in $Y$. There are no pairs that are both in $X$. And all pairs in $Y$ automatically fufil our additional constraint.

Therefore, we can flip the meaning of $S$ for those wizards in $Y$.

Then we can define the variable for our binary string constraint problem:

- If $i \in X$, $S_{(i,v)} = \texttt{1}$ iff $a_i \geq v$
- If $i \in Y$, $S_{(i,v)} = \texttt{0}$ iff $a_i \geq v$

Now, our constraints are as follows:

- If $i \in X$, then $S_{(i,v)}=\texttt{1}$ implies $S_{(i,v-1)} = \texttt{1}$, so there is a penalty $C_{(i,v-1),(i,v)}=\infty$
- If $i \in Y$, then $S_{(i,v)}=\texttt{0}$ implies $S_{(i,v-1)} = \texttt{0}$, so there is a penalty $C_{(i,v),(i,v-1)}=\infty$
- If $i \in X$, then $S_{(i,v)} =\texttt{1}$ will give us a penalty of $B_{(i,v)}=1$ if $v > a_i$
- If $i \in Y$, then $S_{(i,v)} =\texttt{0}$ will give us a penalty of $A_{(i,v)}=1$ if $v > a_i$
- If $i \in X$, then $S_{(i,mn_i)}=\texttt{0}$ will give us a penalty of $A_{(i,mn_i)}=\infty$
- If $i \in Y$, then $S_{(i,mn_i)}=\texttt{1}$ will give us a penalty of $B_{(i,mn_i)}=\infty$
- Finally if $(x,y) \in (X,Y)$ is a pair, then we have the constraint $\max(a_x,a_y) \geq b_y \iff \neg (a_x<b_y \land a_y < b_y)$ which is represented by $C_{(x,b_y),(y,b_u)} = \infty$.

### Calculating Max-Flow using Min-Cut

In the reverse of the section above, we sometimes flow graph where we are interested about the flow through the graph but computing the min-cut using ad-hoc techniques is easier.

However, there are some instances where we are interested in computing the maximum flow but computing the minimum cut is simpler. I have seen this idea in<https://atcoder.jp/contests/arc125/tasks/arc125_e>, <https://codeforces.com/contest/724/problem/E> and <https://codeforces.com/problemset/problem/1919/F2> only (which I coordinated). Are there any problems that require this technique?

### Gallai's Theorem

See $[7]$.

From here on, let write:

- $mVC$ as minimum vertex cover
- $MIS$ as maximum independent set
- $mEC$ as minimum edge cover
- $MM$ as maximum matching

Then we have $mVC + MIS = mEC + MM = V$ along with the inequality $MM \leq mVC$.

### Kőnig's Theorem

Kőnig's theorem makes the inequality $MM \leq mVC$ in Gallai's Theorem tight. Although there is a constructive proof without flows as in $[8]$, but I am convinced by Kulkov as in $[9]$ that the only morally correct way to think about Kőnig's thoerem is via min-cut max-flow. In fact, $[8]$ literally does the min cut construction algorithm but just uses ad-hoc method instead of saying it works because of the residual graph.

The construction is as follows. For a bipartite graph $(X,Y)$, defined two extra vertices $(s,t)$ for the flow network. Add the following edges:

- $s \to x$ with weight $1$
- $x \to y$ with weight $\infty$
- $y \to t$ with weight $1$

Then obviously, max flow is maximal matching and min cut is minimum vertex cover.

I want to note here that the diagrams shown in $[8]$ and $[9]$ *might* be misleading as there might be edges from $A_T$ to $B_S$ as desmonstrated below.

<center>
  <img src="/media/konig.png">
</center>

Now, with regards to our previous discussion on the project selection problem, we can note that this is the exact same problem if $X$ was projects and $Y$ were machines. The project selection problem then finds the set that maximizes $ \mid W \mid - \mid N_G(W) \mid$ for all $W \subseteq X$, which is the portion of the graph that causes us to waste $\mid W \mid - \mid N_G(W) \mid$ vertices on $X$. This segues right into Hall's Theorem.

### Hall's Theorem

We can give a proof of Hall's Theorem as a corollary of Kőnig's theorem. Let the bipartite graph have partition $(X,Y)$ and we want to find a perfect matching covering all $X$.

#### $\exists W \subseteq X, ~ \mid W\mid  > \mid  N_G(W) \mid  \to mVC < X$

Trivially no matching.

#### $mVC < X \to \exists W \subseteq X, ~ \mid W\mid  > \mid  N_G(W) \mid $

Suppose we have vertex cover $VC$ of size $mVC$ that is smaller than $X$. Then take $W = X \cap \overline{VC}$ and $Z = Y \cap VC$. We have $N_G(W) \subseteq Z$ or $VC$ is not a vertex cover. We also have $\mid Z\mid  < \mid W\mid$ since $\mid VC\mid <\mid X\mid$. Therefore, $\mid N_G(W) \mid < \mid W\mid$.

### Dilworth's Theorem

Now, we can prove that Dilworth's Theorem is equivalent to Kőnig's Theorem. Let $MAC$ and $mPP$ the maximum anti-chain and minimum path partition for a poset.

#### Dilworth's Theorem from Kőnig's Theorem

Suppose we have the poset $(S,\leq)$ on $n$ elements

Define a bipartite graph $G=(U,V,E)$ with $U=V=S$, then edge $(u,v)$ exists iff $u < v$

Let the $mPP$ and $MM$ denote the minimum path partition on $S$ and maximum matching on $G$ respectively.

$mPP \leq n-MM$: Suppose that there is edge $(u,v)$ in a maximal matching, then they will be in the same chain.

$MM \geq n-mPP$: Each chain represents a total ordering $v_1 < v_2 < \ldots < v_k$ and we add the edge $(v_i,v_{i+1})$ into our matching.

Therefore, $mPP=n-MM$

Now let $MAC$ and $mVC$ denote the maximum antichain on $S$ and the minimum vertex cover on $G$ respectively.

$MAC \geq n-mVC$: we can construct an antichain from the set of vertices that are not selected in both $U$ or $V$ in the minimum vertex cover.

$ mVC \leq n-MAC $: Let $X$ be a maximal antichain. Consider each element $s \in \overline X$. Define $L_s = \\{x \mid s < x\\}$ and $R_s = \\{x \mid x < s\\}$. If $L_s \cup R_s = \varnothing$, then we can add $s$ into $X$. If both $L_s \neq \varnothing$ and $R_s \neq \varnothing$, then we can find $x_1 < s < x_2$ which is absurd. Therefore, we can uniquely identify each element $s $ as either $\texttt{L}$ or $\texttt{R}$ depending on which of $L_s$ and $R_s$ is empty. To construct our vertex cover, place $s$ in $U$ or $V$ depending on whether it is $\texttt{L}$ or $\texttt{R}$. Now, we have to prove that this covers all edges $(u,v)$. If $u \in X$, then $v \in R_u$. Similarly if $v \in X$, then $u \in L_v$. Now suppose $u,v \notin X$, and that $u$ and $v$ are type $\texttt{R}$ and $\texttt{L}$ respectively. This implies $x_1<u<v<x_2$ which is absurd.

Therefore, $MAC=n-mVC$

Therefore, we have proved Dilworth's theorem that $mPP = MAC$

#### Kőnig's Theorem from Dilworth's Theorem

Suppose we have a bipartite graph $G=(U,V,E)$. Let $sz = \mid U\mid +\mid V\mid$

Define a poset $(S,\leq)$ with $S = U \cup V$ and $u < v $ if edge $(u,v)$ exists.

Let the $MM$ and $mPP$ denote the maximum matching on $G$ and the minimum path partition on $S$ respectively.

$mPP \leq sz-MM$: Suppose that there is edge $(u,v)$ in a maximal matching, then they will be in the same chain.

$MM \geq sz-mPP$: Each chain has either size $1$ or $2$. If it has size $2$ and of the form $u<v$, add $(u,v)$ into our matching.

Therefore, $MM = sz-mPP$

Now let $mVC$ and $MAC$ denote the minimum vertex cover on $G$ and the maximum antichain on $S$ respectively.

By taking complements of each other, it is easy to see that $mVC=sz-MAC$

Therefore, we have proved Kőnig's theorem that $MM = mVC$

### Berge's Theorem

See $[7]$ and $[10]$.

An $M$-augmenting path is a sequence of distinct vertices $v_1,v_2,\ldots,v_{2k}$ such that $v_1$ and $v_{2k}$ are unmatched and $v_{2i},v_{2i+1}$ are matched in $M$. We want to make clear that augmenting paths is only defined with respect to the matching $M$.

Berger's Theorem is that a matching $M$ is maximum iff there are no augmenting paths. See $[7]$.

The main idea here is to take two matchings $M$ and $M'$ and consider their symmetric difference $M \Delta M'$ (union also works I guess).

If we consider their symmetric difference, then all connected components are either cycles or paths. These both are alternating between edges in $M$ and $M'$.

Then to prove Berger's theorem, just suppose that $M'$ has more edges than $M$. Then in some connected component of $M \Delta M'$, there are more $M'$ edges than $M$ edges. This must be a path, so we found an augmenting path.

This is the basis of all matching algorithms.

### Bipartite Matching (Kuhn's Algorithm)

See $[7]$ and $[10]$.

Let $(X,Y)$ be the bipartite graph. Let $S$ denote the set of paths that start from a unmatched vertex in $X$ and alternates between edges in $M$ and not in $M$. There is no restriction that the path needs to end in an unmatched vertex, as in a augmenting path. Then there is an augmenting path iff there is a unmatched vertex in $Y \cap S$. It is easy to construct $S$ each time using DFS.

By the way, you can prove Hall's Theorem by showing that Kuhn's always find a prefect matching, to prove $\forall W \subseteq X, ~ \mid W\mid \leq \mid  N_G(W) \mid  \to MM = V$. Take any free $x \in X$ and let $S$ be the set of vertices such that it is connected to $x$ by augmenting paths. Then suppose that $Y \cap S$ contains no free vertices so that we cannot augment the graph further. Then we must have $\mid X \cap S\mid  \geq \mid Y \cap S\mid +1$ since all vertices in $Y \cap S$ have their corresponding matched vertex in $X \cap S$. and $X \cap S$ also contains $x$. However, by our condition for Hall's Theorem, we also have $\mid X \cap S\mid  \leq \mid Y \cap S\mid $. Contradiction.

### General Matching (Blossom's Algorithm)

See $[7]$ and $[11]$.

The problem with doing the algorithm mentioned above is that we might find an augmenting path that contains repeated vertices due to the fact that odd cycles exists. So we need to contract odd cycles (called Blossoms).

Now, we should note that Blossoms are only defined relative to the matching $M$. There is a reason we need to "lift" all blossom back to the original graph when we apply a new augmenting path. It is because an $M$-blossom is not necessarily an $M'$-blossom. A counterexample can be seen in figure $4$ of $[11]$.

Now, there is another $O(n^3)$ general matching algorithm that is described in $[12]$ but I shall not claim to understand it yet...

### Factor-Critical Graphs

A factor-critical graph is a graph $G$ where the removal of any vertex allows the remaining induced subgraph to have a perfect matching.

Here are some equivalent formulations:

- every vertex $v$ in $G$ has a maximum matching that misses $v$, see $[7]$ and $[11]$
- $G$ has a odd ear decomposition starting from an odd cycles, see $[11]$

The proof of the first formulation is very important lemma in the proofs of the following Tutte-Berge Formula in $[7]$ and $[11]$.

### Tutte-Berge Formula and Edmond-Gallai Decomposition

So there are $2$ different ways to approach this and the next section. They are given in $[11]$ and $[13]$. Their proofs are roughly the same I guess? $[7]$ also proved Tutte-Berge in the same way as $[11]$ but does not go on to Edmond-Gallai. All proofs make extensive use of factor-critical graphs.

The proofs of $[7]$ and $[11]$ rely on the fact that $G$ is factor critical iff every vertex $v$ in $G$ has a maximum matching that misses $v$. Then just split into $2$ cases, there is a vertex $v$ that is contained in every maximum matching, or vertices $v$ are missed by some maximum matching and $G$ is factor critical.

While $[13]$ just proves directly that the maximal set $S$ that minimizes the Tutte-Berge formula splits the rest of the graph into factor-critical components.

Then for Edmond-Gallai, $[13]$ proves that constructs the decomposition $(A,C,D)$ from the $S$ that we found earlier. Whereas $[11]$ starts from scratch from an arbitrary matching along with contractions by Blossom's algorithm.

Anyways, for the sake of future reference here is the Edmond-Gallai Decomposition.

The decomposition $(A,C,D)$ is as follows:

- $D$ is the set of vertices where there exists a maximum matching that does not use them
- $A$ is the vertices that are connected to $D$
- $C$ is the rest of the vertices

In $[13]$, let $S$ be the set we described earlier. Then the rest of the graph $T$ are all factor critical. We will shrink the components into single points and use Hall's theorem. Then, let $H$ be the maximum set of $S$ such that $\mid H\mid  = \mid N(H)\mid $ is true. So that means we are able to miss any point in $T \setminus N(H)$. So $D = T \setminus N(H)$, $A = H$ and $C$ is everything else.

In $[11]$, take any maximum matching $M$. $D$ is the vertices that are connected by **any** alternating path of **even** length from an unmatched vertex, $A$ is the vertices that are connected by **only** alternating paths of **odd** length and $C$ are the rest of the vertices. We want to work with the contracted graph from Blossom's algorithm because now we show that all vertices in a contracted blossom are all even. And there are also no edges between even vertices (or Blossoms). Effectively, we kind of just created the same situation as above where we contracted all components of $T$ above into single points.

Some properties of the $(A,C,D)$ decompositions:

- all components in $D$ are factor critical. In a maximum matching we use that matching and maybe one vertex get matching to something in $A$
- all components in $C$ are perfectly matched in a maximum matching
- obviously from above, $C$ and $D$ are respectively the even and odd components of $A \setminus G$
- all vertices in $A$ are matched to some component in $D$ in a maximum matching
- each subset $X \subseteq A$ has neighbours in at least $\mid X\mid +1$ components in $D$
- $A$ is a possible set that minimizes Tutte-Berge formula
- The number of vertices that are not matched in the maximum matching is the number of components in $D$ minus the number of vertices in $A$

### Some Interesting Problems

These are taken from $[7]$

- For each $k>1$, construct a $k$-regular graph without a perfect matching
- Prove that each $2k$-regular graph contains a $2$-factor
- Prove that each $3$-regular graph without bridges contains a prefect matching

### References

I would not be surprised if many links will be broken in a few years... un lucky

$[1]$ R. Graham, B. Rothschild and J. Spencer, Ramsey Theory <http://people.dm.unipi.it/dinasso/ULTRABIBLIO/Graham_Rothschild_Spencer%20-%20Ramsey%20Theory%20(2nd%20edition).pdf>

$[2]$ A. Seidenberg, A Simple Proof of a Theorem of Erdös and Szekeres, <https://academic.oup.com/jlms/article-abstract/s1-34/3/352/847946>

$[3]$ P. Erdős and G. Szekeres, A Combinatorial Problem in Geometry, <http://www.numdam.org/item/CM_1935__2__463_0.pdf>

$[4]$ T. Uustalu, [Tutorial] My way of understanding Dinitz's ("Dinic's") algorithm, <https://codeforces.com/blog/entry/104960>

$[5]$ T. Uustalu, [Tutorial] The residual graph and working with the set of all minimum cuts, <https://codeforces.com/blog/entry/104960>

$[6]$ Wikipedia, Max-flow min-cut theorem (Project selection problem), <https://en.wikipedia.org/wiki/Max-flow_min-cut_theorem#Project_selection_problem>

$[7]$ A. Schrijver, Matchings and coverings, <https://homepages.cwi.nl/~lex/files/agt1.pdf>

$[8]$ Wikipedia, Kőnig's theorem (graph theory) (Constructive proof without flow concepts), <https://en.wikipedia.org/wiki/K%C5%91nig%27s_theorem_(graph_theory)#Constructive_proof_without_flow_concepts>

$[9]$ O. Kulkov, Kőnig's and Hall's theorems through minimum cut in bipartite graphs, <https://codeforces.com/blog/entry/105697>

$[10]$ CP Algorithms, Kuhn's Algorithm for Maximum Bipartite Matching, <https://cp-algorithms.com/graph/kuhn_maximum_bipartite_matching.html>

$[11]$ M. X. Goemans, Lectures on Nonbipartite Matchings, <https://math.mit.edu/~goemans/18455S20/lecs-matching.pdf>

$[12]$ O. Kulkov, Randomized general matching with Tutte matrix, <https://codeforces.com/blog/entry/92400>

$[13]$ D. B. West, A short proof of the Berge–Tutte Formula and the Gallai–Edmonds Structure Theorem, <https://www.sciencedirect.com/science/article/pii/S0195669811000187>
