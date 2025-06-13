# PROCUREMENT-RISK-ASSESSMENT
# PROCUREMENT-RISK-ASSESSMENT

This repository contains a Jupyter Notebook focused on identifying potentially anomalous or high-risk transactions within government procurement data. Our work aligns with **UN Sustainable Development Goal 16: Peace, Justice, and Strong Institutions** – specifically by aiming to **promote peaceful and inclusive societies, provide access to justice, and build effective, accountable and inclusive institutions at all levels.** By analyzing procurement data, we seek to enhance transparency and accountability, thereby strengthening institutional integrity and reducing opportunities for corruption or inefficiency.

## Table of Contents

-   [Project Overview](#project-overview)
-   [Features Engineered](#features-engineered)
-   [Anomaly Detection Model](#anomaly-detection-model)
-   [Installation](#installation)
-   [Usage](#usage)
-   [Data Source](#data-source)
-   [Output](#output)

## Project Overview

The goal of this project is to build a system that can automatically flag government procurement tenders that exhibit characteristics of potential anomalies or higher risk. By identifying such instances, we aim to contribute to building more transparent, accountable, and effective public institutions, which is a core component of SDG 16. This is achieved through:
1.  **Data Loading**: Importing the raw procurement transaction data.
2.  **Feature Engineering**: Creating new, insightful features that represent different aspects of risk.
3.  **Risk Aggregation**: Combining individual risk features into a composite score.
4.  **Anomaly Detection**: Applying a machine learning model to identify outliers.
5.  **Results Saving**: Exporting the analyzed data with risk scores for further investigation.

## Features Engineered

The notebook engineers several key features to quantify different dimensions of risk:

* **`supplier_risk`**: A logarithmically transformed count of how many times a specific `supplier_name` has been awarded tenders by a particular `agency`. This helps to identify concentrated engagements or patterns of repeated awards.
* **`amount_risk`**: A standardized score (similar to a Z-score) indicating how unusual an `awarded_amt` is for a given `agency` compared to its historical mean and standard deviation. Both unusually high and unusually low amounts contribute to this risk, flagging potential over/under-bidding or irregular pricing.
* **`status_risk`**: A binary flag (`1` or `0`) indicating if the `tender_detail_status` includes suspicious keywords such as 'cancelled', 'direct award', or 'negotiated'. These statuses can sometimes bypass standard competitive processes, warranting closer scrutiny.
* **`total_risk`**: A combined score summing `supplier_risk` and the absolute value of `amount_risk`. (Note: `status_risk` can also be added here for a more comprehensive score based on your analytical needs).

## Anomaly Detection Model

An **Isolation Forest** model is used to identify anomalies within the procurement data based on the engineered risk features.

* **Model**: `IsolationForest` from `sklearn.ensemble`. This model is particularly effective at isolating abnormal data points.
* **Input Features**: The model is trained on `supplier_risk` and `amount_risk` to learn normal patterns and identify deviations.
* **Output**: It generates an `anomaly_score` where:
    * `-1` indicates an anomaly (a data point considered high risk or an outlier).
    * `1` indicates a normal data point.
* **`is_high_risk`**: A binary flag (`1` for high risk, `0` for normal) derived directly from `anomaly_score == -1`, making it easy to filter and analyze flagged transactions.

## Installation

To run this notebook, you'll need Python and the following libraries. You can install them using `pip`:

```bash
pip install pandas numpy scikit-learn
