---
tags: math
---

## Proof that All k-Cells are Compact

Baby Rudin gives the proof in chapter 2. This is an alternative proof based on <https://personal.math.ubc.ca/%7Emalabika/teaching/ubc/fall18/math320/HW7-revised.pdf>.

### Proof that All 1-Cells are Compact

Let the $ 1$-cell be $ [a,b] $.

Define the set $ S = \\{ x \in [a,b] \mid [a,x] \text{ has a finite subcover} \\} $.

Now, the set is not empty as $ a \in S $ and the set is also bounded above by $ b $, therefore $ \text{sup}(S) $ exists.

Suppose that $ \text{sup}(S)=c<b $, then there is an open set that contains $ c $, which therefore contains the range $ (c - \epsilon, c + \epsilon ) $. Since $ S \cap (c-\epsilon,c+\epsilon) $ is not empty, or else $\text{sup}(S) \leq c-\epsilon $. Therefore, we have $ \text{sup}(S) \geq c+\epsilon $, a contradiction.

Therefore, $ \text{sup}(S)=b $. We can prove that $ b \in S $ in a similar way as above. Therefore, $ [a,b] $ has a finite subcover, completing the proof.

### Proof that All k-Cells are Compact using Induction

Suppose that we have proven that all $d$-cells are compact. We want to prove that all $d+1$-cells are compact now.

All points of a $d+1$-cell are of the form $ (x_1,x_2,\ldots,x_{d+1}) $. For each $ y $, we will prove that there is a finite subcover for the $ d+1 $-cell with $ x_{d+1} \in [y-\epsilon_y, y+\epsilon_y ] $ for some $ \epsilon_y > 0 $.

For each point $p$ with $ x_{d+1} = y $, let the largest neighborhood centered at $p$ that is contained in some element of the open cover have radius $2\epsilon_p$. Denote $N_p$ as the neighborhood centered at $ p $ with radius $\epsilon_p$

Since we know that $d$-cells are compact, our open cover formed by $N_p$ has a finite subcover. This finite subcover gives us the desired finite subcover for the $ d+1 $-cell with $ x_{d+1} \in [y-\epsilon_y, y+\epsilon_y ] $, where $ \epsilon_y $ is the minimum of all the $ \epsilon_p $ used in our finite cover of $ N_p $. This is the reason we required that the radius of the neighbourhood in our definition of $N_p$ had radius $2\epsilon_p$ instead of $\epsilon_p$.

This transforms the problem into finding a finite subcover in a $1$-cell over the $d+1$-th dimension, which gives us our desired subcover for $d+1$-cells
