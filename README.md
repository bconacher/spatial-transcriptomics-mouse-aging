# Spatial Transcriptomics of Mouse Brain Development and Aging

This repository contains the analysis code for the manuscript:

**"Spatial Transcriptomics Reveals Regional and Temporal Dynamics of Gene Expression in the Mouse Brain Across Development and Aging"**

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
  - **Adult (12–15 weeks)**
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

### 📊 Data Access

All raw and processed data are available at NCBI GEO: [GSE287202](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE287202)
