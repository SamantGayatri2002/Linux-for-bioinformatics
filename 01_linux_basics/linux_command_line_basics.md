# ✅Linux Command Line Basics for Bioinformatics

## Overview

The Linux command line (terminal or shell) is the primary working environment for bioinformatics.
Most bioinformatics tools are designed to be run from the command line, often on servers,
HPC clusters, or cloud systems where no graphical interface is available.

This document covers **essential Linux commands and concepts** that are required for
day-to-day bioinformatics work.

---

## Why Linux Is Essential in Bioinformatics

- Most bioinformatics tools are developed for Linux
- Large datasets are handled more efficiently via command line
- Automation and reproducibility rely on shell scripting
- Remote servers and clusters use Linux environments

Knowing Linux is **not optional** in bioinformatics — it is a core skill.

---

## Understanding the Terminal Prompt

Example prompt:

```bash
username@machine:~/projects$
````

Meaning:

* `username` → your Linux user
* `machine` → system or server name
* `~/projects` → current directory (`~` means home)
* `$` → ready to accept commands

---

## Basic Navigation Commands

### Check current location

```bash
pwd
```

Prints the **present working directory**.

---

### List files and directories

```bash
ls
ls -l
ls -lh
ls -a
```

Commonly used options:

* `-l` → long listing
* `-h` → human-readable file sizes
* `-a` → include hidden files

---

### Change directory

```bash
cd directory_name
cd ..
cd ~
cd /
cd -
```

Useful shortcuts:

* `..` → parent directory
* `~` → home directory
* `-` → previous directory

---

## Creating Files and Directories

### Create a directory

```bash
mkdir results
mkdir -p project/data/raw
```

`-p` creates parent directories if they don’t exist.

---

### Create empty files

```bash
touch notes.txt
```

Often used to quickly create placeholder files.

---

## Copying and Moving Files

### Copy files

```bash
cp file.txt backup.txt
cp file.txt directory/
cp -r data/ backup_data/
```

---

### Move or rename files

```bash
mv old_name.txt new_name.txt
mv file.txt results/
```

In Linux, **renaming and moving use the same command**.

---

## Deleting Files (⚠️ Use Carefully)

```bash
rm file.txt
rm -i file.txt
rm -r directory/
rm -rf directory/
```

Important notes:

* There is **no recycle bin**
* `rm -rf` permanently deletes everything
* Always double-check before running

**Safe practice**:

```bash
ls files_to_delete*
rm -i files_to_delete*
```

---

## Viewing File Contents

### View small files

```bash
cat file.txt
cat -n file.txt
```

---

### View large files (recommended)

```bash
less large_file.txt
```

Useful keys inside `less`:

* `Space` → next page
* `b` → previous page
* `/text` → search
* `q` → quit

---

### View file start or end

```bash
head file.txt
head -n 20 file.txt

tail file.txt
tail -n 50 log.txt
tail -f log.txt
```

`tail -f` is very useful for monitoring logs in real time.

---

## Counting Content

```bash
wc file.txt
wc -l file.txt
wc -w file.txt
wc -c file.txt
```

Bioinformatics examples:

```bash
grep -c ">" sequences.fasta
echo $(wc -l < sample.fastq)/4 | bc
```

---

## Running Commands with Administrator Privileges

```bash
sudo apt update
sudo apt install package_name
```

Notes:

* `sudo` = superuser do
* Use only when necessary
* Never run daily analysis as root

---

## Getting Help

```bash
man ls
ls --help
```

`man` pages are the most reliable Linux documentation.

---

## Best Practices

* Use **descriptive file names**
* Never work directly on raw data
* Test commands on small files first
* Keep logs for long-running commands
* Avoid running everything as root

---

## Summary

These commands form the **foundation of Linux usage in bioinformatics**.
Mastering them will make advanced tasks like pipeline development,
tool installation, and debugging much easier.

This file intentionally focuses on **practical usage**, not theory.

```



