# TP_Snakemake::::
## I. set up the working environement 
 1.	A connection to the IFB Biosphere Cloud: https://biosphere.francebioinformatique.fr/cloud/ is established
 2.	The deployement of public key is verified and a VM with the BioPipes image on the Cloud located is instantiated
 3.  The ssh key : ssh -A ubuntu@193.54.101.198 is retrieved
 4.  The conda working environment to use Snakemake is activated : conda activate snakemake
 5.  The installation of snekemake version is verified: snakemake --version
## II. import data
  1.  The working environment is organised in a way that meet the standards stated in DMP and data are imported from HPC-formation cluster:
             scp -r studentxx@193.49.167.84:/home/users/shared/data/atacseq/data/subset /home/ubuntu/atacseq/data
## III. Managing the produced data
 1.  Snakefile configuration file is created in the scripts folder and it will list all the transformation rules that will be implement in the workflow
 2.  Snakemake file is lauched for the first rule in order to permit the decompression of the fastq.gz files.
     -As a first step in the learning approach, we have to pass to Snakemake the name of the result files we want to generate:
snakemake --cores all --snakefile scripts/Snakefile tmp/ss_50k_0h_R1_1.fastq tmp/ss_50k_0h_R1_2.fastq tmp/ss_50k_0h_R2_1.fastq tmp/ss_50k_0h_R2_2.fastq tmp/ss_50k_0h_R3_1.fastq tmp/ss_50k_0h_R3_2.fastq tmp/ss_50k_24h_R1_1.fastq tmp/ss_50k_24h_R1_2.fastq tmp/ss_50k_24h_R2_1.fastq tmp/ss_50k_24h_R2_2.fastq tmp/ss_50k_24h_R3_1.fastq tmp/ss_50k_24h_R3_2.fastq

###  Starting this point of the workflow : reproducible and accordance with FAIR principles will be more considered ####

## IV. Handling filenames
   - atacseq/scripts/config/config.yaml : to avoid copying all the file names to the command line (III.2) because this practise is not easily reproducible and not in accordance with FAIR principles a configuation files is created . This YAML or JSON file contains commun variables and paths like the paths to genome reference and samples.
   - the Snakefile should be updated with the new informations in the config.yaml file (NB : the path to config file is also added in Snakefile) :
      1. in all rule section of Snakefile : 
         -the name of the temporary files and the path to the repertory containing these files (for exemple tmp/ss_50k_24h_R3_1.fastq) are mentionned using the 
          expand() function. 
      2. unzip rule :
           -the input , output file and the script are specified  
      3. the script is launched : snakemake --cores all --snakefile scripts/Snakefile . Since Snakemake delete temporary files , they will not be saved. 

## V. Creating the conda environment file : 
   - Purpose: conda is an environment manager where multiple packages and libraries could be installed in this isolated environment. Another particularity of conda is 
               its adaptability to all operating systems.
   -Each Snakemake rule has a conda parameter indicating the path to the config file describing the conda environment env.yaml
              for example: scripts/config/envs/qc.yaml:file containing the fastqc tool (version , channels ) in order to control its automatic installation in a
                                                           specific environemts and insure the control quality of atacseq reads. 

## VI. Updating the Snakefile to include a new rule  : 
   1. in all rule section of Snakefile : 
      -the name of the result files and the path to the repertory containing these results are indicated
   2. rule fastqc_init:
      -the input , output files , the script are specified. 
      -Additionnal parameters such as log , threads , message to improve reproducibility and job efficiency are also added.
      -Conda parameter indicating the path to the config file describing the conda environment env.yaml 
   3. the script is launched: snakemake --cores all --use-conda --snakefile scripts/Snakefile




   
