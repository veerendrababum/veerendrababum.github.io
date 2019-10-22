---
title: "Data Preparation"
permalink: /datapreparation/
author_profile: true

---

## Overview

<p style="text-align: justify;font-family: none;">This section describes clearing and processing of the data collected for testing a hypothesis whether university towns have their housing prices less effected by recessions.</p>

<p style="text-align: justify;font-family: none;">Data acquisition is described in the previous section.</p>

<p style="text-align: justify;font-family: none;">Analysis of the data is described in the next section.</p>

<p style="text-align: justify;font-family: none;">This project is based on assignments from Introduction to Data Science in Python by University of Michigan on Coursera</p>

<p style="text-align: justify;font-family: none;">The analysis for this project was performed in Python.</p>

<p style="text-align: justify;font-family: none;">Cleaning and Processing of the University Town Data</p>

<p style="text-align: justify;font-family: none;">The list of university towns includes entries of both university towns and states in which these towns are located in a single column. State names should be removed from that column and then added as the second column to a data frame with two columns corresponding to university towns and states they are in. The format of the DataFrame is: DataFrame( [ [“Michigan”, “Ann Arbor”], [“Michigan”, “Yipsilanti”] ], columns=[“State”, “RegionName”] )</p>

<p style="text-align: justify;font-family: none;">In addition to this transformation, certain characters and portions of the text need to be removed. Specifically:</p>

<p style="text-align: justify;font-family: none;">1.	For “State”, removing characters from “[” to the end.</p>
<p style="text-align: justify;font-family: none;">2.	For “RegionName”, when applicable, removing every character from either “[” or “ (“ or “:” to the end.</p>
<p style="text-align: justify;font-family: none;">3.	Finally, it is important to note that certain rows in the data represent universities and not towns. Those rows will be subsequently dropped when merging the university town data with housing price data.</p>
<p style="text-align: justify;font-family: none;">The following function performs the transformation described above, and then uses regular expressions to identify and remove the redundant text patterns.</p>

```
def get_list_of_university_towns():
        
    #Create two columns corresponding to university towns and states they are in 
    #and remove redundant data
    university_towns = get_university_data()
    university_towns['State']= university_towns[university_towns["RegionName"].str.contains('edit')]
    university_towns['State'] = (university_towns['State'].str.replace(r'\[.*','').fillna(method='ffill'))
    university_towns['RegionName'] = university_towns['RegionName'].str.replace(r'\W*\(.*','')
    university_towns = university_towns[['State', 'RegionName']]
    university_towns.drop_duplicates(keep=False, inplace=True)
    df1 = university_towns[university_towns.RegionName.str.contains('edit')].index
    university_towns = university_towns.drop(df1).reset_index(drop = True)
    return university_towns

get_list_of_university_towns().head() 

```  


<p style="text-align: justify;font-family: none;">The first five rows of the resulting data frame are as follows:</p>


## Processing of the GDP Data

<p style="text-align: justify;font-family: none;">Because subsequent analysis requires changes in GDP in order to determine the start, the end and the bottom of a recession, the following code creates leads and lags of GDP and then calculates changes in GDP:</p>

<p style="text-align: justify;font-family: none;">The first five rows of the resulting data frame are as follows:</p>

## Processing of the Housing Data

<p style="text-align: justify;font-family: none;">Finally, the monthly housing price data needs to be converted to quarters before it is analyzed along with quarterly GDP figures. The following function averages the monthly prices within each quarter. The resulting data frame has columns for 2000Q1 through 2016Q3, and a multi-index in the shape of [“State”, “RegionName”]. Then the function below merges housing data with state names using state codes. This step is necessary in order to subsequently merge the housing data with the university town data, which include state names but do not include state codes:</p>


```
def convert_housing_data_to_quarters():

    housing_data = load_housing_data()
    housing_data = pd.DataFrame(housing_data.drop(['Metro','CountyName','SizeRank'],  axis=1)).set_index(['RegionID', 'State','RegionName']).stack().reset_index()
    v =  housing_data.rename(columns={'level_3':'Year-Mo',0:'MedianPrice'})
    month_to_qtr = {'01':'q1','02':'q1','03':'q1','04':'q2','05':'q2','06':'q2',
                '07':'q3','08':'q3','09':'q3','10':'q4','11':'q4','12':'q4'}
    v['YearQtr'] = v['Year-Mo'].str[0:4] + (v['Year-Mo'].str[-2:].map(month_to_qtr))
    v = v[v['YearQtr']>='2000q1'].copy()
    v = (v.drop(['Year-Mo'], axis=1)
         .groupby(['RegionID', 'State','RegionName','YearQtr'])
         .mean()
         .unstack())
    v.index = v.index.droplevel()
    v.columns = v.columns.droplevel()
    del v.columns.name
    return v
convert_housing_data_to_quarters()
```