## Introduction

This class is responsible for evolving the attributes of each `#!c++ Particle` entity of our particle system `#!c++ Particle System` object according to dynamical equations.


## Force calculators

### Particle-Particle Force calculators



``` c++
void ParSim::Physics::Force_PP(ParSim::ParticleSystem &ps)
```


``` c++
void ParSim::Physics::Force_PP_PBC(ParSim::ParticleSystem &ps)
```

``` c++ 
void ParSim::Physics::Force_PP_CRB(ParSim::ParticleSystem &ps)
```

``` c++ 
void ParSim::Physics::Force_PP_CRB_WCA(ParSim::ParticleSystem &ps)
```
### Neighbour list Force calculators

``` c++ 
void ParSim::Physics::Neighbours_search_PBC(ParticleSystem &ps)
```

``` c++ 
void ParSim::Physics::Neighbours_search(ParticleSystem &ps)
```

``` c++ 
void ParSim::Physics::Force_NL_PBC(ParSim::ParticleSystem &ps)
```

``` c++ 
void ParSim::Physics::Force_NL_CRB_WCA(ParSim::ParticleSystem &ps)
```

## Integrators

``` c++ 
void ParSim::Physics::Euler_Integrator(ParSim::Particle &par, int step)
```

``` c++ 
void ParSim::Physics::Vel_Verlet_Integrator(ParSim::Particle &par, int step) 
```

``` c++ 
void ParSim::Physics::ERM_Integrator1_sys(ParSim::ParticleSystem &parsym, int step)
```

``` c++ 
void ParSim::Physics::ERM_Integrator2_sys(ParSim::ParticleSystem &parsym, int step)
```


## Evolvers

``` c++ 
void ParSim::Physics::evolve_system(ParticleSystem &parsym, int step)
```

``` c++ 
void ParSim::Physics::evolve_system_ERM(ParticleSystem &parsym, int step)
```

``` c++ 
void ParSim::Physics::evolve_system_ERM_NL(ParticleSystem &parsym, int step)
```
