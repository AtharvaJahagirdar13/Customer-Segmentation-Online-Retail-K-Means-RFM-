Here is the updated README with your requested "Data" section added.

---

# Customer Segmentation using RFM & K-Means Clustering

## Overview

This project performs customer segmentation on an online retail dataset using RFM (Recency, Frequency, Monetary) analysis and K-Means clustering.

The primary goal is to move beyond aggregate metrics and identify distinct, actionable customer segments. By understanding the unique purchasing behaviors of different groups, a business can develop targeted marketing strategies, improve customer retention, and optimize engagement efforts.

This analysis identifies **7 distinct customer segments** (4 core clusters and 3 high-value outlier groups), each with a unique profile and a corresponding strategic action plan.

## Data

The data used for this project can be found [here](https://archive.ics.uci.edu/dataset/502/online+retail+ii)

* **Initial Transactions:** 525,461 [cite: 39]

## Methodology

The project follows a systematic data science workflow, from data cleaning to modeling and interpretation.

### 1. Data Cleaning & Preparation

The raw transactional data required significant cleaning to be suitable for analysis:
* **Invoice Filtering:** Removed non-standard transactions, including cancellations (Invoices starting with 'C' [cite: 66, 69-71][cite_start]) and administrative adjustments (Invoices starting with 'A' [cite: 73]).
* **StockCode Filtering:** Excluded non-product administrative codes (e.g., 'POST', 'DOT', 'M', 'AMAZONFEE', 'BANK CHARGES' [cite: 127-139]) to focus purely on customer purchases.
* **Null Value Handling:** Dropped all rows where `Customer ID` was null[cite: 167], as these transactions cannot be attributed to a specific customer.
* **Zero-Price Filtering:** Removed 28 transactions where `Price` was 0.0 [cite: 176-180], as they do not represent a valid sale.

[cite_start]After cleaning, the dataset was reduced to **406,309 valid transactions** across **4,285 unique customers** [cite: 791][cite_start], retaining 77.3% of the original data[cite: 190, 192].

### 2. Feature Engineering (RFM)

[cite_start]From the cleaned transaction data, three key behavioral features were engineered for each of the 4,285 customers [cite: 210-215, 791]:

* [cite_start]**Recency (R):** The number of days since the customer's last purchase (calculated relative to the newest invoice date in the dataset) [cite: 219-221].
* [cite_start]**Frequency (F):** The total count of unique invoices (purchases) made by the customer[cite: 213].
* [cite_start]**Monetary (M):** The total monetary value (sum of `Quantity` * `Price`) spent by the customer[cite: 195, 213].

### 3. Outlier Detection & Handling

[cite_start]The resulting RFM distributions were heavily right-skewed, particularly for Monetary and Frequency[cite: 233, 294]. K-Means is sensitive to such outliers, as they can disproportionately pull cluster centroids.

[cite_start]To address this, customers were split into two groups using the **1.5 x IQR (Interquartile Range) rule** [cite: 332-333, 342-347]:
* **Core Customers (n=3,809):** Customers falling within the 1.5 IQR range. [cite_start]This group was used for K-Means clustering [cite: 352-354].
* **Outlier Customers (n=476):** Customers identified as outliers in either Monetary, Frequency, or both. [cite_start]These high-value customers were analyzed separately as distinct segments[cite: 716].

### 4. K-Means Clustering

[cite_start]Clustering was performed on the **3,809 core customers**[cite: 352]:
1.  [cite_start]**Scaling:** The R, F, and M features were standardized using `StandardScaler` to have a mean of 0 and a standard deviation of 1, ensuring each feature contributed equally to the clustering algorithm [cite: 435-436, 445-447].
2.  [cite_start]**Optimal K Selection:** The **Elbow Method (Inertia)** [cite: 506-509] [cite_start]and **Silhouette Score** [cite: 513-515] [cite_start]were evaluated for k-values from 2 to 12[cite: 494, 497].
    * [cite_start]The inertia plot showed a distinct "elbow" at **k=4** [cite: 525-543], indicating a significant drop in within-cluster sum of squares.
    * [cite_start]The Silhouette Score for k=4 was strong (approx. 0.38) [cite: 544-565], offering a good balance between cluster separation and a practical number of segments.
3.  [cite_start]**Modeling:** A K-Means model with **n_clusters=4** was fit to the scaled data [cite: 570-572].

---

## Analysis of Segments

The analysis yielded 4 core clusters and 3 outlier clusters, each with a clear behavioral profile.

### Core Customer Segments (n=3,809)

These segments represent the main body of the customer base.

| Cluster ID | Label | Count | Profile | Characteristics |
| :--- | :--- | :--- | :--- | :--- |
| **3** | [cite_start]**REWARD** [cite: 779] | 473 | High Value, High Freq, Recent | [cite_start]**Your Champions.** Spend the most, buy the most often, and have purchased recently [cite: 700-701, 799, 801]. |
| **0** | [cite_start]**RETAIN** [cite: 773] | 998 | High Value, High Freq, Less Recent | [cite_start]**Loyal Customers.** Spend and buy often, but haven't been seen as recently as the "Reward" group [cite: 685-686, 799, 801]. |
| **2** | [cite_start]**NURTURE** [cite: 777] | 1,079 | Low Value, Low Freq, Recent | **New / Promising.** Don't spend much or buy often, but they are *recent*. [cite_start]High potential [cite: 696-697, 799, 801]. |
| **1** | [cite_start]**RE-ENGAGE** [cite: 775] | 1,259 | Low Value, Low Freq, Lapsed | [cite_start]**At-Risk / Lapsed.** Haven't shopped in a long time and didn't spend or buy much when they did [cite: 689-690, 799, 801]. |

### [cite_start]Outlier Customer Segments (n=476) [cite: 716, 791]

These segments, while small in number, represent disproportionately high-value customers who were handled separately from the main clustering.

| Cluster ID | Label | Count | Profile | [cite_start]Characteristics [cite: 763-770] |
| :--- | :--- | :--- | :--- | :--- |
| **-3** | [cite_start]**DELIGHT** [cite: 784] | 224 | [cite_start]**Monetary & Frequency Outliers** [cite: 711, 714] | [cite_start]**Your VIPs.** These are your "whales," with extremely high spending *and* high-frequency purchases[cite: 768]. |
| **-1** | [cite_start]**PAMPER** [cite: 781] | 199 | [cite_start]**Monetary Outliers** [cite: 710, 712] | **High Rollers.** Customers who spend a massive amount but do so infrequently. [cite_start]They make large, rare purchases[cite: 763]. |
| **-2** | [cite_start]**UPSELL** [cite: 783] | 53 | [cite_start]**Frequency Outliers** [cite: 711, 713] | [cite_start]**Enthusiasts.** Customers who buy *very* frequently but have a low-to-moderate spend per order[cite: 765]. |

---

## Actionable Business Strategies

Based on the 7 segments, the following targeted actions are recommended:

| Segment | [cite_start]Strategic Action [cite: 688, 695, 699, 703, 764, 767, 770] |
| :--- | :--- |
| **DELIGHT** | [cite_start]**(VIP Treatment)** Provide the highest tier of service: personal account managers, exclusive access, loyalty rewards[cite: 770]. |
| **REWARD** | [cite_start]**(Recognize & Retain)** Implement a robust loyalty program, offer early access to new products, and ask for reviews[cite: 703]. |
| **PAMPER** | **(Personalize High-Value)** Nurture with personalized outreach. [cite_start]Promote high-ticket items and luxury services[cite: 764]. |
| **RETAIN** | [cite_start]**(Re-engage & Value)** Send personalized "we miss you" offers and targeted promotions to maintain their high engagement[cite: 688]. |
| **UPSELL** | [cite_start]**(Increase Order Value)** Encourage higher spending with product bundles, subscription offers, and "you might also like" recommendations[cite: 767]. |
| **NURTURE** | [cite_start]**(Build the Relationship)** Implement a welcome email series, offer incentives for a second purchase, and provide product education[cite: 699]. |
| **RE-ENGAGE**| [cite_start]**(Win-Back)** Target with aggressive win-back campaigns, deep discounts, or surveys to understand why they lapsed[cite: 695]. |

## Tools & Libraries Used

* [cite_start]**pandas:** For data manipulation and analysis[cite: 22].
* [cite_start]**matplotlib & seaborn:** For data visualization (histograms, boxplots, 3D scatter plots, violin plots)[cite: 23, 24].
* [cite_start]**scikit-learn (sklearn):** For modeling and preprocessing (`KMeans`, `StandardScaler`, `silhouette_score`) [cite: 25-27].
