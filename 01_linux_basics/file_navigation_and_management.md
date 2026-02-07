# File Navigation and Management for Bioinformatics

## Overview

In bioinformatics, files are large, workflows are multi-step, and analyses often run for hours or days.
Incorrect file handling or permission issues can silently break pipelines or cause data loss.

This document explains **how files and directories work in Linux**, with special focus on:
- safe navigation
- directory organization
- file permissions
- ownership
- common permission-related problems in bioinformatics

---

## The Linux File System (Quick Context)

Linux uses a **hierarchical file system**, starting from `/` (root).

Common directories you will interact with:

| Path | Purpose |
|----|----|
| `/` | Root directory |
| `/home/username` | Your personal workspace |
| `/usr` | Installed software |
| `/bin` | Core Linux commands |
| `/mnt` | Mounted drives (important in WSL) |
| `~` | Shortcut for your home directory |

👉 **Rule of thumb:**  
All bioinformatics work should happen inside your **home directory**, not system directories.

---

## Recommended Project Structure (Why This Matters)

A clean project layout prevents confusion and improves reproducibility.

```text
project_name/
├── data/
│   ├── raw/        # untouched input data
│   ├── processed/ # cleaned / filtered data
│
├── scripts/       # bash / python / R scripts
├── results/       # final outputs
├── logs/          # tool logs and stderr
└── docs/          # notes and metadata
````

Why this structure works:

* Raw data is protected
* Outputs are clearly separated
* Logs help debugging
* Anyone else can understand your project

---

## Navigating Directories Safely

### Check where you are

```bash
pwd
```

Never run commands unless you know **exactly** where you are.

---

### Move between directories

```bash
cd project_name
cd data/raw
cd ..
cd ~
cd -
```

`cd -` is very useful when switching between two directories.

---

## Creating Directories Properly

### Create single directory

```bash
mkdir results
```

### Create nested directories

```bash
mkdir -p data/{raw,trimmed,filtered}
```

`-p` avoids errors if parent directories already exist.

---

## Understanding File Permissions (Very Important)

Run:

```bash
ls -l
```

Example output:

```text
-rw-r--r-- 1 gayatri bioinfo 2.1G sample_R1.fq.gz
drwxr-xr-x 2 gayatri bioinfo 4.0K scripts
```

Let’s decode this.

---

## Permission Structure Explained

### Example:

```text
-rw-r--r--
```

This has **three parts**:

### 1️⃣ File type (first character)

| Symbol | Meaning       |
| ------ | ------------- |
| `-`    | Regular file  |
| `d`    | Directory     |
| `l`    | Symbolic link |

---

### 2️⃣ Permission blocks (three groups)

```
rw-   r--   r--
│     │     │
│     │     └── Others
│     └──────── Group
└────────────── Owner
```

Each block can contain:

* `r` → read
* `w` → write
* `x` → execute

---

### What these mean in practice

#### For files:

| Permission | Meaning                              |
| ---------- | ------------------------------------ |
| `r`        | Can read file                        |
| `w`        | Can modify file                      |
| `x`        | Can execute file (scripts, binaries) |

#### For directories:

| Permission | Meaning                    |
| ---------- | -------------------------- |
| `r`        | Can list contents          |
| `w`        | Can create/delete files    |
| `x`        | Can enter (`cd`) directory |

👉 **Key point:**
Without `x` on a directory, you **cannot access files inside**, even if you have read permission.

---

## Common Permission Examples

### Script not executable

```bash
-rw-r--r-- run_pipeline.sh
```

Fix:

```bash
chmod +x run_pipeline.sh
```

Now:

```bash
-rwxr-xr-x run_pipeline.sh
```

---

### Read-only data (recommended for raw data)

```bash
chmod 444 data/raw/sample.fastq
```

Prevents accidental modification.

---

## Numeric Permission Mode (Short Form)

Permissions can also be set using numbers.

| Number | Meaning |
| ------ | ------- |
| 4      | read    |
| 2      | write   |
| 1      | execute |

Examples:

```bash
chmod 755 script.sh
chmod 644 file.txt
```

Interpretation:

* `755` → owner full access, others read+execute
* `644` → owner read/write, others read-only

---

## Ownership: User and Group

From `ls -l`:

```text
gayatri bioinfo
```

* `gayatri` → file owner
* `bioinfo` → group

---

### Change ownership (requires sudo)

```bash
sudo chown gayatri:bioinfo file.txt
```

Used when:

* files copied from another system
* Docker created root-owned files
* permission denied errors appear

---

## Permission Errors You Will See

### ❌ Permission denied

```text
bash: ./run_pipeline.sh: Permission denied
```

Fix:

```bash
chmod +x run_pipeline.sh
```

---

### ❌ Cannot write output

```text
Permission denied: results/output.txt
```

Check:

```bash
ls -ld results
```

Fix:

```bash
chmod u+w results
```

---

## Working with WSL Paths (Important)

Windows files are under:

```bash
/mnt/c/Users/YourUsername/
```

⚠️ Best practice:

* Keep analysis in Linux home (`/home/username`)
* Avoid heavy computation in `/mnt/c`

Reason:

* slower I/O
* permission inconsistencies
* unexpected crashes

---

## Safe Deletion Practices

Always preview:

```bash
ls files*
```

Then delete:

```bash
rm -i files*
```

For directories:

```bash
rm -r directory_name
```

Avoid:

```bash
rm -rf /
```

(Yes, this can destroy systems.)

---

## Logging and Output Redirection

Always save logs:

```bash
command > output.log 2>&1
```

Example:

```bash
fastqc sample.fq.gz > fastqc.log 2>&1
```

Logs help:

* debug failures
* track parameters
* reproduce results

---

## Best Practices Summary

* Understand permissions before changing them
* Never run pipelines as root
* Protect raw data (read-only)
* Use `chmod +x` for scripts
* Keep work inside Linux home
* Always generate logs

---

## Final Takeaway

Most Linux issues in bioinformatics are **not tool problems** —
they are **file permission and organization problems**.

Understanding these concepts early will save you **days of debugging later**.



