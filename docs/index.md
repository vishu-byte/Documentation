
This is a general-purpose classical particle dynamics simulation code that focuses on modelling particulate systems, granular matter etc. This system can be anything ranging from materials like fine powders, grains to cellular assemblies. etc. The discrete element is a soft or a hard sphere.

1. Active systems: Cellular assemblies etc.
2. Granular matter
3. Fluids

1. The benefit of this code is that it generalized; it acts as a base version that can be made specific by adding
   your own code, customizing the present code.

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


[^1]: [Pabitra Masanta](https://www.linkedin.com/in/pabitra-masanta-344036205/) worked on this for his master's thesis work at IIT Bombay

