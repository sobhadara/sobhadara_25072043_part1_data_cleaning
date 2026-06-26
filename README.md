# E-Commerce Operations: Data Cleaning & Analytics Pipeline

## 📌 Problem Summary
This project addresses systemic data integrity issues within a raw retail transactional dataset (`raw_orders.xlsx`). The source data was plagued by transactional duplication, fragmented text attributes, multi-format date records, chronological logical errors, out-of-bounds promotional marks, and severe formulaic drift on calculated sales and profit ledgers. 

The objective of this pipeline is to ingest, clean, audit, and engineer a production-ready reporting layer (`cleaned_orders.xlsx`). This layer supports automated data quality summaries (`data_quality_report.xlsx`) and institutional performance views (`pivot_summary.xlsx`) to drive reliable, audit-ready business intelligence.

---

## 📊 Dataset Description
The database matrix tracks **932 raw transaction entries** spanning across retail performance periods from January 2024 through October 2025. Each record captures a multi-dimensional array across three core pillars:
* **Transactional Primary Keys:** `order_id`, `customer_name`
* **Operational & Logistical Dimensions:** `segment`, `region`, `state`, `city`, `category`, `sub_category`, `ship_mode`, `order_status`, `payment_status`
* **Financial & Inventory Metrics:** `quantity`, `unit_price`, `cost`, `discount`, `sales`, `profit`

---

## 🛠️ Tools Used
* **Python 3.11+** (Core programmatic processing engine)
* **Pandas & NumPy** (Vectorized data manipulation, matrix slicing, and mathematical feature engineering)
* **OpenPyXL** (Multi-tab enterprise Excel sheet generation and formatting)
* **Regular Expressions (Regex)** (String pattern stripping, text normalization, and case standardization)
* **Markdown** (Compliance logging and documentation)

---

## 🧼 Cleaning Steps Performed
1.  **Ingestion De-duplication:** Scanned the raw matrix for absolute cell-by-cell redundancies, purging **20 identical rows** to safeguard performance metrics against artificial inflation.
2.  **Regex Whitespace Sanitization:** Applied regex patterns to eliminate leading, trailing, and irregular inner whitespaces across all text fields.
3.  **Programmatic Title-Casing:** Standardized categorical text values (e.g., converting mixing cases like `completed`, `COMPLETED `, and `Completed`) to a clean Title Case format.
4.  **Temporal Harmonization:** Parsed chaotic date representations (e.g., mixing `21 Jul 2024`, `08/31/2024`, and `2024-02-20`) into an invariant, unified ISO format (`YYYY-MM-DD`).
5.  **Promotional Floor & Ceiling Clamping:** Evaluated alphanumeric discount variables, parsed percent strings, reset negative discounts to a `0.00` floor, and tagged extreme outliers.
6.  **Financial Formula Overwrites:** Eradicated downstream calculation drift by programmatically replacing raw `sales` and `profit` metrics with precise, line-level inventory equations.
7.  **Quality Evaluation Tagging:** Appended an automated evaluation column (`data_quality_flag`) to partition clean entries from flagged entries without discarding operational context.

---

## 🛡️ Business Rules Applied
* **Dimensional Safeguards:** Missing strings within geographical (`region`) or logistical (`ship_mode`) nodes are never dropped. They are filled with a standardized `"Unknown"` placeholder token to preserve transaction row integrity.
* **Promotional Cap Compliance:** Operational promotional discounts must be numeric and are strictly capped at a ceiling of `0.50` (50%). Entries violating this constraint are safely clamped or flagged.
* **Chronological Invariant:** Deliveries must progress forward linearly in time (`order_date` $\le$ `ship_date`). Inverted intervals are immediately isolated as processing anomalies.
* **Performance Performance Segmentation:** Non-completed, cancelled, failed, or returned records are filtered out of all sales and profit summaries to provide an accurate view of realized performance.

---

## 🚨 Summary of Data Quality Issues Found
The automated profiling sequence mapped **132 flagged records** containing one or more relational and mathematical violations:

| Detected Data Anomaly Category | Total Incidents | Operational Risk Mitigation Executed |
| :--- | :---: | :--- |
| **Exact Row Duplicates** | 20 | Dropped permanently from production files during ingestion. |
| **Missing Operational Dimensions** | 48 | Populated with `"Unknown"` string tokens; flagged for review. |
| **Missing / Null Financial Discounts** | 18 | Defaulted to standard `0.00` unpromoted baseline. |
| **Negative Financial Discounts** | 16 | Clamped to a `0.00` accounting floor; flagged for review. |
| **Out-of-Bounds Discounts (> 50%)** | 15 | Parsed text characters to float; flagged for review. |
| **Fulfillment Loop Inversions** | 22 | Logged as an operational sequencing error; flagged for review. |
| **Sales/Profit Calculation Drift** | 131 | Overwritten programmatically using ground-truth formulas. |

---

## 📈 Summary of Final Pivot Reports
The clean reporting architecture generates **6 core pivot summaries** structured across designated, context-specific tabs within `pivot_summary.xlsx`:

1.  **Sales & Profit by Region:** Performance-focused tab highlighting total realized revenue. Sorted in descending order by sales to isolate market scale.
2.  **Sales & Profit by Category & Sub-Category:** Hierarchical product nested breakdown mapping top financial streams across inventory divisions.
3.  **Order Count by Ship Mode:** Administrative log documenting total physical shipping distribution allocations across all gross orders.
4.  **Profit Margin by Customer Segment:** Calculated field matrix analyzing realized net margin performance ratios across core consumer groups.
5.  **Anomalies by Region:** Isolated transaction risk tracker filtering out clean data to display exactly where order cancellations, returns, and payment failures concentrate.
6.  **Monthly Sales Trend:** Chronological timeline aggregation tracing realized sales velocity month-over-month from 2024 into 2025.

---

## 💡 Key Business Insights
* **Highest Efficiency Segment:** The **Corporate Segment** achieves the organization’s peak financial efficiency, generating an aggregate **29.33% profit margin**, closely followed by Home Office at 29.10%.
* **Primary Revenue Anchors:** Product revenue is anchored by the **Technology: Phones** sub-category (generating ₹668,269.04 in realized sales) and the **Furniture: Chairs** line (generating ₹583,968.65).
* **Operational Leakage Hotspot:** The **North Region** represents the highest operational risk profile, leading the company with **86 distinct cancelled, returned, or failed transaction incidents**.
* **Temporal Demand Peak:** Historical demand shows sharp mid-year scaling in **June 2024** (₹392,980.64) and an early-year volume peak in **February 2025** (₹394,995.72).

---

## ⚠️ Assumptions and Limitations
* **Assumptions on Null Discounts:** Empty discount cells were assumed to represent transaction records where no seasonal promotion or coupon was active, defaulting safely to `0.00`.
* **Assumptions on Order ID Collisions:** Shared transactional Order IDs containing unique items are assumed to represent authentic multi-line order invoices. They are preserved in full and marked as `"Review"`.
* **Limitation of Unknown Substitutions:** Imputing missing text values with `"Unknown"` maintains transaction balance sheets but cannot reconstruct geographical or logistics gaps without secondary data integrations.
* **Limitation of Forced Formula Adjustments:** Overwriting drifted sales and profit entries provides immediate mathematical consistency for reporting but masks underlying currency conversions or legacy system rounding issues.

---

## 📸 Screenshots Included
*Below are placeholders for the visual verifications taken from the final individual project workbooks:*

### Clean vs Flagged Summary Sheet
![Data Quality Summary Count](screenshots/dq_summary_count.png)

### Invalid Discount Audit Logs
![Invalid Discount Summary Tab](screenshots/invalid_discount_summary.png)

### Regional Sales and Profit Pivot View (Sorted Descending)
![Regional Sales Pivot Summary](screenshots/regional_sales_pivot.png)

### Lost Value Analytics (Anomalies by Region)
![Anomalous Transactions Filtered View](screenshots/anomalies_pivot.png)