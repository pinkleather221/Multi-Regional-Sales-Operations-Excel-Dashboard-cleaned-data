# Project Overview
This project analyzes transactional sales data for a multi-regional electronics distributor. The objective was to clean, enrich, analyze, and visualize sales performance across regions, countries, channels, products, and sales teams.

## Key Business Insights

1. **Europe generated the highest overall gross profit**, contributing approximately **869,836**, narrowly outperforming the Americas. This suggests strong product performance and healthy margins across European markets.

2. **The Americas region showed the strongest Direct and Online channel performance**, with Online channels generating over **261,944** in gross profit. This indicates a successful digital sales strategy and strong customer adoption of non-retail purchasing channels.

3. **Africa exhibited the highest reliance on Distributor and Retail channels**, while Online contributed a smaller share of revenue compared to other regions. This suggests an opportunity to expand digital sales channels and improve online market penetration.

4. **Asia demonstrated a relatively balanced channel mix**, with Retail, Direct, and Distributor channels contributing comparable levels of revenue and profit. This diversification reduces dependence on a single sales channel and provides greater resilience.

5. **Salesperson performance varied significantly across the organization.** M. Rossi and K. Singh recorded some of the highest revenue per order values (above 20,000), while N. Brown and I. Johnson recorded the lowest productivity levels, indicating potential opportunities for coaching or territory review.

6. **Service level performance fell below the 7-day target across most countries and product categories.** The overall on-time rate was approximately **21.8%**, suggesting supply chain, fulfillment, or scheduling inefficiencies that should be investigated.

7. **Revenue trends fluctuated considerably over time.** Peak sales periods were observed during months such as **July 2023** and **February 2023**, while several months in 2024 and 2025 experienced lower revenue volumes, indicating possible seasonality effects and changing customer demand patterns.

8. **Scenario analysis demonstrated that profitability is highly sensitive to discount policies, cost inflation, and sales volume growth.** Aggressive growth scenarios increased revenue potential, while higher cost inflation significantly reduced profit margins, highlighting the importance of balancing pricing strategy with operational efficiency.


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
