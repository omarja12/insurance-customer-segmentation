# Insurance Customer Segmentation

Segmenting about 6,600 customers of an insurance company from their demographics, value, claims and premiums across five product lines (motor, household, health, life, work), to support marketing and pricing.

The full analysis, with outputs and 70+ figures, is in [`customer_segmentation.ipynb`](customer_segmentation.ipynb).

## Methods compared

| Method | Clusters | R² |
|---|---|---|
| K-means | 4 | **0.43** |
| Gaussian mixture | 4 | 0.38 |
| Hierarchical (Ward) | 4 | 0.37 |
| Clustering by perspective (demographic vs product), merged with Ward | 3 | 0.31 |
| Self-organising map (50 × 50) + K-means on its units | 3 | 0.31 |
| Mean shift | 2 | 0.16 |
| DBSCAN | 1 | 0.00 |

R² is the share of the clustering variables' total variance that lies between clusters, computed on the clustering variables only. Silhouette and elbow both point to 4 clusters for K-means, hierarchical clustering and the Gaussian mixture. K-means gives the most homogeneous segments, separated by motor premium, claims rate, age, customer value and salary. A decision tree trained to predict the merged segments uses almost only motor premium and age.

## Pipeline

1. **Cleaning:** impossible first-policy and birth years removed, education decoded, missing values dropped.
2. **Features:** age and years as customer (as of 2016), one-hot categoricals, percentile-based outlier filter, standardisation.
3. **Exploration of structure:** PCA (two components explain only 43% of the variance), t-SNE projections.
4. **Clustering:** the seven methods above, each with cluster profiles on the original scale, an R² decomposition by variable and a t-SNE view.

## Run it

```bash
pip install -r requirements.txt      # Python 3.10; SOMPY from GitHub; graphviz system package
jupyter nbconvert --to notebook --execute customer_segmentation.ipynb --output executed.ipynb
```

The full run takes about 12 minutes on two CPU cores. The GitHub Actions workflow executes the notebook top to bottom on every push.
