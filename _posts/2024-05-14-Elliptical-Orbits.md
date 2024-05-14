---
tags: misc
---

## Elliptical Orbits

I couldn't solve the below problem so I will now write a blog post about elliptical orbits. Thank you to nor for discussing this with me. You can find his blog at <https://nor-blog.surge.sh/>.

![ ](/media/elliptical_orbit.jpg)

### Definition of Ellipse

![ ](/media/ellipse.png)

An ellipse is uniquely defined from $2$ chosen foci (possibly same) and a distance $d$. The perimeter of the ellipse if the locus of points whose sum of distance to the $2$ chosen foci is $d$. Since the ellipse have $5$ degrees of freedom (two pairs of coordinates from the foci and the distance $d$), $5$ points uniquely determine a ellipse.

Alternatively, the ellipse can also be viewed as a stretched circle with the equation $(\frac xa)^2 + (\frac yb)^2 = 1$, so that the points are given by the curve $(x,y) = (a \cos \theta, b \sin \theta)$. If $a \geq b$, the foci are at $(\pm c,0)$. Since $(a,0)$ is on the ellipse, we have $d=2a$. Therefore, by considering that $(0,b)$ is on the ellipse, $2\sqrt{c^2 + b^2} = 2a$. Clearly, $c = \sqrt{a^2-b^2}$ satisfies the equation.

To verify that it is an ellipse, we will have to verify that $\sqrt{(a \cos \theta + \sqrt{a^2-b^2})^2 + (b \sin \theta)^2} + \sqrt{(a \cos \theta - \sqrt{a^2-b^2})^2 + (b \sin \theta)^2} = 2a$.

$\begin{align}\sqrt{(a \cos \theta \pm \sqrt{a^2-b^2})^2 + (b \sin \theta)^2} &= \sqrt{a^2 \cos^2\theta \pm 2a\sqrt{a^2-b^2} \cos \theta + a^2-b^2 + b^2 \sin^2 \theta} \\ &= \sqrt{a^2 \cos^2\theta \pm 2a\sqrt{a^2-b^2} \cos \theta + a^2 - b^2 \cos^2 \theta} \\ & = \sqrt{(a^2-b^2) \cos^2\theta \pm 2a\sqrt{a^2-b^2} \cos \theta + a^2}  \\ &= a \pm \sqrt{a^2 - b^2} \cos \theta \end{align}$

Therefore, the original statement is clearly true.

There is a video on numberphile about elliptical pool tables, where hitting a ball from one foci to will always allow the ball to go to the other foci.

<iframe src="https://youtu.be/4KHCuXN2F3I"> </iframe>  

Here is proof by bashing synthetic geometry.

Suppose that the ball strikes the surface at angle $\theta$ from the origin, which is at point $R = (a \cos \theta, b \sin \theta)$.

From the equation $f(x,y) = (\frac xa)^2 + (\frac yb)^2 = 1$, we have $\nabla = \begin{pmatrix} \frac{\partial f}{\partial x} \\\\ \frac{\partial f}{\partial y} \end{pmatrix} = 2 \begin{pmatrix} \frac{2x}{a^2} \\\\ \frac{2y}{b^2} \end{pmatrix} = 2 \begin{pmatrix} \frac{\cos \theta}{a} \\\\ \frac{\sin \theta}{b} \end{pmatrix}$.

After scaling all vectors, $\nabla$ should be the average of $\overrightarrow{F_+R}$ and $\overrightarrow{F_-R}$. Where $F_\pm = (\pm \sqrt{a^2-b^2},0)$.

From earlier, we know that $d_\pm = \lvert \overrightarrow{F_\pm R} \rvert = a \pm \sqrt{a^2 - b^2} \cos \theta$.

Therefore, we have $d_- \cdot  \overrightarrow{F_+R} + d_+ \cdot  \overrightarrow{F_-R} = k \cdot \nabla$. One can verify that $k = \frac{1}{2ab^2}$ satisfies the equation.

### Kepler's Second Law

Firstly, from Kepler's second law, we know that the line between the sun and the planet sweeps out an equal area over an equal time interval. The proof of this 
