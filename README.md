# evaporative-cooling
*Automatically exported from code.google.com/p/evaporative-cooling*

# Introduction
Evaporative cooling (EC) feature selection is a command-line data mining software implementation in Java and Fortran for filtering genetic association data. EC integrates Random Forests and Relief-F attribute importance measures in order to balance independent and interaction effects while removing attributes that are irrelevant to the phenotype. EC has been tested on single-nucleotide polymorphism (SNP) data. For those with access to a cluster, the parallel version of EC will be released soon.

# Microsoft Windows GUI version of Evaporative Cooling, EvaCooling.jar

**Config tab:** "Target number of variables" is the final number of SNPs selected. "Number of variables removed each loop" is the number of SNPs removed during each evaporation step. "Number of trees" is a Random Forest parameter. Increasing any of these parameters decreases the run time.

Checking "No grid optimization" fixes the coupling Temperature between Random Forest and Relief-F to unity (Grid Step Size is ignored). This results in faster running but no optimization of the coupling.

By selecting one of the "Calculate Only" check boxes, this tool can function as an iterative backwards elimination version of either Random Forest or Relief-F.

**ArffConverter/** contains FileConversionGUI.jar, which is a gui for converting data to arff. We are working on adding other formats to this utility, but .reformat is a fairly reasonable format to start with.

**Known bug:** If these files are saved in a directory with a very long name, the program may not execute. Saving them in the C:\ root direcory is safe. 

# Tutorial
This page provides the steps necessary to run EC on a genetic association data set. An example data set is provided (XOR.1.out.reformat.arff) for an XOR model with 5% heritability involving 1500 SNPs and 500 cases and 500 controls. The genotypes are encoded as 0/1/2 and the phenotype states are encoded as 0/1. Any nominal encoding is allowed, such as AA/AB/BB for genotypes and Affected/Unaffected for the phenotype. The functional SNPs in the XOR model are X5 and X10. (One of the most common problems with getting EC to run surrounds the parf executable. If you get a runtime error, go to the parf2/ directory and type make clean then make parf. This will either compile the fortran code for your architecture or it will give you informative errors. Often one needs to change the complier (FC variable) in Makefile to one installed on your system, like ifort or gfortran, then clean and make parf.)
## 1. Convert data to arff format

EC uses the Attribute-Relation File Format ([arff](http://www.cs.waikato.ac.nz/~ml/weka/arff.html)). You will find code to convert certain file formats to arff in the fileConverter subdirectory. Other formats will be supported as requested.

Option 1. .out to .reformat. The .out format is the [genomeSIM](http://chgr.mc.vanderbilt.edu/ritchielab/method.php?method=genomesim) file format, which does not contain attribute names. The following command converts .out to the .reformat format, which introduces a row with numbered attribute names starting with X1 and ending with the Class (phenotype) attribute. Subsequent rows correspond to sample instances. For the converted output file name we use the suffix .reformat.

    java -Xms128m -Xmx1500m ExecuteMain 1 XOR.1.out XOR.1.out.reformat

Option 2. .reformat to arff. We use the suffix .arff for the converted output file name.

    java -Xms128m -Xmx1500m ExecuteMain 2 XOR.1.out.reformat XOR.1.out.reformat.arff

## 2. Run EC

General usage. The genetic association data is contained in the input_arff_file and the EC results are written to output_results_file. The parameter remove_number is the number of SNPs that are removed at each evaporation step, and EC stops when stop_number SNPs remain from the original data set.

    java -classpath weka.jar:. -Xms128m -Xmx512m ECO6 input_arff_file output_results_file remove_number stop_number

The following command removes 200 attributes at each evaporation step and stops when 100 best attributes remain. This particular run could take a few hours.

    java -classpath weka.jar:. -Xms128m -Xmx1512m ECO6 XOR.1.out.reformat.arff XOR.ECRESULTS.txt 200 100

The SNPs in the results file XOR.ECRESULTS.txt are ranked by their composite score, column F=E-TS. The other columns are the Random Forest importance score, a transformed version of this score, and a transformed Relief-F score. The file header gives the cross-validation accuracy of the final collection of SNPs and the final coupling temperature.

## 3. Alternative to 2: Run parallel EC
An sge bash script is provided for submitting your job to a cluster. Input: the data file as an arff, the final number of SNPs in the filter, and the number of SNP removed at each filter step. The smaller these numbers, the longer a run will take; rule of thumb is to choose 10% of the original of SNPs. EC works best on a dense memory cluster.

    qsub runParallelEC.sh XOR.1.out.reformat.arff 200 100

The following will be a little faster because it does not optimize the coupling between the EC score components.

    qsub runParallelEC_unitCoupling.sh XOR.1.out.reformat.arff 200 100

# FAQs
## How do I compile the Fortran code in the parf2/ directory using the gfortran compiler?

Make the following changes to the Makefile then run "make parf."

    FC = gfortran

    FFLAGS = -g -pg -fno-range-check 

# Requirements
Java, Fortran 90 compiler (f95, ifort, or the like). EC has been tested on Linux systems. The code should work on Windows or Mac given a fortran compiler.

# References
B.A. McKinney, D. M. Reif, B. C. White, J. E. Crowe Jr., J. H. Moore. "Evaporative cooling feature selection for genotypic data involving interactions." Bioinformatics 2007, 23: 2113-2120. [pdf](http://insilico.utulsa.edu/pubs/Bioinformatics2007.pdf)

B.A. McKinney, J.E. Crowe, Jr., J. Guo, and D. Tian, "Capturing the spectrum of interaction effects in genetic association studies by simulated evaporative cooling network analysis," 2009, PLoS Genetics 2009, 5(3): e1000432. doi:10.1371/journal.pgen.1000432. [open access](http://www.plosgenetics.org/article/info:doi/10.1371/journal.pgen.1000432)

# Acknowledgments
We would like to acknowledge the developers of [PARF](https://code.google.com/p/parf/) (parallel Random Forests, written in Fortran 90) and [WEKA](http://www.cs.waikato.ac.nz/ml/weka) (data mining software written in Java).

Send inquiries to [brett.mckinney@gmail.com](brett.mckinney@gmail.com). 
