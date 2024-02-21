Table of Contents
=================

   * [Introduction](#introduction)
   * [Citation](#citation)
   * [How to install and run GATE](#how-to-install-and-run-saige-and-saige-gene)
   * [UK Biobank GWAS Results](#uk-biobank-gwas-results)
   * [Log for fixing bugs](#log-for-fixing-bugs)
   * [Notes for users before running jobs](#notes-for-users-before-running-jobs)

# Introduction


## Current version is 0.45

GATE (Genetic Analysis of Time-to-Event phenotypes) is an R package with Scalable and accurate genome-wide association analysis of censored survival data in large scale biobanks using frailty models. 

GATE performs single-variant association tests for time-to-event endpoints. GATE uses uses the saddlepoint approximation (SPA)(mhof, J. P. , 1961; Kuonen, D. 1999; Dey, R. et.al 2017) to account for heavy censoring rates. 

GATE is based on joint work by Rouank Dey and Wei Zhou. 


# Citation


# How to install and run GATE (Similar to SAIGE and SAIGE-GENE) 

  https://github.com/weizhouUMICH/SAIGE/wiki/Genetic-association-tests-using-SAIGE

The docker image can be found in the docker hub **wzhou88/saige.survival:0.44.1**

# Notes for users before running jobs
* After installation, the package needs to be called as SAIGE (will update)

* Currently, the only difference between this and the regular SAIGE job is that for step1, two additional arguments are used 
```
eventTimeCol = “”,
eventTimeBinSize = 1
``` 

```
traitType = “survival”
```


eventTimeCol is the column name for the event time, e.g. age of diagnosis
eventTimeBinSize is used to set the bin size for evene times. eventTimeBinSize=1 means the bin size will be 1 and if  eventTimeBinSize is not specified, raw event time values will be used. phenoCol is for event status (0 or 1 is for censored or event) 

# Logs for bug fixing

* 0.45 (June-30-2023) check perfect seperation after expanding the factor covariates. fix the Error in solve.default(t(X1) %*% (X1 * V)) 

* 0.44.1 (June-15-2023) changed the package name to GATE from SAIGE

* 0.44.1 (December-20-2022) fixed the error X1_fg %*% Z : non-conformable argument

* 0.42.1 (August-19-2021) trying to fix the error X1_fg %*% Z : non-conformable argument 

* 0.42 (July-14-2021) fix the "AF > 0.5" error when input file is VCF

* 0.41 (January-11-2021) fix the error from the merging branches


# Installation

##  Install GATE using the conda environment

0. Download and install [miniconda](https://docs.conda.io/en/latest/miniconda.html)


1. Create a conda environment using

     * conda environment file is in the SAIGE folder: [https://github.com/weizhou0/GATE/conda_env/environment-RSAIGE_v0.yml](https://github.com/weizhou0/GATE/conda_env/environment-RSAIGE_v0.yml)

     * After downloading environment-RSAIGE_v0.yml, run following command

     ```
     conda env create -f environment-RSAIGE_v0.yml
     ```

2. Activate the conda environment RSAIGE_v0
     ```
       conda activate RSAIGE
       FLAGPATH=`which python | sed 's|/bin/python$||'`
       export LDFLAGS="-L${FLAGPATH}/lib"
       export CPPFLAGS="-I${FLAGPATH}/include"
     ```
3. Install zlib 
    ```
    conda install conda-forge::zlib
    #zlib should be under the same folder as FLAGPATH
    ```


4. Install GATE from the source code

     ```
       src_branch=main

       repo_src_url=https://github.com/weizhou0/GATE
       git clone --depth 1 -b $src_branch $repo_src_url
       Rscript ./GATE/extdata/install_packages_v0.R
       R CMD INSTALL --library=path_to_final_GATE_library GATE
     ```

     When call GATE in R, set lib.loc=path_to_final_GATE_library. 

     ```
       library(GATE, lib.loc=path_to_final_GATE_library)
     ```

##  Run GATE
   Can only be run in R. 
   fitNULLGLMM(plinkFile='survival.10k',phenoFile='phenos.tsv', phenoCol='HeartFailure',eventTimeCol='HeartFailureTime',covarColList=c('age','sex','birthyear','height'), sampleIDColinphenoFile='particid',eventTimeBinSize=1,traitType='survival',outputPrefix='test.HeartFailure', nThreads=4, LOCO=F)
	       
