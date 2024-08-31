# KARAMOJA PROJECT

# ANALYZING THE FOOD INSECURITY IN KARAMOJA REGION

# BUSINESS PROBLEM

As a Data Analyst, I assessed the Karamoja food security crisis. Where the data contains the districts of the Karamoja Region and the 2 main food crops grown, I can monitor the food security and produce insights on how to improve the food security in the region.

# PROJECT OVERVIEW

By creating an interactive visualization the stakeholders can monitor the food situation in the region and come up with solutions on the most effective way to curb food insecurity.

# OBJECTIVES OF THE STUDY

To determine which crop has the highest productivity

To determine which densely populated areas should be given more focus

To determine which district had the highest crop yeild

To establish the relationship between area and, crop area and yield

# Data Collection and Preparation

The data used will be obtained from Dalberg Data Insights. Data preparation will include cleaning the data to ensure there is no missing data and no duplicates.

Understanding the data

Loading all the required libraries and data

 Importing all libralies
import pandas as pd
import numpy as np


 Loading the zip module to extract data from the zip folder
from zipfile import ZipFile

data = "DATA.zip"

with ZipFile('DATA.zip', 'r') as zip:

  zip.printdir()
  
  zip.extractall()
  

   A function to show all the files in the zip folder
  
file_info_list = zip.infolist()

file_list = []

for file_info in file_info_list:

  file_list.append(file_info.filename)
  
  print("The file list are-",file_list)

   Creating a pandas dataframe to data cleaning
  
df= pd.concat(
    [pd.read_csv(ZipFile(data).open(i), encoding='latin-1') for i in ZipFile(data).namelist() if i.endswith('csv')], # Try reading with 'latin-1' encoding
    ignore_index=True
)

df.shape

 Viewing the first 5  data
df.head(5)

 Viewing the last 5 rows of data
df.tail(5)

  # DATA CLEANING
  
looking for any missing values in our DataFrame and analyzing the best way to handle the missing values.

Ensuring data accuracy by making sure there are no duplicates.

Cleaning data by dropping columns that were not in use.

 looking for missing values
df.isnull().sum()

 All columns in the data set
df.columns

 Dropping Columns that are not in use
df = df.drop('Unnamed: 0', axis=1)
df.head()


Dealing with missing values

The column NAME has the names of the missing district names by replacing the disappeared district name column I will solve the missing value problem. Since I can use District Name column as the location then I can drop the SubCounty Name column

Showing all rows with missing values
df[df.isnull().any(axis=1)]

 Unique values for district
df['DISTRICT_NAME'].value_counts()

 Looking for unique values and their sum for subcounty
df['SUBCOUNTY_NAME'].value_counts()

 A function to replace the missing values of the column district name with the corresponding index on the column name
df['DISTRICT_NAME']= df['DISTRICT_NAME'].fillna(df.loc[52:58, 'NAME'])
df['DISTRICT_NAME']

 Unique values in Karamoja
df['Karamoja'].value_counts()

 Filling the missing values for the karamoja column
df['Karamoja']= df['Karamoja'].fillna('Y')
df['Karamoja']

#Filling the missing values in subcounty column with
df['SUBCOUNTY_NAME']= df['SUBCOUNTY_NAME'].fillna(df.loc[52:58, 'NAME'])
df['SUBCOUNTY_NAME']

df.info()

# ANALYZING THE DATA

Since the data has no issues, we can proceed to analyze the data and solve the Objectives

 Summary statistics
df.describe()

 Grouping district and population,crop area and yield
densely_pop = df.groupby('DISTRICT_NAME').agg({
    'POP': 'sum',
    'Crop_Area_Ha': 'sum',
    'S_Yield_Ha': 'sum',
    'M_Yield_Ha': 'sum'
    }).reset_index()
densely_populated = densely_pop.sort_values(by='POP', ascending=False)
densely_populated

 Analyzing Crop productivity by District
crop = df.groupby('DISTRICT_NAME').agg({
    'Crop_Area_Ha': 'sum',
    'S_Prod_Tot': 'sum',
    'M_Prod_Tot': 'sum'
    }).reset_index()
crop_productivity = crop.sort_values(by='Crop_Area_Ha', ascending=False)
crop_productivity

To Analyze which District has the highest crop yield
crop = df.groupby('DISTRICT_NAME').agg({
    'Crop_Area_Ha': 'sum',
    'S_Yield_Ha': 'sum',
    'M_Yield_Ha': 'sum'
    }).reset_index()
crop_yield = crop.sort_values(by='Crop_Area_Ha', ascending=False)
crop_yield

Maize has the highest yield in Kaabong District

 Correlation between area,crop area and yield
Corr_data = df[['Area','Crop_Area_Ha','S_Yield_Ha','M_Yield_Ha']].corr()
Corr_data

Crop area per Ha has a moderately high positive correlation with the area. sorghum has a low negative correlation with area meaning that an increase in area causes a small decrease in sorghum yield. While in maize a unit change in Area causes an increase of maize yield by 0.05%
There is a moderate positive correlation between the 2 crop yields.

 To export our cleaned data
df.to_csv('Karamoja.csv', index=False)

# CONCLUTION

From the analysis, it can be concluded that:

Kaabong district is the most densely populated district in the Karamoja region, it has a lower Crop area than Kotido but has the highest crop yield

Sorghum has the highest average productivity overall and has highest crop area.

Kaabong is the district with the highest crop yeild

There is weak positive correlation between crop area and yield.

# RECOMENDATIONS

Sorghum has the highest productivity and hence should be given more priority as it is more drought-resistant.

Kaabong as the most densely populated district should have more land allocated for crop production.

More production of drought-resistant crops could solve the food insecurity crisis.

There should be more investigation into which crops perform best in which district as maize performs better in Kaabong District.
