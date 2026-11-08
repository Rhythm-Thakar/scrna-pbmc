# Single-cell RNA-seq of PBMCs (scanpy)

Standard scanpy workflow on the `pbmc3k` dataset (about 2,700 peripheral blood cells, 10x Genomics, loaded with `scanpy.datasets.pbmc3k()`). Notebook: `pbmc3k_scanpy.ipynb`.
Versions: Python 3.10.12, scanpy 1.11.5, anndata 0.11.4, igraph 1.0.0.

## Workflow
- QC: removed cells with more than 2,500 genes (5) or more than 5% mitochondrial counts (57); removed genes seen in fewer than 3 cells. 2,700 -> 2,638 cells, 32,738 -> 13,656 genes
- Normalised to 10,000 counts per cell and log-transformed; 2,000 highly variable genes; scaled (clipped at 10)
- PCA (first 10 of 50 components, chosen from the elbow plot), 10-nearest-neighbour graph, UMAP, Leiden clustering at resolution 0.9 -> 13 clusters
- Clusters named from marker genes (dotplot and ranked genes)

## Cell types
Coarse: T 1,447, Monocytes 638, B 344, NK 155, Dendritic 39, Platelets 15.
Fine T and monocyte labels: Naive-like T 638, Memory-like T 487, GZMK+ cytotoxic T 176, GZMH+ cytotoxic T 146, CD14+ monocytes 432, FCGR3A+ monocytes 206.

## How stable are the labels?
Re-clustering with 20 PCs instead of 10 gives 9 clusters (adjusted Rand index 0.578). Share of each label that stays in one cluster:
- Stable (0.90-1.00): B, platelets, NK, GZMH+ cytotoxic T, CD14+ and FCGR3A+ monocytes, dendritic cells
- GZMK+ cytotoxic T (0.81): most merge with GZMH+ cytotoxic T in the 20-PC run
- Naive-like T (0.73) and Memory-like T (0.82): the two mix, so treat them as a gradient, not two clean types
Details: `results/stability_10_vs_20PC_crosstab.csv`.

## Limits
- One public dataset; labels come from marker genes and are tentative
- Cluster-vs-cluster p-values are optimistic because the clusters were defined on the same data
- Dendritic cells (39) and platelets (15) are small groups; 4 dendritic-labelled cells sit with T cells in the 20-PC run and were not checked

Files: `figures/` (UMAPs, marker dotplot), `results/` (cell annotations, top markers per cluster, stability table).
