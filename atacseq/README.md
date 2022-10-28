# TP_Snakemake
1.	A connection to the IFB Biosphere Cloud: https://biosphere.francebioinformatique.fr/cloud/ is established
2.	The deployement of public key is verified and a VM with the BioPipes image on the Cloud located is instantiated
3.  The ssh key : ssh -A ubuntu@193.54.101.198 is retrieved
4.  The conda working environment to use Snakemake is activated : conda activate snakemake
5.  The installation of snekemake version is verified: snakemake --version
6.  The working environment is organised in a way that meet the standards stated in DMP and data are imported from HPC-formation cluster:
             scp -r studentxx@193.49.167.84:/home/users/shared/data/atacseq/data/subset /home/ubuntu/atacseq/data
7.  Snakefile configuration file is created and it will list all the transformation rules that will be implement in the workflow
