# Spatial Transcriptomics of Mouse Brain Development and Aging

This repository contains the analysis code for the manuscript:

**"Spatial Transcriptomics Reveals Regional and Temporal Dynamics of Gene Expression in the Mouse Brain Across Development and Aging"**

### 📥 Download Processed Dataset

**[Download the annotated Seurat object (v1.0)](https://github.com/bconacher/spatial-transcriptomics-mouse-aging/releases/tag/v1.0-processed-data)**

The processed Seurat object (`mouse_brain_aging_annotated.rds`, ~276 MB) contains all six mouse brain samples, 20,336 spatial spots, and the final anatomical brain-region annotations used in our analysis.

---

## 🧠 Overview

This project investigates region-specific gene expression changes across brain development and aging in mice using spatial transcriptomics. We focus on transcriptional dynamics across key brain regions including the hippocampus, cortex, thalamus, hypothalamus, and striatum, leveraging the 10x Genomics Visium platform.

Key findings highlight:
- **Neurogenesis, gliogenesis, and myelination** during development.
- **Neuroinflammation and glial activation** during aging.
- Distinct transcriptomic patterns in hippocampal subregions (DG, CA1-2, CA3, matrix).

---

## 🐭 Experimental Design

- **Mouse Strain**: C57BL/6J
- **Biological Replicates**: 2 per group
- **Groups**:
  - **Postnatal Day 21 (P21)**
  - **Adult (3 months)**
  - **Aged (28 months)**
- **Tissue Preparation**: Formalin-fixed paraffin-embedded (FFPE) brain sections.

---

## 🧬 Spatial Transcriptomics

- **Platform**: 10x Genomics Visium
  - **P21 & Adult**: Visium Spatial Gene Expression v1
  - **Aged**: Visium Spatial Gene Expression v2 with CytAssist

- **Sequencing**: Illumina NovaSeq 6000 (~300 million read pairs/sample)
- **Data Processing**:
  - Aligned with **Space Ranger v2.0.0** (P21/Adult) and **v3.1.1** (Aged)
  - Downstream analysis in **R** using **Seurat v5.1.0**

---

## 📊 Analysis Pipeline

1. **Preprocessing**:
   - Spot filtering (<250 genes, <500 UMIs removed)
   - CPM normalization and scaling
2. **Dimensionality Reduction**:
   - PCA and Harmony for batch correction
   - UMAP visualization
3. **Clustering**:
   - SNN graph, resolution 0.075
   - Anatomical region annotation based on Allen Brain Atlas
4. **Differential Expression Analysis**:
   - DEGs identified with Wilcoxon Rank Sum test
   - GO enrichment via **clusterProfiler**
5. **Hippocampal Subregion Analysis**:
   - Subclustering into DG, CA1-2, CA3, Matrix
   - Trend analysis across P21 → Adult → Aged

---

## 📂 Repository Structure

- **analysis/**
  - `mouse_brain_aging_analysis.Rmd`: 
    - Complete analysis pipeline and figures for the main dataset including:
      - Preprocessing: Filtering, normalization, batch correction
      - Clustering and dimensionality reduction (PCA, UMAP, Harmony)
      - Differential gene expression (P21 vs Adult, Aged vs Adult)
      - Gene Ontology enrichment (whole brain regions)
  - `hippocampus_subclustering_analysis.Rmd`: 
    - Focused analysis of hippocampal subregions and figures:
      - Subclustering into DG, CA1-2, CA3, Matrix
      - Region-specific DEG analysis
      - Subregion GO enrichment and trend analysis across development and aging
---

## 📊 Data Availability

### Processed and Annotated Seurat Object

The final annotated Seurat object is available for download from [GitHub Releases (v1.0-processed-data)](https://github.com/bconacher/spatial-transcriptomics-mouse-aging/releases/tag/v1.0-processed-data).

**File:** `mouse_brain_aging_annotated.rds` (~276 MB)

The object contains:
- Six mouse brain spatial transcriptomics samples (P21, adult, and aged; two biological replicates each)
- 20,336 spatial spots
- Eight annotated anatomical brain regions
- Refined hypothalamic subclustering
- Raw counts and normalized expression layers
- PCA, Harmony, and UMAP reductions
- Spatial images and spot-level metadata

The object was reduced in size by removing intermediate scaled-expression layers while retaining the saved anatomical annotations.

### Loading the Processed Dataset

```r
library(Seurat)

obj <- readRDS("mouse_brain_aging_annotated.rds")

# Inspect annotations
table(obj$combinedCluster)

# Visualize annotated regions
DimPlot(obj, reduction = "umap", group.by = "combinedCluster", label = TRUE)

# Visualize anatomical assignments on tissue
SpatialDimPlot(obj, images = "aged_1", group.by = "combinedCluster")
```

### Reproducibility Notes

Anatomical regions were annotated using transcriptomic clustering, spatial coordinates, and anatomical references from the Allen Brain Atlas. Some region assignments were manually refined through interactive selection in Seurat. Consequently, running the processing script from raw data may not reproduce identical anatomical assignments without those manual selections.

The released Seurat object preserves the final annotations used in downstream analyses. The intermediate scaled-expression layers were excluded to reduce file size and can be regenerated if required. The original analysis scripts are retained to document the processing workflow; they are not a fully self-contained reconstruction of the interactive annotation steps.

### Raw Data

Raw and processed data are available through NCBI GEO: [GSE287202](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE287202).
