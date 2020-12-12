# Statistical-Genetics-Project

# Prerequisistes
 
0. Install Python 3.7+ : https://www.python.org/downloads/, install R and R studio: https://cran.r-project.org/mirrors.html, install Anaconda (optional): https://anaconda.org/
1. Download the GWAS summary statistics here: https://www.ebi.ac.uk/gwas/studies/GCST007709
2. For the validation step, download the GWAS summary statistics here: https://www.ebi.ac.uk/gwas/studies/GCST005327
3. Clean the data with the munge_sumstats.py utility as part of the LD Score Regression Utility by Bulik et al. found at https://github.com/bulik/ldsc
(Note: You will have to add the following line to line after line 71 in munge_sumstats.py in order for the data to be read in correctly: ___'NONEFFECT_ALLELE': 'A2',___ 
4. The formatted raw data will be left with several blank lines, will cause an error when trying to run the data through FUSION. Remove the blank lines using either R or Python.

5. For the validation step, download the GWAS summary statistics here: https://www.ebi.ac.uk/gwas/studies/GCST005327

# TWAS

For TWAS/FUSION, we follow the method outlined here: <http://gusevlab.org/projects/fusion/>.  

## Installation and Data Downloading

1. Install [TWAS/FUSION Software](https://github.com/gusevlab/fusion_twas).
```
wget https://github.com/gusevlab/fusion_twas/archive/master.zip
unzip master.zip
cd fusion_twas-master
```
2. Download [1000 Genomes Project LD Reference](https://data.broadinstitute.org/alkesgroup/FUSION/LDREF.tar.bz2) data.
```
wget https://data.broadinstitute.org/alkesgroup/FUSION/LDREF.tar.bz2
tar xjvf LDREF.tar.bz2
```
3. Download [plink2r](https://github.com/gabraham/plink2R) package.
```
wget https://github.com/gabraham/plink2R/archive/master.zip
unzip master.zip
```
Then open R and install the necessary packages. 
```
install.packages(c('optparse','RColorBrewer'))
install.packages('plink2R-master/plink2R/',repos=NULL)
```
Note that plink2r is no longer up to date, so you will likely have to run an earlier version of R to use this, or install it via the following line.
```
devtools::install_github("carbocation/plink2R/plink2R", ref="carbocation-permit-r361")
```

4. Download the GTEx tissue data.
This can be taken from the [GTEx website](https://gtexportal.org/home/), but it is not in the correct format.  To avoid frustration, I would suggest using the weights on the [Alkes Price group website](https://alkesgroup.broadinstitute.org/FUSION/WGT/).  Follow the link then download the weights for the tissue of interest.  We use the brain cortex data.

5. Install [TWAS-plotter Software](https://github.com/opain/TWAS-plotter)

Then install the necessary R packages.
```
install.packages(c('data.table','optparse','ggplot2','ggrepel','cowplot'))
```

## TWAS/FUSION

The main function of the TWAS/FUSION is Fusion.assoc_test.R.  We use this on each chromosome individually to find genes which are significantly associated with the outcome of interest (in this case neuroticism).  Run the following code from the command line to perform Fusion.assoc_test.R on all chromosomes.

```
for i in {1..22}
do
Rscript FUSION.assoc_test.R \
  --sumstats ./neurosumvalidation.txt \
  --weights ./WEIGHTS/GTEx.Brain_Cortex.pos \
  --weights_dir ./WEIGHTS/ \
  --ref_ld_chr ./LDREF/1000G.EUR. \
  --chr $i \
  --out ./PGC2.neuroValidation.$i.dat
done
```

We can then take a look at any of the chromosomes.  Using a Bonferroni correction, we say genes with p-value less than or equal to 0.05/1068 (adjusting for the 1068 genes in the GTEx brain cortex reference) are significant.  To see the significant genes in chromosome 22, run the following:

```
cat PGC2.SCZ.22.dat | awk 'NR == 1 || $NF < 0.05/1068'
```

The output is interpreted as shown in the original [TWAS/FUSION guide](http://gusevlab.org/projects/fusion/).

## TWAS-plotter

TWAS-plotter uses the output data from TWAS/FUSION to create a manhattan plot.  Ours is created as shown here.

The first step is to combine the chromosome files from the TWAS without duplicating the header.
```
head -1 ./PGC2.neuro.1.dat > ./prePRSAllChr.dat
tail -n +2 -q ./PGC2.neuro.* >> ./prePRSAllChr.dat
```

Then make the manhattan plot.  Note that we use a false discovery rate corrected p-value as our cutoff.  This was found using the p.adjust function in R with method = "fdr".  From the command line run:

```
Rscript ./TWAS-plotter-master/TWAS-plotter.V1.0.r \
	--twas prePRSAllChr.dat \
	--sig_z 3.08399 \
	--output ManhattanNeuroFDR
```

The plot can be seen in the TWAS output.  If we did not include the --sig_z option, the function would automatically use a bonferroni correction.


# Fine-mapping with FOCUS


1. Install FOCUS from https://github.com/bogdanlab/focus
2. Download the GTEx v8 Brain Cortex tissue data from https://alkesgroup.broadinstitute.org/FUSION/WGT/
3. If you haven't already, download the LD Scores from 1000 Genomes Project (as in TWAS)
4. Create a custom weight database for FOCUS by running  "focus import [DIRECTORY]/GTEx.Brain_Cortex.pos fusion --tissue brain_cortex --name brain_cortex --assay rnaseq --output [DBNAME]". Here, replace [DIRECTORY] with the directory containing GTEx.Brain_Cortex.pos in your working directory, and replace [DBNAME] with your name for the database.
5. Re-format the raw data with the instructions found at https://github.com/bogdanlab/focus/wiki/Cleaning-GWAS-summary-data
6. The formatted raw data will be left with several blank lines, which may cause an error when running the fine-mapping command below. Remove the blank lines using either R or Python.
7. Now, to conduct fine-mapping at chromosome n, run the following command:
"focus finemap neuro.sumstats.gz 1000G_EUR_Phase3_plink/1000G.EUR.QC.[n] focus.db --chr [n] --tissue brain_cortex --out [OUTPUT]". [n] should be replaced with the number of your chromosome, and [OUTPUT] should be replaced with what you wish to call your output file. All output files are in .tsv, which can be opened in Excel or other spreadsheet software for viewing.

All FOCUS commands to produce the output for our project is stored in FOCUS_Output/FOCUS_Command_Line.txt
