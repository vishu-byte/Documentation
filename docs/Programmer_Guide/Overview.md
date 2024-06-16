
## Object-oriented C++
Particle systems inherently have a basic object-oriented flavor. Therefore, we have implemented a simple object-oriented particle system in C++ which is kept as general as possible so that it can be applied to a variety of applications.

Furthermore, particle systems have many features that make them well suited for solutions on high-performance computers. For example, we can usually allocate groups of particles to processors in a straight-forward manner. We usually distribute the force (behavior) calculation among processors. [@zhangobject]

We decided to build a simple base particle system using the object-oriented features of C++ and then use it to develop code for some of the applications that have been used successfully tested.


**Particle System**
In object-oriented terms, particles are objects with a set of attributes and methods.[`#!c++ Particles`](../Programmer_Guide/ParticleSystem/Class:Particles/Description.md)


**Particle System**
A particle system is a collection of discrete entities called particles. Each particle is
described by its state and a collection of attributes. We implement the concept of a particle system
with class [`#!c++ ParticleSystem`](../Programmer_Guide/ParticleSystem/Class:ParticleSystem/Description.md)


We can evolve this particle system, (according to some dynamical equations like Newton's equations) with time by looping 
through the following steps:

1. **Accumulate the forces on each particle**: This is carried out using force calculators, which are methods of class [`#!c++ Physics`](../Programmer_Guide/Physics/Class:Physics/Description.md)
2. **Step forward one time step using a standard differential equation solver**:  This is carried out using various integrators, which are methods of class [`#!c++ Physics`](../Programmer_Guide/Physics/Class:Physics/Description.md)
3. If desired, write the state of particle system to a file (for analysis or rendering)


The force accumulation step is the most time consuming part of the process and whatcharacterizes a particular application. Generally, there are three types of forces [@zhangobject]

1. unary forces in which the force on each particle does not depend on other particles 
2. k-ary forces in which the force on a particle depends on a small set of up to k other particles,
3. n-ary forces where each the force on each particle can depend on all other particles. 

Once force accumulation is complete, we compute the state of all particles at the next time step, which is usually done by using a standard ordinary differential equation solver. Depending on the order of the solver, multiple force accumulations may be required for each time step.

Our aim was to build a flexible software system that can support a multitude of
applications by allowing for different force accumulators, different numerical methods,
including the ability to parallelize the code.


## Organization

Our code is organized into the following key sections.

### Particle System
Particle system creation is handled by classes [`#!c++ Particles`](../Programmer_Guide/ParticleSystem/Class:Particles/Description.md) and [`#!c++ ParticleSystem`](../Programmer_Guide/ParticleSystem/Class:ParticleSystem/Description.md)

### Physics
The physics of the simulation is handled by classes [`#!c++ Physics`](../Programmer_Guide/Physics/Class:Physics/Description.md), [`#!c++ Langevin_Dynamics`](../Programmer_Guide/Physics/Class:LD/Description.md)