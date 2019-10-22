---
title: "Analysis"
permalink: /Analysis/
author_profile: true
---

## Overview

<p style="text-align: justify;font-family: none;">This section tests a hypothesis whether university towns have their housing prices less effected by recessions, using a t-test. The test compares the ratio of the mean housing price in the quarter of the recession bottom to the quarter before the recession starts between university and non-university towns. (price_ratio = recession_bottom / quarter_before_recession).</p>
<p style="text-align: justify;font-family: none;">Data cleaning and processing are described in the previous section.</p>
<p style="text-align: justify;font-family: none;">This project is based on assignments from Introduction to Data Science in Python by University of Michigan on Coursera</p>
<p style="text-align: justify;font-family: none;">The analysis for this project was performed in Python.</p>

<p style="text-align: justify;font-family: none;">Finding Recession Start, End and Bottom</p>
<p style="text-align: justify;font-family: none;">In order to test the hypothesis whether university towns have their housing prices less effected by recessions, we first need to define what we mean by recession.</p>
<p style="text-align: justify;font-family: none;">A recession is defined as starting with two consecutive quarters of GDP decline, and ending with two consecutive quarters of GDP growth.</p>
<p style="text-align: justify;font-family: none;">A recession bottom is the quarter within a recession which had the lowest GDP.</p>
<p style="text-align: justify;font-family: none;">We then proceed by finding the recession start, recession end and recession bottom in the data. We will use these figures for calculating the ratios of the mean housing prices discussed above.</p>
<p style="text-align: justify;font-family: none;">The following function returns the year and quarter of the recession start time as a string value in a format such as 2005Q3:</p>

```
def get_recession_start():
    GDP_Data = get_gdp_data()
    GDP_Data['Change in GDP'] = GDP_Data['GDP'].diff().shift(-1)
    recission = GDP_Data.loc[GDP_Data['Change in GDP'].idxmin()]
    return recission['Qaurters']
get_recession_start()
```
<p style="text-align: justify;font-family: none;">The results show that the recession started in 2008Q3.</p>
<p style="text-align: justify;font-family: none;">The next function returns the year and quarter of the recession end time as a string value in a format such as 2005Q3:</p>
```
def get_recession_end():
    
    GDP_Data = get_gdp_data()
    GDP_Data['Change in GDP'] = GDP_Data['GDP'].diff().shift(-1)
    recission_years = GDP_Data[34:].reset_index(drop = True)
    recission_end = recission_years[(recission_years['GDP'] > recission_years['GDP'].shift(1))&
                (recission_years['GDP'].shift(1) > recission_years['GDP'].shift(2))].copy()
    return recission_end.iloc[0,0]
       
get_recession_end()
```


<p style="text-align: justify;font-family: none;">The next function returns the year and quarter of the recession bottom as a string value in a format such as 2005Q3:</p>

```
def get_recession_bottom():
    GDP_Data = get_gdp_data()
    GDP_Data['Change in GDP'] = GDP_Data['GDP'].diff().shift(-1)
    recission_years = GDP_Data[34:].reset_index(drop = True)
    recission_end = recission_years[(recission_years['GDP'] > recission_years['GDP'].shift(1))&
                (recission_years['GDP'].shift(1) > recission_years['GDP'].shift(2))].copy()
    recission_bottom = recission_years[0:5].reset_index(drop = True)
    recission_bottom_quarter = recission_bottom.loc[recission_bottom['GDP'].idxmin()]
    return recission_bottom_quarter['Qaurters']
get_recession_bottom()
```


## The t-test

<p style="text-align: justify;font-family: none;">First, we calculate the decline (or growth) in housing prices between the recession start and recession bottom. Then we run a t-test to determine whether such price declines in university towns are statistically significantly different from the declines in non-university towns. The t-test returns whether the null hypothesis (that the two groups are the same) is rejected as well as the p-value of the confidence.</p>
<p style="text-align: justify;font-family: none;">The following function returns a tuple (different, p, better) where “different” = True if the the p-value of the t-test p < 0.01 (we reject the null hypothesis), or “different” = False if otherwise (we cannot reject the null hypothesis). The value for “better” is either “university town” or “non-university town” depending on which has a higher mean price ratio (which is equivalent to a reduced market loss).</p>
```
from scipy.stats import ttest_ind, ttest_ind_from_stats
from scipy.special import stdtr
def run_ttest():
    
    university_towns = get_list_of_university_towns()
    university_towns = university_towns[['State', 'RegionName']]
    university_towns['University_Town'] = 1
    university_towns = university_towns.set_index(['State', 'RegionName'])
    
    new_housing_data = convert_housing_data_to_quarters()
    new_housing_data = new_housing_data.merge(university_towns, how = 'left', left_index = True, right_index = True)
    new_housing_data.University_Town[new_housing_data.University_Town.isnull()] = 0
       
    recession_start = get_recession_start()
    recession_bottom = get_recession_bottom()
    y = new_housing_data[[recession_start, recession_bottom, 'University_Town']]
    y['Change'] = y['2009q2'] / y['2008q3']
    
    ut = y[y['University_Town'] == 1]['Change'].dropna()
    not_ut = y[y['University_Town'] == 0]['Change'].dropna()
    t = ttest_ind(ut, not_ut)
    p = t[1]
     
    if p < 0.01:
        different = True    
    else:
        different = False
    
    if ut.mean() < not_ut.mean():
        better = 'non-university town'
    else:
        better = 'university town'
        
    return (different, p, better)
        
run_ttest()
```
## Results
<p style="text-align: justify;font-family: none;">The resulting tuple is (True, 0.0039, ‘university town’), which means that the null hypotheses (the two groups are the same) is rejected with the p-value of 0.0039. This result implies that university towns experienced smaller housing price declines during the recession of 2008-2009 compared to non-university towns.</p>