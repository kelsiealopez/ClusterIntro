# ClusterIntro


testing

  ## Navigating Directories ##

# Change directory from current working directory to a different directory. 

cd /full/path/to/directory 

# Or if it is a subdirectory of your current working directory 

cd directory

# Go back up one directory
cd ../

# Go to the home directory. ~ is a shortcut for home directory
cd ~

				## Working with files ##

# Display file contents
cat ‘file name’

# Concatenate (combine the contents) two files together 
cat ‘file 1 name’ ‘file 2 name’ > ‘new combined files name’

# Make a new blank file
touch ‘file name’

# Open a file
nano ‘file name’

# View and/or edit any file 
nano ‘file name’

# Print the first 10 lines of a file
head ‘file name’

# Print the first 20 lines of a file
head ‘file name’ -n 20


# Delete a file 
rm ‘file name’


# Copy a file to a different directory 
cp ‘/full/oath/to/file.txt’ ‘/full/path/to/different/directory’

# If you have a file in your current working directory that you want to copy somewhere else you don’t need to type the full path to the file 
cp ‘file.txt’ ‘/full/path/to/different/directory’

# If you have a file in your current working directory that you want to copy into a subdirectory one step down. 

cp ‘file.txt’ ‘directory’
cp ‘file.txt’ ‘directory/’

# Copy all the contents of one directory into another directory. -R means recursively
cp -R ‘/full/path/to/‘directory1/’ ‘/full/path/to/‘directory2/’


# Rename a file 
mv ‘old name’ ‘newname’


# Type the commands for shortcuts when typing in the terminal. For example, if I want to erase the whole line I just wrote using one command
ctrl+U


# Shortcut to the beginning of the command line 
ctrl+a

# Shortcut to the end of the command line 
ctrl+e

# Compress a fasta file (Also, check the difference between a fasta file and a fastq file). 
gzip ‘fasta file’

Output = .fasta.gz

Difference: Fastq files also include information on the quality of the reads.

# Uncompress a fasta file 
gunzip ‘compressed fasta file’

# Something I can use to run a command in the background so can keep using the terminal to do other things 
‘Command’ &

# Make a new directory 
mkdir ‘directory name’

# Print how many lines there are in a file 

wc -l ‘file name’

# Print the last few lines of a file
tail ‘file name’


# Cancel a command that I don’t want to run anymore  
ctrl+d

ctrl+c

# Another command I can use to view the content of a file besides nano. It doesn’t let me edit the file 
less ‘file name’

# Set a variable 

variable=”/long/path/to/file.txt”

# Copy /long/path/to/file.txt to a different directory 

cp ${variable} ‘different directory’


# Path to scratch 
/n/holyscratch01/edwards_lab/Users/lbartel

# Scott's Scrub Jay directory 
/n/holylfs05/LABS/informatics/Everyone/scrubjay

# To execute a file
./’file’

Find telomere (TTAGGG)n repeats:
seqtk telo seq.fa > telo.bed 2> telo.count

