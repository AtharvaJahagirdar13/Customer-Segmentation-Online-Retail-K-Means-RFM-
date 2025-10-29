

# Customer Segmentation using RFM & K-Means Clustering

## Overview

This project performs customer segmentation on an online retail dataset using RFM (Recency, Frequency, Monetary) analysis and K-Means clustering.

The primary goal is to move beyond aggregate metrics and identify distinct, actionable customer segments. By understanding the unique purchasing behaviors of different groups, a business can develop targeted marketing strategies, improve customer retention, and optimize engagement efforts.

This analysis identifies **7 distinct customer segments** (4 core clusters and 3 high-value outlier groups), each with a unique profile and a corresponding strategic action plan.

## Data

The data used for this project can be found [here](https://archive.ics.uci.edu/dataset/502/online+retail+ii), I have not included the data as part of the repo as it would inflate the repo's size.

* **Initial Transactions:** 525,461

## Methodology

The project follows a systematic data science workflow, from data cleaning to modeling and interpretation.

### 1. Data Cleaning & Preparation

The raw transactional data required significant cleaning to be suitable for analysis:
* **Invoice Filtering:** Removed non-standard transactions, including cancellations (Invoices starting with 'C') and administrative adjustments (Invoices starting with 'A').
* **StockCode Filtering:** Excluded non-product administrative codes (e.g., 'POST', 'DOT', 'M', 'AMAZONFEE', 'BANK CHARGES') to focus purely on customer purchases.
* **Null Value Handling:** Dropped all rows where `Customer ID` was null, as these transactions cannot be attributed to a specific customer.
* **Zero-Price Filtering:** Removed 28 transactions where `Price` was 0.0, as they do not represent a valid sale.

After cleaning, the dataset was reduced to **406,309 valid transactions** across **4,285 unique customers**, retaining 77.3% of the original data.

### 2. Feature Engineering (RFM)

From the cleaned transaction data, three key behavioral features were engineered for each of the 4,285 customers:

* **Recency (R):** The number of days since the customer's last purchase (calculated relative to the newest invoice date in the dataset).
* **Frequency (F):** The total count of unique invoices (purchases) made by the customer.
* **Monetary (M):** The total monetary value (sum of `Quantity` * `Price`) spent by the customer.

### 3. Outlier Detection & Handling

The resulting RFM distributions were heavily right-skewed, particularly for Monetary and Frequency. K-Means is sensitive to such outliers, as they can disproportionately pull cluster centroids.

To address this, customers were split into two groups using the **1.5 x IQR (Interquartile Range) rule**:
* **Core Customers (n=3,809):** Customers falling within the 1.5 IQR range. This group was used for K-Means clustering.
* **Outlier Customers (n=476):** Customers identified as outliers in either Monetary, Frequency, or both. These high-value customers were analyzed separately as distinct segments.

### 4. K-Means Clustering

Clustering was performed on the **3,809 core customers**:
1.  **Scaling:** The R, F, and M features were standardized using `StandardScaler` to have a mean of 0 and a standard deviation of 1, ensuring each feature contributed equally to the clustering algorithm.
2.  **Optimal K Selection:** The **Elbow Method (Inertia)** and **Silhouette Score** were evaluated for k-values from 2 to 12.
    * The inertia plot showed a distinct "elbow" at **k=4**, indicating a significant drop in within-cluster sum of squares.
    * The Silhouette Score for k=4 was strong (approx. 0.38), offering a good balance between cluster separation and a practical number of segments.
3.  **Modeling:** A K-Means model with **n_clusters=4** was fit to the scaled data.

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

| Cluster ID | Label | Count | Profile | Characteristics |
| :--- | :--- | :--- | :--- | :--- |
| **-3** | **DELIGHT** | 224 | **Monetary & Frequency Outliers** | **Your VIPs.** These are your "whales," with extremely high spending *and* high-frequency purchases. |
| **-1** | **PAMPER** | 199 | **Monetary Outliers** | **High Rollers.** Customers who spend a massive amount but do so infrequently. They make large, rare purchases. |
| **-2** | **UPSELL** | 53 | **Frequency Outliers** | **Enthusiasts.** Customers who buy *very* frequently but have a low-to-moderate spend per order. |

---

## Actionable Business Strategies

Based on the 7 segments, the following targeted actions are recommended:

| Segment                             | Strategic Action                                                                                                                  | Why It Works                                                                                                       |
| :---------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------- |
| **DELIGHT (VIPs)**                  | Send early access invites to Christmas and seasonal product launches. Offer free express shipping and loyalty perks.              | UK retail has strong seasonality (Christmas, Easter, Mother's Day). Rewarding this group increases lifetime value. |
| **REWARD (High-Value Loyalists)**   | Introduce “Refer a Friend” or “Buy Again” campaigns through email. Recommend new collections similar to their past purchases.     | They respond to discovery-based offers rather than discounts.                                                      |
| **PAMPER (High-Spend, Infrequent)** | Offer reminder emails before major holidays, bulk-purchase bundles, and B2B-style volume discounts.                               | Likely corporate or event buyers who shop in batches — prompt them before key retail periods.                      |
| **RETAIN (Loyal but Dormant)**      | Send reactivation emails featuring “Back in Stock” or “Top Picks You Missed.” Include small loyalty credit (£5 off).              | A subtle incentive can revive engagement without hurting margin.                                                   |
| **UPSELL (Frequent, Low Value)**    | Recommend higher-margin alternatives, introduce “multi-buy” offers, and bundle related items (e.g., candle + holder).             | Their buying habit is frequent but low-ticket — push basket size growth.                                           |
| **NURTURE (New Customers)**         | Automated post-purchase emails introducing product range, shipping benefits, and 10% off next order.                              | Strengthens habit formation within 30 days — critical for retention.                                               |
| **RE-ENGAGE (Lapsed)**              | Use targeted win-back campaigns with discount codes and “We miss you” surveys. Remove from marketing list if inactive >12 months. | Prevents wasteful outreach costs while salvaging recoverable customers.                                            |

Key Insights

The majority of UK customers purchase around seasonal cycles  reactivation campaigns should align with holidays.

About 11% of customers contribute over 45% of total sales, showing a classic long-tail distribution.

Frequency outliers are potential subscription or bulk buyers, ideal for targeted B2B or loyalty programs.

Retention is more cost-effective than acquisition focus on RETAIN and NURTURE groups.

## Tools & Libraries Used

* **pandas:** For data manipulation and analysis.
* **matplotlib & seaborn:** For data visualization (histograms, boxplots, 3D scatter plots, violin plots).
* **scikit-learn (sklearn):** For modeling and preprocessing (`KMeans`, `StandardScaler`, `silhouette_score`).
