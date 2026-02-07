# Working with BED, VCF, and GTF Files in Linux

## Overview

BED, VCF, and GTF files describe **genomic coordinates, variants, and annotations**.
They are used extensively in:
- variant calling
- genome annotation
- RNA-Seq analysis
- epigenomics and metagenomics

These files are text-based and are best handled using Linux command-line tools.

---

## BED Files (Genomic Intervals)

### BED Format Basics

Typical BED structure:
```text
chr1    1000    2000    region_1
chr1    3000    4000    region_2
````

Columns:

1. Chromosome
2. Start (0-based, inclusive)
3. End (0-based, exclusive)
4. Feature name (optional)

---

### Inspecting BED Files

View first few entries:

```bash
head regions.bed
```

---

Count regions:

```bash
wc -l regions.bed
```

---

Extract coordinates only:

```bash
cut -f1-3 regions.bed
```

---

Filter by chromosome:

```bash
awk '$1 == "chr1"' regions.bed
```

---

Sort BED file (recommended practice):

```bash
sort -k1,1 -k2,2n regions.bed > regions.sorted.bed
```

---

Calculate region lengths:

```bash
awk '{print $4, $3 - $2}' regions.bed
```

---

Calculate total genomic coverage:

```bash
awk '{sum += $3 - $2} END {print sum}' regions.bed
```

---

## VCF Files (Variants)

### VCF Format Basics

VCF files contain **variants** and metadata.

Example:

```text
##fileformat=VCFv4.2
#CHROM POS ID REF ALT QUAL FILTER INFO
chr1   879317  .  G   A   60   PASS   .
```

Important:

* Header lines start with `#`
* Data starts after the header

---

### Inspecting VCF Files

View header:

```bash
grep "^#" variants.vcf
```

---

View variants only:

```bash
grep -v "^#" variants.vcf | head
```

---

Count variants:

```bash
grep -v "^#" variants.vcf | wc -l
```

---

Extract key columns:

```bash
grep -v "^#" variants.vcf | cut -f1,2,4,5
```

---

Filter by chromosome:

```bash
grep -v "^#" variants.vcf | awk '$1 == "chr1"'
```

---

Filter by quality score:

```bash
grep -v "^#" variants.vcf | awk '$6 > 30'
```

---

Count variants per chromosome:

```bash
grep -v "^#" variants.vcf | cut -f1 | sort | uniq -c
```

---

## GTF Files (Gene Annotations)

### GTF Format Basics

GTF files describe gene and transcript annotations.

Example:

```text
chr1  source  gene  11869  14409  .  +  .  gene_id "ENSG00000223972";
```

Key columns:

1. Chromosome
2. Source
3. Feature type (gene, transcript, exon)
4. Start
5. End
6. Score
7. Strand
8. Frame
9. Attributes

---

### Inspecting GTF Files

View first entries:

```bash
head genes.gtf
```

---

Remove comment lines:

```bash
grep -v "^#" genes.gtf
```

---

Extract only genes:

```bash
awk '$3 == "gene"' genes.gtf
```

---

Count feature types:

```bash
cut -f3 genes.gtf | sort | uniq -c
```

---

Extract gene names:

```bash
grep 'gene_name' genes.gtf | cut -f9
```

---

Extract annotations from a chromosome:

```bash
awk '$1 == "chr1" && $3 == "gene"' genes.gtf
```

---

## Working with Compressed Files

Many BED/VCF/GTF files are compressed.

```bash
zcat file.vcf.gz | head
zgrep "^#" file.vcf.gz
```

---

## Common Mistakes

❌ Forgetting BED is 0-based
❌ Not sorting BED files before downstream tools
❌ Treating VCF headers as data
❌ Editing annotation files directly

---

## Best Practices

* Never modify original annotation files
* Always sort BED files
* Separate headers from data when processing VCF
* Use command-line tools instead of editors
* Document transformations in scripts or logs

---

## Summary

Understanding BED, VCF, and GTF formats is essential for real-world bioinformatics.
Linux tools allow you to:

* inspect large annotation files
* filter and summarize genomic data
* prepare inputs for downstream analysis

These skills are required for **variant analysis, RNA-Seq, and genome annotation workflows**.
