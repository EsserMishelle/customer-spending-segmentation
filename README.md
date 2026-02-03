## Table of Contents


- [Customer Clustering Analysis](#customer-clustering-analysis)
- [Exploratory Data Analysis(EDA)](#exploratory-data-analysiseda)
- [Exploratory Data Analysis(EDA)](#exploratory-data-analysiseda)

- [Spending Distributions](#spending-distributions)
- [Clustering Methodology](#clustering-methodology)
- [Cluster Results](#cluster-results)
- [Cluster Interpretation](#cluster-interpretation)
- [Business Implications](#business-implications)
- [Conclusion](#conclusion)

# Customer Clustering Analysis

## Overview and Analysis Objective
The objective of this analysis is to identify meaningful customer segments based on spending behavior 
using unsupervised clustering techniques. The dataset contains aggregated spending amounts across 
multiple product categories for wholesale customers. Customers are grouped according to their 
purchasing patterns without predefined labels, allowing natural behavioral segments to emerge. 
Categorical variables such as Channel and Region are retained for post-cluster interpretation 
to help contextualize the resulting segments.

## Data Description
The dataset used in this analysis is the Wholesale Customers Data Set from the UCI Machine Learning Repository.
Each observation represents a customer’s purchasing behavior summarized over a fixed observation period.
* It contains aggregated spending amounts for wholesale customers across six product categories: 
Fresh, Milk, Grocery, Frozen, Detergents_Paper, and Delicatessen. These spending variables are continuous and non-negative.
* The dataset also includes two categorical variables: Channel, 
indicating customer type (Horeca or Retail), and Region, indicating geographic location (Lisbon, Oporto, or Other).

## Exploratory Data Analysis(EDA)
The exploratory data analysis focuses on understanding customer spending behavior across product categories and assessing feature suitability for clustering.

<img src="assets/describe.jpg" alt="describe" width="1000">

**Observations (based on info() and describe()):**
* There are 440 rows and 8 variables, with no missing values.
* The spending variables show large ranges and high standard deviations, suggesting substantial differences in purchasing behavior.
* Median values are lower than mean or maximum values, implying the presence of high-spending customers.

### Spending Distributions
Initial histograms of the raw spending variables reveal highly right-skewed distributions, with a small subset of customers accounting for disproportionately large expenditures. These heavy-tailed patterns indicate substantial diversity in purchasing behavior and suggest that distance-based methods may be dominated by extreme values if left unaddressed.

<img src="assets/raw_spending_distribution.jpg" alt="Raw Spending Distributions" width="600">


After applying a log1p transformation, the distributions become more symmetric and centered, with reduced skewness and kurtosis. This transformation preserves relative spending differences while improving numerical stability and interpretability for clustering algorithms.

<img src="assets/log_data_distribution.jpg" alt="Log-Transformed Distributions" width="600">


## Outliers and Variability

Boxplots of the spending variables further highlight the presence of extreme high-spending customers, particularly in the Fresh and Grocery categories. While these observations appear as statistical outliers, they likely represent meaningful customer segments rather than noise.

Accordingly, outliers are retained in the dataset to allow the clustering process to identify and separate high-value customers as distinct behavioral groups.

<img src="assets/boxplot_spending_features.jpg" alt="boxplot_spending_features.jpg" width="700">

## Correlation Structure

Correlation analysis of the numeric spending features reveals strong positive relationships between certain product categories. 
* Grocery and Detergents_Paper spending show a very strong positive correlation, implying similar purchasing behavior.
* Milk spending is also strongly correlated with Grocery, indicating related buying patterns.
* In contrast, grocery shows weak correlation with Fresh and Frozen, suggesting more independent purchasing behavior across these categories.
  
<img src="assets/corr_heatmap.jpg" alt="corr_heatmaps.jpg" width="600">

The scatter plot reinforces the strong positive relationship between Grocery and Detergents_Paper observed in the correlation analysis.

<img src="assets/grocery_vs_detergents_paper_spending.jpg" alt="grocery_vs_detergents_paper_spending.jpg" width="600">

## Categorical Variable Overview
Categorical variables are examined separately from numeric spending features to provide contextual information for post-cluster interpretation.

Horeca customers comprise approximately two-thirds of the dataset, with Retail customers making up the remaining third.

![Channel](assets/channel.jpg)

Customers are primarily concentrated in the “Other” region, with Lisbon and Oporto each representing smaller shares of the overall dataset.

![Region](assets/region.jpg)

➡️ Full analysis: [`1_EDA.ipynb`](1_EDA.ipynb)

## Data Preprocessing (for Clustering)

Clusting is performed using the behavioral variables during exploratory analysis. Categorical variables (Channel and Region) are excluded from clustering and will be retained for post-cluster interpretation.

### Feature Selection

The following six continuous spending variables are used as input features for the basis for segmentation:
* Fresh
* Milk
* Grocery
* Frozen
* Detergents_Paper
* Delicassen

### Tranformation (Log)

As described in EDA, a loglp transformation is applied to the spending features to improve cluster stability by preventing high-spending customers from disproportionately influencing distance calculations.

### Scaling

After transformation, features are standardized using StandardScaler.
Scaling ensures that all variables contribute equally to distance-based clustering algorithms such as K-Means and hierarchical clustering, preventing features with larger numeric ranges from dominating the analysis.

### Principal Components Anaysis (Dimensionality Reduction)

Principal Components Analysis (PCA) is applied to the scaled data to reduce dimensionality while preserving the majority of variance in customer spending behavior. PCA also helps improve clustering performance by reducing noise and collinearity among features.

<img src="assets/pca_explained_variance.jpg" alt="pca_explained_variance.jpg" width="600">
**Analysis:**

The first 3 principal components capture 82.0% of the total variance in the data, exceeding the commonly used 80% threshold for dimensionality reduction. While adding a fourth component would increase cumulative variance to 92.1%, the marginal gain (10.1%) does not justify the added complexity. Therefore, n_components=3 was selected for clustering analysis, balancing information retention with model simplicity.

### Identifying the Number of Clusters (Elbow Method and Silhouette Analysis)
To determine the optimal number of clusters, both the Elbow Method and Silhouette Analysis are used.

* The elbow plot evaluates diminishing reductions in inertia as the number of clusters increases.

* The silhouette score measures how well each point fits within its assigned cluster compared to others, with higher values indicating better-defined clusters.
  
Elbow (Inertia) + Silhouette Charts
![elbow_silhouette_analysis](assets/elbow_silhouette_analysis.jpg)

**Interpretation:**

* Silhouette analysis (red) peaks at k = 3 with an average score of 0.33, indicating acceptable separation for behavioral data. The elbow curve (blue) also shows diminishing returns beyond k = 3, suggesting no substantial improvement from additional clusters. 
* Both silhouette and inertia merge at k = 3, indicating the three-cluster solution provides a better balance between separation and interpretability.

## Hierarchical Clustering (Dendrogram – Supportive Analysis)
To help determine number of clusters, the hierarchical clustering is applied.
![hierachical](assets/hierachical.jpg)

The dendrogram displays a clear separation at higher linkage distances, with a gap around a distance of 14. Splitting the dendrogram here at y=14 results in three main branches, which is consistent with the three-cluster solution assessed by elbow and silhouette methods.

**Therefore, k = 3 is selected as the optimal number of customer segments.**

➡️ Full analysis: [`2_Preprocessing`](2_Preprocessing.ipynb)

## Clustering
This step applies the finalized preprocessing and clustering parameters identified in earlier analysis to assign customers to segments using K-Means clustering. Based on elbow, silhouette, and hierarchical confirmation, k = 3 is selected as the final clustering solution.

- Algorithm: K-Means
- Number of clusters: k = 3
- Input features: PCA-reduced spending variables (3 components)
- Silhouette score: 0.33

### PCA Cluster Visualization

<img src="assets/pca_scatter_plot.jpg" alt="assets/pca_scatter_plot.jpg" width="600">

The scatter plot shows customers projected onto the first two principal components, with points colored by their assigned K-Means cluster. The visualization indicates moderate separation among the three clusters in reduced-dimensional space, supporting the suitability of the selected clustering configuration.


* PCA is applied to display the clustered data in two dimensions for visualization purposes. The plot shows a clear grouping pattern corresponding to the tree clusters identified by K-Means, with some overlap resulting from dimensionality reduction.
* The first two principal components explain approximately 71% of the total variance, which is sufficient for visualizing overall cluster structure without influencing model selection.

➡️ Full analysis: [`3_Clustering`](3_Clustering.ipynb)

## Cluster Profiling
### Silhouette Analysis (Visual Purpose)
Silhouette analysis is used here to visually validate cluster separation and confirm the selected number of clusters.


<img src="assets/sil_2_3_clusters.jpg" alt="sil_2_3_clusters.jpg" width="600">
<img src="assets/sil_4_5_clusters.jpg" alt="sil_4_5_clusters.jpg" width="600">
<img src="assets/sil_6_7_clusters.jpg" alt="sil_6_7_clusters.jpg" width="600">
<img src="assets/sil_8_clusters.jpg" alt="sil_8_clusters.jpg" width="600">
<img src="assets/sil_n_scores.jpg" alt="sil_n_score.jpg" width="1400">

### Silhouette analysis is shown across multiple values of k (2–8) to illustrate the trade-off between statistical separation and business interpretability.

* While k = 2 produces the highest average silhouette score, it results in overly broad clusters that does not produce meanings.
* Both k = 3 and k = 4 appear plausible; however, k = 3 has a higher silhouette score and maintains moderate separation while improving interpretability.
* Therefore, k = 3 is validated to be be the optimal clustering solution.

## Cluster Profiling
Clusters are referenced using descriptive segment names based on dominant spending patterns; detailed interpretations are provided below.

## Spending Profiles by Cluster
<img src="assets/spending_profiles.jpg" alt="spending_profiles.jpg" width="1800">

## Table 1. Average Spending by Product Category per Cluster
<img src="assets/tble_avg_spending_by_category.jpg" alt="tble_avg_spending_by_category.jpg" width="600">

## Table 2. Customer Segment Summary
<img src="assets/tble_customer_segment_summary.jpg" alt="tble_customer_segment_summary.jpg" width="600">

## Cluster Interpretation:

####  **Custer 0 (High-Value Diversified Buyers):** 

* This group represents the highest-spending customers across nearly all product categories, indicating a broad and diversified purchasing profile.
* These customers are large-volume buyers with extensive category coverage and account for a substantial share of overall revenue, making them strategically important to the business.

#### **Cluster 1 (Staple-Focused High-Volume Buyers):**

* While being the smallest cluster, customers in this segment spend disproportionately high amounts on staple goods such as **Grocery, Milk, and Detergents-Paper**, indicating a focus on essential household or operational supplies rather than fresh or specialty products.

* This pattern suggests consistent, necessity-driven purchasing behavior rather than broad or diversified consumption.

#### **Cluster 2 (Lower-Volume General Buyers):**

* This group represents the largest cluster with the lowest average spending behavoir. This pattern suggests smaller-scale or infrequent buyers with limited purchasing volume. 
* These customers may engage in **more selective or occasional purchasing** rather than bulk or high-volume transactions.

## Business Implications  

#### **Cluster 0 (High-Value Diversified Buyers)** - Highest overall spend across categories

This segment represents the most valuable customers with diversified purchasing behavior.

Actionable strategies:

* Prioritize account management and retention programs.

* Provide exclusive promotions, early access, or loyalty incentives.

* Provide credit or debit cards for cashback discounts. 

* Offer cross-category promotions (e.g., Fresh + Frozen + Grocery).

* Use this segment for new product trials or premium offerings due to higher engagement.

#### **Cluster 1 (Staple-Focused High-Volume Buyers)** - High Grocery, Milk, Detergents_Paper

These customers prioritize essential household and operational supplies.

Actionable strategies:

* Offer bundle discounts on products (e.g., Grocery + Detergents_Paper).

* Distribute or email recurring coupons for consumables to encourage repeat purchasing.

* Introduce auto-replenishment or subscription-style offers for high-frequency items.

* In wholesale contexts, target this segment with volume-based pricing or contract pricing.



#### **Cluster 2 (Lower-Volume General Buyers):** - Lower spend, concentrated categories

These customers purchase selectively and represent growth potential.

Actionable strategies:

* Use targeted discounts to encourage category expansion.

* Promote sampling programs (e.g., in-store food samples or trial-sized products).

* Run entry-level bundle promotions to increase basket size.

* Focus on seasonal or promotional campaigns rather than everyday discounts.

# Summary and Opportunities for Further Analysis

This analysis segments customers based solely on aggregated spending behavior across product categories. While the resulting clusters reveal meaningful purchasing patterns, additional data could significantly enhance segmentation quality and business insight.

Future improvements could include:

* Time-series purchasing data to capture seasonality, frequency, and customer lifecycle behavior.

* Transaction-level data to distinguish bulk purchases from frequent small orders.

* Demographic or firmographic attributes (e.g., business size, customer type, location density).

* Channel interaction data to assess differences in purchasing behavior across sales channels.

*Promotional response history to measure price sensitivity and campaign effectiveness.

Incorporating these features would enable more precise segmentation, improved personalization, and stronger predictive modeling for customer behavior.


➡️ Full analysis: [`4_Cluster_Interpretation`](4_Cluster_Interpretation.ipynb)
