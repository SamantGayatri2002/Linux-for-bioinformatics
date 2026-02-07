# WSL2 Memory and Resource Management for Bioinformatics Tools

## Overview

Many bioinformatics tools are **computationally intensive** and may fail on
laptops running WSL2 due to memory and resource constraints.

This issue is **not tool-specific**. It affects assemblers, classifiers,
binning tools, and large-scale analyses such as:

- MetaSPAdes
- geNomad
- WTCC
- MEGAHIT
- Flye
- large Nextflow pipelines

This document explains **how WSL2 manages resources**, how to configure it
safely, and how to adapt settings depending on the tool being run.

---

## Why Heavy Bioinformatics Tools Fail on WSL2

WSL2 runs Linux inside a lightweight virtual machine.
By default, it has **strict resource limits**.

Common causes of failures:
- Limited RAM available to WSL
- Very small default swap (~2 GB)
- High thread counts causing memory spikes
- Disk I/O overhead
- Running tools inside `/mnt/c`

When memory is exhausted, Linux **kills the process** without warning.

---

## Understanding WSL2 Resource Components

### 1. RAM
Physical memory allocated to WSL.

### 2. Swap
Disk-based virtual memory used when RAM is full.
- Slower than RAM
- Prevents crashes
- Essential for memory-heavy tools

### 3. CPU Threads
Each thread consumes:
- memory
- stack space
- I/O resources

More threads ≠ faster for memory-bound tools.

---

## The `.wslconfig` File (Global WSL Settings)

WSL resource limits are controlled from Windows using `.wslconfig`.

📍 Location:
```

C:\Users<WindowsUsername>.wslconfig

````

Example configuration:
```ini
[wsl2]
memory=16GB
swap=32GB
processors=4
````

---

## What This Configuration Means

| Setting      | Effect                           |
| ------------ | -------------------------------- |
| `memory`     | Maximum RAM WSL can use          |
| `swap`       | Virtual memory size              |
| `processors` | Max CPU threads visible to Linux |

⚠️ These settings apply **globally** to WSL, not per tool.

---

## Important Trade-Offs (Must Read)

### Setting `processors=4`

This means:

* Linux will see only 4 CPU cores
* Even if your system has 8, 16, or more cores
* Tools cannot use more than 4 threads

✔️ Good for:

* memory-heavy tools
* stability
* laptops with limited RAM

❌ Not ideal for:

* CPU-light but highly parallel tools
* alignment-heavy workflows

---

## What If a Tool Benefits from More Threads?

Examples:

* Read mapping
* Some QC tools
* Lightweight parallel tasks

### Option 1 — Temporarily Increase Threads

Edit `.wslconfig`:

```ini
processors=24
```

Restart WSL:

```powershell
wsl --shutdown
```

Then run the tool with:

```bash
-tool -t 24
```

---

### Option 2 — Remove Processor Limit (Recommended Default)

If your system has enough RAM, use:

```ini
processors=auto
```

(or remove the line entirely)

Control threads **per tool**, not globally.

---

## Choosing Threads Based on Tool Type

### Memory-bound tools (assemblies, binning)

* Use fewer threads (2–6)
* Increase swap
* Stability > speed

### CPU-bound tools (alignment, QC)

* Use more threads
* Ensure enough RAM
* Monitor system load

---

## Recommended Strategy for WSL Users

### Stable Default Configuration

```ini
[wsl2]
memory=16GB
swap=32GB
```

(no processor limit)

Then:

* Control threads using tool parameters
* Adjust `.wslconfig` only when necessary

---

## Verifying Resources Inside WSL

Check memory:

```bash
free -h
```

Check CPUs:

```bash
nproc
```

Check disk:

```bash
df -h
```

---

## Best Practices for Heavy Tools on WSL

* Never run heavy tools inside `/mnt/c`
* Keep ≥100 GB free disk space
* Monitor memory during runs
* Avoid running multiple heavy tools together
* Log tool output and errors
* Adjust resources thoughtfully

---

## Common Mistakes

❌ Setting very high thread counts
❌ Ignoring memory vs CPU balance
❌ Forgetting `.wslconfig` is global
❌ Assuming more threads = faster
❌ Running heavy tools without swap

---

## Summary

Resource failures on WSL are **system-level issues**, not tool bugs.

A correct approach involves:

* understanding RAM, swap, and threads
* configuring WSL globally with care
* tuning resources per tool

This mindset allows **any heavy bioinformatics tool** to run
reliably on WSL2 laptops.



