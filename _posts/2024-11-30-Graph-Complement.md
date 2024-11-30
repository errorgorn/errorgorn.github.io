---
tags: math
---

## On Chromatic Number of Graph and Its Complement

We want to prove that $n \leq \chi(G) \chi(\overline G) \leq \frac{(n+1)^2}{4}$.

I think it is clear that for the upper bound, we want to prove $\chi(G) + \chi(\overline G) \leq n+1$ instead. The bound above is implied from AM-GM.

### Lower Bound

We color each vertex with a pair $(a,b)$, the color in $G$ and $\overline G$ respectively. The total number of such pairs is clearly $\chi(G) \chi(\overline G)$. If we have $\chi(G) \chi(\overline G) < n$, by pigeon hole we would find two vertices with the same pair of colors. This is a contradiction.

### Upper Bound (Proof 1)

This was my proof 2 years ago. It is the ugliest proof out of the 3 given below.

Lemma 1: We can partition $G$ into empty graphs $\{V_i\}$ with the additional constraint that for all $i<j$ and $v_1 \in V_j$, there exists some $v_2 \in V_i$ such that $v_1$ and $v_2$ are connected.

Proof of Lemma 1: We will algorithmically construct this partition. Start with $\{V_i\}$ being the singleton sets endowed with some arbitrary ordering. If there exists $(i,j,v_1)$ that does not satisfy the constraint, then we are allowed to delete $v_1$ from $V_j$ and add it to $V_i$. The new partition will satisfy the property that all components are empty graphs.

Then our algorithm will evantually terminate. Define the potential as the sum of the index of $V_i$ where each vertex is contained. Initially our potential is $\frac{n(n+1)}{2}$. Each operation will strictly decrease the potential. As for empty sets of $V_i$, just delete them at the end. $\blacksquare$

Let there be $k$ components in our above partition into empty graphs. Then we can construct a partition of $G$ into $n-k+1$ cliques $\{U_j\}$, which is a $n-k+1$ coloring of $\overline G$.

Start with $\{U_j\}$ being the singleton sets. For each $i=2$ to $k$, we will pick an arbitrary vertex $v \in V_i$ and then merge it with some other component in $\{U_i\}$ that is entirely in $V_1 \cup V_2 \cup \ldots \cup V_{i-1}$.

Consider the components of $\{U_j\}$ inside $V_1 \cup V_2 \cup \ldots \cup V_{i-1}$. It is easy to see that $\sum\limits (\mid U_j \mid -1) = i-2$. Therefore, since $v$ has at least $i-1$ vertices to $V_1 \cup V_2 \cup \ldots \cup V_{i-1}$ (at least one to each component), there exists a component of $\{U_j\}$ where all the vertices are connected to $v$. So we merge $v$ with that component.

### Upper Bound (Proof 2)

This proof was given in <https://www.jstor.org/stable/2306658> in 1956.

We proceed by induction on single vertices. 

Suppose our graph has size $n$. We want to prove that $\chi(G) + \chi(\overline G) \leq n+1$.

If $n=1$, obviously $\chi(G)=\chi(\overline G) =1$.

Otherwise, if $n>1$, pick some arbitrary $v \in G$ and consider $H = G-v$.

Case 1: $\chi(H) + \chi(\overline H) \leq n-1$, choose new colors for $v$ in both $G$ and $\overline G$.

Case 2: $\chi(H) + \chi(\overline H) = n$, notice that we only need to choose a new color for $v$ in $G$ if the degree of $v$ is at least $\chi(H)$ in $G$. Ditto for $\overline G$ and $\chi(\overline H)$. But, the total degree of $v$ in $G$ and $\overline G$ is $n-1$. The degree of $v$ cannot be both at least $\chi(H)$ and $\chi(\overline H)$ in both $G$ and $\overline G$ respectively. So we only need to choose a new color for $v$ in one of $G$ or $\overline G$.

### Upper Bound (Proof 3)

This proof was given by Anton Trygub. I think it is the best proof.

Lemma 2: If $\chi(G) \geq c$, then we can find an induced subgraph $H \subseteq G$ such that $\min \deg(H) \geq c-1$.

Proof of Lemma 2: We will prove the contrapositive. Suppose for all induced subgraphs $H \subseteq G$, we can find a vertex that such that $\min \deg(H) < c-1$. Then we can produce an ordering $v_1,v_2,\ldots,v_n$ of the vertices of $G$ such that $v_i$ has at most $c-2$ edges in $v_1,v_2,\ldots,v_{i-1}$. The construction is to go from $i=n$ to $1$ and set $v_i$ as the vertex with minimum degree and delete it from $G$. A greedy coloring of vertices from $i=1$ to $n$ gives us the desired $(c-1)$-coloring. $\blacksquare$

Now, suppose that there is a certificate for the fact that $\chi(G) \geq k+1$ and $\chi(\overline G) \geq n-k+1$ for some $k$. The graph $H_1 \subseteq G$ with $\min \deg_G (H_1) \geq k$ has at least size $k+1$. Similarly, the graph $H_2 \subseteq \overline G$ with $\min \deg_{\overline G} (H_2) \geq n-k$ has at least size $n-k+1$.

Therefore, there is a vertex $v$ that is in $H_1 \cap H_2$. This vertex has degree at least $k$ in $G$ and degree at least $n-k$ in $\overline G$. So the total degree of $v$ is at least $n$. Contradiction.

Now, it seems as though this proof is non-constructive but we can easily make it constructive. For $G$ and $\overline G$ we will run our algorithm in lemma 2 to find a greedy coloring by repeatedly deleting the vertex with the minimum degree. Then we have just proved that if this strategy produces a coloring that uses too many colors, we have a contradiction.

