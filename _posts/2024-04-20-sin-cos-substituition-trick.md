---
tags: math
---

## $\sin$-$\cos$ Substituition Tricks

So as we all know, $\sin(\theta) = \cos(\theta - \frac \tau 4)$. This identity is very useful, as we will see in the integrals $\int \sqrt{1 + \sin(\theta)} \, d\theta$ and $\int \frac{1}{x \sqrt{6x-x^2}} \, dx$.

The trick for both integrals is to note that $1 + \cos(x) =  2\cos^2(\frac{x}{2})$ by double angle formula.

### First Integral

$$\begin{align}\int \sqrt{1+\sin(\theta)} \, d\theta &= \int \sqrt{1+\cos(\theta - \frac \tau 4)} \, d\theta \\  &=  \int \sqrt{2\cos^2(\frac \theta 2 - \frac \tau 8)} \, d\theta \\ &= \sqrt 2 \int \cos(\frac \theta 2 - \frac \tau 8) \, d\theta \\ &= 2 \sqrt 2 \sin(\frac \theta 2 - \frac \tau 8) \\ &=2 \sqrt 2 (\sin(\frac \theta 2) (\frac 1 {\sqrt{2}}) + \cos(\frac \theta 2) (-\frac 1 {\sqrt{2}})) \\ &= 2(\sin(\frac \theta 2) - \cos(\frac \theta 2)) \end{align}$$

### Second Integral

$$\begin{align} \int \frac{1}{x \sqrt{6x-x^2}} \, dx &= \int \frac{1}{x \sqrt{9 - (x-3)^2}} \, dx \\ &= \int \frac{3 \cos(\theta)}{(3\sin(\theta) + 3) \sqrt{9 - (3 \sin(\theta))^2}} \, dx, \quad x-3 = 3 \sin(\theta), \quad \frac{dx}{d\theta} = 3 \cos(\theta) \\ &= \frac 1 3 \int \frac{1}{\sin(\theta)+1} \, d \theta \\ &= \frac 1 3 \int \frac{1}{\cos(\theta - \frac \tau 4)+1} \, d \theta \\ &= \frac 1 3 \int \frac{1}{2 \cos^2(\frac \theta 2 - \frac \tau 8)} \, d \theta \\ & = \frac{1}{3} \tan(\frac \theta 2 - \frac \tau 8)\end{align}$$

We shall simplify $\tan(\frac \theta 2 - \frac \tau 8)$. First, note that $\tan(\frac{\theta}{2}) = \frac{1 - \cos \theta}{\sin \theta}$.

We can expand out $\tan(\frac{\theta - \frac \tau 4}{2}) = \frac{\sin(\theta - \frac \tau 4)}{1 + \cos(\theta - \frac \tau 4)} = \frac{- \cos(\theta)}{1 + \sin(\theta)}  =  \frac{- \sqrt{1-\sin^2(\theta)}}{1 + \sin(\theta)} = \frac{- \sqrt{1-(\frac{x-3}{3})^2}}{1 + \frac{x-3}{3}} = \frac{-\sqrt{6x-x^2}}{x}$.

Therefore, $\int \frac{1}{x \sqrt{6x-x^2}} \, dx = - \frac{\sqrt{6x-x^2}}{3x}$

