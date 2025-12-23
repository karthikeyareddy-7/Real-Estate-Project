
# NYC Manhattan Real Estate Sales Analysis and Prediction

## Overview

This project analyzes the Manhattan real estate sales dataset to understand property price trends, explore relationships between property features, and build a predictive model for sale prices. The analysis includes **data preprocessing, exploratory data analysis (EDA), feature selection, Random Forest modeling, and statistical testing**.

The ultimate goal is to build a predictive model for property sale prices and identify which factors significantly affect property values in Manhattan.

---

## Project Workflow

The project can be broken down into **five main steps**:

### 1. **Data Collection and Loading**

* **Purpose:** Load raw Manhattan real estate sales data into R for analysis.

* **Implementation:**

  ```r
  library(readxl)
  file_path <- "C:/Users/91897/OneDrive/Desktop/rollingsales_manhattan.xls"
  real_estate_data <- read_excel(file_path)
  ```

* **Details:**

  * The dataset contains property sales information including sale price, building class, borough, land area, square feet, year built, and units.
  * Column names are cleaned and standardized for easier handling using `gsub()`.

* **Columns Selected for Analysis:**

  * `Borough`, `Building_Class_Category`, `Residential_Units`, `Commercial_Units`, `Land_Square_Feet`, `Gross_Square_Feet`, `Year_Built`, `Sale_Price`, `Sale_Date`.

---

### 2. **Data Preprocessing**

* **Purpose:** Prepare data for analysis and modeling.

* **Steps:**

  1. Remove unnecessary columns such as `Block`, `Lot`, `Address`, and others.
  2. Convert numeric columns stored as strings (with `$` or `,`) to numeric format.
  3. Handle missing values:

     * Replace missing numeric values with **median**.
     * Replace invalid `Year_Built` values (<1900 or >2013) with 2013.
  4. Remove extreme outliers using the **Interquartile Range (IQR)** method.
  5. Ensure no missing values remain in critical numeric columns.

* **Key Functions Used:**

  ```r
  remove_outliers <- function(df, col_name){ ... }
  real_estate_data_clean <- real_estate_data %>% remove_outliers(...)
  ```

---

### 3. **Exploratory Data Analysis (EDA)**

* **Purpose:** Visualize relationships between variables and detect patterns in the data.
* **Key Visualizations:**

  1. **Scatter plot:** Sale Price vs Gross Square Feet

     ```r
     ggplot(real_estate_data_clean, aes(x=Gross_Square_Feet, y=Sale_Price)) + geom_point()
     ```

     * Shows the correlation between property size and sale price.
  2. **Correlation plot:** Between numerical features like `Sale_Price`, `Gross_Square_Feet`, `Land_Square_Feet`, `Year_Built`.

     ```r
     corrplot(cor_matrix, method="circle")
     ```

     * Helps identify which numeric features are highly correlated with sale price.
  3. **Boxplot:** Sale Price by Borough

     ```r
     ggplot(real_estate_data_clean, aes(x=Borough, y=Sale_Price)) + geom_boxplot()
     ```

     * Shows how property prices vary across Manhattan boroughs.

---

### 4. **Feature Selection and Model Building**

* **Purpose:** Build a predictive model for property sale price using relevant features.

* **Steps:**

  1. Shuffle the dataset and split into **training (80%)** and **testing (20%)** sets.
  2. Train a **Random Forest model** to predict `Sale_Price`.

     ```r
     rf_model <- randomForest(Sale_Price ~ Borough + Building_Class_Category + Residential_Units + ..., data=train_data, ntree=100, mtry=3)
     ```
  3. Evaluate model performance:

     * **R-squared:** Measures how well the model explains variance in Sale Price.
     * **RMSE (Root Mean Squared Error):** Measures average prediction error.
     * **MAE (Mean Absolute Error):** Measures average absolute error.

* **Feature Importance:**

  * The Random Forest model identifies the most influential factors affecting sale price.

  ```r
  varImpPlot(rf_model)
  ```

---

### 5. **Statistical Testing**

* **Purpose:** Check the significance of categorical features (like Borough) on sale price.
* **Methods:**

  1. **Linear regression:** Fit a linear model and check p-values.

     ```r
     lm_model <- lm(Sale_Price ~ Borough + Building_Class_Category + ..., data=train_data)
     summary(lm_model)
     ```
  2. **ANOVA test:** Determine if sale prices differ significantly by Borough.

     ```r
     anova_result <- aov(Sale_Price ~ Borough, data=real_estate_data_clean)
     summary(anova_result)
     ```

---

## Key Takeaways

* **Data Cleaning:** Proper handling of missing values and outliers is critical for accurate modeling.
* **EDA Insights:**

  * Sale price is strongly correlated with **Gross Square Feet**, **Land Square Feet**, and **Number of Units**.
  * Sale price varies significantly across boroughs.
* **Random Forest Model:** Provides robust predictions for property sale prices and identifies the most important features.
* **Statistical Testing:** Confirms that **borough and building class category** significantly impact property prices.

---

## Libraries Used

* `readxl`: Read Excel files.
* `dplyr`: Data manipulation.
* `caret`: Data splitting and modeling support.
* `randomForest`: Random Forest regression.
* `ggplot2`: Data visualization.
* `corrplot`: Visualize correlations.
* `Metrics`: Model evaluation metrics.
* `lmtest`: Statistical testing.

---

## How to Run

1. Install required packages:

   ```r
   install.packages(c("readxl", "dplyr", "caret", "randomForest", "ggplot2", "corrplot", "Metrics", "lmtest"))
   ```
2. Update the `file_path` variable to the location of the dataset.
3. Run the R script step by step or source it in RStudio.
4. Check the plots, model summary, and evaluation metrics.

---

## Project Outcome

This project demonstrates a **complete workflow for real estate price prediction**, from data cleaning to model evaluation. It can be extended to other NYC boroughs or combined with temporal data for **time-series forecasting of property prices**.


