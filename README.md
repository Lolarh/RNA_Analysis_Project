## About Airway Data

Airway data is a RangedSummarizedExperiment object containing count data and sample information. It consists of RNA-seq data from human airway smooth muscle cells. These cells were treated with a glucocorticoid drug called Dexamethasone, commonly used for its anti-inflammatory effects. The dataset contains measurements of gene expression levels under two conditions: treated and untreated with Dexamethasone.

*rowData(airway):*

This function retrieves the metadata associated with each row (typically representing genes) in the airwaydataset. This metadata might include gene identifiers, gene symbols, annotations, and other relevant information about each feature (e.g., genes or transcripts) in the dataset.

Essentially, rowData(airway) provides descriptive information about the rows of the dataset.

*assay(airway):*

This function accesses the actual expression data matrix from the airway dataset. In this matrix, rows correspond to features (such as genes), and columns correspond to samples. Each cell in this matrix contains the expression level (e.g., counts) of a particular gene in a particular sample.

In other words, assay(airway) provides the numerical data of gene expression levels measured across different samples.

## Differential Gene Analysis Preparation

*Step 1: Creating DESeq Object*

DESeqDataSet is mostly used to create this object because the raw count is a Summarized Experiment. If otherwise, DESeqDataSetFromMatrix is used

*Step 2: Normalization*

Normalization ensures counts can be compared across samples. DESeq2 automatically computes size factors, representing the total number of reads per sample, and scales the counts accordingly.

Size Factor Calculation

The size factor is computed using the median-of-ratios method:

Geometric Mean Calculation: For each gene, the geometric mean of its counts across all samples is calculated.

Ratio Calculation: Each gene’s count in a sample is divided by its geometric mean.

Median-of-Ratios: The median of these ratios for each sample becomes the size factor.

Normalization: Raw counts are divided by their size factors, making them comparable across samples.

## Quality Control and Visual Exploration of Samples using PCA and Correlation Heatmap

#### Unsupervised Hierachical Clustering with heatmap using Regularized Log Transformation or Variance Stabilizing Transformation
For unsupervised clustering, it's common to use transformed data to stabilize variance and make the data more suitable for clustering. Two common transformations are regularized log (rlog) and variance stabilizing transformation (VST), a log transformation that moderates the variance across the mean. This helps to improve the visuals of the clustering.

#### Principal Component Analysis Plot
PCA is performed to see how the samples cluster and whether the condition of interest corresponds with the principal components explaining the most variation in the data.

## Differential Expression Analysis

**Dispersion Estimate and Model Fitting**

How well does the data fit to the Model?

The normalized counts (adjusted by size factors) are used in the dispersion modeling. DESeq2 fits a model to the relationship between mean normalized counts and dispersion. The fitting process involves using a relationship between the mean expression level and the dispersion.

Local Regression: A smooth curve (trend line) is fitted to the plot of mean normalized counts vs. dispersion estimates for each gene. This curve represents the expected dispersion at different mean expression levels.

Shrunken Estimates: For each gene, an individual dispersion estimate is shrunk towards the trend line, balancing gene-specific information and the overall trend observed across all genes.

**LOG-FOLD CHANGES**

What is Log Fold Change and why is it used?

Log fold change (logFC) is a commonly used metric in biological and statistical analyses, especially in the context of gene expression studies like RNA-Seq. It quantifies the change in expression levels of a gene between two conditions, such as treated versus untreated, diseased versus healthy, or any other experimental comparison.

What is Fold Change (FC)

Fold change is the ratio of expression levels between two conditions. For example, if a gene has an expression level of 200 in Condition A and 100 in Condition B, the fold change is 200/100 = 2. This means the gene's expression is twice as high in Condition A as in Condition B. To make the scale of change easier to interpret and to handle a wide range of values (including very small values), the fold change is often transformed using the logarithm (commonly base 2).

**Important Interpretations:**

Positive values indicate upregulation (higher expression in the first condition).

Negative values indicate downregulation (lower expression in the first condition).

Zero indicates no change.

MA PLOT to visualize log fold change with and without shrinkage

MA Plot is a plot of the log2 fold change (M) vs. the average expression (A) for each gene. Interpretation: Points above or below the horizontal line indicate up-regulated or down-regulated genes, respectively. The further from the line, the greater the change in expression.

M stands for the log ratio (or "fold change") of expression between two conditions (e.g., treated vs. control).

A stands for the average expression (or the mean) of counts for a gene across the two conditions. The plot is typically created after performing differential expression analysis, where each point represents a gene
