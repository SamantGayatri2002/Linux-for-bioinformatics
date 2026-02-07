# Handling FASTA and FASTQ Files in Linux

## Overview

FASTA and FASTQ are the most commonly used file formats in bioinformatics.
Almost every sequencing-based analysis starts with these files.

Because FASTA/FASTQ files are often **very large**, Linux command-line tools
are the preferred way to inspect, validate, and manipulate them.

This document focuses on **practical Linux-based handling** of FASTA and FASTQ files.

---

## FASTA Format Basics

FASTA format structure:

```text
>sequence_id description
ATCGATCGATCG
ATCGATCGATCG
````

Key points:

* Header lines start with `>`
* Sequence lines contain nucleotides or amino acids
* Sequences may span multiple lines

---

## FASTQ Format Basics

FASTQ format structure (4 lines per read):

```text
@read_id
ATCGATCGATCG
+
IIIIIIIIIIII
```

Meaning:

1. Header line (`@`)
2. Sequence
3. Separator (`+`)
4. Quality scores

⚠️ FASTQ files **must always have lines in multiples of 4**.

---

## Inspecting FASTA Files

### Count sequences

```bash
grep -c "^>" sequences.fasta
```

---

### View headers only

```bash
grep "^>" sequences.fasta
```

---

### View sequences only

```bash
grep -v "^>" sequences.fasta
```

---

### Extract a specific sequence

```bash
grep -A 1 "seq_id" sequences.fasta
```

---

### Calculate total sequence length

```bash
grep -v "^>" sequences.fasta | tr -d '\n' | wc -c
```

---

## Inspecting FASTQ Files

### Count number of reads

```bash
echo $(wc -l < sample.fastq)/4 | bc
```

---

### View first read

```bash
head -n 4 sample.fastq
```

---

### Extract only sequences

```bash
sed -n '2~4p' sample.fastq
```

---

### Extract quality scores

```bash
sed -n '4~4p' sample.fastq
```

---

## Working with Compressed Files (`.gz`)

Most FASTQ files are compressed.

### View compressed FASTQ

```bash
zcat sample.fastq.gz | head
```

---

### Count reads in compressed FASTQ

```bash
echo $(zcat sample.fastq.gz | wc -l)/4 | bc
```

---

### Search patterns in compressed FASTA/FASTQ

```bash
zgrep "^>" sequences.fasta.gz
```

---

## Converting FASTQ to FASTA

### Using awk

```bash
awk 'NR%4==1{print ">"substr($0,2)} NR%4==2{print}' sample.fastq > sample.fasta
```

---

### Using sed

```bash
sed -n '1~4s/^@/>/p;2~4p' sample.fastq > sample.fasta
```

---

## Splitting Multi-FASTA Files

Split one FASTA into multiple single-sequence files:

```bash
awk '/^>/{f=++i".fasta"} {print > f}' sequences.fasta
```

---

## Filtering Sequences by Length

### Remove short sequences (<500 bp)

```bash
awk 'BEGIN{RS=">"; ORS=""} length($2)>=500 {print ">"$0}' sequences.fasta > filtered.fasta
```

---

## Validating FASTQ Format

Check line count:

```bash
wc -l sample.fastq
```

Must be divisible by 4.

---

Check first few reads:

```bash
head -n 40 sample.fastq
```

Look for:

* proper headers
* matching sequence and quality lengths

---

## Common FASTA/FASTQ Mistakes

❌ Editing raw files directly
❌ Running tools without checking format
❌ Assuming FASTQ is valid
❌ Forgetting compression handling

---

## Best Practices

* Always keep raw FASTA/FASTQ untouched
* Work on copies or derived files
* Validate format before analysis
* Use `zcat` / `zgrep` for `.gz` files
* Log every transformation step

---

## Summary

Linux command-line tools allow you to:

* inspect FASTA/FASTQ safely
* extract relevant information
* validate data integrity
* prepare files for downstream analysis

These skills are essential before running **any bioinformatics pipeline**.



