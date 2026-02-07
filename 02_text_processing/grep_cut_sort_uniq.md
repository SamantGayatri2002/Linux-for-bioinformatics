# Text Processing with grep, cut, sort, and uniq

## Overview

Bioinformatics data is mostly **text-based**:
- FASTA / FASTQ
- TSV / CSV tables
- BED, GTF, VCF files
- Log files and reports

Linux provides powerful text-processing tools that allow you to **filter, extract,
summarize, and inspect data without opening large files**.

This document covers four essential commands:
- `grep`
- `cut`
- `sort`
- `uniq`

Mastering these will dramatically speed up your daily work.

---

## grep — Searching Text Patterns

`grep` searches for lines that match a given pattern.

### Basic usage
```bash
grep "pattern" file.txt
````

Example:

```bash
grep "ERROR" analysis.log
```

---

### Case-insensitive search

```bash
grep -i "gene" annotations.txt
```

---

### Show line numbers

```bash
grep -n "chr1" variants.vcf
```

---

### Count matching lines

```bash
grep -c ">" sequences.fasta
```

Useful for counting FASTA sequences.

---

### Exclude matches

```bash
grep -v "^#" variants.vcf
```

Removes comment/header lines in VCF files.

---

### Search in multiple files

```bash
grep "TP53" *.tsv
```

---

### Context search (very useful for logs)

```bash
grep -C 3 "error" pipeline.log
```

Shows 3 lines before and after the match.

---

## cut — Extracting Columns

`cut` extracts specific columns from **tabular data**.

### Basic usage (TSV)

```bash
cut -f1 data.tsv
```

Extracts column 1.

---

### Multiple columns

```bash
cut -f1,3,5 data.tsv
```

---

### Column ranges

```bash
cut -f2-4 data.tsv
```

---

### Different delimiter (CSV)

```bash
cut -d',' -f1,2 data.csv
```

---

### Bioinformatics examples

Extract chromosome and coordinates from BED:

```bash
cut -f1-3 regions.bed
```

Extract gene names from GTF:

```bash
cut -f9 genes.gtf
```

---

## sort — Ordering Data

Sorting is required **before using uniq** and for ranking results.

### Alphabetical sort

```bash
sort genes.txt
```

---

### Numerical sort

```bash
sort -n values.txt
```

---

### Reverse sort

```bash
sort -r genes.txt
```

---

### Sort by column

```bash
sort -k2 data.tsv
```

---

### Numerical sort by column

```bash
sort -k2 -n expression.tsv
```

---

### Bioinformatics examples

Sort genes by expression (highest first):

```bash
sort -k2 -nr expression.tsv
```

Sort BED file by position:

```bash
sort -k1,1 -k2,2n regions.bed
```

---

## uniq — Removing or Counting Duplicates

⚠️ **Important rule:**
`uniq` works correctly **only on sorted input**.

---

### Remove duplicates

```bash
sort genes.txt | uniq
```

---

### Count occurrences

```bash
sort genes.txt | uniq -c
```

---

### Show only duplicates

```bash
sort genes.txt | uniq -d
```

---

### Show unique-only entries

```bash
sort genes.txt | uniq -u
```

---

### Bioinformatics examples

Count unique genes:

```bash
cut -f1 expression.tsv | sort | uniq | wc -l
```

Find most frequent gene:

```bash
cut -f1 expression.tsv | sort | uniq -c | sort -nr | head -n 1
```

---

## Combining Commands (Pipelines)

Linux commands become powerful when **chained using pipes (`|`)**.

Example:

```bash
cut -f1 data.tsv | sort | uniq
```

---

### Real-world pipeline example

Count variants per chromosome:

```bash
grep -v "^#" variants.vcf | cut -f1 | sort | uniq -c
```

---

### Filter and rank genes

```bash
awk '$2 > 50' expression.tsv | cut -f1,2 | sort -k2 -nr | head
```

---

## Common Mistakes to Avoid

❌ Using `uniq` without sorting:

```bash
uniq genes.txt
```

✅ Correct:

```bash
sort genes.txt | uniq
```

---

❌ Forgetting delimiters:

```bash
cut -f2 data.csv
```

✅ Correct:

```bash
cut -d',' -f2 data.csv
```

---

## Best Practices

* Always inspect input format first
* Sort before using `uniq`
* Use pipes instead of intermediate files
* Test commands on small data
* Prefer command-line tools over opening huge files

---

## Summary

`grep`, `cut`, `sort`, and `uniq` form the **foundation of Linux-based
bioinformatics data processing**.

If you can combine these confidently, you can:

* inspect large datasets quickly
* debug pipelines
* generate summaries
* avoid unnecessary scripts

These tools will be used **every single day** in real bioinformatics work.

```


