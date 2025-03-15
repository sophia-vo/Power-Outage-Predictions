# Power-Outage-Predictions
DSC 80 Final Project at UCSD
<br/>
### Introduction

Power outages can have significant economic and social impacts, affecting millions of people across the United States. Understanding the factors that contribute to outage duration is important for improving infrastructure resilience and emergency response.
<br/>

In this project, we analyze a dataset of major power outages in the U.S. to answer the question:
<br/>
**"Is the average outage duration for power outages caused by severe weather equal to the average outage duration for power outages caused by equipment failure?"**
<br/>

Severe weather events, such as hurricanes and storms, are becoming more frequent due to climate change, while equipment failures continue to pose a challenge for power grids. By investigating whether these two causes lead to significantly different outage durations, we can help inform solutions to increase power outage preparedness and help scientists predict the consequences of certain power outages in relation to others. 
<br/>

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


### Data Cleaning and Exploratory Data Analysis
**Data Cleaning Steps:**
<br/>

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


<br/>
**Univariate Analysis:**
<br/>
<iframe
  src="assets/univariate_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<br/>
This plot looks at the distribution of Outage Duration and shows that the distribution of durations is between 0 and 20 thousands. This means that most outages last under 20 thousand minutes. 

<br/>
**Bivariate Analysis:**
<br/>
<iframe
  src="assets/bivariate_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<br/>
This plot looks at the relationship between outage duration and month. The scatter plot shows that there is no significant relationship between the two variables. 

<br/>
**Grouped Table:**
<br/>
<iframe
  src="assets/GROUPED_TABLE.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
We computed the mean outage duration for each cause category across different climate regions. Then, we converted these values into percentages relative to the total outage duration. In most climate regions, severe weather contributes significantly to power outages. Fuel supply emergencies have a high impact in the East North Central (18.66%) and the South (9.60%), possibly due to extreme cold or supply chain issues. Equipment failure has a significant impact in the East North Central (14.52%), likely due to aging infrastructure or extreme weather-related wear and tear. The consistent percentage of outages due to severe weather suggests that power grids need better resilience against climate-related disruptions. Policymakers can use this data to prioritize investments in infrastructure upgrades, focusing on regions where certain outage causes are more prevalent.

### Assessment of Missingness
The column CUSTOMERS.AFFECTED in the dataset is likely Not Missing at Random (NMAR) because the likelihood of a value being missing may depend on the actual number of customers affected, which is an unobserved value. For example, if a power company intentionally omits reporting outages that impact a small number of customers or if large-scale outages are prioritized for reporting while smaller ones are left undocumented, the missingness would depend on the value itself.

To determine whether the missingness is  NMAR, additional data would be needed, such as internal reporting policies of utility companies, time thresholds for reporting outages, or metadata on how the data was collected. If the missingness can be explained by another observed column, such as CLIMATE.REGION (e.g., certain regions may have different infrastructure that affect whether customer impact data is recorded; remote or rural areas may have less consistent reporting compared to urban regions). If the missingness of CUSTOMERS.AFFECTED is strongly correlated with CLIMATE.REGION then the missingness could instead be Missing at Random (MAR) rather than NMAR.

To analyze missingness dependency, we will choose a column with non-trivial missingness and determine whether its missingness is dependent on other columns. We will investigate "CUSTOMERS.AFFECTED", which contains missing values. Our goal is to determine if its missingness is:

- MCAR (Missing Completely at Random): Missingness is independent of other columns and the missing values themselves.
- MAR (Missing at Random): Missingness depends on other columns but not on the missing values themselves.

To analyze the missingness of CUSTOMERS.AFFECTED, we will perform permutation tests to determine whether its missingness depends on other columns in the dataset. We hypothesize that the missingness of CUSTOMERS.AFFECTED may depend on CLIMATE.REGION, since different regions might have varying reporting standards or data collection processes. If missing values are more prevalent in certain regions, this would suggest a dependency. We also hypothesize that the missingness of CUSTOMERS.AFFECTED does not depend on RES.PRICE, as electricity prices are unlikely to directly affect whether CUSTOMERS.AFFECTED is reported.

**Dependency on CLIMATE.REGION:**

Null Hypothesis: The distribution of climate regions is the same for missing and non-missing values of "CUSTOMERS.AFFECTED".
Alternative Hypothesis: The missingness of "CUSTOMERS.AFFECTED" is dependent of climate region.
Test Statistic: Total Variation Distance (TVD) between the distributions of CLIMATE.REGION for missing vs. non-missing values.

The Total Variation Distance (TVD) between the distributions of CLIMATE.REGION for missing vs. non-missing CUSTOMERS.AFFECTED is 0.278, which indicates a significant difference between the two distributions.
P-value (0.0): Since the p-value is 0.0, we reject the null hypothesis.

<iframe
  src="assets/MAR_CLIMATE_REGION.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This means that the missingness of CUSTOMERS.AFFECTED depends on CLIMATE.REGION. The probability that CUSTOMERS.AFFECTED is missing is not random across different climate regions.
Certain regions (e.g., Northeast, West) have a much higher proportion of missing values compared to others. This suggests that regional factors like data reporting standards, infrastructure, or climate-related outages could be influencing whether CUSTOMERS.AFFECTED is recorded. Since the missingness is tied to an observed variable (CLIMATE.REGION), this is an example of Missing at Random (MAR) rather than Not Missing at Random (NMAR).

**Independence from RES.PRICE:**

Null Hypothesis: The missingness of CUSTOMERS.AFFECTED is independent of RES.PRICE.
Alternative Hypothesis: The missingness of CUSTOMERS.AFFECTED depends on RES.PRICE.
Test Statistic: Difference in means for missing vs. non-missing values (absolute difference in the average RES.PRICE between rows where CUSTOMERS.AFFECTED).

Observed Difference in Means: 0.102
P-value: 0.589

<iframe
  src="assets/MCAR_RES_PRICE.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

A p-value of 0.589 is much larger than the typical significance threshold of 0.05, so we fail to reject the null hypothesis. This means there is no strong evidence that the missingness of CUSTOMERS.AFFECTED is dependent on RES.PRICE. The missingness of CUSTOMERS.AFFECTED does not appear to be strongly related to the residential electricity price (RES.PRICE). This suggests that the missingness is likely Missing Completely At Random (MCAR) or Missing At Random (MAR) with respect to RES.PRICE. The probability of CUSTOMERS.AFFECTED being missing is not significantly influenced by the value of RES.PRICE.


### Hypothesis Testing
**Null Hypothesis:** There is no significant difference between the outage duration for outages caused by severe weather and outage duration for outages caused by equipment failure.
<br/>
**Alternative Hypothesis:** There is a significant difference between the outage duration for outages caused by severe weather and outage duration for outages caused by equipment failure.
<br/>
<br/>
**Test Statistic:** Difference in means
  - This is a good choice for this comparison because we are assessing the difference in duration time between two separate, categorical, variables. 
<br/>
<br/>
**Significance Level:** 0.05
  - This is an appropriate significant level as it avoids too many false positives while still asserting confidence
<br/>
<br/>
**p-value**: 0.015
<br/>
<br/>
**Conclusion:** With a significance level of alpha=0.05 and a p-value of 0.015, we reject the null hypothesis as the p-value is less than 0.05. Thus, we are 95% confident that there is a significant difference in outage duration between the cause of severe weather and equipment failure.
<br/>

**Graph:**
<iframe
  src="assets/Emperical_Distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Framing a Prediction Problem

**Prediction Problem:** Predicting the number of customers affected by an outage based on the cause category, the climate region, and the outage duration. 
<br/> &nbsp;  - This is a regression prediction problem as our output is numerical 
<br/> &nbsp;  - We will build a multiclass classification classifier for both categorical variables (cause category and climate region) 

**Response Variable:** The number of customers affected by the outage
<br/> &nbsp;  - We chose this variable because it can help inform companies when determining what their losses would be due to a power outage. The number of customers affected by the outage influences their business because it affects the number of customers who interact with companies. 

**Metric:** RMSE
<br/> &nbsp;  - This metric was chosen because we are building a regression model. F1 score is better suited for classification models. 
<br/> &nbsp;  - R² was not chosen because we are not modeling a linear relationship

**Information needed at the time of prediction**
<br/> &nbsp;  - Month
<br/> &nbsp;  - Outage Duration
<br/> &nbsp;  - Climate Region
<br/> &nbsp;  - Cause Category

These are all necessary at the time of prediction becasue they influence how many customers will can attend a business or company. The climate region is affected due to differences in weather and infrastructure. Additionally, the cause of the outage can influence any repair time neccessary. Further, the outage duration limits how many customers can attend to a business because the longer the outage lasts the less customers that business can serve. Finally, the month affects the number of customers affected because time of year can affect how many people are visiting businesses. 


### Baseline Model
Our baseline model predicts CUSTOMERS.AFFECTED using CAUSE.CATEGORY, CLIMATE.REGION, and OUTAGE.DURATION. Missing values are handled using mean imputation for numerical features with categorical features being filled with the mode. 

**Features:**
<br/> &nbsp;  - CAUSE.CATEGORY: Categorical
<br/> &nbsp;  - CLIMATE.REGION: Categorical
<br/> &nbsp;  - OUTAGE.DURATION: Numerical
<br/>
<br/> &nbsp;  - The categorical variables are encoded using a OneHotEncoder

**Performance Evaluation:** 
<br/> &nbsp;  - The RMSE of our baseline model was 443865.23 and the RMSE of our final model was 318011.48
<br/> &nbsp;  - Thus, this demonstrates that our final model is making more accurate predictions than our baseline model because the RMSE significantly decreased. 

### Final Model
The features we added were Climate Region and Cause Category because both of these factors influence the number of customers affected. Climate Region will affect customers differently due to different weather elements and infrastructure in different parts of the United States. Cause category can also influence customers because it can influence damages caused to the businesses and also affects customers ability to get to the businesses. Thus, both of these will affect the accuracy of the model because they are additional factors that influence customer behavior. 

The hyperparameters we chose were n.estimators, max_depth, min_samples_leaf. We chose n_estimators because more trees can capture more patterns but slow down training. We chose max_depth because it prevents overly complex trees that overfit. We chose min_samples_leaf because it helps regularize by preventing small, unstable splits.

The best performing hyperparameters were model__max_depth: 5, model__min_samples_leaf: 5,'model__n_estimators': 100. 

The modeling algorithm chosen was a RandomForestClassifier which uses a multitude of decision trees to classify the data. 

The final model made more accurate predictions than the baseline model. The RMSE of our baseline model was 443865.23 and the RMSE of our final model was 318011.48. Thus, the final model RMSE was significantly lower than the baseline model RMSE. 

**Visualization:**


### Fairness Analysis

**Group X:** Northern Climate Region
<br/>

**Group Y:** Southern Climate Region
<br/>

**Evaluation Metric:** RMSE
<br/>

**Null Hypothesis:** Our model is fair, the RMSE is the same for the North and South regions. Its precision for the number of customers affected by power outages in the northern climate region and the number of customers affected by power outages in the southern climate region are roughly the same, and any differences are due to random chance.
<br/>

**Alternative Hypothesis:** Our model is unfair, the RMSE is different for the North and South regions. Its precision for the number of customers affected by power outages in the northern climate region is different than its precision for the number of customers affected by power outages in the southern climate region.
<br/>

**Test Statistic:** Absolute Difference in RMSE
<br/>

**Significance Level:** 0.05
<br/>

**p_value:** 0.25
<br/>

**Conclusion:** With a significance level of alpha=0.05 and a p-value of 0.25, we fail to reject the null hypothesis as the p-value is greater than 0.05. Thus, we are 95% confident that there is no significant difference in the RMSE for the North and South Regions. There is evidence to state that our model is fair. 
<br/>

**Visualization:** 
<iframe
  src="assets/FAIRNESS_GRAPH.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

