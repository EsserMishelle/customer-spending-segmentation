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
![Raw_Spending_Distributions](assets/raw_spending_distributions.png)

After applying a log1p transformation, the distributions become more symmetric and centered, with reduced skewness and kurtosis. This transformation preserves relative spending differences while improving numerical stability and interpretability for clustering algorithms.

![Log-Transformed Distributions](assets/eda_distributions_log.png)

[View EDA Notebook](/1_EDA.ipynb)

Spending variables exhibit strong right skew with a small number of high-value customers. A log transformation substantially reduces skewness, improving feature behavior for distance-based clustering.

![Log-Transformed Distributions](assets/eda_distributions_log.png)

A strong positive relationship is observed between Grocery and Detergents_Paper spending, indicating overlapping purchasing behavior among certain customer segments.

![Grocery vs Detergents](assets/eda_grocery_vs_detergents.png)

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
