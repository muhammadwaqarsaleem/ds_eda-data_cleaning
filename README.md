# CS4048: Data Science — Assignment 2: Exploratory Data Analysis

This repository hosts the Exploratory Data Analysis (EDA), data cleaning pipelines, and published dataset artifacts for **CS4048: Data Science (Assignment 2)** at **FAST NUCES, Lahore Campus**[cite: 1, 2]. The analysis rigorously follows the required hypothesis-driven workflow:

$$\text{Question} \longrightarrow \text{Analysis} \longrightarrow \text{Visualization / Statistic} \longrightarrow \text{Observation} \longrightarrow \text{Interpretation}$$


[cite: 1]

---

## 📌 Metadata & Student Information

* **Student Name:** Muhammad Waqar Saleem
* **Roll Number:** 23L-1021
* **Degree Program:** BS Computer Science (7th Semester)[cite: 1]
* **Institution:** FAST School of Computing, FAST-NUCES Lahore[cite: 1]
* **Repository:** [`muhammadwaqarsaleem/ds_eda-data_cleaning`](https://github.com/muhammadwaqarsaleem/ds_eda-data_cleaning.git)
* **Release Version:** `v1.0`[cite: 6]
* **Date:** October 18, 2026[cite: 1]

---

## 📦 Published Datasets & Versioning (Reproducibility)

In compliance with the assignment specification, both cleaned datasets have been versioned and published for direct reuse in **Assignment 3 (Feature Selection & Dimensionality Reduction)**[cite: 1, 6]:

| Dataset | Raw Source | Cleaned CSV Artifact | Version | Schema Dimensions |
| --- | --- | --- | --- | --- |
| **Netflix Movies & TV Shows** | [Kaggle (`shivamb/netflix-shows`)](https://www.kaggle.com/datasets/shivamb/netflix-shows)[cite: 2] | [`23L_1021_A2_netflix_cleaned_v1.0.csv`](https://www.google.com/search?q=https://github.com/muhammadwaqarsaleem/ds_eda-data_cleaning/blob/v1.0/23L_1021_A2_netflix_cleaned_v1.0.csv) | `v1.0`[cite: 6] | 8,807 rows × 20 cols[cite: 2] |
| **Supermarket Sales** | Course Instructor (`supermarket_Sales.csv`)[cite: 2] | [`23L_1021_A2_supermarket_cleaned_v1.0.csv`](https://www.google.com/search?q=https://github.com/muhammadwaqarsaleem/ds_eda-data_cleaning/blob/v1.0/23L_1021_A2_supermarket_cleaned_v1.0.csv) | `v1.0`[cite: 6] | 1,000 rows × 25 cols[cite: 2] |

---

## 📂 Repository Structure

```text
├── data/
│   ├── netflix_titles.csv                       # Raw Kaggle Netflix catalog
│   └── supermarket_Sales.csv                    # Raw Supermarket transaction data
├── 23L_1021_A2.ipynb                            # Fully executed Jupyter Notebook
├── 23L_1021_A2_netflix_cleaned_v1.0.csv         # Cleaned & feature-engineered Netflix CSV
├── 23L_1021_A2_supermarket_cleaned_v1.0.csv     # Cleaned & feature-engineered Supermarket CSV
├── writeup.pdf                                  # Formal summary report (2-4 pages)
└── README.md                                    # Documentation and data dictionary

```

---

## Part A: Netflix Movies and TV Shows EDA

### A1. Dataset Overview & Unit of Observation

* **Unit of Observation:** A single media title cataloged on Netflix globally through mid-2021[cite: 2].
* **Scope:** 8,807 unique video assets across two distinct media classes: standalone feature films (`Movie`) and episodic television bundles (`TV Show`)[cite: 2].

### A2 & A3. Data Quality Audit & Cleaning Decisions

| Defect / Problem | Detection Evidence | Decision Taken | Analytical Justification |
| --- | --- | --- | --- |
| **Schema Shift / Misalignment** | 3 rows contained duration strings in the `rating` column (`'74 min'`, `'84 min'`, `'66 min'`) for Louis C.K. stand-up titles[cite: 3]. | Shifted duration values to `duration`; imputed `rating` as `'Unavailable'`. | Eliminates invalid maturity classes and restores duration parsing without dropping valid rows. |
| **Missing Categorical / Text Values** | High null counts: `director` (2,634 nulls), `country` (831 nulls), `cast` (825 nulls)[cite: 3]. | Imputed explicit category string `'Unknown'`. | Retains over 30% of the dataset; prevents artificial skew from listwise row deletion. |
| **Missing Maturity Ratings** | 4 rows with null `rating` post-realignment[cite: 3]. | Imputed `'Unavailable'`. | Prevents data loss for otherwise complete media records. |
| **Missing Ingestion Dates** | 10 titles missing `date_added`[cite: 3]. | Imputed median addition date conditional on release year cohort. | Retains records; negligible distortion on multi-year macro catalog trends. |
| **Unparsed Temporal Strings** | `date_added` stored as text objects (e.g., `'September 9, 2021'`). | Converted to `datetime64[ns]`; extracted `year_added` and `month_added`. | Enables temporal windowing, chronological ordering, and ingestion lag analysis[cite: 3]. |
| **Composite Duration Field** | `duration` held mixed metrics: minutes for Movies, season counts for TV Shows[cite: 3]. | Parsed into `duration_int` (integer) and `duration_unit` (`min` vs. `Season`). | Separates discrete multi-season series from continuous movie runtimes[cite: 3]. |
| **Delimited Multi-Valued Attributes** | `country` and `listed_in` contained multi-entity comma-separated strings[cite: 3]. | Maintained primary strings; extracted `country_count` and `genre_count`[cite: 3]. | Quantifies international co-production scale and genre complexity without artificial row explosion. |

### A8. Core Insights & Analytical Findings

1. **Catalog Dominance and Episodic Expansion**
* **Observation:** Feature films comprise 69.62% of the historical catalog (6,131 titles) versus 30.38% for TV Shows (2,676 titles). However, the TV Show share of annual additions increased from 19.8% in 2015 to 35.4% in 2020.
* **Interpretation:** Netflix originated as a movie licensing repository but pivoted toward episodic series to boost subscriber retention and lifetime platform engagement.
* *Speculation:* Episodic content encourages sustained weekly viewership, mitigating monthly subscription cancellation rates more effectively than standalone films.


2. **Ingestion Lag Compression (Day-and-Date Transition)**
* **Observation:** The median lag between theatrical release and platform addition collapsed from 4.0 years (pre-2015 titles) to 0.0 years (same-year release) during 2018–2021, with 58.4% of post-2018 additions arriving within 12 months of release.
* **Interpretation:** Netflix transitioned away from long-tail syndication toward proprietary Netflix Originals and day-and-date global premieres.
*Speculation:* Legacy Hollywood studios withholding intellectual property for their own streaming platforms (e.g., Disney+, HBO Max) accelerated Netflix's direct commissioning.


3. **Regional Production Hub Specialization**
* **Observation:** The United States leads catalog volume (3,690 titles), followed by India (1,046) and the United Kingdom (806). In India, 54.2% of all titles fall under *Dramas* and *International Movies*, whereas US output is balanced across *Comedies* (28.4%), *Documentaries* (21.3%), and *Action* (18.1%).
* **Interpretation:** Regional catalog profiles reflect domestic production strengths—India's catalog is driven by feature film productions, while Anglo-American markets feature broader diversification across unscripted and documentary formats.
*Speculation:* International content quotas and regional subscriber viewing habits heavily dictate customized commissioning budgets.


4. **Bimodal Maturity Concentration**
* **Observation:** Mature content dominates: `TV-MA` (3,207 titles; 36.4%) and `TV-14` (2,160 titles; 24.5%) together represent 60.9% of the catalog. Family-oriented classifications (`TV-Y`, `TV-G`, `G`) account for less than 11.5%.
* **Interpretation:** Netflix positions itself primarily toward adult and adolescent demographics rather than functioning as a dedicated family or children's network.
*Speculation:* Household purchasing authority rests with adults, driving content budgets toward prestige adult drama.


5. **Episodic Lifespan Bottlenecks**
* **Observation:** Movie running times follow a bell-shaped distribution centered at $\mu = 99.58$ minutes ($\text{Median} = 98.0$ min, $\text{IQR} = 26.0$ min). TV Shows exhibit an extreme right-skew: 67.0% terminate after Season 1, 83.8% conclude by Season 2, and fewer than 2.4% reach 5 or more seasons.
* **Interpretation:** Films adhere strictly to global theatrical standards (90–120 minutes). In contrast, episodic series face steep algorithmic renewal hurdles, with cancellations common after early seasons if engagement targets are missed.
*Speculation:* Escalating production contracts and diminishing returns on subscriber acquisition past Season 2 disincentivize long-running series.



---

## Part B: Supermarket Sales EDA

### B1. Dataset Overview & Unit of Observation

* **Unit of Observation:** A single point-of-sale (POS) checkout transaction completed at one of three retail branches across Myanmar (Yangon, Naypyitaw, Mandalay) between January 1, 2019, and March 30, 2019[cite: 2].
* **Scope:** 1,000 distinct retail transaction invoices capturing store location, merchandise department, pricing, quantities, payment tender, and customer satisfaction[cite: 2].

### B2 & B3. Data Quality Audit & Cleaning Decisions

| Defect / Problem | Detection Evidence | Decision Taken | Analytical Justification |
| --- | --- | --- | --- |
| **Missing Values & Duplicates** | Evaluated via `df.isnull().sum()` ($0$) and `df.duplicated().sum()` ($0$). | Preserved all 1,000 transaction records. | Dataset is 100% structurally complete with zero primary key collisions. |
| **Zero-Variance Feature** | `Gross margin percentage` contained a single constant value ($4.761905\%$) across all rows ($\sigma^2 = 0$). | Dropped column from active feature set. | Static constants carry zero entropy and cause rank deficiency or singular matrices in linear models. |
| **Collinear Accounting Measures** | `Cost of goods sold`, `Tax 5%`, `Gross income`, and `Total` show mutual correlations of $r = 1.00$. | Retained `total` as primary target; documented dependencies. | Identical linear combinations inflate the Variance Inflation Factor ($\text{VIF} \to \infty$) if used together. |
| **Unparsed Datetime Objects** | `Date` and `Time` stored as raw strings (`M/D/YYYY` and `HH:MM`). | Parsed to native `datetime64[ns]`; created unified `timestamp`. | Enables temporal windowing and chronological analysis[cite: 5]. |
| **Lack of Temporal Granularity** | Raw schema lacked discrete intraday or day-of-week indicators. | Engineered `hour`, `day_of_week`, `day_name`, `time_of_day`, and `is_weekend`. | Allows isolation of peak trading windows, day-of-week trends, and diurnal cycles[cite: 5, 6]. |
| **Non-Standard Header Names** | Headers contained spaces, mixed casing, and special characters (`Tax 5%`). | Normalized all headers to Python `snake_case`. | Prevents syntax errors during dot-notation property access and standardizes pipelines. |
| **High Financial Values (Outliers)** | 9 records exceeded Tukey's upper fence in `total` and `cogs` ($> \$991.74$). | **Retained all 9 outliers unaltered.** | Outliers stem from high-unit ($9\text{--}10$) and high-price ($\ge \$90$) purchases. Deleting valid sales would distort revenue totals[cite: 5]. |

### B8. Core Insights & Analytical Findings

1. **Decoupling of Departmental Volume from Basket Size**
* **Observation:** *Food and beverages* generated the highest cumulative revenue ($56,144.84 across 174 transactions), yet ranked 4th in average basket spend ($322.67). Conversely, *Home and lifestyle* ranked 5th in total revenue ($53,861.91) but ranked 1st in average basket spend ($336.64).
* **Interpretation:** Fast-moving consumer goods drive gross chain revenue through foot traffic and visit frequency, whereas durable home goods rely on larger individual basket values.
*Speculation:* Supermarkets serve as high-frequency stops for daily perishables, while household goods purchases represent planned, higher-expenditure visits.


2. **Multicollinearity Across Point-of-Sale Metrics**
* **Observation:** `cogs`, `tax_5_pct`, `total`, and `gross_income` exhibit pairwise Pearson correlations of $r = 1.00$. `Tax 5%` and `Gross income` are identical down to the fourth decimal place across all 1,000 records.
* **Interpretation:** The financial fields represent deterministic linear rescalings of $\text{COGS} = \text{Unit Price} \times \text{Quantity}$. Gross margin is mathematically fixed at $(1/21) \times 100 \approx 4.761905\%$, providing zero independent statistical information.
*Speculation:* Accounting metrics are computed via register software scripts rather than tracking product-specific margin differences.


3. **Branch Performance and Geographic Uniformity**
* **Observation:** Cumulative revenue across branches varied by less than 4.1% ($106,200 in Yangon vs. $110,569 in Naypyitaw). One-way ANOVA tests confirmed no significant differences in sales ($F = 0.885, p = 0.413$), quantities ($F = 0.151, p = 0.860$), or customer ratings ($F = 2.076, p = 0.126$). Between-branch mean difference ($24.75) accounts for only 10.1% of within-branch standard deviation ($246.10).
* **Interpretation:** Retail performance is homogeneous across urban centers; physical branch location does not explain individual transaction variance.
*Speculation:* Standardized chain-wide store layouts, identical inventories, and centralized pricing enforce uniform purchasing behaviors across cities.


4. **Loyalty Membership Impact on Basket Size**
* **Observation:** Registered Members averaged $327.79 per visit compared to $318.12 for non-member walk-ins (Welch's $t = 0.6215, p = 0.5344$; Mann-Whitney $U = 127453.0, p = 0.5912$). Mean basket item quantities were nearly identical (5.56 vs. 5.46 units).
* **Interpretation:** The loyalty membership program functions primarily as an identifier rather than an incentive system that drives larger basket values.
*Speculation:* The program may lack compelling financial rewards (such as tiered volume discounts or cash-back rebates) necessary to alter shopper spending thresholds.


5. **Customer Satisfaction Ratings Independence**
* **Observation:** Customer satisfaction ratings correlate negligibly with unit price ($r = -0.0088, p = 0.782$), quantity ($r = -0.0158, p = 0.617$), and total spend ($r = -0.0364, p = 0.250$). Ratings also show no significant differences across product lines (ANOVA $p = 0.746$) or store branches (ANOVA $p = 0.126$).
* **Interpretation:** Point-of-sale transactional metrics do not explain customer satisfaction.
*Speculation:* Customer ratings reflect unmeasured service variables (cashier speed, staff courtesy, wait times, cleanliness) or uniform survey acquiescence bias.



---

## 🚀 Setup & Execution Guide

### 1. Environment Configuration

Clone the repository and set up a Python virtual environment:

```bash
# Clone the repository
git clone https://github.com/muhammadwaqarsaleem/ds_eda-data_cleaning.git
cd ds_eda-data_cleaning

# Create and activate virtual environment
python -m venv ds_env
source ds_env/bin/activate  # On Windows: ds_env\Scripts\activate

# Install strictly allowed course dependencies
pip install --upgrade pip
pip install pandas numpy matplotlib seaborn scipy statsmodels jupyter

```

### 2. Running the Analysis

Launch the Jupyter notebook environment:

```bash
jupyter notebook 23L_1021_A2.ipynb

```

Execute cells sequentially from top to bottom[cite: 1]. The notebook is partitioned into:

* **Cells 01–37:** Part A — Netflix Catalog EDA (A1 through A8)[cite: 3, 4]
* **Cells 38–70:** Part B — Supermarket Sales EDA (B1 through B8)[cite: 4, 5, 6]

---

## 📋 Course Compliance & Academic Integrity

* **Prohibited Libraries:** Automated EDA toolkits (e.g., `ydata-profiling`, `Sweetviz`, `AutoViz`) were strictly excluded in accordance with course guidelines[cite: 1]. All analytical transformations and visual encodings were constructed manually using `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, and `statsmodels`[cite: 1].
* **AI Disclosure:** Consulted Google Gemini as an analytical thought partner for statistical validation, visualization formatting, and rubric alignment[cite: 2]. All code execution, data cleaning logic, and written interpretations were independently conducted, verified, and audited[cite: 2].
* **License:** Distributed under the MIT License for educational and non-commercial academic research.
