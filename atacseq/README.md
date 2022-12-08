# Snakemake project

One of the main objectives of this project was to identify DNA accessible regions during transcription.
-To conduct this analysis an ATAC-seq(A Method for Assaying Chromatin Accessibility Genome-Wide) experiments was performed. <br> 
-To see the whole work you can read the original publication " David Gomez-Cabrero et al. 2019 https://doi.org/10.1038/s41597-019-0202-7).<br>

## Experimental design
-Cells collected for 0 and 24hours post-treatment with tamoxifen <br> 
-3 biological replicates of ~50,000 cells -paired end sequencing using Illumina technology with Nextera-based sequencing primers => 12 files (6 forward and 6 reverse) <br>
-reference genome : Mus musculus

## subset data:
SRR4785152 50k_0h_R1_1 0.023G 50k_0h_R1_2 0.024G
SRR4785153 50k_0h_R2_1 0.023G 50k_0h_R2_2 0.025G
SRR4785154 50k_0h_R3_1 0.023G 50k_0h_R3_2 0.025G

SRR4785341 50k_24h_R1_1 0.024G 50k_24h_R1_2 0.025G
SRR4785342 50k_24h_R2_1 0.023G 50k_24h_R2_2 0.024G
SRR4785343 50k_24h_R3_1 0.024G 50k_24h_R3_2 0.025G

## WORKFLOW
## I. set up the working environement 
 1.  A connection to the IFB Biosphere Cloud: https://biosphere.francebioinformatique.fr/cloud/ is established
 2.  The deployement of public key is verified and a VM with the BioPipes image on the Cloud located is instantiated
 3.  The ssh key : ssh -A ubuntu@XXX.XX.XXX.XXX is retrieved
 4.  The conda working environment to use Snakemake is activated : conda activate snakemake
 5.  The installation of snekemake version is verified: snakemake --version
## II. import data
  1.  The working environment is organised in a way that meet the standards stated in DMP and data are imported from HPC-formation cluster:
     -Import fastq sequences (the workflow is applied on subset data ) : <br> 
              scp -r studentxx@XXX.XX.XXX.XX:/home/users/shared/data/atacseq/data/subset /home/ubuntu/atacseq/data <br>
     -Copy trimmomatic and picard configuration file from HPC project : <br>
               scp -r studentXX@XXX.XX.XXX.XX:/opt/apps/picard-2.18.25 /home/ubuntu/atacseq <br>
               scp -r studentXX@XXX.XX.XXX.XX:/opt/apps/trimmomatic-0.38 /home/ubuntu/atacseq

## III. Managing the produced data
  Snakefile configuration file is created in the scripts folder and it will list all the transformation rules that will be implement in the workflow

###  Starting this point of the workflow : reproducible and accordance with FAIR principles will be more considered ####

## IV. Handling filenames
   - atacseq/scripts/config/config.yaml : to avoid copying all the file names to the command line  because this practise is not easily reproducible and not in accordance with FAIR principles a configuation files is created . This YAML or JSON file contains commun variables and paths like the paths to genome reference and samples.
   - the Snakefile should be updated with the new informations in the config.yaml file (NB : the path to config file is also added in Snakefile) :
      1. in all rule section of Snakefile : 
         -the name of the temporary files and the path to the repertory containing these files (for exemple tmp/ss_50k_24h_R3_1.fastq) are mentionned using the 
          expand() function. 
      2. unzip rule :
           -the input , output file and the script are specified  
    
## V. Creating the conda environment file : 
   - Purpose: conda is an environment manager where multiple packages and libraries could be installed in this isolated environment. <br> 
              Another particularity of conda is  its adaptability to all operating systems. <br>
   -Each Snakemake rule has a conda parameter indicating the path to the config file describing the conda environment env.yaml <br>
              for example: scripts/config/envs/qc.yaml:file containing the fastqc tool (version , channels ) in order to control its automatic installation in a
                                                           specific environemts and insure the control quality of atacseq reads. 

## VI. Updating the Snakefile to include a new rule  : 
   1. In all rule section of Snakefile : 
      -the name of the result files and the path to the repertory containing these results are indicated
   2. rule fastqc_init:
      -the input , output files , the script are specified. <br>
      -Additionnal parameters such as log , threads , message to improve reproducibility and job efficiency are also added. <br>
      -Conda parameter indicating the path to the config file describing the conda environment env.yaml <br>
   3. The script is launched from atacseq fold: snakemake --cores all --use-conda --snakefile scripts/Snakefile


