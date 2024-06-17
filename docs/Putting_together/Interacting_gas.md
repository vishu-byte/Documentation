## Gas with periodic boundaries
Here we simulate a gas that is a collection of hard spheres with periodic boundary conditions.

``` c++ title=" main() "

// 1) Creating and initializaing particle system

  /*Parameters*/
 
  int Number_of_particles = 256;
  int Number_of_time_steps = 100000;
  double omega = 0.0;   //no intrinsic rotation                           
  double phi = 0.1;                            // packing fraction
  double L = sqrt(Number_of_particles / (phi)); // Dimension of periodic box

  ParSim::ParticleSystem parsym(Number_of_particles, omega, L); // create a simple system
  ParSim::Physics Physics;
  ParSim::Particle *const particle =
      parsym.get_particles(); // get access to particles


 /*Setting physics parameters -- all game to be played here */
  Physics.parameters[8] = 0.001; // time step
  Physics.parameters[1] = 2.0;   // interaction_diameter sigma
  Physics.parameters[2] = 10.0;  // mass
  Physics.parameters[13] =
      Physics.parameters[2] * (Physics.parameters[1]) *
      (Physics.parameters[1]) / 8.0; // I = (1/8)m*(simga)^2

  Physics.parameters[5] = 0.01;    // gamma_t

  Physics.WCA_parameters[0] = 1.0; // WCA epsilon
  Physics.WCA_parameters[1] =
      Physics.parameters[1]; // WCA sigma

  
  Physics.NL_skin_depth =
      2 * Physics.parameters[1]; // 2*simga, NL_skin_depth, DeltaR
  Physics.NL_tau = 200;          // NL_Tau, tau    

  //Set other parameters you would like..

//2) Main simulation loop--------------
  for (int step = 0; step < Number_of_time_steps; step++) {

    // i) writing data of this state to file (will can be used for rendering and analysis)
            // write the data of this state to a file
    
    // ii)Manipulate particle positions for next iteration.
    Physics.evolve_system_ERM_NL(parsym, step);
  }
 

```

The Evolver looks like this

``` c++ title=" evolve_system_ERM_NL(parsym, step) "
void ParSim::Physics::evolve_system_ERM_NL(ParticleSystem &parsym, int step) {

  // Update Neighbour list after every 200 steps
  if (step == 0 || step % this->NL_tau == 0) {
    Neighbours_search_PBC(parsym);
  }
  // i) Calculate force (F) from positions and velocities (x,v) --
  Force_NL_PBC_WCA(parsym); // links forces on each object

  // ii) Update x,v to x', v'  ----
  ERM_Integrator1_sys(parsym, step);

  // iii) Again calculate force (F') from x',v' ------
  Force_NL_PBC_WCA(parsym);

  //  iv) Update x',v' to xnew, vnew --------
  ERM_Integrator2_sys(parsym, step);
}

```

Output: (as rendered in [Ovito](https://www.ovito.org/))


<figure markdown="span">
  ![MIPS](../open_gas.gif){ width="300" }
  <figcaption> A gas of hard particles with periodic boundary conditions</figcaption>
</figure>