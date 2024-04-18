---
tags: misc
---

## Magic Integral by Parts

Consider the following integrals: $\int \frac {x^2}{\sqrt{1-x^2}} \, dx$ and $\int \frac{1}{(1+x^2)^2} \, dx$.

The usual way is to use a trigonometric substitution for them, but we can do them by parts magically.

### First Integral

$$\begin{align} \int \frac{x^2}{\sqrt{1-x^2}} \, dx&= \int \frac{\sin^2(\theta) \cdot \cos(\theta)}{\cos(\theta)} \, d\theta, \quad x=\sin(\theta), \frac{d x}{d \theta} = \cos(\theta)  \\ &=  \int\sin^2(\theta) \, d \theta \\ &= \int \frac{1-\cos(2\theta)}{2} \, d\theta \\ &= \frac{1}{2} \theta - \frac 14 \sin(2 \theta)  \\ &= \frac{1}{2} \arcsin(x) - \frac{1}{4} \sin(2\arcsin(x))\end{align}$$​

<hr>

$$\begin{align}I &= \int \frac{x^2}{\sqrt{1-x^2}} \, dx \\ &=  \int (x) \cdot (\frac{x}{\sqrt{1-x^2}}) \, dx, \quad \frac{d}{dx} (x) = 1, \quad \int \frac{x}{\sqrt{1-x^2}} \, dx = -\sqrt{1-x^2} \\ &=  - x \sqrt{1-x^2} + \int  \sqrt{1-x^2} \, dx \\ &=  - x \sqrt{1-x^2} + \int  \frac{1-x^2}{\sqrt{1-x^2}} \, dx \\ &=  - x \sqrt{1-x^2} + \arcsin(x) - I  \end{align}$$

Therefore, $2I = \arcsin(x) - x\sqrt{1-x^2}$ and we have $I = \frac{1}{2} (\arcsin(x) - x\sqrt{1-x^2})$.

<hr>

Note that these answers are equal as $\sin(2\arcsin(x)) = \sin(2 \theta) = 2 \sin(\theta) \cos(\theta) =2 x \sqrt{1-x^2}$. 

### Second Integral

$$\begin{align} \int \frac{1}{(1+x^2)^2} \, dx&= \int \frac{\sec^2(\theta)}{\sec^4(\theta)} \, d\theta, \quad x=\tan(\theta), \frac{d x}{d \theta} = \sec^2(\theta)  \\ &=  \int\cos^2(\theta) \, d \theta \\ &= \int \frac{1+\cos(2\theta)}{2} \, d\theta \\ &= \frac{1}{2} \theta  + \frac 14 \sin(2 \theta) \\ &= \frac{1}{2} \arctan(x)  + \frac 14 \sin(2 \arctan(x))\end{align}$$

<hr>

$$\begin{align}I &= \int \frac{1}{1+x^2} \, dx \\ &=  \int (\frac{1}{1+x^2}) \cdot (1) \, dx, \quad \frac{d}{dx} (\frac{1}{1+x^2}) = -\frac{2x}{(1+x^2)^2}, \quad \int 1 \, dx = x \\ &=  \frac{x}{1+x^2} + \int  \frac{2x^2}{(1+x^2)^2} \, dx \\ &=  \frac{x}{1+x^2} + 2I - 2 \int\frac{1}{(1+x^2)^2}\, dx \end{align}$$

We know that $I = \arctan(x)$. Therefore, $\int\frac{1}{(1+x^2)^2}\, dx  = \frac{1}{2}(\frac{x}{1+x^2} + \arctan(x))$

<hr>

Note that these answers are equal as $\sin(2 \arctan(x)) = \sin(2 \theta) = 2 \sin(\theta) \cos(\theta) = \frac{2 \tan(\theta)}{1+\tan^2(\theta)} = \frac{2x}{1+x^2}$

<hr>

Generally, this method should work for any integral of the form $\int \frac{1}{(1 \pm x^2)^a} \, dx$ or $\int \frac{x^2}{(1 \pm x^2)^a} \, dx$. The benefit of this method is that we won't have to simplify stuff like $\sin(2 \arctan(x))$.
