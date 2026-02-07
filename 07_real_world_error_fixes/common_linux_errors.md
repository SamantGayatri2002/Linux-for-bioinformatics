# Common Linux Errors and How to Fix Them (Bioinformatics Context)

## Overview

Most failures in bioinformatics pipelines are **not caused by the tools themselves**,
but by Linux-level issues such as permissions, paths, environments, or missing libraries.

This document lists **common Linux errors**, explains **why they occur**, and provides
clear fixes in a bioinformatics context.

---

## 1. Permission Denied

### Error message
```text
Permission denied
````

### Common causes

* Script not executable
* Files owned by root
* Writing to protected directories
* Mixing users in WSL

### Fixes

Make script executable:

```bash
chmod +x script.sh
```

Check ownership:

```bash
ls -l file_or_directory
```

Fix ownership:

```bash
sudo chown -R username:username project_folder
```

---

## 2. Command Not Found

### Error message

```text
command not found
```

### Common causes

* Tool not installed
* Conda environment not activated
* PATH not updated

### Fixes

Check if tool exists:

```bash
which tool_name
```

Activate Conda environment:

```bash
conda activate env_name
```

Check PATH:

```bash
echo $PATH
```

---

## 3. No Such File or Directory

### Error message

```text
No such file or directory
```

### Common causes

* Typo in filename
* Wrong working directory
* Relative vs absolute path confusion

### Fixes

Check location:

```bash
pwd
ls
```

Use absolute paths when unsure:

```bash
/home/username/project/data/file.fq
```

---

## 4. Library Not Found / Missing Shared Library

### Error message

```text
error while loading shared libraries
```

### Common causes

* Broken Conda environment
* Missing dependencies
* Mixing system and Conda libraries

### Fixes

Check environment:

```bash
conda list
```

Reinstall tool:

```bash
conda install --force-reinstall tool_name
```

Avoid using `sudo` with Conda.

---

## 5. Killed / Out of Memory

### Error message

```text
Killed
```

### Common causes

* Insufficient RAM
* Swap too small
* Too many threads

### Fixes

Check memory:

```bash
free -h
```

Reduce threads:

```bash
-tool -t 4
```

Increase swap (WSL):

* Update `.wslconfig`
* Restart WSL

---

## 6. Disk Space Left on Device

### Error message

```text
No space left on device
```

### Common causes

* Large intermediate files
* Assemblies filling disk
* Logs accumulating

### Fixes

Check disk:

```bash
df -h
```

Find large files:

```bash
du -sh *
```

Clean safely:

* Remove intermediate files
* Compress logs
* Move old projects to external storage

---

## 7. Conda Environment Activation Fails

### Error message

```text
CondaError: Run 'conda init'
```

### Fixes

Initialize Conda:

```bash
conda init
```

Restart terminal.

---

## 8. Script Runs but Produces No Output

### Common causes

* Output redirected incorrectly
* Writing to unexpected directory
* Silent failure

### Fixes

Run with logging:

```bash
tool_command > output.log 2>&1
```

Check log:

```bash
less output.log
```

---

## 9. FASTQ / FASTA Format Errors

### Common causes

* Corrupted files
* Incomplete downloads
* Manual edits

### Fixes

Validate FASTQ:

```bash
wc -l sample.fastq
```

(Line count must be divisible by 4.)

Inspect content:

```bash
head -n 40 sample.fastq
```

---

## 10. Everything Looks Correct but Tool Still Fails

### Checklist

* Correct environment activated?
* Enough RAM and disk?
* Running inside `/home`, not `/mnt/c`?
* Threads appropriate?
* Logs checked?

Most issues are solved by **slowing down and verifying basics**.

---

## Best Practices to Avoid Errors

* Never work as root
* Keep raw data read-only
* Use one Conda environment per project
* Log every command
* Monitor memory and disk usage
* Document fixes when they occur

---

## Final Takeaway

Linux errors are part of bioinformatics — even for experienced users.

What matters is:

* understanding the cause
* fixing the environment, not guessing
* documenting solutions for reuse

This approach turns repeated frustration into reusable knowledge.

