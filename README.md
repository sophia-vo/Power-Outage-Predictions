# Power Outage Predictions
DSC 80 Final Project at UCSD

# Introduction

Power outages can have significant economic and social impacts, affecting millions of people across the United States. Understanding the factors that contribute to outage duration is important for improving infrastructure resilience and emergency response.
<br/>

In this project, we analyze a dataset of major power outages in the U.S. to answer the question:

**"Is the average outage duration for power outages caused by severe weather equal to the average outage duration for power outages caused by equipment failure?"**

Severe weather events, such as hurricanes and storms, are becoming more frequent due to climate change, while equipment failures continue to pose a challenge for power grids. By investigating whether these two causes lead to significantly different outage durations, we can help inform solutions to increase power outage preparedness and help scientists predict the consequences of certain power outages in relation to others. 

**Dataset:** Power Outages

This dataset contains 1,534 rows and provides major power outage data in the 48 contiguous states in the US from January 2000 to July 2016. It holds information for power outages across the US that can be used to identify patterns and make predictions for the potential consequences caused by future outages. The key columns relevant to our analysis are:

**Column Information:**

| Column Name             | Description                                                                    |
|:------------------------|:-------------------------------------------------------------------------------|
| OBS                     | Observation number for tracking individual records.                            |
| YEAR                    | The year in which the outage occurred.                                         |
| MONTH                   | The month in which the outage occurred (1-12).                                 |
| U.S._STATE              | The U.S. state where the outage occurred.                                      |
| CLIMATE.REGION          | The climate region where the outage took place (e.g., Northeast, South, etc.). |
| OUTAGE.START.DATE       | The date when the outage started.                                              |
| OUTAGE.START.TIME       | The time when the outage started.                                              |
| OUTAGE.RESTORATION.DATE | The date when power was restored.                                              |
| OUTAGE.RESTORATION.TIME | The time when power was restored.                                              |
| CAUSE.CATEGORY          | The general cause of the outage (e.g., severe weather, equipment failure).     |
| CAUSE.CATEGORY.DETAIL   | A more detailed description of the specific cause of the outage.               |
| OUTAGE.DURATION         | The total duration of the outage in hours.                                     |
| CUSTOMERS.AFFECTED      | The number of customers impacted by the outage.                                |
| RES.PRICE               | The residential electricity price in cents per kilowatt-hour.                  |
| TOTAL.CUSTOMERS         | The total number of customers served in the affected area.                     |



# Data Cleaning and Exploratory Data Analysis
### Data Cleaning Steps:
1. Reading the Data: Loaded the dataset into the notebook using only the columns specified in "Column Information" so that only the columns necessary to our analysis were included. This reduces unnecessary data and ensures efficient processing. We skipped the first five rows to exclude any metadata or headers that might be present in the original file.

2. Indexing and Dropping Unnecessary Columns: The column `'OBS'` was used to create an index, converting it into integers for proper indexing. The `'OBS'` column was then dropped since it is no longer needed for analysis.

3. Handling Date and Time Columns: Converted the original `'OUTAGE.START.DATE'` and `'OUTAGE.START.TIME'` columns to datetime form and combined them into one pd.Timestamp column (dropping both of the originals). This allowed for ease of analysis as now only one column had to be used for analysis instead of two. `'OUTAGE.START.TIME'` and `'OUTAGE.RESTORATION.TIME'` were converted to timedelta format to further ease analysis. A new 'OUTAGE.START' column was created by combining `'OUTAGE.START.DATE'` and `'OUTAGE.START.TIME'`. Similarly, `'OUTAGE.RESTORATION'` was created by merging `'OUTAGE.RESTORATION.DATE'` and `'OUTAGE.RESTORATION.TIME'`. These new columns provide precise timestamps of outage events. The original date and time columns were dropped, reducing redundancy and improving data clarity.

The first few rows of the cleaned dataframe are below:

|   OBS |   YEAR |   MONTH | U.S._STATE   | CLIMATE.REGION     | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   OUTAGE.DURATION |   CUSTOMERS.AFFECTED |   RES.PRICE |   TOTAL.CUSTOMERS | OUTAGE.START        | OUTAGE.RESTORATION   |
|------:|-------:|--------:|:-------------|:-------------------|:-------------------|:------------------------|------------------:|---------------------:|------------:|------------------:|:--------------------|:---------------------|
|     1 |   2011 |       7 | Minnesota    | East North Central | severe weather     | nan                     |              3060 |                70000 |       11.6  |       2.5957e+06  | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |
|     2 |   2014 |       5 | Minnesota    | East North Central | intentional attack | vandalism               |                 1 |                  nan |       12.12 |       2.64074e+06 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |
|     3 |   2010 |      10 | Minnesota    | East North Central | severe weather     | heavy wind              |              3000 |                70000 |       10.87 |       2.5869e+06  | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |
|     4 |   2012 |       6 | Minnesota    | East North Central | severe weather     | thunderstorm            |              2550 |                68200 |       11.79 |       2.60681e+06 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |
|     5 |   2015 |       7 | Minnesota    | East North Central | severe weather     | nan                     |              1740 |               250000 |       13.07 |       2.67353e+06 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |

### Univariate Analysis:

Understanding the distribution of key variables is important for identifying trends, potential biases, and areas for further investigation. Below, we explore the distributions of **Customers Affected** and **Outage Duration**.

**Distribution of Customers Affected**

The histogram below shows the distribution of the number of customers affected by outages. 
<iframe
  src="assets/univariate1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The data is highly skewed, with most outages affecting a relatively small number of customers, while a few extreme cases impact millions. This right-skewed distribution suggests that a log transformation may be useful in modeling.

To better visualize the majority of the data, we limit the x-axis range to 500,000 customers affected in the scaled version below:
<iframe
  src="assets/univariate2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Distribution of Outage Duration**

The histogram below represents the duration of power outages (in minutes). Similar to customers affected, outage durations are highly skewed, with most outages lasting for a short period while a few last significantly longer. This distribution suggests that extreme events, while rare, can have a significant impact.
<iframe
  src="assets/univariate3.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
To enhance visibility, we focus on outages lasting less than 10,000 minutes in the adjusted histogram:
<iframe
  src="assets/univariate4.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis:

To better understand the relationships between key variables, we explore how **Outage Duration** varies across different causes and months. These insights will help identify trends and guide hypothesis testing.

**Outage Duration vs. Cause Category:**

The box plot below displays the distribution of **Outage Duration** across different **Cause Categories**. 

We observe that **Fuel Supply Emergencies** have the longest median outage duration and the widest interquartile range (IQR), spanning approximately **0 to 20,000 minutes**, and **Severe Weather** has many high outliers, indicating that some storms cause extremely prolonged outages. Most cause categories have outage durations concentrated within **0-5,000 minutes**, suggesting that outages are typically short regardless of cause.

<iframe
  src="assets/bivariate1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Outage Duration vs. Month:**

The scatter plot below examines how Outage Duration changes by month. 
<iframe
  src="assets/bivariate2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
For improved visibility a scaled graph with the median outage durations plotted is shown below:
<iframe
  src="assets/bivariate3.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The red line represents the median outage duration per month, showing fluctuations throughout the year.

We observe that outage durations tend to peak around September and December, where the median reaches approximately 1,400 minutes. For most months, the median outage duration remains below 1,000 minutes, indicating a seasonal effect in outages.

### Interesting Aggregates

To better understand how outage durations vary across different *Cause Categories* and *Climate Regions*, we created a pivot table displaying the percentage of total outage duration each combination contributes. The table below shows the percentage of total outage duration attributed to each Cause Category across different Climate Regions. This helps identify regional vulnerabilities to specific causes.

| Central   | East North Central   | Northeast   | Northwest   | South   | Southeast   | Southwest   | West   | West North Central   |
|:----------|:---------------------|:------------|:------------|:--------|:------------|:------------|:-------|:---------------------|
| 0.18%     | 14.52%               | 0.12%       | 0.39%       | 0.16%   | 0.30%       | 0.06%       | 0.29%  | 0.03%                |
| 5.51%     | 18.66%               | 8.04%       | 0.00%       | 9.60%   | 0.00%       | 0.04%       | 3.38%  | 0.00%                |
| 0.19%     | 1.31%                | 0.11%       | 0.21%       | 0.18%   | 0.28%       | 0.15%       | 0.47%  | 0.01%                |
| 0.07%     | 0.00%                | 0.48%       | 0.04%       | 0.27%   | 0.00%       | 0.00%       | 0.12%  | 0.04%                |
| 0.77%     | 0.40%                | 1.46%       | 0.49%       | 0.64%   | 1.57%       | 1.25%       | 1.11%  | 0.24%                |
| 1.79%     | 2.44%                | 2.43%       | 2.66%       | 2.41%   | 1.46%       | 6.36%       | 1.61%  | 1.34%                |
| 1.48%     | 1.43%                | 0.42%       | 0.08%       | 0.48%   | 0.09%       | 0.18%       | 0.20%  | 0.00%                |

We computed the mean outage duration for each cause category across different climate regions. Then, we converted these values into percentages relative to the total outage duration. In most climate regions, severe weather contributes significantly to power outages. 

Fuel supply emergencies have a high impact in the East North Central (18.66%) and the South (9.60%), possibly due to extreme cold or supply chain issues. Equipment failure has a significant impact in the East North Central (14.52%), likely due to aging infrastructure or extreme weather-related wear and tear. The consistent percentage of outages due to severe weather suggests that power grids need better resilience against climate-related disruptions. Policymakers can use this data to prioritize investments in infrastructure upgrades, focusing on regions where certain outage causes are more prevalent.



# Assessment of Missingness

The column `CUSTOMERS.AFFECTED` in the dataset is likely Not Missing at Random (NMAR) because the likelihood of a value being missing may depend on the actual number of customers affected, which is an unobserved value. For example, if a power company intentionally omits reporting outages that impact a small number of customers or if large-scale outages are prioritized for reporting while smaller ones are left undocumented, the missingness would depend on the value itself.

To determine whether the missingness is  NMAR, additional data would be needed, such as internal reporting policies of utility companies, time thresholds for reporting outages, or metadata on how the data was collected. If the missingness can be explained by another observed column, such as `CLIMATE.REGION` (e.g., certain regions may have different infrastructure that affect whether customer impact data is recorded; remote or rural areas may have less consistent reporting compared to urban regions). If the missingness of `CUSTOMERS.AFFECTED` is strongly correlated with `CLIMATE.REGION` then the missingness could instead be Missing at Random (MAR) rather than NMAR.

To analyze missingness dependency, we will choose a column with non-trivial missingness and determine whether its missingness is dependent on other columns. We will investigate `CUSTOMERS.AFFECTED`, which contains missing values. Our goal is to determine if its missingness is:

- MCAR (Missing Completely at Random): Missingness is independent of other columns and the missing values themselves.
- MAR (Missing at Random): Missingness depends on other columns but not on the missing values themselves.

To analyze the missingness of `CUSTOMERS.AFFECTED`, we will perform permutation tests to determine whether its missingness depends on other columns in the dataset. We hypothesize that the missingness of `CUSTOMERS.AFFECTED` may depend on `CLIMATE.REGION`, since different regions might have varying reporting standards or data collection processes. If missing values are more prevalent in certain regions, this would suggest a dependency. We also hypothesize that the missingness of `CUSTOMERS.AFFECTED` does not depend on `RES.PRICE`, as electricity prices are unlikely to directly affect whether `CUSTOMERS.AFFECTED` is reported.

**Dependency on `CLIMATE.REGION`:**

| CLIMATE.REGION     |      False |      True |
|:-------------------|-----------:|----------:|
| Alaska             | 0.00091659 | 0         |
| Central            | 0.143905   | 0.0970655 |
| East North Central | 0.108158   | 0.0451467 |
| Hawaii             | 0.00458295 | 0         |
| Northeast          | 0.243813   | 0.189616  |
| Northwest          | 0.0568286  | 0.158014  |
| South              | 0.142071   | 0.167043  |
| Southeast          | 0.131072   | 0.0225734 |
| Southwest          | 0.0412466  | 0.106095  |
| West               | 0.12099    | 0.191874  |
| West North Central | 0.00641613 | 0.0225734 |

*Null Hypothesis*: The distribution of climate regions is the same for missing and non-missing values of `CUSTOMERS.AFFECTED`.

*Alternative Hypothesis*: The missingness of `CUSTOMERS.AFFECTED` is dependent of climate region.

*Test Statistic*: Total Variation Distance (TVD) between the distributions of CLIMATE.REGION for missing vs. non-missing values.

The Total Variation Distance (TVD) between the distributions of `CLIMATE.REGION` for missing vs. non-missing `CUSTOMERS.AFFECTED` is 0.278, which indicates a significant difference between the two distributions.

**Total variation distance**: 0.28
**p-value (0.0)**: Since the p-value is 0.0, we reject the null hypothesis.

<iframe
  src="assets/MAR_CLIMATE_REGION.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This means that the missingness of `CUSTOMERS.AFFECTED` depends on `CLIMATE.REGION`. The probability that `CUSTOMERS.AFFECTED` is missing is not random across different climate regions.

Certain regions (e.g., Northeast, West) have a much higher proportion of missing values compared to others. This suggests that regional factors like data reporting standards, infrastructure, or climate-related outages could be influencing whether `CUSTOMERS.AFFECTED` is recorded. Since the missingness is tied to an observed variable (`CLIMATE.REGION`), this is an example of Missing at Random (MAR) rather than Not Missing at Random (NMAR).

**Independence from `RES.PRICE`:**

*Null Hypothesis*: The missingness of `CUSTOMERS.AFFECTED` is independent of `RES.PRICE`.

*Alternative Hypothesis*: The missingness of `CUSTOMERS.AFFECTED` depends on `RES.PRICE`.

*Test Statistic*: Difference in means for missing vs. non-missing values (absolute difference in the average `RES.PRICE` between rows where `CUSTOMERS.AFFECTED`).

**Observed Difference in Means**: 0.102

**P-value**: 0.564

<iframe
  src="assets/MCAR_RES_PRICE.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

A p-value of 0.564 is much larger than the typical significance threshold of 0.05, so we fail to reject the null hypothesis. This means there is no strong evidence that the missingness of `CUSTOMERS.AFFECTED` is dependent on `RES.PRICE`. The missingness of `CUSTOMERS.AFFECTED` does not appear to be strongly related to the residential electricity price (`RES.PRICE`). This suggests that the missingness is likely Missing Completely At Random (MCAR). The probability of `CUSTOMERS.AFFECTED` being missing is not significantly influenced by the value of `RES.PRICE`.

# Hypothesis Testing

**Outage Duration and Cause Category:**  

**Hypotheses:**  

- **Null Hypothesis (H0):** There is no difference in the mean outage duration between outages caused by severe weather and equipment failure. Any observed difference is due to random chance.  

- **Alternative Hypothesis (H1):** The mean outage duration differs between outages caused by severe weather and equipment failure.  

**Observed Difference in Means**  
To test this hypothesis, we first filter the dataset to include only outages caused by **severe weather** and **equipment failure**. We then compute the observed difference in mean outage duration.

**Test Statistic:** Difference in Means
This is a good choice for this comparison because we are assessing the difference in duration time between two separate, categorical, variables.
    
**Significance Level:** 0.05
This is an appropriate significant level as it avoids too many false positives while still asserting confidence

**Permutation Test:**

To determine if this observed difference is statistically significant, we perform a permutation test:

1. Shuffle the `OUTAGE.DURATION` values while keeping the `CAUSE.CATEGORY` labels unchanged.
2. Compute the difference in mean outage durations for each shuffled dataset.
3. Repeat this process 1000 times to generate a null distribution.
4. Calculate the p-value, which represents the proportion of shuffled differences that are as extreme as or more extreme than the observed difference.

*Observed Difference in Means*: 2067.08

*P-value*: 0.0150

*Reject the null hypothesis*: There is evidence of a difference in outage duration between severe weather and equipment failure.

**Conclusion:** With a significance level of alpha=0.05 and a p-value of 0.015, we reject the null hypothesis as the p-value is less than 0.05. Thus, we are 95% confident that there is a significant difference in outage duration between the cause of severe weather and equipment failure.

**Graph:**
<iframe
  src="assets/HT.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>



# Framing a Prediction Problem

**Prediction Problem:** Estimating the Number of Customers Affected by an Outage

**Problem Type:**

This is a regression problem since `CUSTOMERS.AFFECTED` is a continuous numerical value. We aim to predict the number of customers impacted by a power outage based on available features at the time of prediction.

**Response Variable:** `CUSTOMERS.AFFECTED`, the number of customers affected by the outage

We chose this variable because it can help inform companies when determining what their losses would be due to a power outage. The number of customers affected by the outage influences their business because it affects the number of customers who interact with companies. Understanding how many customers are affected by an outage is important for prioritizing emergency responses, resource allocation, and infrastructure improvements

**Predictor Variables:**  

- `OUTAGE.DURATION`: The length of the outage in minutes.  
- `MONTH`: The month when the outage occurred (encoded as 1–12).  

These features are known at the time of prediction as outage duration is recorded as soon as the event ends, and the month is always available. Since we only use features that are available at the time of prediction, this ensures our model can make realistic predictions in real-time scenarios.



# Baseline Model

To establish a starting point for our prediction task, we will train a baseline model using two features: `OUTAGE.DURATION` (numerical) and `MONTH` (numerical). 

### Feature Selection & Preprocessing
**Numerical Features** (`OUTAGE.DURATION`, `MONTH`):  
  - Missing values were imputed using the mean strategy.  
  - Standardization was applied using `StandardScaler()` to normalize numerical values.  

The preprocessing steps were implemented using a `Pipeline` and `ColumnTransformer`.

### Model Choice
We used a Random Forest Classifier with 100 estimators as our baseline model.

### Model Evaluation
The model was trained on the processed dataset and evaluated using **Root Mean Squared Error (RMSE)** on the test set:

\[\text{RMSE} = \sqrt{\frac{1}{n} \sum (y_{\text{true}} - y_{\text{pred}})^2}\]

This metric was chosen to measure the model's error in predicting the number of affected customers, with lower values indicating better performance. RMSE is relevant for building a regression model. F1 score is better suited for classification models. \(R^2\) was not chosen because we are not modeling a linear relationship

**Test RMSE**: 402480.16

### Evaluating Model Generalization to Unseen Data

To ensure our model generalizes well to new, unseen data, we split our dataset into training and test sets, using a train-test-split, to simulate real-world scenarios where the model encounters new data. The model is trained on `features_train` and evaluated on `features_test`- meaning that the model was trained on one set of data and evaluated on another. 

**Is This Model "Good"?**  

No, this model is not good because:

1. **High RMSE:** An RMSE of *402,480.16* suggests that our model has significant prediction errors, likely due to limited features and lack of complexity.  
2. **Limited Features:** Only two numerical features (`OUTAGE.DURATION` and `MONTH`) were used, which may not sufficiently capture the complexity of outage impact.  
3. **No Categorical Encoding:** The model does not yet incorporate key categorical variables (e.g., `CAUSE.CATEGORY`, `CLIMATE.REGION`), which could significantly improve performance.  
4. **Random Forest for Baseline:** While Random Forest is a robust model, it is being used here as a baseline. More sophisticated feature engineering and model tuning are used.  

**Next Steps**  
- Perform feature engineering, such as transforming `OUTAGE.DURATION` using a log transformation if needed.  
- Train different regression models and compare performance using RMSE.  
- Tune hyperparameters to optimize model performance.



# Final Model

The features we added were Climate Region and Cause Category because both of these factors influence the number of customers affected. 

Climate Region will affect customers differently due to different weather elements and infrastructure in different parts of the United States. 

Cause category can also influence customers because it can influence damages caused to the businesses and also affects customers ability to get to the businesses. 

Thus, both of these will affect the accuracy of the model because they are additional factors that influence customer behavior. 

**Information needed at the time of prediction:**
<br/> &nbsp;  - Month
<br/> &nbsp;  - Outage Duration
<br/> &nbsp;  - Climate Region
<br/> &nbsp;  - Cause Category

These are all necessary at the time of prediction becasue they influence how many customers will can attend a business or company. The climate region is affected due to differences in weather and infrastructure. Additionally, the cause of the outage can influence any repair time neccessary. 

Further, the outage duration limits how many customers can attend to a business because the longer the outage lasts the less customers that business can serve. Finally, the month affects the number of customers affected because time of year can affect how many people are visiting businesses. 


### Feature Engineering

**Log Transformation on** `OUTAGE.DURATION`**:** Since outage durations vary widely, applying log1p transformation helps normalize the distribution.

**Standard Scaling on**`MONTH`**:** Keeping `MONTH` as a numerical feature but standardizing it to ensure consistency.

**One-Hot Encoding for Categorical Features:** Encoding `CAUSE.CATEGORY` and `CLIMATE.REGION`

**Imputation:**
- `OUTAGE.DURATION`: Mean imputation.
- `CAUSE.CATEGORY & CLIMATE.REGION`: Most frequent value (mode) imputation.

The hyperparameters we chose were `n.estimators`, `max_depth`, `min_samples_leaf`. We chose `n_estimators` because more trees can capture more patterns but slow down training. We chose `max_depth` because it prevents overly complex trees that overfit. We chose `min_samples_leaf` because it helps regularize by preventing small, unstable splits.

The best performing hyperparameters were `model__max_depth`: 5, `model__min_samples_leaf`: 5,`model__n_estimators`: 100. 

The modeling algorithm chosen was a `RandomForestClassifier` which uses a multitude of decision trees to classify the data. 

**Root Mean Squared Error (RMSE)**: 318011.48

The final model made more accurate predictions than the baseline model. The RMSE of our baseline model was 402480.16 and the RMSE of our final model was 318011.48. Thus, the final model RMSE was significantly lower than the baseline model RMSE. Thus, this demonstrates that our final model is making more accurate predictions than our baseline model because the RMSE significantly decreased. 

This model is generalizable because it improved on the baseline model, which was already generalizable to begin with due to the train-test split, but additionally, this model significantly lowered the RMSE furthering the generalizability because it measures how far the predictions are from the actual values. Thus, a lower RMSE displays more accuracy making the model more generalizable. 

Additionally, this model is generalizable due to the hyperparameter tuning with cross-validation because it tested the model on different subsets. Finally, once the best hyperparameters are found, the final model is retrained on the entire training set. Hence, the final model is generalizable. 



# Fairness Analysis

To determine whether the model performs worse for one group compared to the other, we compare its predictive performance across different climate regions, specifically the Root Mean Squared Error (RMSE) between two groups:

**Group X:** Northern Climate Region

**Group Y:** Southern Climate Region

*North*: Regions classified as East North Central, Northeast, Northwest.

*South*: Regions classified as South, Southeast, Southwest.

### Hypothesis
We use a permutation test to statistically evaluate whether the difference in RMSE between the two groups is due to chance.

**Null Hypothesis (H0):** Our model is fair, the RMSE is the same for the North and South regions. Its precision for the number of customers affected by power outages in the northern climate region and the number of customers affected by power outages in the southern climate region are roughly the same, and any differences are due to random chance.

**Alternative Hypothesis (H1):** Our model is unfair, the RMSE is different for the North and South regions. Its precision for the number of customers affected by power outages in the northern climate region is different than its precision for the number of customers affected by power outages in the southern climate region.

**Evaluation Metric:** RMSE

**Test Statistic:** Absolute Difference in RMSE

**Significance Level:** 0.05

### Computing the Observed Difference in RMSE
We first compute the actual RMSE difference between the two groups using the final trained model:

1. Compute predictions on the test set.
2. Compute separate RMSE values for the North and South regions.
3. Calculate the absolute difference in RMSE.

The observed RMSE difference is the absolute value of South RMSE - North RMSE.

### Permutation Test

1. Shuffle the region labels (North / South) randomly.
2. Recompute RMSE for the shuffled groups.
3. Repeat this process 1,000 times to generate a distribution of RMSE differences under the null hypothesis.
4. Compute the p-value: The proportion of shuffled RMSE differences that are greater than or equal to the observed RMSE difference.

**p-value:** 0.246

**Conclusion:** With a significance level of alpha=0.05 and a p-value of 0.246, we fail to reject the null hypothesis as the p-value is greater than 0.05. Thus, we are 95% confident that there is no significant difference in the RMSE for the North and South Regions, implying that any observed differences are likely due to random chance, and the model does not show significant bias. There is evidence to state that our model is fair. 
<br/>

**Visualization:** 
<iframe
  src="assets/FAIRENESS_GRAPH.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

