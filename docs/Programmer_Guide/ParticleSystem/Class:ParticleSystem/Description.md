## Introduction
This class handles creation of a particle System of particle objects.

The particle system is an instance of the class `#!c++ ParSim::ParticleSystem`. A `#!c++ ParticleSystem` object basically
represents our particle system each entity of which is a `#!c++ Particle` object. 

`#!c++ ParticleSystem` has an array of `#!c++ Particle` objects created on heap.

| Attributes      | Description                          |
| ----------- | ------------------------------------ |
| `#!c++ Particle *particle_array`       | An array of `#!c++ Particle` objects |

Other attributes are total number of particles in a system, size etc.

| Attributes      | Description                          |
| ----------- | ------------------------------------ |
| `#!c++ int no_of_particles`       | Total number of particles in the system |
| `#!c++ double L`       | Size of the system $L$ |
| `#!c++ double phi`       | Area density of the system $\phi$|

$L$ and $\phi$ will be ofcourse related.

## Intialization


``` c++ 
ParSim::ParticleSystem::ParticleSystem(int N, double omega, double dim)
```
Parameterized constructor. Initializes a system of `N` particles on a lattice grid defined by length `dim`. 
Each particle has `omega_activity` set to `omega`.

There are other helpful methods that might be needed.

``` c++ 
double ParSim::ParticleSystem::distance(Particle &par1, Particle &par2)
```
Calculates distance between two particles `par1` and `par2`.
