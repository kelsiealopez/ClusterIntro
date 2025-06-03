# Unix/Linux Command Reference


---

## Table of Contents

- [Navigating Directories](#navigating-directories)
- [Working with Files](#working-with-files)
- [Copying and Moving Files](#copying-and-moving-files)
- [Shortcuts and Tips](#shortcuts-and-tips)
- [Compressing and Uncompressing Files](#compressing-and-uncompressing-files)
- [Viewing File Contents](#viewing-file-contents)
- [Miscellaneous Useful Commands](#miscellaneous-useful-commands)
- [Directories of Interest](#directories-of-interest)
- [Executing Scripts](#executing-scripts)

---

## Navigating Directories

```bash
# Change to a different directory (absolute path)
cd /full/path/to/directory

# Change to a subdirectory (relative path)
cd directory

# Move up one directory
cd ../

# Go to the home directory
cd ~
```
## Working with Files

```bash
# Display file contents
cat file.txt

# Create a new blank file
touch file.txt

# Open or edit a file (Nano editor)
nano file.txt

# Print the first 10 lines of a file
head file.txt

# Print the first 20 lines of a file
head -n 20 file.txt

# Print the last 10 lines of a file
tail file.txt

# Delete a file
rm file.txt
```


## Copying and Moving Files

```bash
# Copy a file to another directory (absolute path)
cp /full/path/to/file.txt /full/path/to/different/directory

# Copy from current directory to another directory
cp file.txt /full/path/to/different/directory

# Copy to a subdirectory
cp file.txt directory/

# Copy all contents of one directory into another (recursive)
cp -R /full/path/to/directory1/ /full/path/to/directory2/

# Rename (move) a file
mv old_name.txt new_name.txt
```

## Compressing and Uncompressing Files

```bash
# Compress a FASTA file
gzip file.fasta

# Output: file.fasta.gz

# Uncompress a FASTA file
gunzip file.fasta.gz
```


## Viewing File Contents
```bash
# View (but not edit) a file
less file.txt

# Count the number of lines in a file
wc -l file.txt
```

## Miscellaneous Useful Commands
```bash
# Run a command in the background
[command] &

# Make a new directory
mkdir directory_name

# Set a variable to a file path
variable="/long/path/to/file.txt"

# Copy a file using a variable
cp ${variable} /path/to/different_directory
```

## Executing Scripts
```bash
# Run an executable script or file in the current directory
# first make the file executable
chmod +x script_name.sh
./script_name.sh
```



```
