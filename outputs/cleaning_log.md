# Model Metadata
* **LLM Architecture**: Gemini Pro
* **Version Tag**: Gemini 1.5 Pro Production (June 2026 Build)
* **Environment Execution Scope**: Local Python Script Pipeline Execution

---

# Data Cleaning Log & Lineage Record

## 1. List of Issues Found
1. **Structural Row Redundancy**: 20 absolute cell mirror duplicates.
2. **Text Parameter Formatting Inconsistencies**: 12 text lines with irregular structural trailing spaces/casing flags.
3. **Multi-Format Temporal Clutter**: Chaotic variations across order and shipping date elements.
4. **Primary Key Transactional Conflicts**: 12 duplicate order IDs representing conflicting line records.
5. **Structural Dimensional Nulls**: Missing values found (26 region blocks, 22 ship_mode lines).
6. **Financial Baseline Nulls**: 18 rows with empty discount marks.
7. **Out-of-Range Promotional Violations**: 15 overrides breaching the internal 50% discount parameter cap.
8. **Negative Metric Corruptions**: 15 entries showing impossible negative markdown settings.
9. **Fulfillment Sequence Inversions**: 21 timeline rows logging shipments preceding order registration.
10. **Mathematical Formula Rounding/Drift**: 64 sales line deviations and 64 profit discrepancies from target formulas.

## 2. Cleaning Actions Performed
* Purged exact row structural duplicates instantly.
* Standardized text inputs to single inner-spaced Title Case via regular expressions.
* Normalised chronological sequences into clean YYYY-MM-DD timelines.
* Overwrote legacy baseline rounding errors using strict formula recalculations.

## 3. Business Rules Applied
* Substituted missing text paths with clear '"Unknown"' tags.
* Clamped negative promotional discounts back to a standard 0.00 base floor.
* Stripped out all non-completed/failed invoice entries from true performance segments.

## 4. Assumptions Made
* Null discount entries alongside clean core data are assumed to mean zero promotion applied (0.00).
* Shared order ID collisions represent operational multi-line log rows and were flagged rather than deleted.

## 5. Records Removed
* **Total Rows Purged**: 20 duplicate rows dropped during initialization.

## 6. Records Flagged
* **Total Rows Flagged**: 132 entries marked as '"Review"' via `data_quality_flag`.

## 7. Limitations of the Data Cleaning Process
* Substituting fields with '"Unknown"' keeps accounting pipelines functional but cannot recover unlogged data without secondary master-key tables.
