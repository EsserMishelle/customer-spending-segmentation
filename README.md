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
The exploratory data analysis focuses on understanding customer spending behavior across product categories and 
assessing feature suitability for clustering.

### Spending Distributions
Initial histograms of the raw spending variables reveal highly right-skewed distributions, with a small subset of customers accounting for disproportionately large expenditures. These heavy-tailed patterns indicate substantial diversity in purchasing behavior and suggest that distance-based methods may be dominated by extreme values if left unaddressed.

![Raw_Spending_Distributions](assets/raw_spending_distribution.jpg)

After applying a log1p transformation, the distributions become more symmetric and centered, with reduced skewness and kurtosis. This transformation preserves relative spending differences while improving numerical stability and interpretability for clustering algorithms.

![Log-Transformed Distributions](assets/log_data_distribution.jpg)

## Outliers and Variability

Boxplots of the spending variables further highlight the presence of extreme high-spending customers, particularly in the Fresh and Grocery categories. While these observations appear as statistical outliers, they likely represent meaningful customer segments rather than noise.

Accordingly, outliers are retained in the dataset to allow the clustering process to identify and separate high-value customers as distinct behavioral groups.

![Boxplots of Spending Features](assets/boxplot_spending_features.jpg)

## Correlation Structure

Correlation analysis of the numeric spending features reveals strong positive relationships between certain product categories. 
* Grocery and Detergents_Paper spending show a very strong positive correlation, implying similar purchasing behavior.
* Milk spending is also strongly correlated with Grocery, indicating related buying patterns.
* In contrast, grocery shows weak correlation with Fresh and Frozen, suggesting more independent purchasing behavior across these categories.
![Grocery vs Detergents](assets/corr_heatmap.jpg)

The scatter plot reinforces the strong positive relationship between Grocery and Detergents_Paper observed in the correlation analysis.

![Grocery vs Detergents](assets/grocery_vs_detergents_paper_spending.jpg)

## Categorical Variable Overview
Categorical variables are examined separately from numeric spending features to provide contextual information for post-cluster interpretation.

Horeca customers comprise approximately two-thirds of the dataset, with Retail customers making up the remaining third.

![Region](assets/region.jpg)

Customers are primarily concentrated in the “Other” region, with Lisbon and Oporto each representing smaller shares of the overall dataset.

![Channel](assets/channel.jpg)


➡️ Full analysis: [`1_EDA.ipynb`](1_EDA.ipynb)
## Preprocessing
- Log transformation
- Scaling
- Feature selection

[View Preprocessing Notebook](2_Preprocessing.ipynb)

## Clustering
- Model selection rationale
- K-Means implementation
- Cluster evaluation

[View Clustering Notebook](3_Clustering.ipynb)

## Cluster Interpretation
- Spending profiles
- Channel & Region composition

[View Clustering Notebook](4_Clustering_Interpretation.ipynb)

## Results
Key insights and takeaways
