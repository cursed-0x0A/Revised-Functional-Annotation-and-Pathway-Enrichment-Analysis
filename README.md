# Revised-Functional-Annotation-and-Pathway-Enrichment-Analysis
Internship Activity - PGC Visayas


R Script Usage Instructions & Technical Notes

This section outlines the essential instructions, environment dependencies, and execution steps required to run the automated R workflow for downstream transcriptomic, differential expression, and pathway enrichment analyses.

1. Required R Environment & Package Dependencies
Ensure your R version (recommended: >= 4.2.0) has all required Bioconductor and CRAN packages installed prior to execution:



# Install BiocManager if not already installed

if (!requireNamespace("BiocManager", quietly = TRUE))
install.packages("BiocManager")

# Install core analysis and annotation packages

BiocManager::install(c(
"DESeq2", "clusterProfiler", "pathview", "AnnotationDbi",
"GO.db", "KEGGREST", "enrichplot", "pheatmap"
))

# Install general data manipulation and plotting packages

install.packages(c("tidyverse", "readr", "tibble", "dplyr", "stringr", "ggplot2", "tidyr"))

2. Input File Prerequisites
Before launching the R script, confirm that the following input files are present in your working directory:

* count_matrix.csv — Raw expression count matrix generated from Salmon/Trinity abundance estimation.
* metadata.csv — Sample grouping file configured with sample names and treatment attributes.


* user_ko.txt — Functional annotation mapping file containing clean feature IDs and KEGG Orthology identifiers (derived from GhostKOALA).
* gene_ontology_annotations.txt — Tab-delimited file mapping feature IDs to Gene Ontology (GO) terms.
* final_trinotate_report.xls — Comprehensive annotation report from the Trinotate suite.

3. Step-by-Execution Guidelines

* Step 3.1 Directory Path Configuration: Modify the output_dir variable within the script to point to your designated local analysis folder (e.g., D:/deseqinputs/analysis_results).


* Step 3.2 Running DESeq2: The script automatically executes model fitting via Wald statistics, setting conditionA as the reference baseline group, and categorizes features based on an adjusted significance threshold.


* Step 3.3 Generating Enrichment & Visualizations: The pipeline automatically compiles GO and KEGG enrichment datasets, constructs publication-ready dot plots, plots volcano maps, and maps biological pathways utilizing the pathview package. To optimize workflow execution speed and prevent network throttling, automated Pathview mapping targeted the top three most statistically significant pathways (head(valid_paths, 3)). Nodes are color-coded to represent log-fold changes from green (down-regulated) to red (up-regulated). In this R script, only the upregulated pathways were mapped, while the downregulated pathways remain optional steps.



Important Execution Note:
Ensure that network connectivity is active when executing the script, as functions like keggList() and bitr_kegg() dynamically query online KEGG databases for pathway description mapping.
