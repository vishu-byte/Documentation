## Physics of the simulation
We can evolve the particle system, (according to some dynamical equations like Newton's equations) with time by looping 
through the following steps:

1. **Accumulate the forces on each particle**: This is carried out using force calculators, which are methods of class [`#!c++ Physics`](../Programmer_Guide/Physics/Class:Physics/Description.md)
2. **Step forward one time step using a standard differential equation solver**:  This is carried out using various integrators, which are methods of class [`#!c++ Physics`](../Programmer_Guide/Physics/Class:Physics/Description.md)
3. If desired, write the state of particle system to a file (for analysis or rendering)

The physics of the simulation is handled by classes [`#!c++ Physics`](../Programmer_Guide/Physics/Class:Physics/Description.md), [`#!c++ Langevin_Dynamics`](../Programmer_Guide/Physics/Class:LD/Description.md)