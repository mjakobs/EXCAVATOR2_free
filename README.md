# EXCAVATOR2_free

This repository hosts a copy of the EXCAVATOR2 copy number caller hosted on sourceforge at (https://sourceforge.net/projects/excavator2tool/)[https://sourceforge.net/projects/excavator2tool/] which has been modified in a way that allows it to be run outside of the directory in which the tool is stored.  
If you use EXCAVATOR2 in your work, please cite the original authors and publication: 

D'Aurizio, R., Pippucci, T., Tattini, L., Giusti, B., Pellegrini, M. and Magi, A., 2016. 
Enhanced copy number variants detection from whole-exome sequencing data using EXCAVATOR2. 
Nucleic acids research, 44(20), pp.e154-e154.

# Prerequisites
* R package `Hmisc`
* Perl
* R
* samtools
* `GCA_000001405.15_GRCh38.bw` and/or `ucsc.hg19.bw` bw file saved in `EXCAVATOR2_free/data/`

# Installation and preparation

```
git clone https://github.com/mjakobs/EXCAVATOR2_free.git
cd EXCAVATOR2_free/data/
mkdir targets
cd targets/
mkdir hg19
mkdir hg38
```

# Preparation of the target file
Must be done in the `EXCAVATOR2_free/` directory.  

# Preparation of analysis

# Running EXCAVATOR2
