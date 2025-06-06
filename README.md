# Unix/Linux Command Reference


---

## Table of Contents

- [Navigating Directories](#navigating-directories)
- [Working with Files](#working-with-files)
- [Copying and Moving Files](#copying-and-moving-files)
- [Compressing and Uncompressing Files](#compressing-and-uncompressing-files)
- [Viewing File Contents](#viewing-file-contents)
- [Miscellaneous Useful Commands](#miscellaneous-useful-commands)
- [Executing Scripts](#executing-scripts)
- [Cluster Login and File Transfers](#cluster-login-and-file-transfers)
- [Job Submission (SLURM Example)](#job-submission-slurm-example)
- [Downloading and Handling Genomic Files](#downloading-and-handling-genomic-files)
- [Python/Conda Environment Setup](#pythonconda-environment-setup)
- [Project Organization Tips](#project-organization-tips)
- [Mini-Exercises](#mini-exercises)

---

## Navigating Directories
```bash
cd /full/path/to/directory         # Change directory (absolute path)
cd directory                       # Change to a subdirectory (relative path)
cd ../                             # Move up one directory
cd ../../                          # Move up two directories
cd ~                               # Go to the home directory
cd                                 # Also go to the home directory 
```
## Working with Files
```bash
cat file.txt                       # Display file contents
touch file.txt                     # Create a new blank file
nano file.txt                      # Open or edit a file (Nano editor)
head file.txt                      # Print the first 10 lines
head -n 20 file.txt                # Print the first 20 lines
tail file.txt                      # Print the last 10 lines
rm file.txt                        # Delete a file
```
## Copying and Moving Files
```bash
cp /full/path/to/file.txt /full/path/to/different/directory         # Copy a file
cp file.txt directory/                                             # Copy to a subdirectory
cp -R /full/path/to/directory1/ /full/path/to/directory2/          # Copy directories recursively
mv old_name.txt new_name.txt                                       # Rename or move a file
```
## Compressing and Uncompressing Files
```bash
gzip file.fasta                   # Compress a FASTA file
gunzip file.fasta.gz              # Uncompress a FASTA file
```
## Viewing File Contents
```bash
less file.txt                     # View (but not edit) a file
zless file.txt.gz                 # View a gzipped file
nano file.txt                     # View and edit a file
wc -l file.txt                    # Count the number of lines in a file
```

## Miscellaneous Useful Commands
```bash
[command] &                       # Run a command in the background
mkdir directory_name              # Make a new directory
variable="/long/path/to/file.txt" # Set a variable to a file path
cp ${variable} /path/to/dir/      # Copy a file using a variable
```
## Executing Scripts
```bash
chmod +x script_name.sh           # Make the script executable
./script_name.sh                  # Run executable script in the current directory
```
## Cluster Login and File Transfers
```bash
ssh your_username@login.cluster.address.edu                       # Log onto the cluster
scp file.txt your_username@login.cluster.address.edu:/cluster/dir # Upload file to cluster
scp your_username@login.cluster.address.edu:/cluster/dir/file.txt . # Download file from cluster
```
## Job Submission (SLURM Example)
# Sample SLURM job script (save as myjob.sbatch):

```bash
#!/bin/bash
#SBATCH --job-name=example
#SBATCH --time=01:00:00
#SBATCH --mem=2G
#SBATCH --cpus-per-task=2

python your_script.py
```

## Submitting and managing jobs:

```bash
sbatch myjob.sbatch                  # Submit a job
squeue -u your_username              # See your jobs in the queue
scancel JOBID                        # Cancel a job by its ID
```
## Downloading and Handling Genomic Files
# Example: Downloading a genome from NCBI using datasets CLI:

```bash
datasets download genome accession GCA_013396695.1 --include gff3,gtf,rna,cds,protein,genome,seq-report
unzip ncbi_dataset.zip
```

##Or using wget:

```bash
Copy code
wget ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCA_013/396/695/GCA_013396695.1_name/xxx.fna.gz
```
## Python/Conda Environment Setup
```bash
module load anaconda3                             # Load anaconda/conda if required by the cluster
conda create -n myenv python=3.10                 # Create a new environment
conda activate myenv                              # Activate your environment
conda install biopython                           # Install a package (example)
conda deactivate                                  # Deactivate the environment
```

## Project Organization Tips
#Make a new directory for each project or analysis:
```bash
mkdir project_name
cd project_name
#Use clear and consistent file naming.
#Keep a README file in each directory.
#Regularly clean up old files, compress large files, and watch your quota.
#Use relative paths rather than hardcoded absolute paths where practical.
#Mini-Exercises
#Pull a genome and decompress it

#a. Download a genome (FASTA) from NCBI

#b. Uncompress it with gunzip

#c. Count the number of sequences with grep '^>' genome.fna | wc -l

#Extract all gene names from a GFF or GTF file

#Set up a conda environment and install Biopython

#Write and execute a small Python or Bash script

#Copy a file from your computer to the cluster and back
```
