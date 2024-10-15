# RNA Differential Gene Expression Analysis with Airway Dataset

This project demonstrates RNA-seq analysis using the **Airway dataset**, focusing on differential gene expression (DGE) analysis. The dataset involves human airway smooth muscle cells treated with the glucocorticoid drug **Dexamethasone** and includes gene expression measurements for both treated and untreated conditions.

## About the Airway Data

The **Airway** dataset is a `RangedSummarizedExperiment` object, containing:
- **Count data**: Raw RNA-seq read counts.
- **Sample information**: Metadata about the samples, including treatment conditions.

### Key Functions:
- **`rowData(airway)`**: Retrieves metadata (e.g., gene identifiers) for each row (gene) in the dataset.
- **`assay(airway)`**: Accesses the matrix of gene expression levels (counts) for each sample.

## Differential Gene Expression Analysis Workflow

### Step 1: Creating DESeq Object
- **`DESeqDataSet`**: Used to create an object from the `SummarizedExperiment`.
- **`DESeqDataSetFromMatrix`**: Used if raw counts are in matrix form.

### Step 2: Normalization
Normalization ensures counts can be compared across samples using **size factors**, calculated using the median-of-ratios method:
- Geometric mean of counts across samples.
- Ratio of gene counts to geometric mean.
- Median of these ratios becomes the **size factor**.

## Quality Control and Visual Exploration

### 1. Hierarchical Clustering and Heatmaps
- **Regularized Log Transformation (rlog)** and **Variance Stabilizing Transformation (VST)** are used to stabilize variance, improving clustering visuals.

### 2. Principal Component Analysis (PCA)
- **PCA** helps visualize sample clustering, showing whether experimental conditions (e.g., treated vs. untreated) align with major components of variation.

## Differential Expression Analysis

### Dispersion Estimates and Model Fitting
- **Normalized counts** are used to model the relationship between mean expression levels and dispersion.
- **Local regression** fits a curve to plot dispersion estimates across mean counts, shrinking estimates toward the trend for better accuracy.

### Log Fold Changes (logFC)
- **Log Fold Change** measures the change in gene expression between two conditions (e.g., treated vs. untreated).
  - **Positive logFC**: Upregulation.
  - **Negative logFC**: Downregulation.
  - **Zero logFC**: No change.

### MA Plot
- **MA Plot** visualizes log fold changes (M) vs. average expression (A), showing up- or down-regulated genes based on their distance from the horizontal line.

