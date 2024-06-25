
This is a versatile classical particle dynamics simulation code that focuses on numerically modelling particulate systems/ granular matter. The system can be anything ranging from materials like colloids, grains to active and living systems like cellular assemblies. 
This code incorporates Discrete-Element-Method (DEM) simulation techniques [@zhu2007discrete] for application to both passive and active granular matter.

## Key features

* Simulates a diverse range of physical systems including colloids, cellular assemblies, spinners etc.
* Availability of various integration schemes (Velocity-Verlet, Leimkuhler-Matthews etc.) and force calculation
  algorithms.
* Realistic Contact force models predefined.
* Customizable and extendable codebase for specific research needs.
* Easy to use


## Organization of this documentaion

This documentation is organized into the following parts:

1. The [Physics Guide](General_Physics/Overview.md) with information about general physics that goes behind this simulation code.
2. The [Programmer Guide](Programmer_Guide/Overview.md) with information about code structure: header files and methods.
3. The [Hands-on examples](Putting_together/Interacting_gas.md) with some working examples to illustrate the use of this code.

Organization of this material is simple and as readable as it can be.

## Gallery
The following examples depict the cases where this simulation code was used to reproduce results from the literature of 
Active matter and Granular matter physics.

<figure markdown="span">
  ![MIPS](mips.gif){ width="300" }
  <figcaption> Motility Induced Phase Separation (Cates, M. E., & Tailleur, J. (2015). "Motility-induced phase separation.")</figcaption>
</figure>


<figure>
<img src="./DP.gif" alt="My caption " width="300" /><figcaption>

Directed percolation [1, see footnote] (Grober et al. "Unconventional colloidal aggregation in chiral bacterial baths." Nature Physics).

</figcaption>
</figure>

<figure markdown="span">
  ![CG](Confined_chiral.gif){ width="300" }
  <figcaption>Confined gas of active spinners (Tsai, J-C., et al. "A chiral granular gas").</figcaption>
</figure>


[^2]: [Pabitra Masanta](https://www.linkedin.com/in/pabitra-masanta-344036205/) worked on this for his master's thesis work at IIT Bombay

