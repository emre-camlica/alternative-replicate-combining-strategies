# Alternative Replicate Combining Strategies

This repository contains code and analyses used to evaluate variant calling performance under different sequencing depths and input types (FASTQ vs. BAM), as presented in the accompanying manuscript.  
All evaluations were performed on human whole-genome sequencing (WGS) and whole-exome sequencing (WES) datasets using benchmark reference VCFs.

---

## 📁 Notebooks

### 1. `All.ipynb`

**Description**  
Analyzes variant calling performance in high-confidence genomic regions.  

- **WGS files** are filtered using the Genome in a Bottle (GIAB) high-confidence BED file:  
  https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/AshkenazimTrio/HG002_NA24385_son/latest/GRCh38/

- **WES files** are filtered using both the GIAB high-confidence BED file and the exome regions BED file:  
  https://hgdownload.soe.ucsc.edu/gbdb/hg38/exomeProbesets/S04380110_Covered.bb  

**Contents**
- Compares called variants against benchmark reference VCFs using the `cyvcf2` library.
- Computes standard metrics: Precision, Recall, and F1 Score.
- Generates bar charts to visualize performance across pipelines.
- Produces results for **All variants**, **SNPs only**, and **Indels only**.

**Input files**
- VCF files filtered for high-confidence regions (WGS and WES).
- Benchmark VCFs for evaluation.

---

### 2. `Difficult Regions.ipynb`

**Description**  
Evaluates variant calling performance in **difficult genomic regions** by applying the GIAB "difficult regions" BED filter in addition to the high-confidence filters:  
https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/genome-stratifications/v3.5/GRCh38@all/Union/GRCh38_alldifficultregions.bed.gz  

**Contents**
- Computes Precision, Recall, and F1 Score for all variants, SNPs, and Indels.
- Generates bar plots to highlight performance differences across difficult genomic regions.

**Input files**
- VCF files filtered by both high-confidence and difficult region filters.
- Benchmark reference VCF.

---

### 3. `Non Difficult Regions.ipynb`

**Description**  
Evaluates variant calling performance in the **complement of difficult regions** using the GIAB "non-difficult regions" BED file along with the high-confidence filters:  
https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/genome-stratifications/v3.5/GRCh38@all/Union/GRCh38_notinalldifficultregions.bed.gz  

**Contents**
- Computes Precision, Recall, and F1 Score for all variants, SNPs, and Indels.
- Generates bar plots to illustrate pipeline performance outside difficult regions.

**Input files**
- VCF files filtered by high-confidence and non-difficult region filters.
- Benchmark reference VCF.

---

### 4. `Jaccard and PCA.ipynb`

**Description**  
Generates global comparisons of variant profiles using **all variants only** (not SNP/Indel filtered).  

**Contents**
- Computes Jaccard similarity between variant sets.
- Visualizes a Jaccard distance heatmap using hierarchical clustering.
- Performs Principal Component Analysis (PCA) to reveal the global structure of variant profiles.

---

## 🔎 Filtering

### High-confidence & Exome Filtering
- **WGS files** are filtered using the GIAB high-confidence BED file.  
- **WES files** are filtered using both the GIAB high-confidence BED file and the exome regions BED file.  

This ensures that only variants within trusted callable regions (and within captured exome regions for WES) are included in the evaluation.

### SNP/Indel Filtering
For all three evaluation notebooks (`All.ipynb`, `Difficult Regions.ipynb`, `Non Difficult Regions.ipynb`), results are generated for **All variants**, **SNPs**, and **Indels**.  
Filtering is performed using `bcftools` as follows:

- **SNPs**  
  ```bash
  bcftools view -v snps "$file" -o "$output"

- **Indels**  
  ```bash
  bcftools view -v indels "$file" -o "$output"

---

### 5. `Variant Counts.ipynb`

**Description**  
Counts the total number of variants and computes the **True Positive (TP)**, **False Positive (FP)**, and **False Negative (FN)** counts for each VCF file.  
This notebook provides a more detailed breakdown of variant calling results beyond summary metrics.

**Contents**
- Computes and outputs:
  - Total number of variants.
  - TP, FP, and FN counts for each dataset.

**Input files**
- All evaluated VCF files (WGS and WES at multiple depths).
- Corresponding benchmark VCFs.

---

### 6. `Jaccard and PCA.ipynb`

**Description**  
Generates global comparisons of variant profiles using **all variants only** (not SNP/Indel filtered).  
This notebook includes the latest WES datasets at 50× and 100× coverage in addition to all previous datasets.

**Contents**
- Computes Jaccard similarity between variant sets.
- Visualizes a Jaccard distance heatmap using hierarchical clustering.
- Performs Principal Component Analysis (PCA) to reveal the global structure of variant profiles.

**Input files**
- All VCF files filtered by high-confidence (and exome regions for WES).

## 📦 Requirements 
To run the notebooks, the following Python packages can be installed:
  ```bash
  pip install cyvcf2 pandas numpy matplotlib seaborn scikit-learn

## License
This project is licensed under the MIT License – see the LICENSE file for details.

