# EXCAVATOR2_free

This repository hosts a copy of the EXCAVATOR2 copy number caller hosted on sourceforge at https://sourceforge.net/projects/excavator2tool/ which has been modified in a way that allows it to be run outside of the directory in which the tool is stored.  
If you use EXCAVATOR2 in your work, please cite the original authors and publication: 

D'Aurizio, R., Pippucci, T., Tattini, L., Giusti, B., Pellegrini, M. and Magi, A., 2016. 
Enhanced copy number variants detection from whole-exome sequencing data using EXCAVATOR2. 
Nucleic acids research, 44(20), pp.e154-e154.

**Please refer to the original documentation, hosted in `EXCAVATOR2_free/docs/` for any questions regarding the functions and parameters used in EXCAVATOR2.  **

# Prerequisites
* R package `Hmisc`
* `Perl`
* `R`
* `samtools`
* `bedtools`
* `ucscsuite`
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
Make sure `perl`, `R`, `samtools`, `bedtools`, and `ucscsuite` are loaded in your environment.  

```
perl TargetPerla.pl SourceTarget_GRCh38.txt /path/to/your/target.bed name-of-your-target_w50000 50000 hg38
```
Window size can be adjusted by changing `50000` to another value, and reference genome can be adjusted by changing `hg38`.  
This step will create the reference files require to run EXCAVATOR2.  

**NOTE:** EXCAVATOR2 will run into problems if the target bed file contains duplicated or overlapping regions.  

# Preparation of analysis

Preparation of the analysis, and running EXCAVATOR2 can be done in any directory.  

Make sure that `perl`, `R`, and `samtools` are loaded in your environment.  Then create your EXCAVATOR2 analysis folder anywhere on your filesystem, e.g.:
```
mdkir EXCAVATOR2_Analysis_A
cd EXCAVATOR2_Analysis_A
mkdir preparation
```
You will then need to create a folder (`EXCAVATOR2_Analysis_A/preparation/`) for the output of the preparation step, and an input file for the preparation step.  

The structure of the input file (`Analysis_A_preparation_input.txt`) should as follows:
```
path/to/sample1.bam path/to/EXCAVATOR2_Analysis_A/preparation/sample1 sample1
path/to/sample2.bam path/to/EXCAVATOR2_Analysis_A/preparation/sample2 sample2
path/to/sample3.bam path/to/EXCAVATOR2_Analysis_A/preparation/sample3 sample3
path/to/sample4.bam path/to/EXCAVATOR2_Analysis_A/preparation/sample4 sample4
path/to/sample5.bam path/to/EXCAVATOR2_Analysis_A/preparation/sample5 sample5
path/to/sample6.bam path/to/EXCAVATOR2_Analysis_A/preparation/sample6 sample6
```

In `EXCAVATOR2_Analysis_A/` run:
```
perl /path/to/EXCAVATOR2_free/EXCAVATORDataPrepare.pl Analysis_A_preparation_input.txt \
 --processors 6 --target name-of-your-target_w50000 --assembly hg38
```

# Running EXCAVATOR2

Once you have run the command to prepare the data for your analysis, you can call copy number changes using EXCAVATOR2 in your `EXCAVATOR2_Analysis_A/` directory.  
You will need to create an input file for EXCAVATOR2, the structure of which will differ depending on whether you will be running a paired or a pooled analysis.  
For a pooled analysis where `sample1` is a control sample, and samples 2-6 are all tumour samples, your input file `EXCAVATOR2_input_pooled.txt` would look as follows:
```
C1 path/to/EXCAVATOR2_Analysis_A/preparation/sample1 sample1
T1 path/to/EXCAVATOR2_Analysis_A/preparation/sample2 sample2
T2 path/to/EXCAVATOR2_Analysis_A/preparation/sample3 sample3
T3 path/to/EXCAVATOR2_Analysis_A/preparation/sample4 sample4
T4 path/to/EXCAVATOR2_Analysis_A/preparation/sample5 sample5
T5 path/to/EXCAVATOR2_Analysis_A/preparation/sample6 sample6
```
For a paired analysis where samples 1, 3, and 5 are a control samples that are matched to samples 2, 4, and 6, respectively, your input file `EXCAVATOR2_input_paired.txt` would look like this:
```
C1 path/to/EXCAVATOR2_Analysis_A/preparation/sample1 sample1
T1 path/to/EXCAVATOR2_Analysis_A/preparation/sample2 sample2
C2 path/to/EXCAVATOR2_Analysis_A/preparation/sample3 sample3
T2 path/to/EXCAVATOR2_Analysis_A/preparation/sample4 sample4
C3 path/to/EXCAVATOR2_Analysis_A/preparation/sample5 sample5
T3 path/to/EXCAVATOR2_Analysis_A/preparation/sample6 sample6
```

To run the paired analysis, make sure that `perl`, `R`, and `samtools` are loaded in your environment. Then run the following code inside the `EXCAVATOR2_Analysis_A/` folder:
```
perl /path/to/EXCAVATOR2_free/EXCAVATORDataAnalysis.pl EXCAVATOR2_input_paired.txt \
 --processors 6 --target name-of-your-target_w50000 --assembly hg38 \
 --output /path/to/EXCAVATOR2_Analysis_A/results --mode paired
```
If you want to run the pooled analysis, run the following:
```
perl /path/to/EXCAVATOR2_free/EXCAVATORDataAnalysis.pl EXCAVATOR2_input_pooled.txt \
 --processors 6 --target name-of-your-target_w50000 --assembly hg38 \
 --output /path/to/EXCAVATOR2_Analysis_A/results --mode pooled
```

**Do not run more than one analysis in the same folder, e.g. do not run both a paired and a pooled analysis in `EXCAVATOR2_Analysis_A/`, create a separate folder for paired, and a separate folder for pooled.  **
