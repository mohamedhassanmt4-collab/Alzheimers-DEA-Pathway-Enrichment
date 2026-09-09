# 🧠 Alzheimer's Brain Transcriptomics: DEA, Enrichment & Pathway Network Analysis

A complete R pipeline for analyzing differential gene expression in Alzheimer's disease brain tissue, from raw GEO data to biomarker candidate discovery.

## 📋 Overview

This project investigates gene expression differences between Alzheimer's disease (case) and control brain samples using public microarray data. The analysis spans **four regions of the brain** and multiple **biological contrasts** (sex and disease status), followed by functional enrichment and network-based pathway analysis to identify candidate biomarkers.

## 📊 Data Source

- **Primary dataset:** [GSE48350](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE48350) (NCBI GEO)
- Additional datasets were screened (`GSE5281`, `GSE15222`, `GSE11882`) and evaluated for metadata completeness; **GSE48350** was selected as the primary dataset based on missing-data quality checks.
- Brain regions analyzed: **Hippocampus (HC)**, **Superior Frontal Gyrus (SFG)**, **Post-Central Gyrus (PCG)**, **Entorhinal Cortex (EC)**

## 🔬 Pipeline Stages

### 1️⃣ Data Loading & Standardization
- Retrieval of expression sets directly from GEO via `GEOquery`
- Standardization of phenotype metadata (sex, brain region, disease status) across studies
- Sample counting and stratification checks (sex × disease × brain region)

### 2️⃣ Preprocessing
- Log2 transformation of expression values
- Brain region annotation and filtering
- Probe-to-gene symbol mapping (Gene Symbol / Entrez ID)
- Median collapsing for duplicate gene probes
- Construction of a clean, annotated `ExpressionSet`

### 3️⃣ Differential Expression Analysis (DEA)
Performed with `limma`, across **4 contrasts**:
| Contrast | Comparison |
|---|---|
| 1 | Female case vs. Female control |
| 2 | Male case vs. Male control |
| 3 | Male case vs. Female case |
| 4 | All cases vs. All controls |

Each contrast is computed both **across all tissue** and **per brain region** (HC, PCG, EC, SFG), producing region-specific and global differential expression results.

### 4️⃣ Visualization
- Custom **volcano plots** highlighting top up/down-regulated genes per contrast
- Dynamically generated plotting functions built via R metaprogramming (`make_expr_function`) to avoid code duplication across dozens of contrast/tissue combinations

### 5️⃣ Functional Enrichment Analysis
For significant DEGs (adjusted p < 0.05), enrichment was performed across three complementary databases:
- 🧬 **GO (Gene Ontology)** — Biological Process terms via `clusterProfiler`
- 🧪 **KEGG** pathways
- 🔄 **Reactome** pathways via `ReactomePA`

Includes bar plots and enrichment map (`emapplot`) visualizations for both up- and down-regulated gene sets.

### 6️⃣ Pathway Network Analysis & Biomarker Discovery
Using `pathfindR`:
- Active subnetwork-based pathway enrichment (Reactome, STRING interactome)
- Fuzzy clustering of enriched terms to identify representative pathways
- Heatmap visualization of top enriched terms across clusters
- Term-gene network diagrams for top pathways
- **Biomarker candidate extraction:** genes from representative pathways cross-referenced against the DEG list, ranked by adjusted p-value and fold change, and exported separately for up- and down-regulated candidates

## 🛠️ Tech Stack

| Category | Tools |
|---|---|
| Data retrieval | `GEOquery`, `Biobase` |
| Differential expression | `limma` |
| Enrichment analysis | `clusterProfiler`, `org.Hs.eg.db`, `ReactomePA`, `enrichplot` |
| Pathway networks | `pathfindR` |
| Visualization | `ggplot2`, `ggrepel`, `reshape2` |
| Data wrangling | `dplyr` |

## 📁 Output Structure

```
Results/DEA/          → Per-contrast, per-tissue differential expression tables (CSV)
Results/Reactome_STRING/ → pathfindR enrichment results
Cluster/               → Fuzzy-clustered enrichment terms
Biomarkers/            → Candidate biomarker gene lists (up/down-regulated)
Top10_Pathways/        → Individual pathway network diagrams (PDF)
```

## ▶️ How to Run

1. Clone this repository and open the `.Rmd` file in RStudio.
2. Install the required packages (see **Tech Stack** above) — most are available via `BiocManager::install()`.
3. Run chunks sequentially from the top — each stage saves intermediate results (`.RData`, `.csv`) so later stages can be re-run independently once earlier outputs exist.

## 👤 Author

**Mohamed Hassan**
Pharmacist (B.Pharm) | Bioinformatics Researcher
[LinkedIn](https://www.linkedin.com/in/mohamed-hassan-3772a6244/)

## 📝 Note

This is a personal learning/research project built while completing a bioinformatics diploma. Feedback and suggestions are welcome!

## 📄 License

This project is licensed under the MIT License.
