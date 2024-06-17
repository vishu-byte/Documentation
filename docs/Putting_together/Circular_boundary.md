## Gas inside circular walls
Here we simulate a gas that is a collection of hard spheres with circular walls.

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

  
  Physics.circular_wall_parameters[0] = 1.0; // WCA epsilon
  Physics.circular_wall_parameters[1] =
      Physics.parameters[1]; // WCA sigma
  
  Physics.NL_skin_depth =
      2 * Physics.parameters[1]; // 2*simga, NL_skin_depth, DeltaR
  Physics.NL_tau = 200;          // NL_Tau, tau    

  //Set other parameters you would like..

// 2)Reading initial state positions

  /*If desired, one can read initial state from a file*/

  std::ifstream input_state("init_state.txt");
  int i = 0;
  double temp;

  if (!input_state) { // file couldn't be opened
    std::cerr << "\033[31mError: Init_state file could not be opened"
              << std::endl;
    exit(1);
  }

  while (input_state >> particle[i].x >>
         particle[i].y) { // read particle x and y and z in temp
    i++;
  }

  std::cout << "\033[32mInput state read for " << i << " particles ..."
            << std ::endl;

  input_state.close();  

//3) Main simulation loop--------------
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
    Neighbours_search(parsym);
  }
  // i) Calculate force (F) from positions and velocities (x,v) --
  Force_NL_CRB_WCA(parsym); // links forces on each object

  // ii) Update x,v to x', v'  ----
  ERM_Integrator1_sys(parsym, step);

  // iii) Again calculate force (F') from x',v' ------
  Force_NL_CRB_WCA(parsym);

  //  iv) Update x',v' to xnew, vnew --------
  ERM_Integrator2_sys(parsym, step);
}

```

Output: (as rendered in [Ovito](https://www.ovito.org/))


<figure markdown="span">
  ![MIPS](../confined_gas.gif){ width="300" }
  <figcaption> A gas of hard particles with circular boundary</figcaption>
</figure>