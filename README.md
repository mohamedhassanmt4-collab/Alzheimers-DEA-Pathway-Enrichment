# 🧠 Alzheimer's Brain Transcriptomics: DEA, Enrichment & WGCNA Analysis

A collection of R pipelines analyzing Alzheimer's disease gene expression data — from differential expression and functional enrichment to weighted gene co-expression network analysis (WGCNA) — using public GEO datasets.

## 📁 Repository Contents

| File | Analysis Type | Dataset |
|---|---|---|
| `Alzheimers-brain-DEA-enrichment-analysis.Rmd` | Differential expression, functional enrichment (GO/KEGG/Reactome), and pathway network analysis (pathfindR) | GSE48350 |
| `brain-GSE48350-WGCNA-analysis.Rmd` | Weighted Gene Co-expression Network Analysis (WGCNA) | GSE48350 |

Both analyses use the same source dataset but apply different, complementary methodologies to explore gene expression patterns associated with Alzheimer's disease.

## 📊 Data Source

- **Dataset:** [GSE48350](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE48350) (NCBI GEO)
- Additional datasets were initially screened (`GSE5281`, `GSE15222`, `GSE11882`) and evaluated for metadata completeness; **GSE48350** was selected based on missing-data quality checks.
- Brain regions covered: **Hippocampus (HC)**, **Superior Frontal Gyrus (SFG)**, **Post-Central Gyrus (PCG)**, **Entorhinal Cortex (EC)**

---

## 🔬 Pipeline 1: Differential Expression & Enrichment Analysis

`Alzheimers-brain-DEA-enrichment-analysis.Rmd`

### Stages
1. **Data loading & standardization** — retrieval via `GEOquery`, phenotype metadata cleanup (sex, brain region, disease status)
2. **Preprocessing** — log2 transform, probe-to-gene mapping, median collapsing of duplicate probes
3. **Differential Expression Analysis (DEA)** with `limma`, across 4 contrasts:

   | Contrast | Comparison |
   |---|---|
   | 1 | Female case vs. Female control |
   | 2 | Male case vs. Male control |
   | 3 | Male case vs. Female case |
   | 4 | All cases vs. All controls |

   Each contrast computed both across all tissue and per brain region.
4. **Visualization** — custom volcano plots (top up/down-regulated genes per contrast), built via dynamically generated plotting functions
5. **Functional enrichment** — GO (Biological Process), KEGG, and Reactome pathway enrichment via `clusterProfiler` / `ReactomePA`
6. **Pathway network analysis & biomarker discovery** — active subnetwork enrichment via `pathfindR`, fuzzy clustering of enriched terms, term-gene network diagrams, and biomarker candidate extraction (ranked by adjusted p-value and fold change)

---

## 🕸️ Pipeline 2: WGCNA (Weighted Gene Co-expression Network Analysis)

`brain-GSE48350-WGCNA-analysis.Rmd`

### Stages
1. **Data loading** — expression matrix and phenotype data retrieved via `GEOquery`
2. **Quality control** — outlier gene/sample detection (`goodSamplesGenes`), hierarchical clustering and PCA to flag outlier samples for exclusion
3. **Normalization & trait preparation** — colData cleanup, sample alignment between expression and phenotype data
4. **Network construction** — soft-thresholding power selection via scale-free topology fit, followed by `blockwiseModules` to build the co-expression network and detect gene modules
5. **Module-trait relationships** — correlation of module eigengenes with sample traits (disease state, gender), visualized as a correlation heatmap (`CorLevelPlot`)
6. **Intramodular analysis** — module membership and gene significance calculations to identify driver genes within trait-associated modules

---

## 🛠️ Tech Stack

| Category | Tools |
|---|---|
| Data retrieval | `GEOquery`, `Biobase` |
| Differential expression | `limma` |
| Enrichment analysis | `clusterProfiler`, `org.Hs.eg.db`, `ReactomePA`, `enrichplot` |
| Pathway networks | `pathfindR` |
| Co-expression networks | `WGCNA`, `CorLevelPlot` |
| Visualization | `ggplot2`, `ggrepel`, `gridExtra`, `reshape2` |
| Data wrangling | `dplyr`, `tidyverse` |

## ▶️ How to Run

1. Clone this repository and open the `.Rmd` file you want to run in RStudio.
2. Install the required packages (see **Tech Stack** above) — most are available via `BiocManager::install()`.
3. Run chunks sequentially from the top. Each pipeline is self-contained and independent — you don't need to run one before the other.

## 👤 Author

**Mohamed Hassan**
Pharmacist (B.Pharm) | Bioinformatics Researcher
[LinkedIn](https://www.linkedin.com/in/mohamed-hassan-3772a6244/)

## 📝 Note

This is a personal learning/research project built while completing a bioinformatics diploma. Feedback and suggestions are welcome!

## 📄 License

This project is licensed under the MIT License.
