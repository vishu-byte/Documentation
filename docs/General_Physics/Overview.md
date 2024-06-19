A typical system would be a collection of large numbers of soft or hard spheres that can interact with
each other via repulsion or attraction and contact forces (e.g frictional forces)
These entities have transational as well as rotational degrees of freedom. Therefore, dynamical equations will involve the
evolution of both kinds of degrees of freedom.

### Equations of motion
Given the sum of forces $\mathbf{f_{i}}$ on a particle $i$ ($\mathbf{f_{i}}$  can be from other particles, from walls or any potential), the problem in hand is to integrate the Newton's equations of motions for the translational and rotational
degrees of freedom:

$$
m_{i}\frac{d^{2}\mathbf{{r_{i}}}}{dt^{2}} = \mathbf{f_{i}} + \mathbf{f^{\text{medium}}} + \mathbf{f^{active}_{i}}
$$

$$
I_{i}\frac{d\mathbf{{\omega_{i}}}}{dt} = \mathbf{\Gamma _{i}} + \mathbf{\Gamma ^{\text{medium}}} +  \mathbf{\Gamma ^{active} _{i}}
$$

where $m_{i}, I_{i}$ are masses and momentum of inertia of $i^{th}$ particle, whose center of mass is at $\mathbf{r_{i}}$. ($\mathbf{f_{i}}, \mathbf{\Gamma _{i}}$) are the forces and torques on $i^{th}$ particle from other particles or walls. There can also be intrinsic translational and rotational active forces and torques ($\mathbf{f^{active}_{i}},\mathbf{\Gamma ^{active} _{i}}$).Finally, there are forces due to medium ($\mathbf{f^{\text{medium}}},\mathbf{\Gamma ^{\text{medium}}}$).
In addition to these, there can be noise in the system such as due to a thermal bath in which case we need to integrate
Langevin equations.

We can integrate these dynamical equations using tools from numerical integration as thoroughly described in the books
[@allen2017computer,@rapaport2004art,@poschel2005computational]
Specifically, we have borrowed the following integration schemes from these books

1. Euler integration
2. Verlet integration: Leap-Frog, Velocity Verlet
3. BAOAB algorithm of Leimkuhler and Matthews. (for Langevin Dynamics)


The typically short-ranged interactions in granular media allow for optimization by using linked-cell (LC) or alternative methods in order to make the neighborhood search more efficient.


## Contact force models

<figure markdown="span">
  ![Inter-particle force model](../particle.png){ width="300" }
  <figcaption>Contact-force model</figcaption>
</figure>

The realistic and detailed modeling of the deformations of particles in contact with each other is much too complicated;
therefore, we relate the interaction force to the overlap $\delta$ of the two particles.[@luding2008cohesive]

The force on particle $i$, from particle $j$, at contact point p, can be decomposed into a radial/central and a tangential part :

$$
\mathbf{f_{ij}} = f_{ij}^{c}\mathbf{n}  + f_{ij}^{t}\mathbf{t}
$$

where $\mathbf{n.t} = 0$. The tangential force leads to a torque.


### Radial/Central interaction
As typically, interactions involved are repulsive. These can be modeled in two ways

**Spring-like force:** 

This is for modeling soft-repulsion between spheres.

$$
f_{ij}^{c} = \frac{1}{2}k|\mathbf{r_{i}} - \mathbf{r_{j}}|^{2}
$$


**Generalized Weeks-Chandler-Andersen (WCA) potential:**

This is for modeling hard interaction.

$$
U(r)= 4\epsilon \left[ (\frac{\sigma}{r})^{2l} - (\frac{\sigma}{r})^{l} \right]  + \epsilon ,  \ \ \text{for} \ \ r\leq 2^{1/6}\sigma \\    
 = 0, \ \ \text{for} \ \  r > 2^{1/6}\sigma
$$

$$
f_{ij}^{c} = -\frac{dU(r)}{dr}\hat{\mathbf{{r_{ij}}}}
$$


where $\epsilon$ sets the interaction energy scale and $l$ is the potential stiffness.

Details about WCA interaction are in [@weeks1971role].


### Tangential interaction



$$
\mathbf{f_{ij}^{t}} =   \mu(\boldsymbol{\Omega_{ij}}\times \mathbf{r_{ij}})
$$

$\boldsymbol{\Omega_{ij}} = \hat{\mathbf{z}}(\omega_{i} + \omega_{j})/2$ is average rotation speed of a pair of particles.

Other models for tangential or central forces can be found in [@luding2008cohesive].
