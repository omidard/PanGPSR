# panGPR: Pangenome-Scale Metabolic Reconstruction & Structural Analysis

[![Nextflow](https://img.shields.io/badge/nextflow-%E2%89%A522.10.1-brightgreen.svg)](https://www.nextflow.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Docker](https://img.shields.io/badge/docker-compatible-blue.svg)](https://www.docker.com/)
[![Powered by CarveMe](https://img.shields.io/badge/Powered%20by-CarveMe-orange)](https://github.com/cdanielmachado/carveme)

**panGPR** is a robust, production-grade Nextflow pipeline designed for the automated reconstruction of Genome-Scale Metabolic Models (GEMs) at the pangenome level. It seamlessly integrates genome retrieval, rigorous quality control, metabolic reconstruction, and high-throughput structural analysis to map **Gene-Protein-Reaction (GPR)** associations across entire bacterial species.

---

## 🚀 Features

* **Automated Data Retrieval:** Fetches genomes directly from NCBI based on taxonomy.
* **Rigorous QC:** Filters contamination using **CheckM2** and **GTDB-Tk**.
* **Metabolic Reconstruction:** Builds species-specific GEMs using **CarveMe**.
* **Structural Divergence:** Performs automated homology modeling (**SWISS-MODEL**) and structural alignment (**TM-align**) for thousands of reactions.
* **Pangenome Analysis:** Clustered protein analysis using **CD-HIT**.
* **Interactive Visualization:** Generates comprehensive HTML reports and publication-ready plots.

### Workflow Overview

```mermaid
graph TD
    A[NCBI Download] --> B(CheckM2 QC)
    B --> C{GTDB Filter}
    C -->|Pass| D[Bakta Annotation]
    D --> E[CarveMe GEMs]
    D --> F[CD-HIT Clustering]
    E --> G[Metabolic Matrix]
    F --> H[Reaction Sequences]
    H --> I[SWISS-MODEL]
    I --> J[TM-Align Structure]
    G --> K[Final Visualization]
    J --> K
🛠️ Installation & Setup
1. Prerequisites
Ensure you have the following installed:

Nextflow (>=22.10.1)

Conda or Mamba

2. Clone Repository
Bash
git clone https://github.com/omidard/panGPR.git
cd panGPR
3. Setup Databases
The pipeline requires reference databases. Configure these paths in nextflow.config or pass them via command line flags:

CheckM2 Database: (uniref100.KO.1.dmnd)

Bakta Database: (Light or full version)

GTDB-Tk Database: (Release 214 or later)

🏃 Quick Start
Run the pipeline with default settings to analyze a specific taxon.

Bash
nextflow run main.nf \
    --taxon "Lactobacillus iners" \
    --email "your_email@university.edu" \
    --swissmodel_token "YOUR_API_TOKEN" \
    -with-conda \
    -resume
⚡ Advanced Usage
For production runs requiring fine-tuned control over inputs, outputs, and analysis modules:

Bash
nextflow run main.nf \
    \
    # --- Mandatory Inputs ---
    --taxon "Lactobacillus iners" \
    --email "omidard@biosustain.dtu.dk" \
    --swissmodel_token "YOUR_TOKEN" \
    \
    # --- Input / Output Control ---
    --outdir "./results_v1" \
    --download_limit 0 \
    \
    # --- Analysis Tuning ---
    --cdhit_identity 80 \
    --universe "grampos" \
    \
    # --- Module Skipping (Opt-out) ---
    --skip_gtdb false \
    --skip_structure false \
    \
    # --- Execution Flags ---
    -with-conda \
    -resume \
    -with-report run_report.html \
    -with-timeline timeline.html
⚙️ Parameter Reference
Mandatory Arguments
Parameter	Description	Example
--taxon	Scientific name of the organism (NCBI Taxonomy).	"Lactobacillus iners"
--email	User email for NCBI Entrez access.	"user@example.com"
--swissmodel_token	API token for SWISS-MODEL structural analysis.	"a1b2c3d4..."
Input & Output
Parameter	Description	Default
--outdir	Directory for all results and reports.	results
--download_limit	Max genomes to download (0 = all).	0
Analysis Settings
Parameter	Description	Default
--cdhit_identity	Sequence identity % for pangenome clustering.	80
--universe	CarveMe universe template (grampos, gramneg, etc).	grampos
Skip Flags (Optimization)
Parameter	Description	Default
--skip_gtdb	Skip taxonomic filtering step.	false
--skip_structure	Skip structural modeling & alignment (saves significant time).	false
--skip_viz	Skip final visualization generation.	false
Database Overrides
Parameter	Description
--checkm2_db	Path to CheckM2 .dmnd file.
--bakta_db	Path to Bakta database folder.
--gtdb_db	Path to GTDB-Tk database folder.
📂 Output Structure
The pipeline generates a clean, organized results directory:

Plaintext
results/
├── 01_genomes/             # Raw FASTA files from NCBI
├── 02_qc/                  # CheckM2 Quality Reports
├── 03_filtered/            # Cleaned high-quality genomes
├── 05_annotation/          # Bakta annotations (.gff, .faa)
├── 06_pangenome/           # CD-HIT clusters & presence/absence matrix
├── 07_GEMs/                # Metabolic Models (SBML .xml format)
├── 08_metabolic_analysis/  # GPR Associations & Reaction Matrix
├── 10_swiss_model/         # Homology models (PDB files)
├── 12_structalign/         # TM-align structural alignments
├── 13_analysis/            # Sequence vs Structure divergence stats
└── 14_visualization/       # Final plots, heatmaps, and summary PDFs
📬 Contact & Citation
Author: Omid Ardalani

Email: omidard@dtu.dk

Institution: The Novo Nordisk Foundation Center for Biosustainability (DTU Biosustain)

If you use panGPR in your research, please cite:

Ardalani, O., et al. (2025). panGPR: A pipeline for structural and metabolic pangenomics.
