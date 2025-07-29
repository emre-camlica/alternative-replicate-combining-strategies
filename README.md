# Alternative Replicate Combining Strategies

This repository contains code and analysis used for evaluating variant calling performance under different sequencing depths and input types (FASTQ vs. BAM), as presented in the accompanying manuscript. All analyses were performed on human whole-genome sequencing (WGS) and whole-exome sequencing (WES) datasets using benchmark reference VCFs.

The results are organized into three Jupyter Notebook files, each focusing on different aspects of the evaluation.

---

## 📁 Notebooks

### 1. `High_Confidence_Regions.ipynb`

**Description**:  
Analyzes variant calling performance in high-confidence genomic regions.  

- All WGS files are filtered using only the high-confidence BED file provided by the Genome in a Bottle (GIAB) consortium:  
  https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/AshkenazimTrio/HG002_NA24385_son/latest/GRCh38/

- All WES files are filtered using both the GIAB high-confidence BED file and the exome regions BED file:  
  https://hgdownload.soe.ucsc.edu/gbdb/hg38/exomeProbesets/S04380110\_Covered.bb

**Contents**:
- Compares called variants against the benchmark reference VCF using the `cyvcf2` library.
- Computes standard evaluation metrics: Precision, Recall, and F1 Score.
- Generates bar charts showing performance for each pipeline.
- Includes Principal Component Analysis (PCA) to visualize the global structure of variant profiles.
- Computes and visualizes a Jaccard similarity heatmap using hierarchical clustering.

**Input files**:
- VCF files filtered for high-confidence regions (WGS and WES).
- Benchmark VCFs for high-confidence evaluation.

---

### 2. `Difficult_Regions.ipynb`

**Description**:  
In addition to the high-confidence filters described above, all VCFs in this notebook were further filtered using the GIAB "difficult regions" BED file:  
https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/genome-stratifications/v3.5/GRCh38%40all/Union/GRCh38\_alldifficultregions.bed.gz

**Contents**:
- Computes Precision, Recall, and F1 Score for each pipeline by comparing called variants to the benchmark.
- Generates bar plots to visualize performance trends across difficult genomic regions.

**Input files**:
- VCF files filtered by both high-confidence and difficult region filters.
- Corresponding benchmark reference VCF.

---

### 3. `SNPs_Indels_Comparison.ipynb`

**Description**:  
In this notebook, SNP and Indel variants are evaluated separately. VCF files are first filtered using the high-confidence BED file and then filtered by variant type using `bcftools`.

**Filtering Commands**:
- **SNPs**:  
  `bcftools view -v snps "$file" -o "$output"`
- **Indels**:  
  `bcftools view -v indels "$file" -o "$output"`

**Contents**:
- Computes and compares performance metrics separately for SNPs and Indels.
- Generates bar plots to illustrate differences between variant types across pipelines.

---

## 📦 Requirements

To run the notebooks, the following Python packages are required:

```bash
pip install cyvcf2 pandas numpy matplotlib seaborn scikit-learn
