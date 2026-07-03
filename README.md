# RNA-seq Preprocessing Pipeline: SARS-CoV-2 Age Comparison in Mice

## Overview
This project implements a complete RNA-seq preprocessing pipeline, from raw reads in NCBI SRA through to a gene-level count matrix ready for differential expression analysis. The dataset compares the transcriptomic response to SARS-CoV-2 infection between young and mid-age mice, with PBS vehicle controls for each age group.

## Biological Question
How does the host transcriptomic response to SARS-CoV-2 infection differ between young and mid-age mice? Age is a major risk factor for severe COVID-19 disease outcomes, and this mouse model provides a controlled system to study the age-dependent molecular basis of that difference.

## Data
Four RNA-seq samples from NCBI SRA:

| Sample | SRA Accession | Description |
|---|---|---|
| Young + SARS-CoV-2 | SRR24206824 | Young mice, infected |
| Mid-age + SARS-CoV-2 | SRR24206825 | Mid-age mice, infected |
| Young + PBS | SRR24206826 | Young mice, vehicle control |
| Mid-age + PBS | SRR24206827 | Mid-age mice, vehicle control |

All data downloads automatically within the notebook using SRA Toolkit. Reads are subsampled to 5 million per sample for computational feasibility.

## Methods & Tools

**Command-line bioinformatics tools (installed via apt/pip within the notebook):**

| Tool | Stage | Purpose |
|---|---|---|
| SRA Toolkit (`fastq-dump`) | Data download | Download FASTQ reads from NCBI SRA |
| FastQC | Quality control | Per-read quality assessment (pre- and post-trim) |
| Cutadapt | Trimming | 5' hard trim (200bp) + 3' quality trim (Phred < 28) |
| HISAT2 | Alignment | Splice-aware alignment to mm10 reference genome |
| featureCounts (Subread) | Quantification | Gene-level read count matrix from SAM files |

**Python analysis:**
- `pandas` — count matrix loading and inspection
- `matplotlib` — library size comparison plot

**Reference:** mm10 (GRCm38), Ensembl release 102 gene annotation

## Output
`gene_counts_SARS_CoV2_age_comparison.csv` — a gene × sample count matrix with one row per Ensembl gene ID, ready for differential expression analysis with PyDESeq2 or DESeq2 (R).

## How to Run

**Google Colab (recommended — all tools install automatically)**
1. Upload `RNAseq_Preprocessing_Pipeline.ipynb` to Google Colab
2. Run all cells top to bottom
3. Expect total runtime of ~30–60 minutes (dominated by SRA download and HISAT2 alignment)

**Note on compute:** HISAT2 alignment is set to `-p 8` threads. Colab provides up to 2 CPUs by default — adjust this value based on available hardware.

## Project Structure
```
rnaseq-preprocessing-pipeline/
├── README.md
├── RNAseq_Preprocessing_Pipeline.ipynb
└── requirements.txt
```

## Connection to Differential Expression Analysis
The count matrix produced by this pipeline feeds directly into a DESeq2-style differential expression analysis. For an example of that downstream analysis in Python using PyDESeq2, see the companion repository `differential-gene-expression`.
