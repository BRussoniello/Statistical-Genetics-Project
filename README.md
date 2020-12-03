# Statistical-Genetics-Project

# Prerequisistes
--Add links for the studies from gwas catalog:
1. Original: https://www.ebi.ac.uk/gwas/studies/GCST007709
2. Validation: https://www.ebi.ac.uk/gwas/studies/GCST005327
--Will have to detail the data cleaning process for the sumstats stuff. zzz.
--Probably need to mention installing python for FOCUS


# FOCUS

1. Install FOCUS from https://github.com/bogdanlab/focus
2. Download the GTEx v8 Brain Cortex tissue data from https://alkesgroup.broadinstitute.org/FUSION/WGT/
3. If you haven't already, download the LD Scores from 1000 Genomes Project (as in TWAS)
4. Create a custom weight database for FOCUS by running  "focus import [DIRECTORY]/GTEx.Brain_Cortex.pos fusion --tissue brain_cortex --name brain_cortex --assay rnaseq --output [DBNAME]". Here, replace [DIRECTORY] with the directory containing GTEx.Brain_Cortex.pos in your working directory, and replace [DBNAME] with your name for the database.
5. Now, to start fine-mapping at chromosome N, run the following command:
"focus finemap neuro.sumstats.gz 1000G_EUR_Phase3_plink/1000G.EUR.QC.[n] focus.db --chr [n] --tissue brain_cortex --out [OUTPUT]"

