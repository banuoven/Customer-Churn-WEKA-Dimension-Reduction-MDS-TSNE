# Dimension Reduction Methods — MDS and t-SNE

**Authors:** Banu Öven, Eyüp Erman Erman

## 1. Introduction

While linear dimensionality reduction techniques like PCA excel at maximizing global
variance, they often struggle to preserve complex, non-linear structures and pairwise
distances embedded within high-dimensional datasets. To address this, two manifold learning
techniques were deployed on the Customer Churn dataset:

- **Multidimensional Scaling (MDS)** — preserves the global pairwise dissimilarity matrix.
- **t-Distributed Stochastic Neighbor Embedding (t-SNE)** — prioritizes local neighborhoods
  using a probabilistic framework.

## 2. Multidimensional Scaling (MDS) Analysis

### 2.1 Stress Statistic vs. Number of Dimensions

The MDS Stress statistic was computed across dimensions ranging from 1 to 5 to evaluate how
well the lower-dimensional space represents the original high-dimensional distances.

<img width="1208" height="614" alt="01_mds_stress_vs_dimensions" src="https://github.com/user-attachments/assets/9e4e12b2-04bd-4d81-983a-de83943cbf1e" />


The Stress value decreases monotonically as dimensionality increases. In a 2D projection, the
stress stabilizes at a moderate threshold — compressing the 13-dimensional customer behavior
space into 2D introduces some geometric distortion, but the result remains highly
interpretable.

### 2.2 Performance Comparison of MDS Configurations (4 Maps)

Four configurations were tested, crossing algorithm type (Metric vs. Non-Metric) with
preprocessing (with vs. without standardization).

<img width="1205" height="600" alt="02_mds_4panel_comparison" src="https://github.com/user-attachments/assets/4da23586-e3ac-4908-bd2e-6457f527a908" />


- **Impact of standardization:** without standardization, features with massive numerical
  magnitudes (e.g. `Seconds of Use`) dominate the Euclidean distance calculations, causing
  points to clump or stretch awkwardly. Standardization (mean = 0, std = 1) allows true
  multivariate relationships to dictate the coordinates.
- **Metric vs. Non-Metric MDS:** Metric MDS preserves actual distance values and is highly
  sensitive to scale/extreme values. Non-Metric MDS preserves only the rank order of
  distances — on unstandardized data it suffers numerical instability from rank skewness, but
  on standardized data it produces a smoother, more cohesive map that better captures the
  underlying structural topology.

## 3. t-SNE Analysis and the Impact of Standardization

![t-SNE Standardization Comparison]<img width="1210" height="580" alt="03_tsne_standardization_comparison" src="https://github.com/user-attachments/assets/1d709017-6adc-4b65-829b-37355d917ddd" />


- **Without standardization:** scale dominance by unscaled features prevents t-SNE from
  computing meaningful conditional probabilities, producing a chaotic map with no logical
  boundaries between churned (red) and non-churned (blue) instances.
- **With standardization:** the true power of t-SNE emerges — the standardized map forms
  distinct, well-separated local clusters ("islands"), indicating that customer usage
  behaviors are highly localized and correlate strongly with churn labels.

## 4. Outlier Detection and Comparison with PCA

### 4.1 Locating Outliers on MDS and t-SNE Maps

Outliers appear as isolated points or small, fragmented clusters detached from the main data
density on both maps.

- **MDS:** outliers are pushed to the extreme periphery, since their large absolute distance
  from the rest of the dataset skews the global embedding.
- **t-SNE:** because the algorithm preserves local affinities rather than global distances,
  extreme outliers are rejected by dense neighborhoods and form isolated "mini-islands" at
  the edges of the embedding.

### 4.2 Are these the same outliers as in PCA?

- **Unstandardized maps:** yes — the same high-magnitude features (e.g. anomalous extreme
  users with thousands of call minutes) drive outlier detection identically across PCA, MDS,
  and t-SNE.
- **Standardized maps:** no — PCA identifies outliers via global linear variance along the
  first few principal components, while standardized t-SNE captures non-linear, localized
  anomalies. A point that looks like a clear outlier in PCA may be pulled into a cohesive
  local cluster by t-SNE if it shares strong local similarities with a specific subgroup. The
  topological representation of outliers therefore differs fundamentally between linear (PCA)
  and non-linear (t-SNE) embeddings.

## 5. Conclusion

Non-linear dimension reduction techniques provide a richer visual segmentation of the
Customer Churn dataset than linear methods alone. MDS validates the multidimensional geometry
through its stress analysis and confirms that standardization is mandatory for meaningful
distance preservation. Standardized t-SNE provides the highest-quality visual separation,
mapping customers into distinct behavioral clusters. Combining variance-minimization (PCA),
global distance scaling (MDS), and local neighborhood mapping (t-SNE) offers a comprehensive
data exploration foundation for predictive customer retention.
