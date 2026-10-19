================================================================================
                    CS4048: DATA SCIENCE — ASSIGNMENT 2
               EXPLORATORY DATA ANALYSIS OF REAL-WORLD DATASETS
================================================================================
Student Name: Muhammad Waqar Saleem
Roll Number:  23L-1021
Degree:       BS Computer Science (FAST NUCES Lahore)
Repository:   https://github.com/muhammadwaqarsaleem/ds_eda-data_cleaning.git
Release Tag:  v1.0
Date:         October 18, 2026
================================================================================

--------------------------------------------------------------------------------
1. DATASET PUBLICATION & REPRODUCIBILITY METADATA
--------------------------------------------------------------------------------
- Cleaned Netflix Dataset:
  * URL: https://github.com/muhammadwaqarsaleem/ds_eda-data_cleaning/23L_1021_A2_netflix_cleaned_v1.0.csv
  * Version: v1.0 (Tagged Release)
  * Base Source: Kaggle (shivamb/netflix-shows; 8,807 rows, 12 columns)
- Cleaned Supermarket Sales Dataset:
  * URL: https://github.com/muhammadwaqarsaleem/ds_eda-data_cleaning/23L_1021_A2_supermarket_cleaned_v1.0.csv
  * Version: v1.0 (Tagged Release)
  * Base Source: Course Instructor (supermarket_Sales.csv; 1,000 rows, 17 columns)


================================================================================
PART A: NETFLIX MOVIES & TV SHOWS EDA
================================================================================

--------------------------------------------------------------------------------
A1. OVERVIEW & UNIT OF OBSERVATION
--------------------------------------------------------------------------------
The Netflix dataset contains metadata for 8,807 entertainment titles cataloged 
on the platform up to mid-2021. A single row represents an individual, cataloged 
media asset (either a standalone Movie or an episodic TV Show season bundle), 
characterized by creative credits, geographic production origin, parental 
guidance maturity rating, release year, addition date, and category tags.

--------------------------------------------------------------------------------
A2 & A3. DATA QUALITY & CLEANING SUMMARY TABLE
--------------------------------------------------------------------------------
| Problem Identified        | Detection Evidence           | Decision Taken            | Analytical Justification                       |
|---------------------------|------------------------------|---------------------------|------------------------------------------------|
| Schema Shift / Alignment  | 3 rows with rating values    | Corrected shift: moved    | Eliminates invalid rating classes and restores |
| in Duration & Rating      | '74 min', '84 min', '66 min' | duration strings to       | duration numeric parsing without row loss.     |
|                           | (Louis C.K. stand-up titles) | duration; marked rating   |                                                |
|                           |                              | as 'Unavailable'.         |                                                |
| Missing Values in Text    | director (2,634 nulls),      | Imputed explicit category | Preserves 30%+ of the dataset for multi-genre  |
| & Categorical Attributes  | cast (825 nulls),            | string 'Unknown';         | and geographic analyses without introducing     |
|                           | country (831 nulls)          | avoided row drops.        | artificial imputation bias.                    |
| Missing Maturity Ratings  | rating (4 nulls post-fix)    | Imputed 'Unavailable'.    | Prevents discarding titles with valid metadata. |
| Missing Platform Dates    | date_added (10 nulls)        | Imputed median date added | Retains records; minimal impact on macro-level |
|                           |                              | by release-year cohort.   | chronological volume trend analyses.           |
| Unparsed Temporal String  | date_added stored as object  | Converted to datetime64;  | Enables temporal sorting, aggregation, and     |
|                           | strings (e.g., 'Sep 9, 2021')| extracted year_added and  | catalog acquisition lag calculations.          |
|                           |                              | month_added.              |                                                |
| Composite Duration Field  | duration mixed 'X min' and   | Parsed into duration_int  | Separates discrete season counts from          |
|                           | 'Y Season(s)' text strings   | and duration_unit flag    | continuous movie running times for EDA.        |
| Multi-Valued Attributes   | country and listed_in        | Split on comma delimiters;| Prevents string-aggregation artifacts; allows  |
|                           | contained comma-separated    | engineered country_count  | true per-entity marginal frequency counts.     |
|                           | lists                        | and genre_count features. |                                                |

Cleaned output exported as: 23L_1021_A2_netflix_cleaned_v1.0.csv (8,807 rows, 20 columns).

--------------------------------------------------------------------------------
A8. NETFLIX INSIGHTS & INTERPRETATIONS
--------------------------------------------------------------------------------
1. Catalog Dominance and Expansion Shift:
   - Observation: Movies comprise 69.62% of the historical catalog (6,131 titles) 
     versus 30.38% for TV Shows (2,676 titles). However, the share of TV Shows added 
     accelerated from 19.8% in 2015 to 35.4% of all additions in 2020.
   - Interpretation: Netflix began as a film-licensing clearinghouse but pivoted 
     toward episodic series to drive subscriber retention and lifetime value.
   - Speculation: Episodic content fosters multi-week platform engagement and 
     reduces monthly subscriber churn compared to single-view feature films.

2. Catalog Ingestion Delay (Catalog Lag Dynamics):
   - Observation: The median delay between theatrical release and platform addition 
     dropped from 4.0 years for pre-2015 titles to 0.0 years (same-year release) by 
     2018–2021; 58.4% of content added after 2018 was under 12 months old.
   - Interpretation: Netflix shifted aggressively from archival library licensing 
     to direct Day-and-Date global streaming distributions and Original productions.
   - Speculation: Studio licensing windows tightened as legacy media conglomerates 
     launched competing platforms (Disney+, HBO Max), forcing proprietary output.

3. Geographic Production Hub Specialization:
   - Observation: The United States produces the highest volume (3,690 titles), 
     followed by India (1,046 titles) and the United Kingdom (806 titles). Genre 
     profiles diverge sharply: 54.2% of Indian titles belong to 'Dramas' and 
     'International Movies', whereas US output shows high diversification across 
     'Comedies' (28.4%), 'Documentaries' (21.3%), and 'Action' (18.1%).
   - Interpretation: Production ecosystems reflect domestic market structures; 
     India’s catalog mirrors Bollywood feature dominance, whereas Western hubs 
     balance scripted episodic series with documentary journalism.
   - Speculation: Regulatory content quotas and local market viewing habits drive 
     tailored genre commissioning across regional production subsidiaries.

4. Bimodal Concentration in Content Maturity Ratings:
   - Observation: TV-MA (3,207 titles; 36.4%) and TV-14 (2,160 titles; 24.5%) account 
     for 60.9% of the entire catalog. Family-oriented content (TV-Y, TV-G, G) 
     represents less than 11.5% of total catalog assets.
   - Interpretation: Netflix’s core library targets mature adult and adolescent 
     demographics, distinguishing itself from family-centric streaming competitors.
   - Speculation: Adults hold individual payment authority for streaming accounts, 
     incentivizing platforms to prioritize mature, prestige drama acquisitions.

5. Duration Uniformity and Limited Episodic Lifespans:
   - Observation: Movie running times follow an approximately normal distribution 
     centered at Mean = 99.58 minutes (Median = 98.0 min, IQR = 26.0 min). TV Shows 
     exhibit extreme right-skew: 67.0% conclude after Season 1, and 83.8% end by 
     Season 2; fewer than 2.4% reach 5 or more seasons.
   - Interpretation: Feature films adhere to rigid industry exhibition standards 
     (90–120 minutes). In contrast, streaming episodic series face steep renewal 
     hurdles, with platforms canceling shows early if viewership drops post-Season 1.
   - Speculation: Escalating actor contracts and diminishing returns on new subscriber 
     acquisition beyond Season 2 disincentivize long multi-season commitments.

================================================================================
PART B: SUPERMARKET SALES EDA
================================================================================

--------------------------------------------------------------------------------
B1. OVERVIEW & UNIT OF OBSERVATION
--------------------------------------------------------------------------------
The Supermarket Sales dataset comprises 1,000 retail transaction records logged 
across three metropolitan supermarket branches in Myanmar (Yangon, Naypyitaw, 
Mandalay) over a three-month operational quarter (Jan 1, 2019 – Mar 30, 2019). 
A single row represents a distinct point-of-sale (POS) checkout transaction invoice, 
recording item category, unit price, quantity, tax, total spend, customer tier, 
tender type, date-time, and satisfaction rating.

--------------------------------------------------------------------------------
B2 & B3. DATA QUALITY & CLEANING SUMMARY TABLE
--------------------------------------------------------------------------------
| Problem Identified        | Detection Evidence           | Decision Taken            | Analytical Justification                       |
|---------------------------|------------------------------|---------------------------|------------------------------------------------|
| Missing Values & Duplicates| df.isnull().sum() == 0;      | None required; preserved  | Dataset is 100% structurally complete and      |
|                           | duplicated().sum() == 0      | all 1,000 records.        | contains zero key collision defects.          |
| Invariant Static Column   | gross_margin_pct has single  | Dropped column from the   | Feature has zero variance (sigma^2 = 0);       |
| (Zero Variance)           | unique value: 4.761905%      | active feature matrix.    | provides zero entropy and induces singularity. |
| Perfect Multicollinearity | cogs, tax_5_pct, gross_income| Retained total as target;  | Variables are exact scalar multiples (r=1.00); |
| in Accounting Ledger      | show r = 1.00 with total     | documented collinearity.  | keeping all four inflates VIF to infinity.     |
| Unparsed Temporal Fields  | date and time stored as text | Parsed to datetime64;     | Enables extraction of cyclical features        |
|                           | objects ('M/D/YYYY', 'HH:MM')| combined into timestamp.  | necessary for intraday and weekly analysis.    |
| Missing Intraday Signals  | Schema lacked discrete       | Engineered hour, day_name,| Essential for testing bimodal shopping peaks   |
|                           | operational shopping bins    | time_of_day, is_weekend.  | and day-of-week revenue variations (Q4).       |
| Non-Standard Header Names | Mixed casing and characters  | Renamed all headers to    | Eliminates property access syntax errors and   |
|                           | ('Tax 5%', 'Customer...')    | clean snake_case.         | standardizes schema for Assignment 3.          |
| High-Spend Outliers       | 9 rows exceed Q3 + 1.5*IQR   | Retained all 9 outliers   | Baskets represent valid high-unit/high-price   |
| in Total ($991.74+)       | in total and cogs            | unaltered.                | transactions; deletion distorts branch sales.  |

Cleaned output exported as: 23L_1021_A2_supermarket_cleaned_v1.0.csv (1,000 rows, 25 columns).

--------------------------------------------------------------------------------
B8. SUPERMARKET SALES INSIGHTS & INTERPRETATIONS
--------------------------------------------------------------------------------
1. Decoupling of Product Line Revenue from Basket Size:
   - Observation: 'Food and beverages' generated the highest cumulative revenue 
     ($56,144.84 across 174 transactions), yet ranked 4th in average basket spend 
     ($322.67). Conversely, 'Home and lifestyle' ranked 5th in total revenue 
     ($53,861.91) but ranked 1st in average basket spend ($336.64).
   - Interpretation: High revenue in consumables is foot-traffic and volume-driven, 
     whereas home furnishing sales rely on higher per-ticket monetary expenditure.
   - Speculation: Supermarkets serve as frequent grocery stops for daily essentials, 
     while home goods purchases represent deliberate, periodic high-ticket visits.

2. Exact Multicollinearity Across Point-of-Sale Accounting Measures:
   - Observation: cogs, tax_5_pct, total, and gross_income exhibit mutual pairwise 
     Pearson correlations of r = 1.00. Tax 5% and Gross income are identical down 
     to the 4th decimal place across all 1,000 records (passing np.isclose with 0 residuals).
   - Interpretation: The financial metrics are deterministic linear scalings of 
     COGS (Unit Price x Quantity). Gross margin percentage is mathematically fixed 
     at (1/21)*100 = 4.761905%, contributing zero statistical information.
   - Speculation: The store records accounting line-items through automated register 
     scripts rather than real-time product-specific cost accounting.

3. Geographic and Performance Uniformity Across Branches:
   - Observation: Total revenue across branches differed by less than 4.1% ($106,200 
     in Yangon vs. $110,569 in Naypyitaw). One-way ANOVA tests showed no significant 
     difference in sales (F=0.885, p=0.413), quantities (F=0.151, p=0.860), or 
     ratings (F=2.076, p=0.126). Between-branch mean difference ($24.75) accounted 
     for only 10.1% of within-branch standard deviation ($246.10).
   - Interpretation: Retail performance is homogeneous across geographic territories; 
     branch location does not explain individual transaction variance.
   - Speculation: Standardized chain-wide store layouts, identical inventory catalogs, 
     and uniform pricing models produce homogeneous customer behaviors across cities.

4. Loyalty Membership Fails to Drive Basket Expansion:
   - Observation: Registered Members averaged $327.79 per transaction versus $318.12 
     for Normal walk-in customers (Welch's t = 0.6215, p = 0.5344; Mann-Whitney 
     U p = 0.5912). Mean item quantity was virtually identical (5.56 vs. 5.46 units).
   - Interpretation: The customer loyalty card program functions as an identification 
     mechanism rather than a value driver that stimulates greater basket expenditure.
   - Speculation: The membership scheme may lack tiered financial incentives, 
     exclusive member discounts, or gamified thresholds needed to alter buyer habits.

5. Customer Satisfaction Decoupled from Transaction Variables:
   - Observation: Customer satisfaction ratings correlate negligibly with unit price 
     (r = -0.0088, p = 0.782), quantity (r = -0.0158, p = 0.617), and total spend 
     (r = -0.0364, p = 0.250). Ratings showed no significant variation across product 
     lines (ANOVA F = 0.540, p = 0.746) or store branches (ANOVA F = 2.076, p = 0.126).
   - Interpretation: Point-of-sale spending economics do not explain customer sentiment. 
   - Speculation: Customer ratings reflect unmeasured service variables (cashier speed, 
     staff courtesy, wait times, cleanliness) or uniform survey acquiescence bias.

================================================================================
