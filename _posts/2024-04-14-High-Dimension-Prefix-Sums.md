---
tags: CP
---

## High Dimensional Prefix Sums

Hi, so everyone probably codes 2D prefix sums like this:

```c++
for (int x=1;x<=n;x++){
    for (int y=1;y<=m;y++){
		arr[x][y]+=arr[x-1][y]+arr[x][y-1]-arr[x-1][y-1];
    }
}
```

and 3D prefix sums like this:

```c++
for (int x=1;x<=n;x++){
    for (int y=1;y<=m;y++){
        for (int z=1;z<=k;z++){
			arr[x][y][z]+=arr[x-1][y][z]+arr[x][y-1][z]+arr[x][y][z-1]
                			-arr[x-1][y-1][z]-arr[x-1][y][z-1]-arr[x][y-1][z-1]
                			+arr[x-1][y-1][z-1];
        }
    }
}
```

This works for low dimensions, but when we go up into higher dimensions, the complexity explodes. Even though the above two codes looks like $O(s)$ where $s=nm$ and $s=nmk$ respectively, we are treating the dimension of the prefix sum as a constant, whereas if the dimension $d$ was a variable, then the complexity becomes $O(s \cdot 2^d)$ as we need $2^d-1$ terms inside the inner loop for PIE to be done correctly.

Instead, we can do prefix sums in $O(s \cdot d)$ in the following way.

2D:

```c++
for (int x=1;x<=n;x++){
    for (int y=1;y<=m;y++){
		arr[x][y]+=arr[x-1][y];
    }
}

for (int x=1;x<=n;x++){
    for (int y=1;y<=m;y++){
		arr[x][y]+=arr[x][y-1];
    }
}
```

The reason this works is because this does prefix sums on the columns first, and then on the rows.

As an illustration of this code we will explictly show the transformation of a $3 \times 3$ array.

$$\begin{bmatrix} 1 & 1000 & 1000000 \\ 10 & 10000 & 10000000 \\ 100 & 100000 & 100000000 \end{bmatrix} \to \begin{bmatrix} 1 & 1000 & 1000000 \\ 11 & 11000 & 11000000 \\ 111 & 111000 & 111000000 \end{bmatrix} \to \begin{bmatrix} 1 & 1001 & 1001001 \\ 11 & 11011 & 11011011 \\ 111 & 111111 & 111111111 \end{bmatrix}$$

And the same code obviously works for 3D:

```c++
for (int x=1;x<=n;x++){
    for (int y=1;y<=m;y++){
        for (int z=1;z<=k;z++){
			arr[x][y][z]+=arr[x-1][y][z];
        }
    }
}

for (int x=1;x<=n;x++){
    for (int y=1;y<=m;y++){
        for (int z=1;z<=k;z++){
			arr[x][y][z]+=arr[x][y-1][z];
        }
    }
}

for (int x=1;x<=n;x++){
    for (int y=1;y<=m;y++){
        for (int z=1;z<=k;z++){
			arr[x][y][z]+=arr[x][y][z-1];
        }
    }
}
```

Of course for low dimensions, $O(s \cdot 2^d)$ and $O(s \cdot d)$ does not really make much of a difference for low dimensions, it matters when dimensions are very high. Specifically, we will look at $s = 2^d$, or more common known as sum over subsets (SOS).

### Sum Over Subsets

Problem statement: You are given an array $A_0, A_1, \ldots, A_{2^n-1}$, find array $B_0, B_1, \ldots, B_{2^n-1}$ such that $B_{i} = \sum\limits_{i \& j = j} A_j$. For example, $B_5 = A_0 + A_1 + A_4 + A_5$. This is known as the zeta transform and we may write $B = \zeta(A)$. While there are tutorials of how to do this like <https://codeforces.com/blog/entry/45223>, I think they are over complicating it.

Firstly, if we write the code naively as follow:
````c++
int brr[1<<n];
for (int x=0;x<(1<<n);x++){
    for (int y=x;;y=(y-1)&x){
        brr[x]+=arr[y];
        if (y==0) break;
    }
}
````

This code is actually $O(3^n)$. The number of times the inner loops runs is $\sum\limits_{i = 0}^{2^n-1} 2^{\text{popcount(i)}} = \sum\limits_{i=0}^n \binom{n}{i} \cdot 2^i = (1+2)^n = 3^n$.

The easier justification for it is we look at a certain bit for $(i,j)$, there are only $3$ possibilities: $(0,0)$, $(1,0)$ and $(1,1)$. So the total number of possibilities is $3^n$.

Actually, if we write $i$ in binary, it gives us the index in a $\underbrace{2 \times 2 \times \ldots \times 2}_{n \text{ times}}$ array. For example $5 \to (1,0,1,0,0,\ldots)$. From this, we can view $B$ as the high dimension prefix sum of $A$ in this array.

And well, we already seen above how to do this in $O(s \cdot d) = O(2^n \cdot n)$.

For reference, this is the code:

```c++
for (int bit=0;bit<n;bit++){
    for (int bm=0;bm<(1<<n);bm++) if (bm&(1<<bit)){
        arr[bm]+=arr[bm^(1<<bit)];
    }
}
```

Just for knowledge, $\zeta$ is invertible and we define $\mu$ as its inverse, so that $\mu(\zeta(A)) = \mu(\zeta(A)) = A$.

or-convolution of $A$ and $B$ is defined as $C_i = \sum\limits_{j \lor k = i} A_j \cdot B_k$. It is given by $C = \mu(\zeta(A) \cdot  \zeta(B))$.

### Sum over Divisors

Problem statement: You are given an array $A_1, A_2, \ldots, A_n$, find array $B_1, B_2, \ldots, B_n$ such that $B_{i} = \sum\limits_{j \mid i} A_j$. For example $B_{50} = A_1 + A_2 + A_5 + A_{10} + A_{25} + A_{50}$. This is also known as the zeta transform... 

The naive solution will just iterate over all divisors. The time complexity $\sum\limits_{i=1}^n d_i = \sum\limits_{i=1}^n \lfloor \frac{n}{i} \rfloor  = O(n \log n)$.

However, we can view each number as a vector of it's prime divisors. Suppose $x = 2^{p_2} 3^{p_3} 5^{p_5} \ldots$, it would be converted into the vector $[p_2,p_3,p_5,\ldots]$. For example:

- $1 \to [0,0,0,0,\ldots]$
- $7 \to [0,0,0,1,\ldots]$
- $24 \to [3,1,0,0,\ldots]$
- $50 \to [1,0,2,0,\ldots]$

It becomes clear that when our array is indexed by this vector, that our operation again becomes a high dimensional prefix sum.

The time complexity would be $\sum\limits_{\substack{i \leq n\\i \text{ prime}}} \lfloor \frac{n}{i} \rfloor  \approx \int_0^n \frac{n}{i \log i} \, di = O(n \log \log n)$, where the approximation comes from prime number theorem.

Btw, this is actually related to the Riemann Zeta Function. Define the Dirichlet generating function of $A$ and $B$ respectively as $\mathcal{A}(s) = \sum\limits_n \frac{A_n}{n^s}$ and $\mathcal{B}(s) = \sum\limits_n \frac{B_n}{n^s}$. As we know, the Riemann Zeta Function is $\zeta(s) = \sum\limits_n \frac{1}{n^s}$.

Then, we have $\mathcal B (s) = \zeta(s) \cdot \mathcal A (s)$.

Then we can also define the function $\mu(s)$ that is the inverse of $\zeta(s)$ so that we have $\mu(s) \zeta(s) = \zeta(s) \mu(s) = 1$.

Let us figure out what $\mu(s)$ is.

$\mu(s) = \frac{1}{\zeta(s)} = \prod\limits_{p} \frac{1}{1+p^{-s}+p^{-2s}+ \ldots} = \prod\limits_{p} \frac{1}{\frac{1}{1-p^{-s}}} = \prod\limits_{p} 1-p^{-s}$

So $[\frac{1}{n^s}] \mu(s)$ gives us the mobius function. This can also be viewed as PIE on prime factors.

