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

## Exploratory Data Analysis
- Distribution analysis
- Correlation analysis (numeric features only)
- Outlier assessment

[View EDA Notebook](notebooks/01_eda.ipynb)
[View EDA Notebook](/01_eda.ipynb)
## Preprocessing
- Log transformation
- Scaling
- Feature selection

[View Preprocessing Notebook](notebooks/02_preprocessing.ipynb)

## Clustering
- Model selection rationale
- K-Means implementation
- Cluster evaluation

[View Clustering Notebook](notebooks/03_clustering.ipynb)

## Cluster Interpretation
- Spending profiles
- Channel & Region composition

## Results
Key insights and takeaways
