# Bangladesh-Crime-Pattern-Analysis-Using-K-Means-Clustering
An unsupervised machine learning project that analyzes crime patterns in Bangladesh using K-Means clustering. The project combines data cleaning, exploratory analysis, crime-rate normalization, and clustering to identify groups of crime incidents with similar temporal, environmental, demographic, and geographic characteristics.

The resulting clusters are visualized using **PCA** and profiled to understand the main characteristics that distinguish each group.

---

## 📌 Project Overview

The dataset contains **6,574 crime incidents** covering crime types such as bodyfound, murder, rape, assault, kidnap, and robbery, along with information about:

* Time and season
* District and division
* Weather conditions
* Population and demographics
* Local facilities

Since there is no predefined target variable, this is an **unsupervised learning problem**. K-Means is used to discover naturally occurring groups of similar incidents.

The `crime` column is excluded from the clustering process and is used afterward to profile and interpret the resulting clusters.

---

## 📂 Dataset

**File:** `Bangladesh_Crime_Dataset_A.csv`
**Size:** 6,574 rows × 25 columns

| Category     | Features                                                                                      |
| ------------ | --------------------------------------------------------------------------------------------- |
| Time         | `incident_month`, `incident_week`, `incident_weekday`, `weekend`, `part_of_the_day`, `season` |
| Location     | `incident_district`, `incident_division`                                                      |
| Weather      | `precip`, `visibility`, `heatindex`                                                           |
| Demographics | Population, household size, density, literacy rate                                            |
| Facilities   | Religious institutions, playgrounds, parks, police stations, schools, colleges                |
| Crime        | `crime` — used for cluster profiling only                                                     |

---

## 🧹 Data Preprocessing

The dataset required several cleaning and transformation steps before clustering:

| Issue                                | Approach                                                                              |
| ------------------------------------ | ------------------------------------------------------------------------------------- |
| Missing values                       | Median imputation for numerical features and mode imputation for categorical features |
| Duplicate rows                       | Removed                                                                               |
| Inconsistent categorical values      | Lowercased and trimmed categorical columns                                            |
| Mild numerical outliers              | Replaced using the IQR-based median approach                                          |
| Extreme values                       | Retained when considered potentially valid                                            |
| Invalid categorical numerical values | Checked against expected ranges                                                       |
| Highly skewed `density_per_kmsq`     | Applied `log1p` transformation                                                        |
| Numerical features                   | Standardized with `StandardScaler`                                                    |
| Categorical features                 | One-hot encoded with `OneHotEncoder`                                                  |

---

## 📊 Exploratory Data Analysis

The exploratory analysis examined crime patterns across time, location, population, and local characteristics.

### Key Findings

* **Night** is the most common time of day for reported incidents, with 3,151 incidents.
* **Dhaka** has the highest raw crime count with 1,391 incidents.
* **June** has the highest number of reported incidents, while **December** has the lowest.
* **Rainy season** is the most common season in the dataset.
* **Bodyfound** is the most frequently recorded crime type.

### Crime Count vs. Crime Rate

Raw crime counts are strongly influenced by population size. Dhaka has the highest count, but this does not necessarily mean it has the highest crime rate.

After normalizing to **crimes per 100,000 people**, **Bandarban** has the highest crime rate at approximately **97 incidents per 100,000 people**.

This highlights why population normalization is important when comparing crime levels between districts of different sizes.

District-level analysis also showed a strong correlation between **total population and reported crime count (r = 0.96)**. This relationship likely reflects district size rather than a causal relationship between population and crime.

---

## 🤖 K-Means Clustering

### Model Development

K-Means was evaluated using values of **K = 2 through K = 8**.

Two methods were used to select the number of clusters:

* **Elbow method** using inertia
* **Silhouette score**

Both indicated that **K = 3** provided a suitable balance between cluster separation and complexity.

The final model used:

```text
n_clusters = 3
n_init = 10
random_state = 42
```

### Cluster Visualization

After training, **Principal Component Analysis (PCA)** was used to reduce the transformed feature space to two dimensions for visualization.

The PCA projection showed three relatively distinct clusters with limited overlap.

---

## 🧩 Cluster Profiles

| Characteristic    | Cluster 0                        | Cluster 1                           | Cluster 2                     |
| ----------------- | -------------------------------- | ----------------------------------- | ----------------------------- |
| General profile   | Mid-year, rainy-season incidents | Early-year, hotter/drier conditions | Large, highly populated areas |
| Typical month     | ~7.9                             | ~2.9                                | ~6.2                          |
| Common division   | Dhaka                            | Chattogram                          | Dhaka                         |
| Avg. population   | ~523K                            | ~513K                               | ~8.9M                         |
| Literacy rate     | ~53%                             | ~53%                                | ~74%                          |
| Most common crime | Bodyfound (24%)                  | Murder (29%)                        | Bodyfound (28%)               |
| Notable pattern   | Higher murder and rape share     | Highest murder share                | Higher robbery share (18%)    |

Across all three clusters, **nighttime incidents in Dhaka district** are the most common combination.

### Main Takeaway

The clusters appear to be separated by different types of characteristics:

* **Cluster 2** is primarily distinguished by its very large population and number of local facilities.
* **Clusters 0 and 1** are more strongly differentiated by **time of year and weather conditions**.
* Crime-type distributions provide additional context for understanding the differences between clusters but were not used to create them.

---

## 🛠️ Tech Stack

* **Python**
* **pandas & NumPy** — data processing
* **Matplotlib & Seaborn** — visualization
* **scikit-learn** — preprocessing and machine learning

Key techniques:

`KMeans` · `PCA` · `StandardScaler` · `OneHotEncoder` · `ColumnTransformer` · `silhouette_score`

---

## 📁 Project Structure

```text
├── BangladeshCrimePattern.ipynb
├── Bangladesh_Crime_Dataset_A.csv
└── README.md
```

---

## 🔮 Future Improvements

* Compare K-Means with **DBSCAN** and hierarchical clustering.
* Evaluate clusters using additional metrics such as the **Davies-Bouldin Index**.
* Perform clustering at the **district level** to focus more specifically on geographic differences.
* Analyze cluster stability across different preprocessing methods and random seeds.
* Include additional statistical comparisons between clusters.

---

## 📚 References

* [scikit-learn Documentation](https://scikit-learn.org/)
* [pandas Documentation](https://pandas.pydata.org/docs/)
* [NumPy Documentation](https://numpy.org/doc/)
