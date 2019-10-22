---
title:"Are University towns have their mean housing prices less effected by recessions?"
date: 2019-10-22
tags: [Data Cleaning, Data Analysis]
excerpt: "Data Cleaning, Data Analysis, Data Science"
---

# H1 Data Acquisition

# H2 Overview

This section describes data acquisition process for testing a hypothesis whether university towns have their housing prices less effected by recessions. This analysis requires a list of university towns, mean / median housing prices by city for the purpose of comparing housing prices in university towns versus non-university towns, and GDP growth for determining the timing of recessions.

The next step, which includes cleaning and processing of the data, is described here.

This project is based on assignments from Introduction to Data Science in Python by University of Michigan on Coursera

The analysis for this project was performed in Python.


# H2 Data

All the data have been downloaded from the Coursera website. The original sources of the data and code for loading the data are described below.
A list of university towns in the United States is available from the Wikipedia page on college towns. A university town is a city which has a high percentage of university students compared to the total population of the city. The following function imports a list of university towns and states in which these towns are located from a text file into a pandas data frame.

```
import pandas as pd
import numpy as np
from scipy.stats import ttest_ind
import re
def get_university_data():
    university_towns_data = pd.read_csv('/Users/15832/Desktop/Travel/Data Science/New folder/Coursera Course/course1_downloads/course1_downloads/university_towns.txt', sep='/n',engine='python',header = None)
    df = university_towns_data
    df.columns = ["RegionName"]
    return df
get_university_data().head()
```

The first five rows of the resulting data frame are as follows:


The GDP over time of the United States in current dollars (the chained value in 2009 dollars in quarterly intervals) is from Bureau of Economic Analysis, US Department of Commerce, in the file gdplev.xlsx. A quarter is a specific three month period, Q1 is January through March, Q2 is April through June, Q3 is July through September, Q4 is October through December. GDP data used in the analysis are from the first quarter of 2000 onward. The following function imports the GDP data from an Excel file into a pandas data frame and drops the data that are not used in the analysis:


The first five rows of the resulting data frame are as follows:

Housing data for the United States is from the Zillow research data site. In particular, the datafile for all homes at a city level, City_Zhvi_AllHomes.csv, has median home sale prices at a fine grained level. The following function imports the housing data from a CSV file into a pandas data frame:
def load_housing_data():
    
   
The first five rows and seven columns of the resulting data frame are as follows:
   
Finally, the following dictionary was used to map state names to two letter acronyms:


Next step: Data Preparation.

#H1 Data Preparation

#H2 Overview

This section describes clearing and processing of the data collected for testing a hypothesis whether university towns have their housing prices less effected by recessions.

Data acquisition is described in the previous section.

Analysis of the data is described in the next section.

This project is based on assignments from Introduction to Data Science in Python by University of Michigan on Coursera

The analysis for this project was performed in Python.

Cleaning and Processing of the University Town Data

The list of university towns includes entries of both university towns and states in which these towns are located in a single column. State names should be removed from that column and then added as the second column to a data frame with two columns corresponding to university towns and states they are in. The format of the DataFrame is: DataFrame( [ [“Michigan”, “Ann Arbor”], [“Michigan”, “Yipsilanti”] ], columns=[“State”, “RegionName”] )

In addition to this transformation, certain characters and portions of the text need to be removed. Specifically:

1.	For “State”, removing characters from “[” to the end.
2.	For “RegionName”, when applicable, removing every character from either “[” or “ (“ or “:” to the end.
3.	Finally, it is important to note that certain rows in the data represent universities and not towns. Those rows will be subsequently dropped when merging the university town data with housing price data.
The following function performs the transformation described above, and then uses regular expressions to identify and remove the redundant text patterns.
def get_list_of_university_towns():
    


The first five rows of the resulting data frame are as follows:


Processing of the GDP Data

Because subsequent analysis requires changes in GDP in order to determine the start, the end and the bottom of a recession, the following code creates leads and lags of GDP and then calculates changes in GDP:





The first five rows of the resulting data frame are as follows:



Processing of the Housing Data

Finally, the monthly housing price data needs to be converted to quarters before it is analyzed along with quarterly GDP figures. The following function averages the monthly prices within each quarter. The resulting data frame has columns for 2000Q1 through 2016Q3, and a multi-index in the shape of [“State”, “RegionName”]. Then the function below merges housing data with state names using state codes. This step is necessary in order to subsequently merge the housing data with the university town data, which include state names but do not include state codes:
def convert_housing_data_to_quarters():

    

Next step: Analysis.
# H1 Analysis

# H2 Overview
This section tests a hypothesis whether university towns have their housing prices less effected by recessions, using a t-test. The test compares the ratio of the mean housing price in the quarter of the recession bottom to the quarter before the recession starts between university and non-university towns. (price_ratio = recession_bottom / quarter_before_recession).
Data cleaning and processing are described in the previous section.
This project is based on assignments from Introduction to Data Science in Python by University of Michigan on Coursera
The analysis for this project was performed in Python.

Finding Recession Start, End and Bottom
In order to test the hypothesis whether university towns have their housing prices less effected by recessions, we first need to define what we mean by recession.
A recession is defined as starting with two consecutive quarters of GDP decline, and ending with two consecutive quarters of GDP growth.
A recession bottom is the quarter within a recession which had the lowest GDP.
We then proceed by finding the recession start, recession end and recession bottom in the data. We will use these figures for calculating the ratios of the mean housing prices discussed above.
The following function returns the year and quarter of the recession start time as a string value in a format such as 2005Q3:

The results show that the recession started in 2008Q3.
The next function returns the year and quarter of the recession end time as a string value in a format such as 2005Q3:



The next function returns the year and quarter of the recession bottom as a string value in a format such as 2005Q3:




#H2 The t-test

First, we calculate the decline (or growth) in housing prices between the recession start and recession bottom. Then we run a t-test to determine whether such price declines in university towns are statistically significantly different from the declines in non-university towns. The t-test returns whether the null hypothesis (that the two groups are the same) is rejected as well as the p-value of the confidence.
The following function returns a tuple (different, p, better) where “different” = True if the the p-value of the t-test p < 0.01 (we reject the null hypothesis), or “different” = False if otherwise (we cannot reject the null hypothesis). The value for “better” is either “university town” or “non-university town” depending on which has a higher mean price ratio (which is equivalent to a reduced market loss).

#H2 Results
The resulting tuple is (True, 0.0039, ‘university town’), which means that the null hypotheses (the two groups are the same) is rejected with the p-value of 0.0031. This result implies that university towns experienced smaller housing price declines during the recession of 2008-2009 compared to non-university towns.

