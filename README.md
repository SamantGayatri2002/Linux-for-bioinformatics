# 🐧 Linux for Bioinformatics

![Linux](https://img.shields.io/badge/Linux-Command%20Line-blue)
![Bioinformatics](https://img.shields.io/badge/Bioinformatics-Practical-green)
![WSL](https://img.shields.io/badge/WSL-Ubuntu-orange)
![Conda](https://img.shields.io/badge/Conda-Miniconda-blueviolet)
![Status](https://img.shields.io/badge/Status-Actively%20Maintained-success)

---

## 📌 About This Repository

**linux-for-bioinformatics** is a curated and structured collection of **Linux command-line notes, bioinformatics-specific workflows, installation guides, environment management practices, and real-world error fixes**.

This repository is built from **hands-on experience** gained while:
- working on bioinformatics projects  
- setting up tools on Linux & WSL  
- attending workshops and trainings  
- debugging real installation and runtime errors  

The goal is simple:  
> **Document what actually works — and why.**

---

## 🎯 Who Is This For?

This repository is especially useful for:

- 🧬 **Bioinformatics students**
- 🧪 **Bioinformatics interns**
- 💻 **Wet-lab researchers transitioning to computational work**
- 🧠 **Beginners learning Linux for bioinformatics**
- 🔧 Anyone tired of re-solving the same Linux & Conda errors

If you’ve ever thought *“I fixed this once… why is it broken again?”* — this repo is for you.

---

## 🧠 What Makes This Repo Different?

✔️ Focused on **bioinformatics use cases**, not generic Linux  
✔️ Explains **why errors happen**, not just copy-paste fixes  
✔️ Covers **WSL-specific issues** (often ignored online)  
✔️ Emphasizes **reproducibility & best practices**  
✔️ Written from a **learner’s perspective**, not a textbook  

---

## 📂 Repository Structure

```

linux-for-bioinformatics/
│
├── 01_linux_basics/                # Core Linux commands & navigation
├── 02_text_processing/             # grep, awk, sed, sort, uniq
├── 03_bioinformatics_file_formats/ # FASTA, FASTQ, BED, VCF handling
├── 04_wsl_and_ubuntu/              # WSL + Ubuntu setup & fixes
├── 05_package_management/          # Miniconda, Conda, environments
├── 06_git_and_github/              # Git & GitHub practical usage
├── 07_real_world_error_fixes/      # Debugging & system-level fixes
├── templates/                      # Reusable note templates
└── README.md

```

Each section is **modular**, so you can read only what you need.

---

## 🧩 Topics Covered

### 🐧 Linux & Shell
- File system navigation
- Permissions & ownership
- Command chaining, pipes & redirection
- Bash loops & automation basics

### 🧬 Bioinformatics on Linux
- FASTA / FASTQ manipulation
- BED / VCF / GTF processing
- Large file handling
- Log monitoring & sanity checks

### 🪟 WSL & Ubuntu
- Installing Ubuntu on Windows (WSL2)
- Common WSL pitfalls
- Fixing Ubuntu opening as root
- Memory & performance considerations

### 📦 Environment Management
- Why Miniconda (not Anaconda)
- Conda environments & channels
- Dependency conflicts (“dependency hell”)
- Conda vs Docker (when to use what)

### 🧯 Real-World Error Fixes
- MetaSPAdes memory crashes on WSL2
- Swap configuration & RAM limits
- Thread vs memory trade-offs
- Safe system-level debugging

### 🌐 Git & GitHub
- Cloning repositories
- Detaching from upstream repos
- Pushing to private repositories
- Safe Git practices for research

---

## 🚦 Status of the Repository

- 🔄 **Actively updated**
- 🧱 Structure is stable
- 📝 Content grows as new tools & errors are encountered

This is a **living repository**, not a finished textbook.

---

## ⚠️ Important Notes

- This repository does **not** contain confidential or proprietary data  
- Commands are tested, but **always read before running**
- System-specific paths and usernames are generalized intentionally
- Use `sudo` carefully — understand before executing

---

## 🤝 Contributions

This repo is primarily a personal knowledge base, but:
- suggestions
- corrections
- improvements  

are always welcome via issues or pull requests.

---

## 📜 License

This repository is shared for **educational and learning purposes**.  
Feel free to use, adapt, and learn — with attribution where appropriate.

---

## ⭐ Final Note

> *“If it’s not written down, it will be forgotten.”*

This repository exists so future-me — and others — don’t have to fight the same battles again.

