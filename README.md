# Housing Price EDA & Prediction — Alteryx

## Overview
This project performs an Exploratory Data Analysis (EDA) on a housing dataset to identify the key variables that predict sale price. Three linear regression models were built and compared to understand which property features — from garage quality to roof material — have the greatest impact on what a home sells for.

**Research Question:** What independent variables best predict house sale price?

---

## 🛠️ Tools & Technologies
- Alteryx Designer
- Linear Regression (OLS)
- Pearson's Correlation Analysis
- EDA Workflow (Alteryx)

---

## 📂 Dataset
- **File:** `03_Housingdata.csv`
- **Size:** 2,903 records × 82 variables
- **Target Variable:** Sale Price

---

## 🔍 Analysis

### 1️⃣ Sale Price Summary Statistics

The dataset contains complete sale price data with no null values. The mean ($189,554.77) is higher than the median ($168,712), indicating a right-skewed distribution driven by high-value outliers.

![Sale Price Summary Statistics](image/a1f1data.png)

The dataset includes a mix of variable types: Byte, Double, Int16, Int32, String, V_String, and Unknown.

![Variable Types](images/a1f2_variable_types.png)

---

### 2️⃣ EDA on Continuous Variables

The histogram below shows the right-skewed distribution of sale prices, with most homes concentrated between $100,000–$250,000 and a long tail toward higher values. The standard deviation of $84,258 reflects the wide range of property types in the dataset.

![Sale Price Distribution](image/a2f1_saleprice_stats.png)

The box plot highlights the spread between the minimum ($13,869) and maximum ($806,942), with Q3 ($224,204) sitting well above the median — indicating the upper half of the distribution is more spread out.

![Sale Price Box Plot](image/a2f2_saleprice-minmax.png)

The scatterplot of Sale Price vs. Overall Quality confirms a clear positive relationship — as overall quality increases, sale prices rise as well.

![Scatterplot: Sale Price vs Overall Quality](image/saleprice_scatterplot.png)

The violin plot groups sale prices by year sold (2006–2010). The median price remains relatively consistent across all five years, suggesting prices were stable in this period.

![Violin Plot: Sale Price by Year Sold](image/Saleprice_violin_plot.png)

---

### 3️⃣ Pearson's Correlation Analysis

Pearson's correlation was used to identify which variables are most strongly associated with sale price. Overall Quality (0.796) and Above Grade Living Area (0.705) emerged as the strongest predictors.

![Pearson Correlation Analysis](image/a3f1_pearson_correlation.png)

The correlation matrix heatmap visualizes pairwise correlations across all numerical variables. Red indicates positive correlation and blue indicates negative correlation.

![Correlation Matrix Heatmap](image/a3f2_scatterplot.png)

**Garage Cars vs. Garage Area (r = 0.89):** A strong positive correlation — as the number of cars a garage can accommodate increases, garage area increases proportionally.

![Garage Cars vs Garage Area](image/a3f3_area_car_correlation.png)

**Garage Cars vs. Sale Price (r = 0.65):** A moderate positive relationship — properties with larger garages tend to sell at higher prices.

![Garage Cars vs Sale Price](image/a3f4_car_price_correlation.png)

**Pool Area vs. Sale Price:** Pool area shows a very weak relationship with sale price, with most properties having no pool at all.

![Pool Area vs Sale Price](image/a3f5_price_pool_correlation.png)

---

### 4️⃣ Categorical Variable Frequency Counts

Gable is by far the most common roof style, followed by Hip. Rarer styles like Gambrel, Flat, Mansard, and Shed make up a very small share of the dataset.

![Roof Style Frequency](image/a4f1_roofstyle_histogram.png)

Composite Shingle (CompShg) dominates roof material. Membrane (Membran) has an extremely low frequency — notable because it is the highest-impact predictor in the regression model.

![Roof Material Frequency](image/a4f2_roofmalt_histo.png)

The vast majority of properties fall under "Normal" for Condition 1. Proximity to positive features (PosN) is rare, which aligns with its unexpected negative coefficient in the regression models.

![Condition 1 Frequency](image/a4f3_cond1_histo.png)

---

### 5️⃣ Linear Regression 1 — Full Model (All Variables Except Order & PID)

**Formula:**

![Regression 1 Formula](image/a5f1_formula1.png)

**Model Performance:**

![Regression 1 R-Squared](image/a5f2_rsquared1.png)

- **R² = 0.931** — 93.1% of sale price variance explained
- **Adjusted R² = 0.929**

**Top Predictors:**

![Regression 1 Variables](image/a5f3_variables1.png)

Roof material dominates the top predictors. Membrane roofing (Roof.MatlMembran) has the highest coefficient at +$730,000 — statistically significant (p < 2.2e-16), though it appears in very few properties.

---

### 6️⃣ Linear Regression 2 — Excluding Roof Material, Year Built, Year Remodeled, Year Sold

**Formula:**

![Regression 2 Formula](image/a6f1_formula2.png)

**Model Performance:**

![Regression 2 R-Squared](image/a6f2_rsquared2.png)

- **R² = 0.901** — 90.1% of sale price variance explained
- **Adjusted R² = 0.899**

**Top Predictors:**

![Regression 2 Variables](image/a6f3_variables2.png)

Without roof material in the model, neighborhood and garage quality emerge as the top drivers. Green Hill neighborhood adds ~$113,000 to predicted price; excellent garage quality adds ~$105,000, while excellent garage condition unexpectedly reduces price by ~$89,600.

---

### 7️⃣ Linear Regression 3 — Excluding Garage Condition & Roof Variables

**Formula:**

![Regression 3 Formula](image/a7f1_formula3.png)

**Model Performance:**

![Regression 3 R-Squared](image/a7f2_rsquared3.png)

- **R² = 0.897** — 89.7% of sale price variance explained
- **Adjusted R² = 0.895**

**Top Predictors:**

![Regression 3 Variables](image/a7f3_variables3.png)

Proximity to positive features (Condition.2PosN) shows a negative coefficient of ~−$100,000 — an unexpected finding that may reflect buyer behavior or data limitations around rare categories.

---

### 8️⃣ Alteryx Workflows

**EDA Workflow** — data ingestion, field type selection, summary statistics, EDA containers, and correlation analysis:

![EDA Workflow](image/a8f1_EDA_workflow.png)

**Regression Workflow** — three separate linear regression containers built from the same cleaned dataset:

![Regression Workflow](image/a8f2_regression_workflow.png)

---

## 📊 Key Findings

- The full model (R² = 0.931) is the strongest predictor of sale price
- Roof material type is the highest-impact variable, though membrane roofing is extremely rare in the dataset
- Overall Quality and Above Grade Living Area are the strongest continuous predictors
- Garage size and neighborhood are significant drivers once roof variables are removed
- Proximity to positive features showed a counterintuitive negative effect, likely reflecting buyer bias or sparse data
- Variables like MS Zoning and Neighborhood raise ethical considerations around location-based pricing biases

---

## Hypothesis

- **H₀:** There is no significant relationship between overall property quality and sale price
- **H₁:** There is a significant relationship between overall property quality and sale price

The scatterplot and Pearson correlation (r = 0.796) both strongly support rejecting the null hypothesis.

---

## Limitations
- Dataset may reflect availability or reporting bias — accuracy of seller/agent-reported data cannot be verified
- Sample size of 2,903 may not fully represent all housing areas in the dataset
- High-impact variables like membrane roofing have very low frequency, which may inflate their estimated coefficients
- Neighborhood and MS Zoning variables can encode existing socioeconomic biases

---

## References
Housing dataset used for DSC-501 coursework, Utica University. File: `03_Housingdata.csv`
