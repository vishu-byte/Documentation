## Introduction
This class handles systems that obey Langevin Dynamics. It is kept separated from other class for ease-of-use.

## Force calculators

``` c++
void ParSim::Langevin_Dynamics::Neighbours_search(ParticleSystem &ps)
```

``` c++
void ParSim::Langevin_Dynamics::Neighbours_search_PBC(ParticleSystem &ps)
```

``` c++ title="Force calculator with WCA interaction and PBC"
void ParSim::Langevin_Dynamics::Force_NL_PBC_WCA(ParSim::ParticleSystem &ps)
```

1. This Force Calculator uses Neighbour lists of each particle object to calculate force.
2. Interaction: WCA
3. Boundary: Periodic boundary conditions. 



## Integrators
This section contains information for the integrators. 

* We need a new integrator other than conventional ones that integrate the Langevin Dynamics eqautions.
* Optimal one is by Leimkuhler and Matters 2013a.

### BOAOB algorithm: LM Integrators

!!! warning
    Equations for theta evolution involve omega dependent force. BOAOB method is best for forces that are 
    derived from a position dependent potentianl only. We are still using the same integrator for theta.

LM integration is carried out by the following two methods in parts

``` c++ title="First Part "
void ParSim::Langevin_Dynamics::LM_Intergrator1(ParSim::ParticleSystem &parsym, int step)
```

1. Updates $v(t)$ to $v(t+(dt/2))$ 
2. Updates $x(t)$ to $x(t+(dt/2))$ using $v(t+(dt/2))$
3. Adds Fluctuation and Dissipation changing $v(t+(dt/2))$ to $v'(t+(dt/2))$
4. Updates  $x(t+(dt/2))$ to $x(t + dt)$ using $v'(t+(dt/2))$

For updating $v'(t+(dt/2))$ to $v(t+dt)$ we need to calculate forces (that are position dependent) using $x(t + dt)$; 

``` c++ title="Second part "
void ParSim::Langevin_Dynamics::LM_Intergrator2(ParSim::ParticleSystem &parsym, int step)
```

1. Updates $v'(t+(dt/2))$ to $v(t+dt)$ using new forces $f(t+dt)$


## Evolvers

Task of an Evolver is to bring together appropriate force calculators and intergrators in the correct order of
execution as required by the integration scheme you are using.

### LM Evolver

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






