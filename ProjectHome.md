## Introduction ##
Evaporative cooling (EC) feature selection is a command-line data mining software implementation in Java and Fortran for filtering genetic association data.  EC integrates Random Forests and Relief-F attribute importance measures in order to balance independent and interaction effects while removing attributes that are irrelevant to the phenotype.  EC has been tested on single-nucleotide polymorphism (SNP) data.  For those with access to a cluster, the parallel version of EC will be released soon.

[New Windows GUI version](http://code.google.com/p/evaporative-cooling/wiki/winEC)

[Quick Start for \*nix command line](http://code.google.com/p/evaporative-cooling/wiki/Tutorial)

[FAQs](http://code.google.com/p/evaporative-cooling/wiki/FAQs)

## Requirements ##
Java, Fortran 90 compiler (f95, ifort, or the like). EC has been tested on Linux systems.  The code should work on Windows or Mac given a fortran compiler.

## References ##
B.A. McKinney, D. M. Reif, B. C. White, J. E. Crowe Jr., J. H. Moore. "Evaporative cooling feature selection for genotypic data involving interactions." _Bioinformatics_ 2007, **23**: 2113-2120. [pdf](http://138.26.140.125/pdf/Bioinformatics.pdf)

B.A. McKinney, J.E. Crowe, Jr., J. Guo, and D. Tian, "Capturing the spectrum of interaction effects in genetic association studies by simulated evaporative cooling network analysis," 2009,  _PLoS Genetics_ 2009, 5(3): e1000432. doi:10.1371/journal.pgen.1000432. [open access](http://www.plosgenetics.org/article/info:doi/10.1371/journal.pgen.1000432)

## Acknowledgments ##
We would like to acknowledge the developers of [PARF](http://code.google.com/p/parf/) (parallel Random Forests, written in Fortran 90) and [WEKA](http://www.cs.waikato.ac.nz/ml/weka/) (data mining software written in Java).

<p><p>
Send inquiries to <a href='mailto:brett.mckinney@gmail.com'>brett.mckinney@gmail.com</a>.<br>
<p>
<a href='http://insilico.utulsa.edu/'>McKinneyLab</a>