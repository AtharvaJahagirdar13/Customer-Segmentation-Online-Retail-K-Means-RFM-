

# Customer Segmentation using RFM & K-Means Clustering

## Overview

This project performs customer segmentation on an online retail dataset using RFM (Recency, Frequency, Monetary) analysis and K-Means clustering.

The primary goal is to move beyond aggregate metrics and identify distinct, actionable customer segments. By understanding the unique purchasing behaviors of different groups, a business can develop targeted marketing strategies, improve customer retention, and optimize engagement efforts.

This analysis identifies **7 distinct customer segments** (4 core clusters and 3 high-value outlier groups), each with a unique profile and a corresponding strategic action plan.

## Data Source

The analysis utilizes the "Online Retail II" dataset, which contains transactional data from a UK-based online retailer between 2009 and 2010.

* [cite_start]**Initial Transactions:** 525,461 [cite: 39]

## Methodology

The project follows a systematic data science workflow, from data cleaning to modeling and interpretation.

### 1. Data Cleaning & Preparation

The raw transactional data required significant cleaning to be suitable for analysis:
* [cite_start]**Invoice Filtering:** Removed non-standard transactions, including cancellations (Invoices starting with 'C') and administrative adjustments (Invoices starting with 'A')[cite: 69, 73, 144].
* [cite_start]**StockCode Filtering:** Excluded non-product administrative codes (e.g., 'POST', 'DOT', 'M', 'AMAZONFEE', 'BANK CHARGES') to focus purely on customer purchases[cite: 128, 153].
* [cite_start]**Null Value Handling:** Dropped all rows where `Customer ID` was null, as these transactions cannot be attributed to a specific customer[cite: 167].
* [cite_start]**Zero-Price Filtering:** Removed 28 transactions where `Price` was 0.0, as they do not represent a valid sale[cite: 176, 180].

[cite_start]After cleaning, the dataset was reduced to **406,309 valid transactions** across **4,285 unique customers**, retaining 77.3% of the original data[cite: 183, 191, 791].

### 2. Feature Engineering (RFM)

From the cleaned transaction data, three key behavioral features were engineered for each of the 4,285 customers:

* [cite_start]**Recency (R):** The number of days since the customer's last purchase[cite: 220, 221].
* [cite_start]**Frequency (F):** The total count of unique invoices (purchases) made by the customer[cite: 213].
* [cite_start]**Monetary (M):** The total monetary value (sum of `Quantity` * `Price`) spent by the customer[cite: 195, 213].

### 3. Outlier Detection & Handling

[cite_start]The resulting RFM distributions were heavily right-skewed, particularly for Monetary and Frequency[cite: 231, 232]. K-Means is sensitive to such outliers, as they can disproportionately pull cluster centroids.

[cite_start]To address this, customers were split into two groups using the **1.5 x IQR (Interquartile Range) rule**[cite: 332, 347]:
* **Core Customers (n=3,809):** Customers falling within the 1.5 IQR range. [cite_start]This group was used for K-Means clustering[cite: 354].
* [cite_start]**Outlier Customers (n=476):** Customers identified as outliers in either Monetary, Frequency, or both[cite: 716, 791]. These high-value customers were analyzed separately as distinct segments.

### 4. K-Means Clustering

Clustering was performed on the **3,809 core customers**:
1.  [cite_start]**Scaling:** The R, F, and M features were standardized using `StandardScaler` to have a mean of 0 and a standard deviation of 1, ensuring each feature contributed equally to the clustering algorithm[cite: 445, 447].
2.  [cite_start]**Optimal K Selection:** The **Elbow Method (Inertia)** and **Silhouette Score** were evaluated for k-values from 2 to 12[cite: 497, 498].
    * [cite_start]The inertia plot showed a distinct "elbow" at **k=4**, indicating a significant drop in within-cluster sum of squares[cite: 525, 530, 532].
    * [cite_start]The Silhouette Score for k=4 was strong (approx. 0.38), offering a good balance between cluster separation and a practical number of segments[cite: 535, 546].
3.  [cite_start]**Modeling:** A K-Means model with **n_clusters=4** was fit to the scaled data[cite: 571].

---

## Analysis of Segments

The analysis yielded 4 core clusters and 3 outlier clusters, each with a clear behavioral profile.

### Core Customer Segments (n=3,809)

These segments represent the main body of the customer base.

| Cluster ID | Label | Count | Profile | Characteristics |
| :--- | :--- | :--- | :--- | :--- |
| **3** | **REWARD** | 473 | High Value, High Freq, Recent | **Your Champions.** Spend the most, buy the most often, and have purchased recently. |
| **0** | **RETAIN** | 998 | High Value, High Freq, Less Recent | **Loyal Customers.** Spend and buy often, but haven't been seen as recently as the "Reward" group. |
| **2** | **NURTURE** | 1,079 | Low Value, Low Freq, Recent | **New / Promising.** Don't spend much or buy often, but they are *recent*. High potential. |
| **1** | **RE-ENGAGE** | 1,259 | Low Value, Low Freq, Lapsed | **At-Risk / Lapsed.** Haven't shopped in a long time and didn't spend or buy much when they did. |

### Outlier Customer Segments (n=476)

These segments, while small in number, represent disproportionately high-value customers who were handled separately from the main clustering.

| Cluster ID | Label | Count | Profile | [cite_start]Characteristics [cite: 763-770] |
| :--- | :--- | :--- | :--- | :--- |
| **-3** | **DELIGHT** | 224 | **Monetary & Frequency Outliers** | **Your VIPs.** These are your "whales," with extremely high spending *and* high-frequency purchases. |
| **-1** | **PAMPER** | 199 | **Monetary Outliers** | **High Rollers.** Customers who spend a massive amount but do so infrequently. They make large, rare purchases. |
| **-2** | **UPSELL** | 53 | **Frequency Outliers** | **Enthusiasts.** Customers who buy *very* frequently but have a low-to-moderate spend per order. |

---

## Actionable Business Strategies

Based on the 7 segments, the following targeted actions are recommended:

| Segment | [cite_start]Strategic Action [cite: 685-703, 763-770] |
| :--- | :--- |
| **DELIGHT** | **(VIP Treatment)** Provide the highest tier of service: personal account managers, exclusive access, loyalty rewards. |
| **REWARD** | **(Recognize & Retain)** Implement a robust loyalty program, offer early access to new products, and ask for reviews. |
| **PAMPER** | **(Personalize High-Value)** Nurture with personalized outreach. Promote high-ticket items and luxury services. |
| **RETAIN** | **(Re-engage & Value)** Send personalized "we miss you" offers and targeted promotions to maintain their high engagement. |
| **UPSELL** | **(Increase Order Value)** Encourage higher spending with product bundles, subscription offers, and "you might also like" recommendations. |
| **NURTURE** | **(Build the Relationship)** Implement a welcome email series, offer incentives for a second purchase, and provide product education. |
| **RE-ENGAGE**| **(Win-Back)** Target with aggressive win-back campaigns, deep discounts, or surveys to understand why they lapsed. |

## Tools & Libraries Used

* [cite_start]**pandas:** For data manipulation and analysis[cite: 22].
* [cite_start]**matplotlib & seaborn:** For data visualization (histograms, boxplots, 3D scatter plots, violin plots)[cite: 23, 24].
* [cite_start]**scikit-learn (sklearn):** For modeling and preprocessing (`KMeans`, `StandardScaler`, `silhouette_score`) [cite: 25-27].
