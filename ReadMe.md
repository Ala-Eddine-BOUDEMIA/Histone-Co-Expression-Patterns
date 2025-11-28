# Pan-Cancer Analysis of Histone Co-Expression Patterns

![Project Banner](/ReadMe_Images/Logos.png)

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.8%2B-blue)](https://www.python.org/)
[![R](https://img.shields.io/badge/R-4.0%2B-blue)](https://www.r-project.org/)


This repository contains the code and analysis for my Master's thesis project at **Institut Curie (UMR3664)**, conducted under the supervision of Dr. Geneviève Almouzni and Dr. Tina Karagyozova.

---


## 📌 Project Overview

While biochemical studies have characterized specific histone-chaperone interactions, a systematic understanding of their co-regulation across diverse biological contexts is still missing.

The objective of this project was to perform a large-scale, in-silico co-expression analysis to investigate two fundamental questions:
1.  How are histone variants and their chaperones transcriptionally regulated across different healthy tissues?
2.  How do these expression patterns change in the context of distinct cancers?

This study provides a first broad approach to characterizing these co-regulation patterns, unveiling how the transcription of these critical genes varies across tissues and disease states. The results serve as a foundational resource for identifying common regulatory principles and the potential relevance of individual variants and chaperones in disease.

### Key Achievements:
*   **Large-Scale Transcriptomic Analysis:** Systematically analyzed the expression of a curated set of histone and chaperone genes across **20,000+ RNA-Seq samples** from the TCGA (33 cancer types) and GTEx (53 healthy tissues) cohorts.
*   **Identified Tissue-Specific Signatures:** Demonstrated that distinct tissue types can be stratified based on their unique histone/chaperone expression patterns using unsupervised machine learning (PCA, UMAP, Hierarchical Clustering).
*   **Mapped Co-Expression Networks:** Constructed gene co-expression networks using WGCNA, revealing distinct modules of co-regulated genes and uncovering context-dependent changes between healthy and tumor states.
*   **Built a Reproducible Pipeline:** Developed a modular bioinformatics pipeline in Python and R for the processing, analysis, and visualization of large-scale transcriptomic data.

---

## 🔬 Key Scientific Findings

This analysis provided a broad, first-pass characterization of histone variant and chaperone co-regulation across healthy and diseased tissues. 
The main conclusions are summarized below:

*   **Tissue-Specific Expression Signatures:** 
    The expression patterns of histone variants and chaperones are distinct enough to stratify samples by their tissue of origin. 
    Non-linear dimensionality reduction methods (UMAP) were crucial for this, significantly outperforming linear methods like PCA. 
    While separation was strong, some intermixing occurred between tissues from similar systems (e.g., digestive tract), 
    likely reflecting shared cell types and developmental origins.

*   **Dysregulation and Loss of Specificity in Cancer:** 
    The ability to distinguish tissues was markedly reduced in the cancer (TCGA) context compared to healthy tissue (GTEx). 
    This is likely due to the de-differentiation of cancer cells. 
    A key finding was the loss of tissue-specific expression patterns, 
    such as the down-regulation of testis-specific histone variants in testicular cancers compared to their clear expression in normal testis.

*   **Identification of Co-Regulation Modules:** 
    Gene-level clustering revealed several major co-regulated groups. 
    A distinct and highly correlated module of **replicative histones** was consistently observed. 
    Another key module contained **replacement variants** and cell-cycle-associated chaperones (e.g., CENP-A, HJURP, and CAF-1 subunits), 
    suggesting co-regulation in a cell-cycle-dependent manner.

*   **Acknowledged Technical Limitations:** 
    The interpretation of these results is framed by a critical technical consideration: 
      the use of **poly(A) selection** in the library preparation for both GTEx and TCGA. 
      This protocol inherently leads to the under-detection of replicative histone transcripts (which lack poly(A) tails), 
      making it a major confounder that this analysis accounts for in its conclusions. 
      Other challenges included unbalanced sample distribution and heterogeneous metadata.

### For a full discussion, figures, and methodology, please see the complete M1 Internship Report PDF included in this repository.
---

## 🛠️ Usage & Pipeline

This project is a reproducible bioinformatics pipeline built with Python and R. The analysis is modular, allowing for the exploration of different gene sets and data subsets.

### 1. Installation
Clone the repository and install the required packages:
```
git clone https://github.com/Ala-Eddine-BOUDEMIA/Histone-Co-Expression-Patterns.git
cd Histone-Coexpression
pip install -r requirements.txt
```

2. Data Preparation

Raw data was sourced from the recount2 project. The initial processing step converts per-base coverage to read counts.
```
# Inside R environment
source("R_scripts/init_data.R")
```

3. Running the Analysis Pipeline
The pipeline is a series of Python scripts designed to be run sequentially. A central Config.py file allows for easy parameterization of datasets (GTEx/TCGA) and gene subsets.
```
# 1. Normalize raw counts to Counts Per Million (CPM)
python3 CPM.py

# 2. Generate data subsets for analysis (e.g., by tissue, top expressed genes)
python3 0_GenerateData.py

# 3. Run dimensionality reduction and clustering
python3 3_PCA.py
python3 5_UMAP.py
python3 7_Clustering.py

# ... and so on for the rest of the analysis scripts.
```
