# Installing Ubuntu on Windows Using WSL (Windows Subsystem for Linux)

## Overview

Windows Subsystem for Linux (WSL) allows you to run a **full Linux environment**
directly on Windows without using a virtual machine or dual boot.

For bioinformatics users on Windows, WSL is the **recommended way** to:
- run Linux-only tools
- use Conda and Docker
- follow standard bioinformatics workflows

This guide explains **what WSL is, why it matters, and how to install Ubuntu correctly**.

---

## What Is WSL?

WSL is a feature developed by Microsoft that enables Windows users to run
Linux distributions (such as Ubuntu) natively.

With WSL:
- Linux runs alongside Windows
- No separate Linux UI is required
- You can use Bash directly from Windows Terminal

---

## Why WSL Is Important for Bioinformatics

- Most bioinformatics tools are Linux-based
- Many tools fail or behave unpredictably on Windows
- Tutorials and pipelines assume a Linux environment
- HPC and cloud systems also use Linux

WSL provides a **Linux-like environment on a Windows laptop**, making learning
and development much easier.

---

## System Requirements

Before installing WSL, ensure you have:

- Windows 10 or Windows 11
- Administrator access
- Active internet connection
- Virtualization enabled in BIOS (usually enabled by default)

---

## Step 1 — Enable Required Windows Features

WSL requires two Windows features to be enabled.

### How to enable

1. Press **Start** → search for **Windows Features**
2. Open **Turn Windows features on or off**
3. Enable the following:
   - ✅ Windows Subsystem for Linux
   - ✅ Virtual Machine Platform
4. Click **OK**
5. Restart your computer when prompted

---

## Step 2 — Install Ubuntu

You can install Ubuntu using **two methods**.

---

### Option A — Install Using PowerShell (Recommended)

1. Open **Windows Terminal / PowerShell as Administrator**
2. Run:
```powershell
wsl --install
````

What this command does:

* Enables WSL (if not already enabled)
* Installs WSL2
* Installs Ubuntu as the default distribution

After installation:

* Ubuntu launches automatically
* You will be asked to create:

  * Linux username
  * Linux password

⚠️ Use **lowercase letters only** for username.

---

### Option B — Install via Microsoft Store

Recommended only for **genuine Windows installations**.

Steps:

1. Open **Microsoft Store**
2. Search for **Ubuntu**
3. Select **Ubuntu 22.04 LTS**
4. Click **Install**
5. Launch Ubuntu
6. Create username and password

---

## Step 3 — Verify WSL Installation

Open Ubuntu and run:

```bash
lsb_release -a
```

Check WSL version from PowerShell:

```powershell
wsl -l -v
```

Ensure:

* Version = 2
* Status = Running or Stopped

---

## Understanding Linux Username vs Windows Username

Important distinction:

* Linux username ≠ Windows login
* Linux password is **not visible while typing**
* This is normal behavior

Never confuse the two.

---

## Where Your Files Are Stored

Inside Ubuntu:

```bash
/home/username/
```

Windows files are accessible at:

```bash
/mnt/c/Users/YourWindowsUsername/
```

⚠️ Best practice:

* Keep bioinformatics projects in Linux home directory
* Avoid running heavy analyses inside `/mnt/c`

---

## Updating Ubuntu (First Thing to Do)

After installation:

```bash
sudo apt update
sudo apt upgrade
```

This ensures system packages are up to date.

---

## Common Beginner Mistakes

❌ Running everything as root
❌ Working directly inside `/mnt/c`
❌ Forgetting Linux password
❌ Skipping system updates

---

## Best Practices

* Always work as a normal Linux user
* Use `sudo` only when required
* Keep projects inside `/home`
* Restart WSL if something behaves oddly:

```powershell
wsl --shutdown
```

---

## Summary

WSL allows Windows users to work in a **Linux-native bioinformatics environment**
without leaving Windows.

A correct WSL setup:

* prevents tool installation issues
* improves performance
* aligns with real-world bioinformatics systems

This setup is the foundation for everything that follows.




