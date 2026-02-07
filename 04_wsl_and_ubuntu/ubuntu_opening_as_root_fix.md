# Ubuntu Opening as Root in WSL — Causes and Permanent Fix

## Overview

A common and confusing issue in WSL is Ubuntu opening directly as the `root` user
instead of the normal Linux user.

This behavior is **not normal** and can cause:
- permission problems
- accidental system changes
- broken Conda environments
- unsafe bioinformatics workflows

This guide explains **why this happens** and how to fix it correctly.

---

## Why Ubuntu Should NOT Run as Root

Running daily work as root is dangerous because:
- files may become owned by root
- permission errors appear later
- Conda and bioinformatics tools may break
- mistakes can affect the entire system

Best practice:
> Always work as a normal user and use `sudo` only when required.

---

## How to Check Which User You Are Using

Inside Ubuntu, run:
```bash
whoami
````

Expected output:

```text
your_username
```

Problematic output:

```text
root
```

---

## Scenario 1 — Ubuntu Opens as Root but User Already Exists

This happens when:

* the default user was not set correctly
* Ubuntu was launched using root at some point

### Step 1: Switch to your user

```bash
su - your_username
```

Enter your Linux password.

---

### Step 2: Set default user permanently (PowerShell)

Open **PowerShell / Windows Terminal** (not Ubuntu):

```powershell
ubuntu config --default-user your_username
```

OR

```powershell
wsl -d Ubuntu -u your_username
```

---

### Step 3: Verify

Restart Ubuntu and run:

```bash
whoami
```

---

## Scenario 2 — Ubuntu Opens as Root and No User Exists

This occurs when:

* Ubuntu installation was interrupted
* username was never created

---

### Step 1: Confirm root

```bash
whoami
```

---

### Step 2: Create a new user

```bash
adduser your_username
```

Follow prompts to set password.

---

### Step 3: Grant sudo privileges

```bash
usermod -aG sudo your_username
```

---

### Step 4: Exit Ubuntu

```bash
exit
```

---

### Step 5: Set default user (PowerShell)

```powershell
ubuntu config --default-user your_username
```

---

### Step 6: Verify

```bash
whoami
```

---

## Common Errors and Fixes

### Permission denied errors

Cause:

* files created earlier as root

Fix:

```bash
sudo chown -R your_username:your_username /home/your_username
```

---

### Conda not working

Cause:

* Conda installed as root

Fix:

* Remove root-owned Conda
* Reinstall Conda as normal user

---

## Best Practices

* Never run daily analysis as root
* Always verify user after Ubuntu launch
* Use `sudo` intentionally
* Fix ownership issues early

---

## Summary

If Ubuntu opens as root:

* it is fixable
* it should be fixed immediately
* it prevents long-term permission problems

Correct user configuration in WSL is **essential** for stable bioinformatics work.



