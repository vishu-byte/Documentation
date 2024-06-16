## A system that obeys Langevin Dynamics

Consider a system of `N` particles. The dynamical equation of any of these particles is given by [@allen2017computer]:

$$
\dot{\mathbf{r}} = \mathbf{v} = \mathbf{p}/m
$$

$$
\dot{\mathbf{p}} = \mathbf{f} - \zeta\mathbf{v} + \sigma \dot{\mathbf{w}}
$$

$\dot{\mathbf{w}}$ is Wiener Process. 

$$
d\mathbf{w} = \mathbf{w}(t+dt) = \sqrt{t}\mathbf{G}
$$

$\mathbf{G}$ is a Gaussian random variable with zero mean and unit variance.

## Numerical Integration
The above dynamical equations are integrated using BOAOB algorithm (Leimkuhler and Matthews (2013a)) [@allen2017computer].


### Code workflow


#### Initialize and Parameters set-up
This is how the main program file should look like

``` c++ title=" main() "

int Number_of_particles;
int Number_of_time_steps;
double L;        //system size 
double omega;   //Rotational activity

//1) Initialize Particle system and Langevin Dyanmics
 ParSim::ParticleSystem parsym(Number_of_particles, omega, L); // create a simple system (using lattice init)
 ParSim::Langevin_Dynamics Langevin_Dyanmics;


/*Setting physics parameters --  */
  Langevin_Dyanmics.parameters[8] = 0.001; // time step
  Langevin_Dyanmics.parameters[1] = 2.0;   // interaction_diameter sigma
  Langevin_Dyanmics.parameters[2] = 10.0;  // mass
  Langevin_Dyanmics.parameters[13] =
      Langevin_Dyanmics.parameters[2] * (Langevin_Dyanmics.parameters[1]) *
      (Langevin_Dyanmics.parameters[1]) / 8.0; // I = (1/8)m*(simga)^2

  // Fluctuation-Dissipation Parameters
  Langevin_Dyanmics.FD_parameters[0] = 100.0; // jeta_t
  Langevin_Dyanmics.FD_parameters[1] = 100.0; // jeta_r
  Langevin_Dyanmics.FD_parameters[2] = 1.0;   // kbt
  Langevin_Dyanmics.FD_parameters[3] = 0.0;   // D
 //Intialize other parameters

//2) Main simulation loop--------------
  for (int step = 0; step < Number_of_time_steps; step++) {

    // i) writing data of this state to file (will can be used for rendering and analysis)

    if (step % 200 == 0) {
    // write the data of this state to a file
    }

    // ii) Manipulate particle positions for next iteration.
    Langevin_Dyanmics.evolve_system_LM_NL(parsym, step);
  }
 

```


#### Integrators
Already provided integrators:

``` c++ 
void ParSim::Langevin_Dynamics::LM_Intergrator1(ParSim::ParticleSystem &parsym, int step)
void ParSim::Langevin_Dynamics::LM_Intergrator2(ParSim::ParticleSystem &parsym, int step)
```

#### Evolver
Evolver will look something like this.

``` c++ title=" LM evolver "
void ParSim::Langevin_Dynamics::evolve_system_LM_NL(ParticleSystem &parsym, int step) {

  // Update Neighbour list after every NL_tau steps
  if (step == 0 || step % this->NL_tau == 0) {
    Neighbours_search(parsym);
  }
  // LM integration: (x(t),v(t)) -- > (x(t + dt),v(t + dt))

  // i) Calculate force f(t) using (x(t)) --
  Force_NL_PBC_WCA(parsym);

  // ii) Update (x(t), v(t))  to (x(t+dt) , v'(t+(dt/2))
  LM_Intergrator1(parsym, step);

  // iii) Again calculate force f(t+dt) from x((t+dt))
  Force_NL_PBC_WCA(parsym);

  //  iv) Update v'(t+(dt/2)) to v(t+dt) --------
  LM_Intergrator2(parsym, step);
}
```