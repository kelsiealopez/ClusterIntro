# Unix/Linux Command Reference


---

## Table of Contents

- [Navigating Directories](#navigating-directories)
- [Working with Files](#working-with-files)
- [Copying and Moving Files](#copying-and-moving-files)
- [Compressing and Uncompressing Files](#compressing-and-uncompressing-files)
- [Miscellaneous Useful Commands](#miscellaneous-useful-commands)
- [Request an interactive session](#request-an-interactive-session)
- [Executing Scripts](#executing-scripts)
- [Job Submission (SLURM Example)](#job-submission-slurm-example)
- [Submitting and managing jobs:](#submitting-and-managing-jobs)
- [Submitting Batch Jobs:](#Submitting-Batch-Jobs)
- [Python/Conda Environment Setup](#pythonconda-environment-setup)
- [Using grep and awk for searching and processing text](#using-grep-and-awk-for-searching-and-processing-text)
- [Project Organization Tips](#project-organization-tips)
- [Try downloading data from ncbi](#try-downloading-data-from-ncbi)

---

## Navigating Directories
```bash
cd /full/path/to/directory         # Change directory (absolute path)
cd directory                       # Change to a subdirectory (relative path)
cd ../                             # Move up one directory
cd ../../                          # Move up two directories
cd ~                               # Go to the home directory
cd                                 # Also go to the home directory 
mkdir directory_name				# make a new directory
pwd             # print current directory
```
## Working with Files
```bash
cat file.txt                       # Display file contents
touch file.txt                     # Create a new blank file
nano file.txt                      # Open or edit a file (Nano editor). Also can be used to create a new blank file that you can edit in. 
head file.txt                      # Print the first 10 lines
head -n 20 file.txt                # Print the first 20 lines
tail file.txt                      # Print the last 10 lines
rm file.txt                        # Delete a file
less file.txt                     # View (but not edit) a file
zless file.txt.gz                 # View a gzipped file
zcat file.txt.gz | head -n 20 		  # view the first 20 lines of a gzipped file
wc -l file.txt                    # Count the number of lines in a file

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

## Miscellaneous Useful Commands
```bash
[command] &                       # Run a command in the background by adding '&' to the end of it. 
variable="/long/path/to/file.txt" # Set a variable to a file path
cp ${variable} /path/to/dir/      # Copy a file using a variable
[command_1] | [command_2]  # pipe two commands together
[command_1] -p -e \        # split a command over two lines to make it easier to read with '\' at the end of the line. 
-x -t 
```

## Request an interactive session
```bash

salloc -p test -t 120:00 -c 32 --mem 100000 				# request an interactive session for two hours, with 32 cores, and 100 Gb of memory on the test partition

```

## Executing Scripts
```bash
chmod +x script_name.sh           # Make the script executable
./script_name.sh                  # Run executable script in the current directory
```

## Job Submission (SLURM Example)
# Sample SLURM job script (an example from when I ran repeat modeler):

```bash

nano 250204_Tha_Dol_RepeatModeler.sh

#!/bin/bash
#SBATCH -p edwards # partition to submit to 
#SBATCH -c 16      # cores requested 
#SBATCH -t 3-00:00  # time requested in days-hours:minutes
#SBATCH -o output_250204_Tha_Dol_RepeatModeler_%j.out # name of output file
#SBATCH -e errors_250204_Tha_Dol_RepeatModeler_%j.err  # name of error file
#SBATCH --mem=100000 # memory requested in kb (thousand). This is 100 Gb (billion)
#SBATCH --mail-type=END # email me whe nmy job is done running 

# make a directory for storing logs
mkdir -p logs
# build new RepeatModeler BLAST database with a name that includes an ID (e.g., a species code, specimen ID, etc.) 
# and genus/species. Modify accordingly.
/n/home03/kelsielopez/repeat-annotation/RepeatModeler-2.0.3/BuildDatabase -name 18-293_Thamnophilus_doliatus -engine ncbi ThaDol_18-293.p_ctg.fa
# now run RepeatModeler with 16 cores and send results from STDOUT and STDERR streams to 00_repeatmodeler.log
# in my experience, this command takes 1-3 days with vertebrate genomes
/n/home03/kelsielopez/repeat-annotation/RepeatModeler-2.0.3/RepeatModeler -pa 16 -engine ncbi -database 18-293_Thamnophilus_doliatus 2>&1 | tee 00_repeatmodeler.log


sbatch 250204_Tha_Dol_RepeatModeler.sh & # submit job to slurm and have it run in the background '&'

```

## Submitting and managing jobs:

```bash
sbatch 250204_Tha_Dol_RepeatModeler.sh                  # Submit a job
squeue -u kelsielopez              # See your jobs in the queue
showq -u kelsielopez               # also
scancel %JOBID                        # Cancel a job by its ID
seff %JOBID                        # check the efficiency of a job that has completed 
```


## Submitting Batch Jobs

```bash

# this is how you can submit only one script, to do things on many samples in parallel using this one batch script. it submits 4 jobs to the partition. one for each sample
# make a new file called samples.txt

nano samples.txt

# it is just four lines with my samples names. no extra spaces or lines. then save it
HMRG_6388
HMRG_6386
HMRG_6431
HMRG_6433

# make new file called Dipcall.sh
nano Dipcall.sh

#!/bin/bash
#SBATCH -p edwards,shared,test # submits to whichever partition opens first. Make sure to set the time and cpu to whatever is the maximum for the least powerful one (test limit is 12 hrs so even though shared and edwards goes longer, i set it to 12) 
#SBATCH -c 12
#SBATCH -t 0-02:00
#SBATCH -o Dipcall_%A_%a.out
#SBATCH -e Dipcall_%A_%a.err 
#SBATCH --mem=100000
#SBATCH --mail-type=END
#SBATCH --array=1-4 # this ssubmits 4 batch jobs. one for each sample name in sample.txt. 1 = HMRG_6388 , 2 = HMRG_6386 , 3 = HMRG_6431, 4 = HMRG_6433

# Load sample name from the nth line of samples.txt.
SAMPLE=$(sed -n "${SLURM_ARRAY_TASK_ID}p" samples.txt)

genome_indir="/n/netscratch/edwards_lab/Lab/kelsielopez/hap_assemblies/prefixed" # directory of my assemblies
ref_name="/n/netscratch/edwards_lab/Lab/kelsielopez/hap_assemblies/prefixed/hemMar_complete_sorted_JBAT.FINAL.full.soft.mask" # directory with my reference genome assembly

echo "Processing sample: $SAMPLE"

run-dipcall $SAMPLE ${ref_name}.fasta \
    ${genome_indir}/${SAMPLE}.hap1.p_ctg.fa \
    ${genome_indir}/${SAMPLE}.hap2.p_ctg.fa > ${SAMPLE}.mak

make -j2 -f ${SAMPLE}.mak
gunzip ${SAMPLE}.pair.vcf.gz
dipcall-aux.js vcfpair -s ${SAMPLE} ${SAMPLE}.pair.vcf > ${SAMPLE}.fixed.vcf


```

## Python/Conda Environment Setup
```bash
module load python/3.10.9-fasrc01                            # Load python from fasrc
conda create -n myenv python=3.10                 # Create a new environment
conda activate myenv                              # Activate your environment
conda install biopython                           # Install a package (example)
conda deactivate                                  # Deactivate the environment
```

```bash
# Set an alias 

cd 							# go to home directory 
nano .bashrc 				# open your bashrc dot file

alias scratch='cd /n/holyscratch01/edwards_lab/Users/kelsielopez'    # set 'scratch' to be a shortcut to take me to my scratch directory
alias storage='cd /n/holylfs04/LABS/edwards_lab/Lab/klopez'				# set 'storage' to be a shortcut to take me to my storage directory
alias net='cd /n/netscratch/edwards_lab/Lab/kelsielopez'				# set 'net' to be a shortcut to take me to my net scratch directory
alias py_env='module load python/3.10.9-fasrc01 && source activate python_env1' # set 'py_env' to be a shortcut to let me activate my main python environment


source .bashrc # save changes in the .bashrc file.  

```

## Using grep and awk for searching and processing text 
```bash

# Basic grep usage:
grep "PATTERN" filename                  # Print lines containing PATTERN from filename

# Examples:
grep "chr1" genes.gff                    # Find all lines with 'chr1' in a GFF file
grep -v "PATTERN" filename               # Show all lines that do NOT contain PATTERN
grep -i "pattern" filename               # Case-insensitive search

# Find all FASTA headers (lines starting with '>'):
grep "^>" genome.fasta

# Count the number of matches:
grep -c "PATTERN" filename

# Print line numbers with matches:
grep -n "PATTERN" filename

# Search recursively in all files in a directory:
grep -r "PATTERN" /path/to/folder

# Basic awk usage:
awk '{print $1}' filename                # Print the first column of each line

# Examples:
awk '{print $1, $3}' genes.gff           # Print the first and third columns from the GFF file

# Print all lines where the value in the third column is "gene":
awk '$3 == "gene"' genes.gff

# Calculate the sum of numbers in the second column:
awk '{sum += $2} END {print sum}' filename

# Print lines where the value in the fifth column is greater than 100:
awk '$5 > 100' filename

# Combine grep and awk, e.g. to print gene names from matching lines:
grep "gene" genes.gff | awk '{print $9}'
```


```bash

## Example job script that uses variables and a loop. 

nano HTseq_second_round_outgroup.sh

#!/bin/bash
#SBATCH -p shared
#SBATCH -c 2
#SBATCH -t 3-00:00
#SBATCH -o HTseq_second_round_outgroup_%j.out
#SBATCH -e HTseq_second_round_outgroup_%j.err 
#SBATCH --mem=100000
#SBATCH --mail-type=END

module load python/3.10.9-fasrc01
source activate python_env1

input_directory="/n/netscratch/edwards_lab/Lab/kelsielopez/Thamnophilus/outgroup/second_round/star"
filenames="/n/netscratch/edwards_lab/Lab/kelsielopez/Thamnophilus/outgroup/batch_Nov2024/filenames_no_extension.txt"
genome_dir_gtf="/n/netscratch/edwards_lab/Lab/kelsielopez/Thamnophilus/ncbi_dataset/data/GCA_013396695.1/Sakesphorus_luctuosus.gtf"
output_directory="/n/netscratch/edwards_lab/Lab/kelsielopez/Thamnophilus/outgroup/second_round/htseq"


cat ${filenames} |
while read LINE;
do 
htseq-count -m intersection-strict --stranded=reverse ${input_directory}/${LINE}_Aligned.out.sam \
${genome_dir_gtf} \
-i gene_id --additional-attr=gene_name > ${output_directory}/${LINE}_counts
done;


```




## Try downloading data from ncbi
```bash

salloc -p test -t 30:00 --mem 50000 				# request an interactive session for 30 minutes with 50 Gb of memory so this goes faster 

cd /path/to/scratch								# change into your scratch directory

module load python/3.10.9-fasrc01                    # Load python from fasrc
conda create -n py_env python=3.10                 # Create a new environment

conda activate py_env                              # Activate your environment

conda install conda-forge::ncbi-datasets-cli      # download ncbi-datasets (https://anaconda.org/conda-forge/ncbi-datasets-cli)

datasets										 # test that it was installed successfully 

datasets download genome accession GCA_038380795.1 --include gff3,rna,cds,protein,genome,seq-report 	# download genome from ncbi datasets

ls 				# see if it downloaded
unzip ncbi_dataset.zip 					# unzip the contents 

mv ncbi_dataset new_name # rename the directory to something else 

cd new_name # change into that directory 

cd data # keep moving down directories until you find the sequence

cd GCA_038380795.1 # again

head GCA_038380795.1_CAS-CDF_Pyrnan_1.0_genomic.fna # view tthe first 10 lines of the fasta file

cd ../../ # practice moving up and down directories

grep -e ">" GCA_038380795.1_CAS-CDF_Pyrnan_1.0_genomic.fna # see the names of some of the contigs 

grep -e ">" GCA_038380795.1_CAS-CDF_Pyrnan_1.0_genomic.fna | wc -l # see how many contigs there are

```
