# Common WSL Issues and Best Practices for Bioinformatics

## Overview

WSL is extremely powerful, but it is **not identical to a native Linux system**.
Many bioinformatics issues on Windows laptops come from misunderstandings
about how WSL works under the hood.

This document covers:
- common WSL problems
- why they happen
- how to fix or avoid them
- best practices for stable bioinformatics workflows

---

## Understanding WSL2 Limitations

WSL2 runs Linux inside a lightweight virtualized environment.
Because of this:

- RAM is limited by Windows
- swap space is small by default
- disk I/O behaves differently
- performance depends on configuration

These factors directly affect bioinformatics tools.

---

## Issue 1 — Out of Memory Errors

### Symptoms
- Tools suddenly stop
- Process gets killed
- Messages like:
```text
Killed
Out of memory
````

### Why this happens

* WSL2 limits available RAM
* Default swap size is only ~2 GB
* Bioinformatics tools (e.g. assemblers) are memory-hungry

### Quick checks

```bash
free -h
```

---

### Best practice

* Use fewer threads
* Monitor memory usage
* Avoid running multiple heavy tools simultaneously

Advanced memory tuning (swap configuration) is covered separately.

---

## Issue 2 — Poor Performance in `/mnt/c`

### Symptoms

* Tools run very slowly
* Pipelines feel sluggish
* Unexpected hangs

### Cause

* `/mnt/c` accesses Windows NTFS filesystem
* Linux tools are optimized for ext4 (Linux filesystem)

### Solution

✔️ Always keep projects inside:

```bash
/home/username/
```

❌ Avoid:

```bash
/mnt/c/Users/...
```

---

## Issue 3 — Permission Errors

### Symptoms

```text
Permission denied
```

### Common causes

* Running tools as root
* Files created by Windows tools
* Mixing users

### Fix

Check ownership:

```bash
ls -l
```

Fix ownership:

```bash
sudo chown -R username:username project_folder
```

---

## Issue 4 — Conda Environments Break

### Symptoms

* Conda activate fails
* Tools not found
* Permission errors

### Causes

* Conda installed as root
* Mixing system Python and Conda
* Broken environment paths

### Best practice

* Install Conda as **normal user**
* Never use `sudo conda`
* Use one Conda env per project

---

## Issue 5 — WSL Becomes Unresponsive

### Symptoms

* Ubuntu doesn’t open
* Commands freeze
* Terminal hangs

### Fix

From PowerShell:

```powershell
wsl --shutdown
```

Then reopen Ubuntu.

---

## Issue 6 — Disk Space Issues

### Symptoms

* Tools fail without clear errors
* Writes stop mid-run

### Check disk usage

```bash
df -h
du -sh *
```

### Best practice

* Keep at least 50–100 GB free
* Assemblers need a lot of temporary space

---

## Recommended WSL Workflow for Bioinformatics

1. Install Ubuntu on WSL2
2. Update system regularly
3. Install Miniconda (user-level)
4. Create per-project Conda environments
5. Keep projects in Linux home
6. Redirect logs for every major tool
7. Monitor memory and disk usage

---

## What NOT to Do in WSL

❌ Run pipelines as root
❌ Work directly in `/mnt/c`
❌ Ignore memory limits
❌ Install tools system-wide without reason
❌ Skip logs

---

## Best Practices Summary

* Treat WSL like a real Linux system
* Respect memory and disk limits
* Use fewer threads for stability
* Keep environments clean and isolated
* Restart WSL when in doubt

---

## Final Takeaway

Most WSL bioinformatics problems are **environment issues**, not tool issues.

A properly configured WSL setup can handle **serious bioinformatics work**
when used with discipline and awareness.


This is where your repo becomes *industry-relevant* 📦🧬
```
