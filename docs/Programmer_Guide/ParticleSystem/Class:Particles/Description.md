## Introduction
This class handles creation of particle objects and their attributes

Particle objects are instances of the `#!c++ ParSim::Particle` class. 
Each `#!c++ Particle` object has attributes like position variables $(x, y)$, velocity components $(vx,vy)$ etc.


## Physical Attributes

All the general purpose attributes and their physical essence is listed in the following table.

| Attributes      | Description                          |
| ----------- | ------------------------------------ |
| `#!c++ double x`       | position $x$ coordinate |
| `#!c++ double y`       | position $y$ coordinate |
| `#!c++ double vx`    | velocity component along $x$ direction |
| `#!c++ double vy`    | velocity component along $y$ direction |
| `#!c++ double alpha`    | angular position/orientation of particle |
| `#!c++ double omega`    | angular velocity of particle |
| `#!c++ double force_radial[2]`    | Components of radial force on the particle [$x$-component, $y$-component] |
| `#!c++ double force_tangential[2]`  | Components of tangential force on the particle [$x$-component, $y$-component]  |
| `#!c++ double torque`    | scalar torque on particle |
| `#!c++ double radius`       | radius of particle |
| `#!c++ double vx_activity`       | Active velocity along $x$ direction |
| `#!c++ double vy_activity`       | Active velocity along $x$ direction|
| `#!c++ double omega_activity`       | Active angular velocity|
| `#!c++ double theta`       | Active translational velocity director|

We have used forces along radial and tangential directions instead of x and y directions because in general the former are
easier to calculate and code.

Each `#!c++ Particle` object also as a c++ linked-list called `#!c++ Neighbours` that is meant for containing list of the
particle's neighbours.

| Attributes      | Description                          |
| ----------- | ------------------------------------ |
| `#!c++ std:: list<int> Neighbours`       | linked-list that contains a particle's neighbours' indicies|

Some integration schemes (specifically of $\mathcal{O}(dt^{2})$ and higher order), need to have information about a particle's poistion, velocity and forces of previous time step. The following attributes are meant to store this information.

| Attributes      | Description                          |
| ----------- | ------------------------------------ |
| `#!c++ double position_prev[2]`       | position $(x,y)$ of previous step |
| `#!c++ double velocity_prev[2]`       | position $(vx,vy)$ of previous step |
| `#!c++ double alpha_prev`       | orientation of previous step|
| `#!c++ double omega_prev`       | angular velocity of previous step |
| `#!c++ double force_radial_prev[2]`       | radial force components of previous step|
| `#!c++ double force_tangential_prev[2]`       | tangential force components of previous step |
| `#!c++ double torque_prev`       | torque components of previous step |


If one requires a calculation of pressure or stress (using Irving Kirkwood formula) then `#!c++ sigma[4]` is the $\sigma_{ab}$ matrix.

| Attributes      | Description                          |
| ----------- | ------------------------------------ |
| `#!c++ double sigma[4]`       | $\sigma_{ab}$ matrix in Irving-Kirkwood formula |


## Intialization


``` c++ 
ParSim::Particle::Particle()
```
Default constructor. Initializes all attributes to `0`.


``` c++ 
ParSim::Particle::Particle(double x_cor, double y_cor, double v_x, double v_y, double orientation)
```
Parameterized constructor. Intializes with received values for `x`,`y`,`vx`,`vy` and `alpha` respectively.


``` c++ 
ParSim::Particle::Particle(int N, double omega, double L)
```
Parameterize constructor. Intializes particles on a lattice grid using `#!c++ void ParSim::Particle::Lattice_initialize`

``` c++ 
void ParSim::Particle::Lattice_initialize(int N, double omega_act, double L)
```
Initalizes `N` particles on a lattice grid of side-length `L`. Receives`omega_activity` as an input `omega_act`. `vx,vy,alpha,omega,theta` are randomly initialized using uniform real distributions.

