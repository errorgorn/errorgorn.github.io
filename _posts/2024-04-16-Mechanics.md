---
tags: notes
---

## Mechanics

### Kinematics

If we have functions $x(t),v(t),a(t)$ representing position, velocity and acceleration, then we have $v=\frac{d}{dt} x$ and $a=\frac{d^2}{dt^2}  x$. Speed is defined as $ \lvert v(t) \rvert$, read question properly if asking for velocity or speed.

For forces, generally $F=ma$. For friction force, it is applied by a surface with coefficients of friction $\mu_s$ and $\mu_k$ respectively for static and kinetic, when the object is not moving and moving respectively. If the surface has normal force $N$ on the object, the static and kinetic frictions are $\mu_sN$ and $\mu_k N$ respectively. 

For higher dimensions, just split into components using trigo. As $x(t),v(t),a(t)$ are all independent across dimensions.

### Work, Power, Energy

The definition of work is $W = \int \vec F \, d \vec x$. Note that it is integrating over the cross product of $\vec F$ and $d \vec x$, so the direction matter. For usual 1D case with constant force, we have $W = F d$.

Power is defined as $P=\frac{d}{dt} W = \frac{\Delta W}{t}$. If we have constant force and velocity, $P = Fv$.

For now, we are only concerned with translational kinectic energy and gravitational potential energy.

$KE = \frac{1}{2} mv^2$ and $GPE = mgh$. 

Then, we have $\Delta W = (KE_2+GE_2) - (KE_1+GE_1)$.

This has some unintuitive results, such that as long as a ball's speed is unchanged, we can change its direction without any work. However, surely a bent pipe can change the ball's direction without doing any work.

### Momentum

Momentum is defined as $p = mv$. Impulse is defined as the change in momentum or $J = p_2 - p_1$.

For higher dimensions, momentum is independent across dimensions.

### Rotation

Define the angular velocity as $\omega = \frac{v}{r}$. It is defined such that $x(t) = \sin(w t)$ tracks the $x$-position on a circle of radius $1$ centered at the origin with linear speed of edge being $v$. $\omega$ is defined for convinience to plug into trigo equations. The frequency and period of the oscillation is $f = \frac{\omega}{\tau}$ and $T = \frac{\tau}{\omega}$.

To maintain the circular path, the object has to be constantly accelerated towards the axis of the rotation with acceleration $a_{rad} = \frac{v^2}{r} = w^2 r$. The tangential acceleration is only responsible for increasing the speed of the object and it can be treated as 1D motion.

 For angular momentum and energy, we define the moment of inertia $I = \sum_i m_i r_i^2 =  \iiint_Q p(x,y,z) ||r||^2 \, dV$. It is the analogue of mass for rational mechanics.

Some common values of $I$ are as follow:

| Hoop about symmetric axis                                    | Hoop about diameter                                          | Solid Sphere                                                 | Hollow Sphere                                                | Rod about center                                             | Rod about side                                               |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![img](https://cdn.discordapp.com/attachments/752406106009239585/1229770692275470407/image.png?ex=6630e435&is=661e6f35&hm=90f7f2ac96200540e4408c379e7bd9b664015d54211ac5ec6da153eab4ed8209&) | ![](https://cdn.discordapp.com/attachments/752406106009239585/1229770709836759091/image.png?ex=6630e439&is=661e6f39&hm=ef7d16718db9397d6f0330d744d1a041413adad5b687d923c283e1c5730a19f6&) | ![ ](https://cdn.discordapp.com/attachments/752406106009239585/1229770721148797081/image.png?ex=6630e43b&is=661e6f3b&hm=2dae18b4728aee89b4731945216ef78d9ebdd5a0f3315eda2942e309f02aafa7&) | ![img](https://cdn.discordapp.com/attachments/752406106009239585/1229770732691783772/image.png?ex=6630e43e&is=661e6f3e&hm=f4fbc9942b82b86fe857cc748fc78bb122a81cf142f0b0f21633722516765e3a&) | ![img](https://cdn.discordapp.com/attachments/752406106009239585/1229770743412166696/image.png?ex=6630e441&is=661e6f41&hm=4e4df9a7e3023c33dce93ac74b93caed18b8e1ff1e90d8f1be4c4e6cd159110e&) | ![img](https://cdn.discordapp.com/attachments/752406106009239585/1229770758570377246/image.png?ex=6630e444&is=661e6f44&hm=fb56008a2c9102b1a861d35022b573d0143bea95e170dc18de12c804cf0801b8&) |
| $I=MR^2$                                                     | $I=\frac{1}{2}MR^2$                                          | $I=\frac{2}{5}MR^2$                                          | $I=\frac{2}{3}MR^2$                                          | $I=\frac{1}{12}MR^2$                                         | $I=\frac{1}{3}MR^2$                                          |
| All mass is distance $R$ away.                               | $\int_{0}^\tau sin^2 \theta \, d \theta = \frac{\tau}{2}$    | $\int_{0}^{1} 2 \tau r^3 \sqrt{1-r^2} \, dr = \frac{4\tau}{15}$ | $\int_{0}^{\frac \tau 2} \tau sin^3 \theta \, d\theta = \frac{4\tau}{3}$ | $\int_{0}^{\frac 12} 2 x^2 \, dx = \frac{1}{12}$             | $\int_{0}^{1} x^2 \, dx = \frac{1}{3}$                       |

We also have the parrallel axis theorem. Which states that $I = I_{\text{CM}} + md^2$ where the axis is displaced from the center of mass by distance $d$. This is because $I = \int ||\vec x + \vec d|| \, dm = \int \vec x \cdot \vec x  \, dm + 2\int \vec x \cdot \vec y  \, dm + \int \vec y \cdot \vec y  \, dm = I_{\text{CM}} + 0 + md^2$. The second term is $0$ since $\int \vec x \, dm = \vec 0$ as the average of the vectors $\vec x$ must be the position of the center of mass, which is the origin.

The rotational angular momentum is defined as $L = I\omega$ and the rotational kinetic energy is defined as $KE_{rot} = \frac{1}{2} I \omega^2$.

Rolling without slipping is when a sphere rolls on a floor and the contact point has $0$ speed. 
![ ](https://cdn.discordapp.com/attachments/752406106009239585/1229770665612279828/image.png?ex=6630e42e&is=661e6f2e&hm=62c01f49c70c90b3a069f1be11c08a9c62b23ed76aedbd36c1a91d267aaa7df0&)

We have $\omega = \frac{v}{r}$. So $KE_{tot} = KE_{trans} + KE_{rot} = \frac{1}{2} mv^2 + \frac{1}{2} I\omega^2 = \frac{v^2}{2} (m + \frac{I}{r^2})$.
A ball can roll without slipping on a frictionless surface if it has constant velocity. However, if it has changing velocity, it needs friction force to convert translational speed into rotational speed so that it can be maintain rolling without slipping

### Simple Harmonic Motion

Simple harmonic motion (SHM) happens when the restoring force of a system is linear to its displacement. The two instances where this is studied are springs and pendulums. In springs, the restoring force is $kx$, while in a pendulum, the restoring force is $mg \sin \theta \approx mg \theta$. For pendulums, we will use the small angle approximation so that the system becomes SHM.

The ODE $a(t) = \frac{d^2}{dx^2} x(t)= - \omega^2 \cdot x(t)$ for some $c$ describes the SHM system. A solution to this ODE is $x(t) = b_1 \sin(\omega t + b_2)$. We find that $v(t) =\omega b_1 \cos(\omega t + c_2)$ and $a(t) = - \omega^2 b_1 \sin(\omega t + c_2) = - \omega^2 \cdot x(t)$.

Since the pair $(\cos \theta, \sin \theta)$ geometrically traces out the path of a constant velocity particle moving in a unit circle, it is convenient for us to think about this as circular motion. Therefore, we may define $\omega$ geometrically as the rotational speed of the SHM system (in radians per second) when projected onto a unit circle. We will also define the frequency and period as $f = \frac{w}{\tau}$ and $T = \frac{1}{f}$.

For a spring system, $a(t) = - \frac{F}{m} = - \frac{k x(t)}{m}$. Therefore, $\omega = \sqrt\frac{k}{m}$.

For a pendulum system, $a(\theta) = - \frac{F}{m} = - \frac{mg \theta}{m} = - g \theta= - g \frac{x(t)}{L}$. Therefore, $\omega = \sqrt \frac{g}{L}$.

### Gravity

The force of gravity between two objects is $F = \frac{GMm}{r^2}$. Note that we define the gravitational constant of earth as $g = \frac{GM_{\text{earth}}}{r_{\text{earth}}^2}$.

Earlier, we defined an object to have GPE. But to be more precise, GPE is a quantity that exists between a system of two objects. As convention, we define the GPE of two objects infinitely far apart to be $0$. Then, then GPE of two objects is $\int_{\infty}^R \frac{GMm}{r^2} \, dr = -\frac{GMm}{r}$. It is fine that GPE is negative. This is because we only care about the relative change in energy. Indeed, for GPE defined earlier, we consider the GPE to be $0$ when we have $r = r_{\text{earth}}$, we also use the approximation $\frac{GMm}{r} - \frac{GMm}{r+h} = \frac{GMm h}{r(r+h)} \approx m \cdot \frac{GM}{r^2} \cdot h = mgh$ Indeed for translational KE, it is dependent on our frame of reference. If the observer moves, the KE will also change.

### Conservation rules

Energy and momentum are conserved, they can be spammed for equations. Note that the system must be closed. For example, a human pushing a ball does not violate COE as the human has stored potential energy to activate muscles. Another example is a ball bouncing on the ground. COM does not hold as we did not consider the change in momentum of the ground (the earth).

Note that for COAM, angular momentum is conserved around a single axis. For example, if you bring $2$ spinning disks together, the angular momentum about their respective axis will not add up. Instead you should choose a specific axis and consider the other's AM about that axis using the full form of the angular moment formula $\vec L = \vec R \times m \vec v_{\text{CM}} + I_{\text{CM}} \vec \omega$. Note that for a ball moving in a straight line, COAM is also preserved, as $\lvert \vec L \rvert = \lvert \vec R \times m \vec v \rvert = m \lvert \vec R \rvert \lvert \vec v \rvert \sin \theta = m \lvert \vec v \rvert \lvert R_{\perp} \rvert$.
