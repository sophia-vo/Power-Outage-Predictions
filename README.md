# Power-Outage-Predictions
DSC 80 Final Project at UCSD

### Introduction
**Dataset:** Power Outages
<br/> &nbsp;  -  This dataset explores the major power outage data in the 48 contiguous states in the US from January 2000 to July 2016. 
<br/>
<br/>
**Question:** Is the average outage duration for power outages caused by severe weather equal to the average outage duration for power outages caused by experiment failure?
<br/> &nbsp;  -  This dataset is important as it holds information for power outages across the US that can be used to identify patterns and make predictions for the potential consequences caused by future outages. 
<br/> &nbsp;  -  This question is significant as analyzing the differences in effects of causes of power outages can help inform solutions to increase power outage preparedness and help scientists predict the consequences of certain power outages in relation to others. 
<br/>
<br/>
**Rows:** 1534 
<br/>
<br/>
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

### Assessment of Missingness
### Hypothesis Testing
**Null Hypothesis:** There is no significant difference between the outage duration for outages caused by severe weather and outage duration for outages caused by equipment failure.
<br/>
**Alternative Hypothesis:** There is a significant difference between the outage duration for outages caused by severe weather and outage duration for outages caused by equipment failure.
<br/>
<br/>
**Test Statistic:** Difference in means
<br/>
<br/>
**Significance Level:** 0.05
<br/>
<br/>
**p-value**: 0.015
### Framing a Prediction Problem
### Baseline Model
### Final Model
### Fairness Analysis
