# TP_Snakemake
I. set up the working environement :
 1.	A connection to the IFB Biosphere Cloud: https://biosphere.francebioinformatique.fr/cloud/ is established
 2.	The deployement of public key is verified and a VM with the BioPipes image on the Cloud located is instantiated
 3.  The ssh key : ssh -A ubuntu@193.54.101.198 is retrieved
 4.  The conda working environment to use Snakemake is activated : conda activate snakemake
 5.  The installation of snekemake version is verified: snakemake --version
II. import data
  1.  The working environment is organised in a way that meet the standards stated in DMP and data are imported from HPC-formation cluster:
             scp -r studentxx@193.49.167.84:/home/users/shared/data/atacseq/data/subset /home/ubuntu/atacseq/data
III. Managing the produced data
 1.  Snakefile configuration file is created in the scripts folder and it will list all the transformation rules that will be implement in the workflow
 2.  Snakemake file is lauched for the first rule in order to permit the decompression of the fastq.gz files.
     -As a first step in the learning approach, we have to pass to Snakemake the name of the result files we want to generate:
snakemake --cores all --snakefile scripts/Snakefile tmp/ss_50k_0h_R1_1.fastq tmp/ss_50k_0h_R1_2.fastq tmp/ss_50k_0h_R2_1.fastq tmp/ss_50k_0h_R2_2.fastq tmp/ss_50k_0h_R3_1.fastq tmp/ss_50k_0h_R3_2.fastq tmp/ss_50k_24h_R1_1.fastq tmp/ss_50k_24h_R1_2.fastq tmp/ss_50k_24h_R2_1.fastq tmp/ss_50k_24h_R2_2.fastq tmp/ss_50k_24h_R3_1.fastq tmp/ss_50k_24h_R3_2.fastq

IV. Handling filenames
   atacseq/scripts/config/config.yaml : to avoid copying all the file names to the command line (III.2) because this practise is not easily reproducible and not in accordance with FAIR principles a configuation files is created . This YAML or JSON file contains commun variables and paths like the paths to genome reference and samples.
   -the Snakefile should be updated with the new informations in the config.yaml file by passing to Snakemake the name of the result files and the path to the repertory containing this results (for exemple tmp/ss_50k_24h_R3_1.fastq). These modifications are written in rule all section and the path to the config.yaml file is written in configfile variable 
   -
   
