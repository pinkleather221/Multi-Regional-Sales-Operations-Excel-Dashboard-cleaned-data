# Project Overview
This project analyzes transactional sales data for a multi-regional electronics distributor. The objective was to clean, enrich, analyze, and visualize sales performance across regions, countries, channels, products, and sales teams.

## Data Quality Issues Identified
* **Missing values** were identified in:
  * City
  * Salesperson
  * Channel
* **Inconsistent date relationships** were found where:
  * `RequiredDate` occurred earlier than `OrderDate`.
* **Discount values** above normal business thresholds (>30%) were identified and flagged as potential anomalies.
* A small number of records contained **missing categorical information** that could affect reporting and segmentation analyses.
* **No exact duplicate records** were found after review of the transactional dataset.

## Data Cleaning Rules Applied

### Missing Values
* Missing values in **City**, **Salesperson**, and **Channel** were replaced with **"Unknown"** to preserve records while maintaining reporting consistency.

### Date Validation
* Records where `RequiredDate` was earlier than `OrderDate` were corrected by imputing a revised `RequiredDate` by adding the average number of days from the difference between the required date and order date.
* `LeadTimeDays` was then recalculated using the corrected dates.

### Derived Metrics
The following calculated fields were created:
* `GrossRevenue` = UnitPrice × Quantity × (1 − DiscountPct)
* `CostOfGoods` = UnitCost × Quantity
* `GrossProfit` = GrossRevenue − CostOfGoods
* `MarginPct` = GrossProfit ÷ GrossRevenue
* `LeadTimeDays` = RequiredDate − OrderDate

### Standardized Dimensions
Additional analytical dimensions were created:
* **Month** (MMM-YYYY)
* **Quarter** (Q1–Q4)
* **Region Hierarchy** (Region → Country → City)
* **PriceBand** (Low, Medium, High) using quantile-based classification

### Quality Flags
* High discounts were flagged for review.
* Orders meeting the 7-day service target were identified using a binary service-level flag.

## Assumptions
* Missing categorical values were assumed to be unknown rather than removed to avoid unnecessary data loss.
* Discounts greater than 30% were retained in the dataset but flagged for analysis because they may represent legitimate promotional activity.
* Service level performance was measured using a 7-day lead time target.
* Price bands were generated using quantile segmentation to ensure a balanced distribution across Low, Medium, and High categories.
* Scenario modeling assumed that changes in discount caps, cost inflation, and quantity uplift affect all transactions consistently across regions and product categories.
* Revenue and profitability calculations were performed using the cleaned dataset after all corrections and transformations were completed.
