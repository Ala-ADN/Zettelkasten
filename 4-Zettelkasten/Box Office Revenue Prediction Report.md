## 1. Executive Summary

This report analyzes a machine learning pipeline designed to predict movie box office revenue. The project utilizes the **Python Data Science stack** (Pandas, NumPy, Scikit-Learn, Seaborn) to process a complex dataset containing a mix of numerical, categorical, text, and temporal data.

The methodology follows a classic Data Science lifecycle:

1. **Data Ingestion & Exploration**: Handling raw CSVs and inspecting structure.
2. **Complex Data Parsing**: Decomposing JSON-formatted string columns.
3. **Feature Engineering**: Creating high-value signals from raw text and categorical entities.
4. **Exploratory Data Analysis (EDA)**: Visualizing distributions and correlations.
5. **Preprocessing**: Imputation, scaling, and transformation.
6. **Iterative Modeling**: Training, evaluating, and refining models based on feature importance.
7. **Deployment**: Serving the model via a web interface.

---
## 2. Data Ingestion and Initial Exploration

The foundation of this analysis is a dataset containing 3,000 movie records. The data is rich in metadata but requires significant cleaning due to the presence of nested JSON objects and missing values.

The project begins by importing necessary libraries for data manipulation (`pandas`, `numpy`), visualization (`matplotlib`, `seaborn`, `plotly`), and machine learning (`sklearn`).
### 2.1. Loading and Inspection

The data is loaded from `train.csv` and `test.csv`. The project immediately identifies a critical structural challenge:

* **Target Variable**: `revenue` (Continuous numerical).
* **Data Structure**: 22 features, but many (e.g., `genres`, `cast`, `production_companies`) are stored as **JSON strings** (lists of dictionaries) rather than simple atomic values.
* **Missing Values**: Initial inspection reveals high missing value ratios in columns like `belongs_to_collection` and `homepage`.

> **Note**: Identifying JSON columns early is crucial because standard CSV parsers treat them as simple strings. They require specific parsing logic (`ast.literal_eval`) to be useful.

### 2.3. Dataset Structure

The dataset consists of **3,000 entries** and **23 columns**. The target variable is `revenue`.

|Column|Non-Null Count|Dtype|De projection|
|---|---|---|---|
|`id`|3000|int64|Unique identifier for the movie.|
|`belongs_to_collection`|604|object|JSON string indicating franchise association (High Missingness).|
|`budget`|3000|int64|Production budget (contains 0s as missing values).|
|`genres`|2993|object|JSON string of genres associated with the movie.|
|`homepage`|946|object|URL of the movie's official website.|
|`imdb_id`|3000|object|Unique ID for IMDB.|
|`original_language`|3000|object|Language code (e.g., 'en', 'fr').|
|`original_title`|3000|object|The title in the original language.|
|`overview`|2992|object|Text summary of the plot.|
|`popularity`|3000|float64|A numeric metric of popularity.|
|`poster_path`|2999|object|Path/URL to the poster image.|
|`production_companies`|2844|object|JSON string of production companies.|
|`production_countries`|2945|object|JSON string of countries where produced.|
|`release_date`|3000|object|Date of release (needs parsing).|
|`runtime`|2998|float64|Duration of the movie in minutes.|
|`spoken_languages`|2980|object|JSON string of languages spoken.|
|`status`|3000|object|Release status (e.g., Released, Rumored).|
|`tagline`|2403|object|Marketing tagline.|
|`title`|3000|object|Movie title.|
|`Keywords`|2724|object|JSON string of plot keywords.|
|`cast`|2987|object|JSON string of actors.|
|`crew`|2984|object|JSON string of technical crew.|
|`revenue`|3000|int64|**Target Variable**: Total box office revenue.|
### 2.3. Sample Record (Example: _Whiplash_)
To understand the complexity of the raw data, consider this sample row for the movie _Whiplash_. Note the mixture of standard text, dates, numericals, and nested JSON lists.

|Feature|Value (Raw)|
|---|---|
|**Title**|`Whiplash`|
|**Homepage**|`http://sonyclassics.com/whiplash/`|
|**IMDB ID**|`tt2582802`|
|**Original Lang**|`en`|
|**Overview**|`Under the direction of a ruthless instructor, ...`|
|**Popularity**|`64.299990`|
|**Release Date**|`10/10/14`|
|**Runtime**|`105.0`|
|**Status**|`Released`|
|**Tagline**|`The road to greatness can take you to the edge.`|
|**Revenue**|`13,092,000`|
|**Spoken Langs**|`[{'iso_639_1': 'en', 'name': 'English'}]`|
|**Keywords**|`[{'id': 1416, 'name': 'jazz'}, {'id': 1523, 'n...`|
|**Cast**|`[{'cast_id': 5, 'character': 'Andrew Neimann',...`|
|**Crew**|`[{'credit_id': '54d5356ec3a3683ba0000039', 'de...`|

---
## 3. Handling Unstructured Data (JSON Parsing)

A significant portion of the project is dedicated to unpacking the JSON-formatted columns.
### 3.1. parsing Strategy

The project defines a helper function `text_to_dict` which applies `ast.literal_eval`.

* **Motivation**: `ast.literal_eval` safely evaluates a string containing a Python literal (like a dictionary) without the security risks of `eval()`.
* **Application**: This is applied to columns: `'belongs_to_collection'`, `'genres'`, `'production_companies'`, `'production_countries'`, `'spoken_languages'`, `'Keywords'`, `'cast'`, `'crew'`.
### 3.2. Entity Extraction

Once parsed into Python objects, the project uses `extract_unique_values` to flatten these lists and understand the cardinality (number of unique values) for each feature.

* **Example**: Determining how many unique *Genre* names or *Production Company* names exist in the dataset.

---
## 4. Feature Engineering: The "Top-N" Strategy

Because features like `Keywords` or `Production Companies` have extremely high cardinality (thousands of unique values), One-Hot Encoding them all would lead to the **Curse of Dimensionality**. The project employs a "Top-N" strategy to handle this.
### 4.1. The Strategy

1. **Count Frequencies**: Calculate how often each entity (e.g., "Warner Bros.") appears.
2. **Select Top N**: Keep the top most frequent entities (e.g., Top 15 genres, Top 10 companies).
3. **Categorize**: Create a new list where valid entities are kept, and everything else is labeled `'Other'`.
4. **Indicator Columns**: Create binary columns (0 or 1) for the Top N entities.
### 4.2. Implementation per Feature

* **Genres**: Top 15 selected. Dominated by "Drama" and "Comedy".
* **Production Companies**: Top 10 selected.
* **Production Countries**: Top 5 selected (Dominated by USA).
* **Spoken Languages**: Top 5 selected (Dominated by English).
* **Keywords**: Top 25 selected (High cardinality reduced to most impactful themes).
* **Cast & Crew**: Top 20 selected.
### 4.3. Specific Feature Logic

* **Belongs to Collection / Homepage**: These were converted to binary flags (`1` if present, `0` if null). The hypothesis is that franchise movies or movies with dedicated websites have higher marketing budgets and revenue.
* **Cast Gender**: A specific function `count_gender_occurrences` counts the number of actors with gender ID '1', '2', or 'Other' per movie, creating numerical features representing the gender balance of the cast.

---
## 5. Visual Analysis and Statistical Insights

The project uses a custom `seaborn` theme for visualization.

### 5.1. Categoricals

![[Pasted image 20260213000916.png]]
* **Finding**: The status feature is unnecessary
### 5.2. Numerical Distributions
![[Pasted image 20260213001055.png]]
* **Finding**: `budget`, `popularity`, and `revenue` are highly right-skewed (long tail). Most movies have low budgets, with a few massive blockbusters.

### 5.3. Plot VS target
![[Pasted image 20260213001534.png]]
- finding: high positive corelation between revenue and budget (highly indicative feature) and no correlation with missing values (can be treated as just the mean with no impact)

---
## 6. Advanced Preprocessing

Before modeling, the project performs rigorous cleaning based on the visual insights.
### 6.1. Imputation

* **Budget**: Values of `0` are replaced with `NaN` and then filled with the **mean budget**.
* **Runtime**: Values of `0` are treated similarly (Mean imputation).
* **Release Date**: Missing dates filled with the mean date.
### 6.2. Text Feature Extraction

Instead of complex NLP (like TF-IDF), the project uses a proxy for information density: **Word Counts**.

* `title_word_count`: Length of the title.
* `tag_word_count`: Length of the tagline.
* `overview_word_count`: Length of the plot summary.
* `has_tagline`: Binary flag.
### 6.3. Date Decomposition

The `release_date` is rich in information. It is decomposed into:

* **Timestamp**: Seconds since epoch (numerical representation for regression).
* (Later in iteration 2): Year, Month, and Season.

---
## 7. Model Training: Iteration 1

The project establishes a baseline using several regression algorithms.
### 7.1. Setup

* **Split**: 80% Train, 20% Test.
* **Cross-Validation**: 5-Fold K-Fold CV to ensure robust error estimation.
* **Metric**: RMSE (Root Mean Squared Error).
### 7.2. Model Selection

Four models are tested:

1. **Decision Tree**: Simple, interpretable, prone to overfitting.
2. **Bagging Regressor**: Ensemble meta-estimator.
3. **Random Forest**: Ensemble of trees (reduces variance).
4. **KNN (K-Nearest Neighbors)**: Distance-based regression.

**Result**: Random Forest outperformed the others (lowest RMSE).
### 7.3. Feature Importance Analysis

Using the Decision Tree, the project extracts feature importance scores.

* **Insight**: `spoken_languages`, `production_countries`, and specific `crew` names had very low importance.
* **Action**: These "noise" features are dropped for the next iteration to reduce model complexity.

---
## 8. Model Training: Iteration 2 (Refinement)

The second pass improves the data representation based on the first pass's findings.
### 8.1. Refined Feature Engineering

* **Dropping Features**: Low-importance features identified in Iteration 1 are removed.
* **New Date Features**: `release_year`, `release_month`, and `release_season` (Winter, Spring, Summer, Fall) are added to capture seasonality (e.g., Summer Blockbusters).
* **Runtime Binning**: `runtime` is binned into `Short`, `Medium`, and `Long` categories.
### 8.2. Log Transformation (Crucial Step)

To address the skewness observed in the EDA phase, the project applies a log transformation to `budget` and `popularity`.
* **Motivation**: This normalizes the distribution, reducing the impact of outliers (blockbusters) and making the relationship between features and target more linear, which often helps models converge better.
### 8.3. Re-evaluation

The models are retrained on this refined feature set. Random Forest remains the top performer.

---
## 9. Final Model & Deployment

The project concludes by training the **Random Forest Regressor** on the full training dataset (not just the CV folds).

1. **Final Training**: `solution_model.fit(X_train, y_train)`
2. **Validation**: Predicts on `X_test` and calculates final RMSE.
3. **Persistence**: The model is serialized and saved as `final_model.pkl` using the `pickle` library.
### 9.1 Web Deployment (Gradio Interface)

This deployment phase completes the **CRISP-DM** cycle. By moving the model from a theoretical experiment into a usable business tool
### 9.1. Deployment Architecture

- **Framework**: **Gradio**, a Python library designed for rapidly creating machine learning demos.
- **Hosting**: **Hugging Face Spaces**, a platform for hosting ML applications.
- **Live Application**: [https://kacemath-data-mining-tp.hf.space/](https://kacemath-data-mining-tp.hf.space/)

### 9.2. Functionality

1. **Accepts User Input**: Provides fields for critical predictive features such as `Budget`, `Runtime`, `Genre` (Dropdown), and `Release Date`.
2. **Processes Input**: Applies the same preprocessing steps (log transformation, encoding) as the training script in real-time.
3. **Generates Prediction**: Feeds the processed input into `final_model.pkl` to generate a revenue forecast.

---
## 10. Proposed Added Value

The primary value proposition of this project is the **quantification of unstructured metadata** to predict financial risk and success.

- **Risk Mitigation for Investors**: By accurately predicting revenue based on pre-release attributes (budget, cast, genre), production studios and investors can better assess the potential ROI of a project before greenlighting it.
- **Optimization of Marketing Spend**: The analysis of the `has_tagline` and `homepage` features provides actionable insights. The correlation found between these features and revenue suggests that digital presence and marketing "hooks" (taglines) are statistically significant drivers of success
- **Seasonality Insight**: By decomposing release dates into seasons, the model implicitly learns the "blockbuster windows" (Summer/Holiday seasons), aiding in optimal release scheduling.

---
### 11. Conclusion

This project demonstrates a robust Data Science workflow. It does not blindly apply models; it iteratively cleans data, engineers features based on domain logic, validates assumptions via visualization, and refines the model by removing noise and normalizing skewed distributions in a rigorous Data Mining workflow.