# Power-Outage-Predictions
DSC 80 Final Project at UCSD
<br/>
### Introduction

Power outages can have significant economic and social impacts, affecting millions of people across the United States. Understanding the factors that contribute to outage duration is important for improving infrastructure resilience and emergency response.
<br/>

In this project, we analyze a dataset of major power outages in the U.S. to answer the question:
<br/>
"Is the average outage duration for power outages caused by severe weather equal to the average outage duration for power outages caused by equipment failure?"
<br/>

This question is important because severe weather events, such as hurricanes and storms, are becoming more frequent due to climate change, while equipment failures continue to pose a challenge for power grids. By investigating whether these two causes lead to significantly different outage durations, we can help inform solutions to increase power outage preparedness and help scientists predict the consequences of certain power outages in relation to others. 
<br/>

**Dataset:** Power Outages
This dataset contains 1,534 rows and provides major power outage data in the 48 contiguous states in the US from January 2000 to July 2016. It holds information for power outages across the US that can be used to identify patterns and make predictions for the potential consequences caused by future outages. The key columns relevant to our analysis are:

**Column Information:**
<br/>
_OBS:_ The number of the power outage
<br/>
_YEAR:_ Indicates the year when the outage event occurred
<br/>
_MONTH:_ Indicates the month when the outage event occurred
<br/>
_U.S._STATE:_ Represents all the states in the continental U.S.
<br/>
_CLIMATE.REGION:_ U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.)
<br/>
_CAUSE.CATEGORY:_ Categories of all the events causing the major power outages
<br/>
_CAUSE.CATEGORY.DETAIL:_ Detailed description of the event categories causing the major power outages
<br/>
_OUTAGE.DURATION:_ Duration of outage events (in minutes)
<br/>
_CUSTOMERS.AFFECTED:_ Number of customers affected by the power outage event
<br/>
_OUTAGE.START:_ This variable indicates the day of the year and the time of the day when the outage event started (as reported by the corresponding Utility in the region)
<br/>
_OUTAGE.RESTORATION:_ This variable indicates the day of the year and the time of the day when power was restored to all the customers (as reported by the corresponding Utility in the region)


### Data Cleaning and Exploratory Data Analysis
**Data Cleaning Steps:**
<br/>
1. Loaded the dataset into the notebook using only the columns specified in "Column Information" so that only the columns necessary to our analysis were included. 
2. Dropped row zero and reset the index as it did not contain actual data just the units for the data displayed in the rest of the dataframe
3. Set the index as an integer 'OBS' and dropped its column form so that the power outages could be properly identified and ordered for the analyses.
4. Converted the original 'OUTAGE.START.DATE' and 'OUTAGE.START.TIME' columns to datetime form and combined them into one pd.Timestamp column (dropping both of the originals). This allowed for ease of analysis as now only one column had to be used for analysis instead of two. Additionally, the combination of these two columns make comparison between dates easier as is specifies the exact time an outage occured.
5. Repeated step 4 with the 'OUTAGE.RESTORATION.DATE' AND 'OUTAGE.RESTORATION.TIME' columns to create the 'OUTAGE.RESTORATION' column to further ease analysis.

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

**Metric:**

**Information needed at the time of prediction**
<br/> &nbsp;  - 98qtuhluahlsh


### Baseline Model
Our baseline model predicts CUSTOMERS.AFFECTED using CAUSE.CATEGORY, CLIMATE.REGION, and OUTAGE.DURATION. Missing values are handled using mean imputation for numerical features with categorical features being filled with the mode. 

**Features:**
<br/> &nbsp;  - CAUSE.CATEGORY: Categorical
<br/> &nbsp;  - CLIMATE.REGION: Categorical
<br/> &nbsp;  - OUTAGE.DURATION: Numerical
<br/>
<br/> &nbsp;  - The categorical variables are encoded using a OneHotEncoder

**Performance Evaluation:** safhalfuhdhsd

### Final Model
### Fairness Analysis

**Group X:** Northern Climate Region
<br/>
**Group Y:** Southern Climate Region
<br/>
**Evaluation Metric:** RMSE
<br/>
**Null Hypothesis:** Our model is fair. Its precision for the number of customers affected by power outages in the northern climate region and the number of customers affected by power outages in the southern climate region are roughly the same, and any differences are due to random chance.
<br/>
**Alternative Hypothesis:** Our model is unfair. Its precision for the number of customers affected by power outages in the northern climate region is different than its precision for the number of customers affected by power outages in the southern climate region.
<br/>
**Test Statistic:** Absolute Difference in Means
<br/>
**Significance Level:** 0.05
<br/>
**p_value:** 
<br/>
**Conclusion:** 
