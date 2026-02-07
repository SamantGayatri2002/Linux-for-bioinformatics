# Text Processing with awk and sed for Bioinformatics

## Overview

In bioinformatics, data is often stored in **large, structured text files**
(FASTA, FASTQ, BED, GTF, VCF, TSV).
Opening these files in editors is slow or impossible.

`awk` and `sed` allow you to:
- filter rows based on conditions
- extract and transform columns
- perform calculations
- modify text streams on the fly

These tools are essential for **fast, reproducible, command-line workflows**.

---

## awk — The Column-Aware Power Tool

`awk` works line by line and is especially powerful for **column-based data**.

### Basic structure
```bash
awk 'pattern { action }' file
````

If `pattern` is true, `action` is executed.

---

## Understanding Fields and Records

* `$1, $2, $3 ...` → columns
* `$0` → entire line
* `NR` → line number
* `NF` → number of columns

Example:

```bash
awk '{print $1, $3}' data.tsv
```

---

## Basic Examples

### Print specific columns

```bash
awk '{print $1, $2}' expression.tsv
```

---

### Add line numbers

```bash
awk '{print NR, $0}' file.txt
```

---

### Filter rows by condition

```bash
awk '$2 > 100' expression.tsv
```

---

### Skip header row

```bash
awk 'NR > 1' data.tsv
```

---

## Bioinformatics Examples with awk

### Count FASTA sequences

```bash
awk '/^>/{count++} END {print count}' sequences.fasta
```

---

### Calculate average expression

```bash
awk '{sum += $2; n++} END {print sum/n}' expression.tsv
```

---

### Filter BED regions by length

```bash
awk '{ if ($3 - $2 > 1000) print }' regions.bed
```

---

### Extract genes from a chromosome

```bash
awk '$1 == "chr17"' variants.vcf
```

---

### Convert FASTQ to FASTA

```bash
awk 'NR%4==1{print ">"substr($0,2)} NR%4==2{print}' sample.fastq > sample.fasta
```

---

## Working with Delimiters

### Tab-separated files (default)

```bash
awk '{print $1, $3}' file.tsv
```

---

### CSV files

```bash
awk -F',' '{print $1, $2}' file.csv
```

---

### Custom output delimiter

```bash
awk '{print $1 "\t" $2}' file.txt
```

---

## Associative Arrays (Very Powerful)

### Count occurrences

```bash
awk '{count[$1]++} END {for (i in count) print i, count[i]}' genes.txt
```

---

### Average expression per gene

```bash
awk '{sum[$1]+=$2; n[$1]++} END {for (g in sum) print g, sum[g]/n[g]}' expression.tsv
```

---

## sed — Stream Editor

`sed` is used to **edit text streams**, not columns.

Common uses:

* replace text
* delete lines
* extract line ranges

---

## Basic sed Syntax

```bash
sed 'command' file
```

---

## Common sed Operations

### Replace text (first occurrence)

```bash
sed 's/old/new/' file.txt
```

---

### Replace all occurrences

```bash
sed 's/old/new/g' file.txt
```

---

### Delete lines matching a pattern

```bash
sed '/^#/d' file.txt
```

Used to remove headers or comments.

---

### Delete specific line

```bash
sed '1d' file.txt
```

Deletes the first line (header).

---

### Print matching lines only

```bash
sed -n '/pattern/p' file.txt
```

---

## Bioinformatics Examples with sed

### Remove FASTA headers

```bash
sed '/^>/d' sequences.fasta
```

---

### Extract only FASTQ sequences

```bash
sed -n '2~4p' sample.fastq
```

---

### Convert FASTQ to FASTA

```bash
sed -n '1~4s/^@/>/p;2~4p' sample.fastq > sample.fasta
```

---

### Remove empty lines

```bash
sed '/^$/d' file.txt
```

---

## Combining awk and sed

### Remove header and filter by value

```bash
sed '1d' data.tsv | awk '$2 > 50'
```

---

### Count genes per chromosome

```bash
cut -f1 variants.vcf | sed '/^#/d' | sort | uniq -c
```

---

## When to Use awk vs sed

| Task                     | Use       |
| ------------------------ | --------- |
| Column-based logic       | awk       |
| Mathematical operations  | awk       |
| Simple text substitution | sed       |
| Line deletion/extraction | sed       |
| FASTQ/FASTA conversion   | awk / sed |

---

## Common Mistakes

❌ Forgetting delimiters:

```bash
awk '{print $1}' file.csv
```

✅ Correct:

```bash
awk -F',' '{print $1}' file.csv
```

---

❌ Editing files in-place without backup:

```bash
sed -i 's/old/new/g' file.txt
```

⚠️ Use carefully or add backup:

```bash
sed -i.bak 's/old/new/g' file.txt
```

---

## Best Practices

* Test awk/sed commands on small files first
* Avoid in-place edits unless necessary
* Prefer pipes over temporary files
* Document complex commands in scripts
* Use awk for logic, sed for cleanup

---

## Summary

`awk` and `sed` allow you to process massive bioinformatics datasets efficiently
without writing full scripts.

Once comfortable with these tools, you can:

* debug pipelines faster
* avoid unnecessary Python/R scripts
* work directly on servers and HPC systems

They are **core skills** for any serious bioinformatician.


