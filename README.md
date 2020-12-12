# Statistical-Genetics-Project

# Prerequisistes
 
0. Install Python 3.7+ : https://www.python.org/downloads/, install R and R studio: https://cran.r-project.org/mirrors.html, install Anaconda (optional): https://anaconda.org/
1. Download the GWAS summary statistics here: https://www.ebi.ac.uk/gwas/studies/GCST007709
2. For the validation step, download the GWAS summary statistics here: https://www.ebi.ac.uk/gwas/studies/GCST005327

# FUSION
1. Clean the data with the munge_sumstats.py utility as part of the LD Score Regression Utility by Bulik et al. found at https://github.com/bulik/ldsc
(Note: You will have to add the following line to line after line 71 in munge_sumstats.py in order for the data to be read in correctly: ___'NONEFFECT_ALLELE': 'A2',___ 
2. The formatted raw data will be left with several blank lines, will cause an error when trying to run the data through FUSION. Remove the blank lines using either R or Python.


# Fine-mapping with FOCUS

* __TODO__:
 * Add section for hiccups

1. Install FOCUS from https://github.com/bogdanlab/focus
2. Download the GTEx v8 Brain Cortex tissue data from https://alkesgroup.broadinstitute.org/FUSION/WGT/
3. If you haven't already, download the LD Scores from 1000 Genomes Project (as in TWAS)
4. Create a custom weight database for FOCUS by running  "focus import [DIRECTORY]/GTEx.Brain_Cortex.pos fusion --tissue brain_cortex --name brain_cortex --assay rnaseq --output [DBNAME]". Here, replace [DIRECTORY] with the directory containing GTEx.Brain_Cortex.pos in your working directory, and replace [DBNAME] with your name for the database.
5. Re-format the raw data with the instructions found at https://github.com/bogdanlab/focus/wiki/Cleaning-GWAS-summary-data
6. The formatted raw data will be left with several blank lines, which may cause an error when running the fine-mapping command below. Remove the blank lines using either R or Python.
7. Now, to conduct fine-mapping at chromosome n, run the following command:
"focus finemap neuro.sumstats.gz 1000G_EUR_Phase3_plink/1000G.EUR.QC.[n] focus.db --chr [n] --tissue brain_cortex --out [OUTPUT]". [n] should be replaced with the number of your chromosome, and [OUTPUT] should be replaced with what you wish to call your output file. All output files are in .tsv, which can be opened in Excel or other spreadsheet software for viewing.

All FOCUS commands to produce the output for our project is stored in FOCUS_Output/FOCUS_Command_Line.txt
