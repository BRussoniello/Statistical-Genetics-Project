# Statistical-Genetics-Project

# Prerequisistes
--Add links for the studies from gwas catalog
--Will have to detail the data cleaning process for the sumstats stuff. zzz.
--Probably need to mention installing python for FOCUS


# FOCUS

1. Install FOCUS from https://github.com/bogdanlab/focus
2. Download the GTEx v8 Brain Cortex tissue data from https://alkesgroup.broadinstitute.org/FUSION/WGT/
3. If you haven't already, download the LD Scores from 1000 Genomes Project (as in TWAS)
4. Create a custom weight database for FOCUS by running  "focus import [DIRECTORY]%/GTEx.Brain_Cortex.pos fusion --tissue brain_cortex --name brain_cortex --assay rnaseq --output [DBNAME]". Here, replace [DIRECTORY] with the directory containing GTEx.Brain_Cortex.pos in your working directory, and replace [DBNAME] with your name for the database.
5. 
