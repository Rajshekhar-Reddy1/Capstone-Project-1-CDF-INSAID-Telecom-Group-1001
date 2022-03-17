
![image](https://user-images.githubusercontent.com/87597168/158847580-bc27a096-0107-4b91-8636-a678e9c64aa5.png)

https://colab.research.google.com/drive/1SCn1R0X-gUZdfLRm4BvoueW8Fl7PXDsi#scrollTo=078a1feb

Table of Contents

1.Introduction

2.Problem Statement

3.Installing & Importing Libraries

4.Data Acquisition & Description

5.Data Pre-Profiling

6.Data Pre-Processing

7.Data Post-Profiling

8Exploratory Data Analysis

9.Summarization

10.Thanks



https://raw.githubusercontent.com/pratikbarjatya/Telecom-Capstone-Project/master/images/capstone_analysis.png

Capstone Project 1 (CDF) INSAID Telecom Group 1001.ipynb
Capstone Project 1 (CDF) INSAID Telecom Group 1001.ipynbC_
Capstone Project 1 (CDF) Insaid Telecom Team 1001
Table of Contents
Introduction
Problem Statement
Installing & Importing Libraries
Data Acquisition & Description
Data Pre-Profiling
Data Pre-Processing
Data Post-Profiling
Exploratory Data Analysis
Summarization

1. Introduction
It's always wonderful to see services customized to your needs.Businesses try to understand your behavior and adjust their offerings, so as to ensure you feel attached to their services.

InsaidTelecom, one of the leading telecom players, understands that customizing offering is very important for its business to stay competitive.
Currently, InsaidTelecom is seeking to leverage behavioral data from more than 60% of the 50 million mobile devices active daily in India to help its clients better understand and interact with their audiences.
image.png

In this assignment, you are going to study the demographics of a user (gender and age) based on their app download and usage behaviors. The Data is collected from mobile apps that use InsaidTelecom services. Full recognition and consent from individual user of those apps have been obtained, and appropriate anonymization have been performed to protect privacy. Due to confidentiality, we won't provide details on how the gender and age data was obtained.


2. Problem Statement
Insaidians are expected to build a dashboard to understand user's demographic characteristics based on their:

mobile usage
geolocation, and
mobile device properties.
Doing so will help millions of developers and brand advertisers around the world pursue data-driven marketing efforts which are relevant to their users and catered to their preferences.

To help the customer the consultants are expected to have depth of clarity in the underlying data.

How much effort has been put into cleansing and purifying the data will decide how closely have you looked at the data. How detailed is the observation stated in the submission report and finally how well a group presents their consulting journey.*

Please remember this is an analytics consulting hence, your efforts in terms of finding user behavior is going to directly impact the company's offerings.

Do help the company understand what is the right way forward and suggest actionable insights from marketing and product terms.


3. Installing & Importing Libraries
[ ]
#-------------------------------------------------------------------------------------------------------------------------------
import pandas as pd                                                 # Importing package pandas (For Panel Data Analysis)
from pandas_profiling import ProfileReport                          # Import Pandas Profiling (To generate Univariate Analysis)
#-------------------------------------------------------------------------------------------------------------------------------
import numpy as np                                                  # Importing package numpys (For Numerical Python)
#-------------------------------------------------------------------------------------------------------------------------------
import matplotlib.pyplot as plt                                     # Importing pyplot interface to use matplotlib
%matplotlib inline
import seaborn as sns                                               # Importing seaborn library for interactive visualization
from matplotlib.legend_handler import HandlerBase

#-------------------------------------------------------------------------------------------------------------------------------
import scipy as sp                                                  # Importing library for scientific calculations
#-------------------------------------------------------------------------------------------------------------------------------
import warnings                                                     # Importing warning to disable runtime warnings
warnings.filterwarnings("ignore")                                   # Warnings will appear only once

4. Data Acquisition & Description
events_data

when a user uses mobile on INSAID Telecom network, the event gets logged in this data.
Each event has an event id, location (lat/long), and the event corresponds to frequency of mobile usage.
timestamp: when the user is using the mobile.
gender_age_train

Devices and their respective user gender, age and age_group
phone_brand_device_model

device ids, brand, and models phone_brand: note that few brands are in Chinese
Events Data details
[ ]
events_data = pd.read_csv('events_data.csv')
events_data

[ ]
events_data['state'].nunique()
32
[ ]
events_data.shape
(3252950, 7)
[ ]
events_data.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3252950 entries, 0 to 3252949
Data columns (total 7 columns):
 #   Column     Dtype  
---  ------     -----  
 0   event_id   int64  
 1   device_id  float64
 2   timestamp  object 
 3   longitude  float64
 4   latitude   float64
 5   city       object 
 6   state      object 
dtypes: float64(3), int64(1), object(3)
memory usage: 173.7+ MB
[ ]
events_data.isna().sum()
event_id       0
device_id    453
timestamp      0
longitude    423
latitude     423
city           0
state        377
dtype: int64
[ ]
events_data['event_id'].nunique()
3252950
[ ]
events_data['device_id'].nunique()
60865
[ ]
events_data['city'].nunique()
933
[ ]
# Distribution of City in the Event Table
fig, ax = plt.subplots(figsize = (16,8))

sns.barplot(x =events_data['city'].value_counts().keys()[:10], y=events_data['city'].value_counts()[:10])
plt.xlabel('City Name')
plt.ylabel('Counts')
plt.xticks(rotation = 90)
plt.title('City')
plt.show()

[ ]
# Distribution of State in the Event table
fig, ax = plt.subplots(figsize = (16,8))

sns.barplot(x =events_data['state'].value_counts().keys(), y=events_data['state'].value_counts())
plt.xlabel('State Name')
plt.ylabel('Counts')
plt.xticks(rotation = 90)
plt.title('State')
plt.show()

Observations:
Events data set shape is 3252950 Observations, and 7 Feature Columns for analysis.
Device_id unique, anonymized string of numbers and letters that identifies every individual smartphone or tablet in the world. Missing Values in device_id is 453
Timestamp datatype is Object, needs to be converted in timestamp for better analysis.
Longitude and Latitude can provide location of the device.
Longitude and Latitude is observed with 423 Missing values
State has 377 Missing values
Feature event_id shows 3252950 unique events
We can observe that in 3252950 events there are 60865 Unique device_id.
Gender_Age_Train Data details
[ ]
gender_age_train = pd.read_csv("gender_age_train.csv")
[ ]
gender_age_train

[1]
0s
gender_age_train.shape

[ ]
0s
gender_age_train.info()
[ ]
gender_age_train.describe()

[ ]
gender_age_train.isnull().sum()
device_id    0
gender       0
age          0
group        0
dtype: int64
[ ]
gender_age_train["gender"].value_counts()
M    47904
F    26741
Name: gender, dtype: int64
[ ]
gender_age_train['device_id'].nunique()
74645
[ ]
100*(gender_age_train['gender'].value_counts()/ gender_age_train.shape[0])
M    64.175765
F    35.824235
Name: gender, dtype: float64
[ ]
# Distribution of Gender in gender_age_train table
fig, ax = plt.subplots(figsize = (10,6))

sns.barplot(x =gender_age_train['gender'].value_counts().keys(), y=gender_age_train['gender'].value_counts())
plt.xlabel('User by Gender')
plt.ylabel('Counts')
plt.title('Gender')
plt.show()

[ ]
# Distribution of Age in gender_age_train table
fig, ax = plt.subplots(figsize = (10,6))

sns.histplot(gender_age_train['age'], color = '#FD625E', bins = 50, kde = True)
plt.xlabel('Age')
plt.ylabel('Frequency')
plt.title('Age Distribution')
plt.show()

[ ]
#Group
fig, ax = plt.subplots(figsize = (10,6))

sns.barplot(x =gender_age_train['group'].value_counts().keys(), y=gender_age_train['group'].value_counts())
plt.xlabel('Group')
plt.ylabel('Counts')
plt.title('Age Group')
plt.show()

Observations:
Shape of the gender_age_train dataset is (74645,4).
No missing value is observed.
Age :
It is observed that MINIMUM Age is 1 and Max is 96.
Avearge Users are of age 31 Years.
Median Age of user is 29 Years.
Some device_id is negative, we have remove the negative sign form the device id.
In the given dataset we can observe that Male users are more than Female Users.
Service users are 64% Male and 35% are Female.
It is observed that users from 1- 96 Years.
Users between 20-40 years are high, they are very active user of the service.
We can also see that there are users between 1-15 and 80-96 years.
Users between age group 23-26 & 32-38 are high and are Male Users.
Female Users between age group 33-42 are high.
[ ]
gender_age_train['device_id'].plot.hist(figsize = (12, 6))

[ ]
abs(gender_age_train['device_id']).plot.hist(figsize = (12, 6))

Phone_Brand_Device_Model Data details
[ ]
phone_brand_device_model = pd.read_csv("phone_brand_device_model.csv")
[ ]
phone_brand_device_model


[ ]
phone_brand_device_model.shape
(87726, 3)
[ ]
phone_brand_device_model.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 87726 entries, 0 to 87725
Data columns (total 3 columns):
 #   Column        Non-Null Count  Dtype 
---  ------        --------------  ----- 
 0   device_id     87726 non-null  int64 
 1   phone_brand   87726 non-null  object
 2   device_model  87726 non-null  object
dtypes: int64(1), object(2)
memory usage: 2.0+ MB
[ ]
phone_brand_device_model.describe()

[ ]
phone_brand_device_model['device_id'].nunique()
87726
[ ]
phone_brand_device_model.phone_brand.nunique()
116
[ ]
phone_brand_device_model.device_model.nunique()
1467
[ ]
abs(phone_brand_device_model['device_id']).plot.hist(figsize = (12, 6))

Observations:
There are 87726 records in the dataset.

missing valuse in the dataset.

1467 total device model and 116 phone brand available in the dataset.


5. Data Pre-Profiling
[ ]
#profile_Event = ProfileReport(events_data)
#profile_Event.to_file(output_file='Pre Profiling Report_Event.html')
#profile_Event
[ ]
#profile_Age_Gender = ProfileReport(gender_age_train)
#profile_Age_Gender.to_file(output_file='Pre Profiling Report_Age_Gender.html')
#profile_Age_Gender
[ ]
#profile_Phone_Brand = ProfileReport(phone_brand_device_model)
#profile_Phone_Brand.to_file(output_file='Pre Profiling profile_Phone_Brand.html')
#profile_Phone_Brand

6. Data Pre-Processing
Device_id in all three dataset change from negative to positive using abs().
Timestamp datatype convert to valid timestamp using pandas to_datetime.
Find the missing Null values in Device_id, latitude, longitude and State and try to fill these data.
Check for outlier in age and remove it.
Phone_brand and Device_model names change from Chinese to English.
Device_id in all three dataset change from negative to positive using abs().
[ ]
#Converting the negative device id to positive value

events_data["device_id"] = abs(events_data["device_id"])
gender_age_train['device_id'] = abs(gender_age_train['device_id'])
phone_brand_device_model['device_id'] = abs(phone_brand_device_model['device_id'])
Timestamp datatype convert to valid timestamp using pandas to_datetime.
[ ]
#Converting the timestamp datatype to Datetime format.

events_data["timestamp"] = pd.to_datetime(events_data["timestamp"])
[ ]
events_data.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3252950 entries, 0 to 3252949
Data columns (total 7 columns):
 #   Column     Dtype         
---  ------     -----         
 0   event_id   int64         
 1   device_id  float64       
 2   timestamp  datetime64[ns]
 3   longitude  float64       
 4   latitude   float64       
 5   city       object        
 6   state      object        
dtypes: datetime64[ns](1), float64(3), int64(1), object(2)
memory usage: 173.7+ MB
[ ]
#Missing values in Event Data table
events_data.isna().sum()
event_id       0
device_id    453
timestamp      0
longitude    423
latitude     423
city           0
state        377
dtype: int64
Filling Missing Values for State
[ ]
↳ 5 cells hidden
Filling Missing Values for Device Id
[ ]
# Filling missing device_id
device_id_null = events_data[events_data['device_id'].isna()]
device_id_null

[ ]
device_id_null['city'].unique()
array(['Indore', 'Jaipur', 'Hoshiarpur', 'Pune', 'Visakhapatnam', 'Delhi',
       'Chennai', 'Bardoli', 'Jetpur'], dtype=object)
[ ]
device_id_null[device_id_null['city'] == 'Indore']

[ ]
# Unique Longitudes and Latitude
device_id_null['longitude'].unique(), device_id_null['latitude'].unique()
(array([75.882956, 75.888487, 75.846007, 75.923332, 75.992551, 73.862756,
        73.860165, 83.357991, 77.292481, 73.926499, 80.343613, 80.309272,
        77.274814, 83.371738, 75.95805 , 83.342711, 75.836167, 80.335435,
        73.169345, 70.686387, 77.303153]),
 array([22.814519, 26.948689, 26.960796, 22.777781, 31.561747, 18.628057,
        18.566925, 17.805195, 28.719966, 18.614812, 13.153332, 13.149176,
        28.721053, 17.752819, 22.817526, 17.822906, 26.95399 , 13.189053,
        21.194283, 21.790693, 28.728888]))
[ ]
# Null Device id
fig, ax = plt.subplots(figsize = (12,6))
sns.barplot(x = device_id_null['city'].value_counts().keys(), y = device_id_null['city'].value_counts(), color = 'b')
plt.xlabel('City Name')
plt.ylabel('Counts')
plt.xticks(rotation = 90)
plt.title('Device ID Missing')
plt.show()

[ ]
events_data.isna().sum()
event_id       0
device_id    453
timestamp      0
longitude    423
latitude     423
city           0
state          0
dtype: int64
[ ]
#unique device id 
unique_device_id = events_data.drop_duplicates(['device_id'])
unique_device_id

[ ]
unique_device_id[unique_device_id['city'] == 'Jaipur']

[ ]
#unique device id
fig, ax = plt.subplots(figsize = (12,6))
sns.barplot(x = unique_device_id['city'].value_counts().keys()[:50], y = unique_device_id['city'].value_counts()[:50], color = 'b')
plt.xlabel('City Name')
plt.ylabel('Counts')
plt.xticks(rotation = 90)
plt.title('Unique Device ID City')
plt.show()

[ ]
# pune_missing_device_id 
pune = device_id_null[device_id_null['city'] == 'Pune']
pune['longitude'].unique()
array([73.862756, 73.860165, 73.926499])
[ ]
pune

[ ]
visakhapatnam = device_id_null[device_id_null['city'] == 'Visakhapatnam']
visakhapatnam['longitude'].unique(), visakhapatnam['latitude'].unique()
(array([83.357991, 83.371738, 83.342711]),
 array([17.805195, 17.752819, 17.822906]))
[ ]
events_data['device_id'] = np.where(events_data['longitude'] == 75.888487, 9.177251e+17, events_data.device_id).astype(float)
events_data['device_id'] = np.where(events_data['longitude'] == 75.846007, 8.460337e+18, events_data.device_id).astype(float)
events_data['device_id'] = np.where(events_data['longitude'] == 75.836167, 3.562356e+18, events_data.device_id).astype(float)
events_data['device_id'] = np.where(events_data['longitude'] == 73.862756, 9.027086e+18, events_data.device_id).astype(float)
[ ]
events_data.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3252950 entries, 0 to 3252949
Data columns (total 7 columns):
 #   Column     Dtype         
---  ------     -----         
 0   event_id   int64         
 1   device_id  float64       
 2   timestamp  datetime64[ns]
 3   longitude  float64       
 4   latitude   float64       
 5   city       object        
 6   state      object        
dtypes: datetime64[ns](1), float64(3), int64(1), object(2)
memory usage: 173.7+ MB
[ ]
events_data['device_id'].isna().sum()
348
[ ]
events_data['device_id'].nunique()
60865
Using Folium to detect Anomaly
[ ]
%matplotlib inline
# NULL Device Id Plotting
fig, ax = plt.subplots(figsize = (12, 8))
sns.scatterplot(x = device_id_null['latitude'], y = device_id_null['longitude'])
plt.show()

[ ]
#Null Device_id Locations 
null_device_locations = device_id_null[['city','latitude', 'longitude']].values.tolist()
null_device_locations
[['Indore', 22.814519, 75.88295600000002],
 ['Jaipur', 26.948689, 75.888487],
 ['Jaipur', 26.960796, 75.846007],
 ['Indore', 22.777781, 75.92333199999999],
 ['Hoshiarpur', 31.561747, 75.99255099999998],
 ['Pune', 18.628057, 73.862756],
 ['Pune', 18.566925, 73.86016500000002],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Pune', 18.614812, 73.92649899999998],
 ['Jaipur', 26.960796, 75.846007],
 ['Pune', 18.614812, 73.92649899999998],
 ['Pune', 18.628057, 73.862756],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Jaipur', 26.948689, 75.888487],
 ['Pune', 18.628057, 73.862756],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Pune', 18.614812, 73.92649899999998],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Indore', 22.814519, 75.88295600000002],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Indore', 22.817526, 75.95805],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Jaipur', 26.95399, 75.836167],
 ['Indore', 22.817526, 75.95805],
 ['Hoshiarpur', 31.561747, 75.99255099999998],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Indore', 22.814519, 75.88295600000002],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Jaipur', 26.960796, 75.846007],
 ['Pune', 18.628057, 73.862756],
 ['Pune', 18.566925, 73.86016500000002],
 ['Indore', 22.777781, 75.92333199999999],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Jaipur', 26.948689, 75.888487],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Indore', 22.814519, 75.88295600000002],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Jaipur', 26.95399, 75.836167],
 ['Jaipur', 26.948689, 75.888487],
 ['Indore', 22.777781, 75.92333199999999],
 ['Pune', 18.566925, 73.86016500000002],
 ['Pune', 18.614812, 73.92649899999998],
 ['Bardoli', 21.194283, 73.16934499999998],
 ['Jaipur', 26.95399, 75.836167],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Indore', 22.777781, 75.92333199999999],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Jetpur', 21.790693, 70.686387],
 ['Jaipur', 26.95399, 75.836167],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Jaipur', 26.960796, 75.846007],
 ['Pune', 18.614812, 73.92649899999998],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Bardoli', 21.194283, 73.16934499999998],
 ['Jetpur', 21.790693, 70.686387],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Pune', 18.614812, 73.92649899999998],
 ['Indore', 22.817526, 75.95805],
 ['Indore', 22.814519, 75.88295600000002],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Indore', 22.817526, 75.95805],
 ['Jaipur', 26.95399, 75.836167],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Pune', 18.614812, 73.92649899999998],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Pune', 18.614812, 73.92649899999998],
 ['Jaipur', 26.960796, 75.846007],
 ['Bardoli', 21.194283, 73.16934499999998],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Jetpur', 21.790693, 70.686387],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Bardoli', 21.194283, 73.16934499999998],
 ['Jaipur', 26.948689, 75.888487],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Hoshiarpur', 31.561747, 75.99255099999998],
 ['Jaipur', 26.95399, 75.836167],
 ['Pune', 18.628057, 73.862756],
 ['Indore', 22.777781, 75.92333199999999],
 ['Jaipur', 26.95399, 75.836167],
 ['Pune', 18.566925, 73.86016500000002],
 ['Indore', 22.814519, 75.88295600000002],
 ['Indore', 22.814519, 75.88295600000002],
 ['Indore', 22.817526, 75.95805],
 ['Pune', 18.566925, 73.86016500000002],
 ['Jaipur', 26.95399, 75.836167],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Pune', 18.628057, 73.862756],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Pune', 18.614812, 73.92649899999998],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Bardoli', 21.194283, 73.16934499999998],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Jaipur', 26.95399, 75.836167],
 ['Jetpur', 21.790693, 70.686387],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Pune', 18.628057, 73.862756],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Hoshiarpur', 31.561747, 75.99255099999998],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Indore', 22.777781, 75.92333199999999],
 ['Bardoli', 21.194283, 73.16934499999998],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Jaipur', 26.960796, 75.846007],
 ['Pune', 18.628057, 73.862756],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Jaipur', 26.948689, 75.888487],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Jaipur', 26.960796, 75.846007],
 ['Jaipur', 26.95399, 75.836167],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Pune', 18.566925, 73.86016500000002],
 ['Indore', 22.777781, 75.92333199999999],
 ['Jaipur', 26.960796, 75.846007],
 ['Jaipur', 26.948689, 75.888487],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Pune', 18.628057, 73.862756],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Jaipur', 26.95399, 75.836167],
 ['Indore', 22.777781, 75.92333199999999],
 ['Pune', 18.566925, 73.86016500000002],
 ['Jaipur', 26.948689, 75.888487],
 ['Jetpur', 21.790693, 70.686387],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Pune', 18.566925, 73.86016500000002],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Indore', 22.817526, 75.95805],
 ['Jaipur', 26.95399, 75.836167],
 ['Jaipur', 26.95399, 75.836167],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Pune', 18.628057, 73.862756],
 ['Pune', 18.614812, 73.92649899999998],
 ['Jaipur', 26.960796, 75.846007],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Jaipur', 26.960796, 75.846007],
 ['Jaipur', 26.960796, 75.846007],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Hoshiarpur', 31.561747, 75.99255099999998],
 ['Pune', 18.614812, 73.92649899999998],
 ['Pune', 18.628057, 73.862756],
 ['Jaipur', 26.960796, 75.846007],
 ['Bardoli', 21.194283, 73.16934499999998],
 ['Jaipur', 26.948689, 75.888487],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Jaipur', 26.95399, 75.836167],
 ['Indore', 22.777781, 75.92333199999999],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Indore', 22.814519, 75.88295600000002],
 ['Hoshiarpur', 31.561747, 75.99255099999998],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Indore', 22.814519, 75.88295600000002],
 ['Jaipur', 26.960796, 75.846007],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Indore', 22.817526, 75.95805],
 ['Indore', 22.777781, 75.92333199999999],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Indore', 22.814519, 75.88295600000002],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Jaipur', 26.95399, 75.836167],
 ['Pune', 18.566925, 73.86016500000002],
 ['Pune', 18.628057, 73.862756],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Jaipur', 26.948689, 75.888487],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Bardoli', 21.194283, 73.16934499999998],
 ['Pune', 18.614812, 73.92649899999998],
 ['Pune', 18.628057, 73.862756],
 ['Pune', 18.566925, 73.86016500000002],
 ['Jaipur', 26.95399, 75.836167],
 ['Indore', 22.817526, 75.95805],
 ['Hoshiarpur', 31.561747, 75.99255099999998],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Pune', 18.614812, 73.92649899999998],
 ['Bardoli', 21.194283, 73.16934499999998],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Pune', 18.628057, 73.862756],
 ['Indore', 22.777781, 75.92333199999999],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Jaipur', 26.948689, 75.888487],
 ['Jaipur', 26.960796, 75.846007],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Bardoli', 21.194283, 73.16934499999998],
 ['Jetpur', 21.790693, 70.686387],
 ['Pune', 18.566925, 73.86016500000002],
 ['Pune', 18.566925, 73.86016500000002],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Indore', 22.814519, 75.88295600000002],
 ['Pune', 18.566925, 73.86016500000002],
 ['Jaipur', 26.95399, 75.836167],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Indore', 22.814519, 75.88295600000002],
 ['Bardoli', 21.194283, 73.16934499999998],
 ['Indore', 22.817526, 75.95805],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Jetpur', 21.790693, 70.686387],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Pune', 18.566925, 73.86016500000002],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Jaipur', 26.95399, 75.836167],
 ['Bardoli', 21.194283, 73.16934499999998],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Jetpur', 21.790693, 70.686387],
 ['Indore', 22.817526, 75.95805],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Jetpur', 21.790693, 70.686387],
 ['Indore', 22.777781, 75.92333199999999],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Jaipur', 26.95399, 75.836167],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Jaipur', 26.960796, 75.846007],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Hoshiarpur', 31.561747, 75.99255099999998],
 ['Pune', 18.628057, 73.862756],
 ['Indore', 22.814519, 75.88295600000002],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Jaipur', 26.960796, 75.846007],
 ['Pune', 18.614812, 73.92649899999998],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Jaipur', 26.95399, 75.836167],
 ['Bardoli', 21.194283, 73.16934499999998],
 ['Hoshiarpur', 31.561747, 75.99255099999998],
 ['Jaipur', 26.95399, 75.836167],
 ['Hoshiarpur', 31.561747, 75.99255099999998],
 ['Jaipur', 26.960796, 75.846007],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Indore', 22.814519, 75.88295600000002],
 ['Jaipur', 26.960796, 75.846007],
 ['Jetpur', 21.790693, 70.686387],
 ['Hoshiarpur', 31.561747, 75.99255099999998],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Pune', 18.628057, 73.862756],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Indore', 22.817526, 75.95805],
 ['Indore', 22.777781, 75.92333199999999],
 ['Jaipur', 26.95399, 75.836167],
 ['Bardoli', 21.194283, 73.16934499999998],
 ['Jetpur', 21.790693, 70.686387],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Jaipur', 26.960796, 75.846007],
 ['Indore', 22.817526, 75.95805],
 ['Pune', 18.628057, 73.862756],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Jaipur', 26.948689, 75.888487],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Hoshiarpur', 31.561747, 75.99255099999998],
 ['Jaipur', 26.960796, 75.846007],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Pune', 18.566925, 73.86016500000002],
 ['Jaipur', 26.948689, 75.888487],
 ['Jaipur', 26.95399, 75.836167],
 ['Bardoli', 21.194283, 73.16934499999998],
 ['Pune', 18.614812, 73.92649899999998],
 ['Pune', 18.566925, 73.86016500000002],
 ['Jaipur', 26.95399, 75.836167],
 ['Pune', 18.614812, 73.92649899999998],
 ['Pune', 18.614812, 73.92649899999998],
 ['Jaipur', 26.948689, 75.888487],
 ['Indore', 22.817526, 75.95805],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Pune', 18.614812, 73.92649899999998],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Jaipur', 26.960796, 75.846007],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Jaipur', 26.960796, 75.846007],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Pune', 18.566925, 73.86016500000002],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Hoshiarpur', 31.561747, 75.99255099999998],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Pune', 18.566925, 73.86016500000002],
 ['Jaipur', 26.960796, 75.846007],
 ['Jaipur', 26.960796, 75.846007],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Pune', 18.628057, 73.862756],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Pune', 18.614812, 73.92649899999998],
 ['Pune', 18.566925, 73.86016500000002],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Pune', 18.614812, 73.92649899999998],
 ['Jetpur', 21.790693, 70.686387],
 ['Jaipur', 26.95399, 75.836167],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Indore', 22.817526, 75.95805],
 ['Jetpur', 21.790693, 70.686387],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Jetpur', 21.790693, 70.686387],
 ['Indore', 22.814519, 75.88295600000002],
 ['Jaipur', 26.948689, 75.888487],
 ['Pune', 18.628057, 73.862756],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Pune', 18.614812, 73.92649899999998],
 ['Indore', 22.817526, 75.95805],
 ['Indore', 22.814519, 75.88295600000002],
 ['Jaipur', 26.960796, 75.846007],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Jaipur', 26.960796, 75.846007],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Jaipur', 26.948689, 75.888487],
 ['Pune', 18.566925, 73.86016500000002],
 ['Pune', 18.614812, 73.92649899999998],
 ['Pune', 18.614812, 73.92649899999998],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Indore', 22.777781, 75.92333199999999],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Pune', 18.614812, 73.92649899999998],
 ['Pune', 18.628057, 73.862756],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Delhi', 28.721053, 77.27481399999998],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Jaipur', 26.948689, 75.888487],
 ['Hoshiarpur', 31.561747, 75.99255099999998],
 ['Jaipur', 26.948689, 75.888487],
 ['Pune', 18.566925, 73.86016500000002],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Jaipur', 26.948689, 75.888487],
 ['Pune', 18.628057, 73.862756],
 ['Pune', 18.566925, 73.86016500000002],
 ['Jaipur', 26.948689, 75.888487],
 ['Indore', 22.777781, 75.92333199999999],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Jaipur', 26.960796, 75.846007],
 ['Jetpur', 21.790693, 70.686387],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Indore', 22.814519, 75.88295600000002],
 ['Jaipur', 26.948689, 75.888487],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Indore', 22.817526, 75.95805],
 ['Pune', 18.566925, 73.86016500000002],
 ['Pune', 18.628057, 73.862756],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Jaipur', 26.948689, 75.888487],
 ['Indore', 22.817526, 75.95805],
 ['Jaipur', 26.948689, 75.888487],
 ['Bardoli', 21.194283, 73.16934499999998],
 ['Jaipur', 26.95399, 75.836167],
 ['Pune', 18.628057, 73.862756],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Hoshiarpur', 31.561747, 75.99255099999998],
 ['Indore', 22.777781, 75.92333199999999],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Indore', 22.777781, 75.92333199999999],
 ['Pune', 18.566925, 73.86016500000002],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Jaipur', 26.948689, 75.888487],
 ['Chennai', 13.149176, 80.30927199999998],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Jetpur', 21.790693, 70.686387],
 ['Chennai', 13.153332, 80.34361299999998],
 ['Pune', 18.628057, 73.862756],
 ['Delhi', 28.728888, 77.30315300000002],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Jaipur', 26.95399, 75.836167],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Jaipur', 26.948689, 75.888487],
 ['Visakhapatnam', 17.822906, 83.342711],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Delhi', 28.719966000000003, 77.29248100000002],
 ['Chennai', 13.189053, 80.33543499999998],
 ['Jaipur', 26.948689, 75.888487],
 ['Visakhapatnam', 17.805195, 83.357991],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Hoshiarpur', 31.561747, 75.99255099999998],
 ['Visakhapatnam', 17.752819, 83.371738],
 ['Jaipur', 26.948689, 75.888487]]
[ ]
import folium
device_map = folium.Map(location = [22.660325, 88.388361])
fg = folium.FeatureGroup(name ='null_id_locations')

for i in null_device_locations:
    fg.add_child(folium.Marker(location = [i[1], i[2]], popup=i[0],
                              icon = folium.Icon(color = 'red')))

device_map.add_child(fg)

[ ]
#Device locations of the events data
fig, ax = plt.subplots(figsize = (12, 8))
sns.scatterplot(y = events_data['latitude'], x= events_data['longitude'])
plt.ylabel('LATITUDE')
plt.xlabel('LONGITUDE')
plt.xticks(ticks = np.arange(0, 100,5))
plt.yticks(ticks = np.arange(0,60,5))

plt.show()

[ ]
#Creating a separate data for Anomaly Locations
data_anomaly = events_data[events_data['longitude'] < 69.5]
data_anomaly['longitude'].unique(),data_anomaly['latitude'].unique()
(array([69.2075, 12.5674, 12.567 , 55.2708]),
 array([34.5553, 41.8719, 25.2048]))
[ ]
anomaly_locations = data_anomaly[['city','latitude', 'longitude']].values.tolist()
[ ]
#Anomaly Locations
anomaly_locations
[['Chennai', 34.5553, 69.2075],
 ['Jaipur', 41.8719, 12.5674],
 ['Chennai', 41.8719, 12.5674],
 ['Delhi', 41.8719, 12.5674],
 ['Pune', 41.8719, 12.567],
 ['Chennai', 41.8719, 12.5674],
 ['Chennai', 41.8719, 12.5674],
 ['Masaurhi', 34.5553, 69.2075],
 ['Pune', 34.5553, 69.2075],
 ['Masaurhi', 34.5553, 69.2075],
 ['Chennai', 25.2048, 55.2708],
 ['Indore', 34.5553, 69.2075],
 ['Indore', 25.2048, 55.2708],
 ['Visakhapatnam', 41.8719, 12.5674],
 ['Ilkal', 25.2048, 55.2708],
 ['Pune', 41.8719, 12.567],
 ['Pune', 34.5553, 69.2075],
 ['Pune', 34.5553, 69.2075],
 ['Indore', 41.8719, 12.5674],
 ['Chennai', 34.5553, 69.2075],
 ['Delhi', 41.8719, 12.5674],
 ['Indore', 34.5553, 69.2075],
 ['Delhi', 34.5553, 69.2075],
 ['Pune', 25.2048, 55.2708],
 ['Purnia', 41.8719, 12.5674],
 ['Indore', 41.8719, 12.5674],
 ['Purnia', 41.8719, 12.5674],
 ['Indore', 25.2048, 55.2708],
 ['Pune', 41.8719, 12.567],
 ['Jaipur', 41.8719, 12.5674],
 ['Visakhapatnam', 41.8719, 12.5674],
 ['Jaipur', 34.5553, 69.2075],
 ['Delhi', 25.2048, 55.2708],
 ['Pune', 25.2048, 55.2708],
 ['Visakhapatnam', 34.5553, 69.2075],
 ['Indore', 25.2048, 55.2708],
 ['Indore', 34.5553, 69.2075],
 ['Visakhapatnam', 25.2048, 55.2708],
 ['Jaipur', 41.8719, 12.5674],
 ['Indore', 41.8719, 12.5674],
 ['Delhi', 34.5553, 69.2075],
 ['Delhi', 34.5553, 69.2075],
 ['Bari', 25.2048, 55.2708],
 ['Visakhapatnam', 34.5553, 69.2075],
 ['Delhi', 41.8719, 12.5674],
 ['Visakhapatnam', 41.8719, 12.5674],
 ['Jaipur', 34.5553, 69.2075],
 ['Chennai', 25.2048, 55.2708],
 ['Purnia', 41.8719, 12.5674],
 ['Chennai', 34.5553, 69.2075],
 ['Ilkal', 25.2048, 55.2708],
 ['Visakhapatnam', 25.2048, 55.2708],
 ['Masaurhi', 34.5553, 69.2075],
 ['Delhi', 25.2048, 55.2708],
 ['Ilkal', 25.2048, 55.2708],
 ['Jaipur', 34.5553, 69.2075],
 ['Chennai', 25.2048, 55.2708],
 ['Visakhapatnam', 25.2048, 55.2708],
 ['Bari', 25.2048, 55.2708],
 ['Delhi', 25.2048, 55.2708],
 ['Visakhapatnam', 34.5553, 69.2075],
 ['Pune', 25.2048, 55.2708],
 ['Bari', 25.2048, 55.2708]]
[ ]
#Plotting Anomaly Device Locations on Map
import folium
device_map = folium.Map(location = [22.660325, 88.388361])
fg = folium.FeatureGroup(name ='anomaly_id_locations')

for i in anomaly_locations:
    fg.add_child(folium.Marker(location = [i[1], i[2]], popup=i[0],
                              icon = folium.Icon(color = 'blue')))

device_map.add_child(fg)

[ ]
events_data.isna().sum()
event_id       0
device_id    348
timestamp      0
longitude    423
latitude     423
city           0
state          0
dtype: int64
Using Simple Imputation to fill null values of device id, Longitude and Latitude
[ ]
events_data_Null_device_id = pd.isnull(events_data["device_id"])
events_data[events_data_Null_device_id]
events_data[events_data_Null_device_id].groupby(['city','state']).count()

[ ]
events_data_Null_latlong = pd.isnull(events_data["latitude"])
events_data[events_data_Null_latlong]
events_data[events_data_Null_latlong].groupby(['city','state']).count()

[ ]
from sklearn.impute import KNNImputer
events_data_impute=events_data.filter(['event_id','device_id','longitude','latitude'],axis=1)
[ ]
from sklearn.impute import SimpleImputer
imr = SimpleImputer(missing_values=np.nan, strategy='most_frequent')
imr = imr.fit(events_data_impute.values)
imputed_data = imr.transform(events_data_impute.values)
print (imputed_data)
[[2.76536800e+06 2.97334779e+18 7.72256760e+01 2.87301400e+01]
 [2.95506600e+06 4.73422136e+18 8.83883610e+01 2.26603250e+01]
 [6.05968000e+05 3.26449965e+18 7.72568090e+01 2.87579060e+01]
 ...
 [1.31622700e+06 6.40604027e+18 7.72355780e+01 2.87640650e+01]
 [3.81262000e+05 2.92074111e+18 8.33260440e+01 1.77654880e+01]
 [5.22592000e+05 3.21275047e+18 7.73085330e+01 9.77991800e+00]]
[ ]
events_data_df= pd.DataFrame(data=imputed_data, columns=["event_id", "device_id","longitude","latitude"])
events_data_df

[ ]
events_data_df.isna().sum()
event_id     0
device_id    0
longitude    0
latitude     0
dtype: int64
[ ]
events_data_imputed=pd.merge(events_data,events_data_df,on=['event_id'])
events_data_imputed

[ ]
events_data_imputed.drop(['device_id_x', 'latitude_x', 'longitude_x'], axis=1, inplace=True)
[ ]
events_data_imputed.rename(columns={'device_id_y': 'device_id', 'longitude_y': 'longitude', 'latitude_y': 'latitude'}, inplace=True)
[ ]
events_data_imputed.head()

[ ]
events_data_imputed.isna().sum()
event_id     0
timestamp    0
city         0
state        0
device_id    0
longitude    0
latitude     0
dtype: int64
[ ]
events_data_final = events_data_imputed
[ ]
events_data_final.isna().sum()
event_id     0
timestamp    0
city         0
state        0
device_id    0
longitude    0
latitude     0
dtype: int64
Segregate the Unique Device id records from event sample
[ ]
unique_device_id = events_data_final.drop_duplicates(['device_id'])[['device_id']]
unique_device_id

[ ]
delhi_df = events_data_final[events_data_final['state'] == 'Delhi']
[ ]
unique_device_id_delhi = delhi_df.drop_duplicates(['device_id', 'state'])[['device_id', 'state']]
unique_device_id_delhi

[ ]
events_data_final['device_id'].nunique()
60865
[ ]
unique_device_id_delhi['device_id'].nunique()
4910
[ ]
delhi_df = events_data_final[events_data_final['state'] == 'Delhi']
delhi_df

Detecting the Outlier from Age_Gender Data
[ ]
plt.figure(figsize=(10,5))
sns.boxplot(x='age',data=gender_age_train)

[ ]
plt.figure(figsize=(10,5))
sns.boxplot(y='gender',x='age',data=gender_age_train)

[ ]
gender_age_train['age'].values
array([35, 35, 35, ..., 20, 37, 25], dtype=int64)
[ ]
plt.figure(figsize=(10,5))
plt.subplot(1,2,1)
sns.distplot(gender_age_train['age'])
plt.subplot(1,2,2)
sns.boxplot(gender_age_train['age'])
plt.show()

[ ]
percentile25 = gender_age_train['age'].quantile(0.25)
percentile75 = gender_age_train['age'].quantile(0.75)
[ ]
percentile75 - percentile25
11.0
[ ]
upper_limit = percentile75 + 1.5 * 11
lower_limit = percentile25 - 1.5 * 11
[ ]
upper_limit
52.5
[ ]
lower_limit
8.5
[ ]
gender_age_train[gender_age_train['age'] > upper_limit]
gender_age_train[gender_age_train['age'] < lower_limit]

[ ]
new_outlier_df = gender_age_train[gender_age_train['age'] < upper_limit]
new_outlier_df.shape
(71490, 4)
[ ]
gender_age_train['age'].describe()
count    74645.000000
mean        31.410342
std          9.868735
min          1.000000
25%         25.000000
50%         29.000000
75%         36.000000
max         96.000000
Name: age, dtype: float64
[ ]
plt.figure(figsize=(10,5))
plt.subplot(2,2,1)
sns.distplot(gender_age_train['age'])
plt.subplot(2,2,2)
sns.boxplot(gender_age_train['age'])
plt.subplot(2,2,3)
sns.distplot(new_outlier_df['age'])
plt.subplot(2,2,4)
sns.boxplot(new_outlier_df['age'])
plt.show()

[ ]
gender_age_train_final = gender_age_train.copy()
gender_age_train_final['age'] = np.where(
    gender_age_train_final['age'] > upper_limit,
    upper_limit,
    np.where(
        gender_age_train_final['age'] < lower_limit,
        lower_limit,
        gender_age_train_final['age']
    )
)
[ ]
plt.figure(figsize=(10,5))
plt.subplot(2,2,1)
sns.distplot(gender_age_train['age'])
plt.subplot(2,2,2)
sns.boxplot(gender_age_train['age'])
plt.subplot(2,2,3)
sns.distplot(gender_age_train_final['age'])
plt.subplot(2,2,4)
sns.boxplot(gender_age_train_final['age'])
plt.show()

[ ]
gender_age_train_final.head()

Translating the Chinese language to English in Phone_Brand_Device_Model Data
[ ]
phone_brand_device_model['phone_brand'] = phone_brand_device_model['phone_brand'].replace(to_replace = ['vivo', '小米', 'OPPO',
        '三星', '酷派', '联想 ', '华为', '奇酷', '魅族', '斐讯',
       '中国移动', 'HTC', '天语', '至尊宝', 'LG', '欧博信', '优米', 'ZUK', '努比亚', '惠普',
       '尼比鲁', '美图', '乡米', '摩托罗拉', '梦米', '锤子', '富可视', '乐视', '海信', '百立丰',
       '一加', '语信', '海尔', '酷比', '纽曼', '波导', '朵唯', '聆韵', 'TCL', '酷珀', '爱派尔',
       'LOGO', '青葱', '果米', '华硕', '昂达', '艾优尼', '康佳', '优购', '邦华', '赛博宇华',
       '黑米', 'Lovme', '先锋', 'E派', '神舟', '诺基亚', '普耐尔', '糖葫芦', '亿通', '欧新',
       '米奇', '酷比魔方', '蓝魔', '小杨树', '贝尔丰', '糯米', '米歌', 'E人E本', '西米', '大Q',
       '台电', '飞利浦', '唯米', '大显', '长虹', '维图', '青橙', '本为', '虾米', '夏新', '帷幄',
       '百加', 'SUGAR', '欧奇', '世纪星', '智镁', '欧比', '基伍', '飞秒', '德赛', '易派',
       '谷歌', '金星数码', '广信', '诺亚信', 'MIL', '白米', '大可乐', '宝捷讯', '优语', '首云',
       '瑞米', '瑞高', '沃普丰', '摩乐', '鲜米', '凯利通', '唯比', '欧沃', '丰米', '恒宇丰',
       '奥克斯', '西门子', '欧乐迪', 'PPTV'],
        value= ['vivo', 'Xiaomi', 'OPPO', 'Samsung', 'Coolpad', 'Lenovo', 'Huawei', 'Qiku', 'Meizu', 'Phixun',
       'China Mobile', 'HTC', 'Tianyu', 'Supreme treasure', 'LG', 'Oboxin', 'Youmi', 'ZUK', 'Nubia', 'HP',
       'Nibiru', 'Meitu', 'Xiangmi', 'Motorola', 'Mengmi', 'Hammer', 'InFocus', 'LeTV', 'Hisense', 'Bailifeng',
       'OnePlus', 'Yuxin', 'Haier', 'Cooby', 'Newman', 'Waveguide', 'Duowei', 'Ling Yun', 'TCL', 'Cooper', 'Aipel' ,
       'LOGO', 'Scallion', 'Guomi', 'Asus', 'Onda', 'Aiuni', 'Konka', 'Yougo', 'Banghua', 'Cyber Yuhua',
       'Black Rice', 'Lovme', 'Pioneer', 'E Pie', 'Shenzhou', 'Nokia', 'Pure', 'Candied Hulu', 'Yitong', 'Ouxin',
       'Mickey', 'Cool Doo Cube', 'Blue Devil', 'Little Poplar', 'Belfeng', 'Glutinous Rice', 'Mi Song', 'Eren Eben', 'Simi', 'Big Q' ,
       'Taipower', 'Philips', 'Weimi', 'Daxian', 'Changhong', 'Vitu', 'Qingcheng', 'Original', 'Shrimp', 'Xiaxin', 'Huang',
       'Baika', 'SUGAR', 'Okey', 'Century Star', 'Chi-Mag', 'Obi', 'Kivu', 'Femtosecond', 'Desai', 'Epai',
       'Google', 'Venus Digital', 'Guangxin', 'Noahs letter', 'MIL', 'White Rice', 'Big Cola', 'Baojixun', 'Youyu', 'Shouyun',
       'Rimi', 'Rigao', 'Wopfeng', 'Mole', 'Xianmi', 'Kellytong', 'Vip', 'Owo', 'Fengmi', 'Hengyufeng',
       'Ox', 'Siemens', 'Oraldi', 'PPTV'])
[ ]
#Phone Brand
fig, ax = plt.subplots(figsize = (12,6))

sns.barplot(x =phone_brand_device_model['phone_brand'].value_counts().keys()[:50], 
            y=phone_brand_device_model['phone_brand'].value_counts()[:50],
            color='b')
plt.xlabel('Brand Name')
plt.ylabel('Counts')
plt.title('Phone Brands')
plt.xticks(rotation = 90)
plt.show()

[ ]
phone_brand_device_model['device_model'] = phone_brand_device_model['device_model'].replace(to_replace = ['红米Note2','红米Note3','大神F1Plus','note顶配版','星星1号','红米note', '青春版','荣耀4A','魅蓝Note 2',
                        '荣耀7i','荣耀畅玩4C','红米2A','荣耀畅玩5','荣耀7',  '红米1S','麦芒4','荣耀6','荣耀畅玩4X','荣耀3X畅玩版','荣耀X2',
                        '魅蓝NOTE','荣耀6 Plus','荣耀+','荣耀畅玩4','黄金斗士A8','小米note','荣耀3C','小米4C','红米note增强版','天鉴W900', '荣耀畅玩5X',
                        '红米2','大神F1','魅蓝2','Mate 7 青春版','乐檬K3 Note','火星一号','乐檬K3', '联想黄金斗士S8','大神F2','魅蓝','荣耀6 plus','大神F2全高清版',
                        '灵感XL','旗舰版','坚果手机','红米', '超级手机1 Pro','畅享5','魅蓝metal','超级手机1','超级手机1s','荣耀U8860','纽扣','荣耀畅玩平板T1', '红辣椒 X1',
                        '春雷HD','ivvi 小i','荣耀畅玩4C运动版','麦芒3','小鲜2','大器2', '锋尚','大神X7','小苹果','乐玩','大神Note3','么么哒3N','锋尚Pro','红辣椒','荣耀3X',
                        '三星big foot','荣耀3C畅玩版','ivvi 小骨Pro', '红辣椒XM', '远航3', 'My 布拉格', 'metal 标准版', '畅享5S', '红辣椒Note', '锋尚Max', '2016版 Galaxy', '红牛V5', 
                        '2016版 Galaxy', '锋尚2','金钢','野火S','2016版 Galaxy','红米3','小辣椒 X3','荣耀平板T1-','大神Note','7295A青春版','大Q Note','么么哒',
                        '小辣椒 M2','黄金斗士Note8','炫影S+','风华3','TALK 7X四核','2016版 Galaxy', '天鉴T1','土星一号', '麦芒3S','飞马','联想VIBE X2', '威武3', '红辣椒任性版',
                        '小辣椒 9', '大观4', '超级手机Max','星星2号','雷霆战机','威武3C', '倾城L3','Z9 mini 精英版','小辣椒S1', '小辣椒 5','T03锋至版','乐玩2C','么么哒3S','乐K31',
 '黄金斗士S8畅玩版','时尚手机'],
                            value= ['Redmi Note2', 'Redmi Note3', 'Great God F1Plus', 'note top version', 'Star 1', 'Redmi note',  'Youth Edition','Honor 4A','Meizu Note 2','Honor 7i','Honor Play 4C','Redmi 2A','Honor Play 5','Honor 7', 'Redmi 1S', 'Maimang 4', 'Honor 6', 'Honor Play 4X', 'Honor 3X Play Edition', 'Honor X2', 'Mei Lan NOTE', 'Honor 6 Plus',
                        'Honor+', 'Honor Play 4', 'Golden Fighter A8', 'Xiaomi Note', 'Honor 3C', 'Xiaomi 4C', 'Redmi Note Enhanced Edition', 'Tianjian W900',
                        'Honor Play 5X','Red Rice 2','Great God F1','Meizu 2','Mate 7 Youth Edition','Lemeng K3 Note','Mars One','Lemeng K3',
                        'Lenovo Gold Fighter S8', 'Dashen F2', 'Charm Blue', 'Honor 6 plus', 'Dashen F2 Full HD Version', 'Inspiration XL', 'Ultimate Edition', 'Nut Phone', 'Red Rice' ,
                        'Super Phone 1 Pro', 'Enjoy 5', 'Meizu Metal', 'Super Phone 1', 'Super Phone 1s', 'Honor U8860', 'Button', 'Honor Play Tablet T1', 'Red Pepper X1', 'Chunlei HD', 'ivvi Xiaoi', 'Honor Play 4C Sports Edition', 'Maimang 3', 'Xiaoxian 2','Big 2','Feng Shang','Great God X7','Little Apple','Have fun','Great God Note3','Momoda 3N','Fengshang Pro','Red chilli','Honor 3X','Samsung big foot','Honor 3C Play Edition','ivvi Osicles Pro', 'Red Pepper XM','voyage 3','My Prague','metal Standard Edition','Enjoy 5S','Red Pepper Note','Feng Shang Max','2016 Edition Galaxy','Red Bull V5','2016 Edition Galaxy','Feng Shang 2','Golden Steel','Wildfire S','2016 Edition Galaxy','Red Rice 3','Pepper X3','Honor Tablet T1-','Great God Note','7295A Youth Edition','Big Q Note','mwah','Pepper M2','Golden Warrior Note8', 'Hyunying S+','Fenghua 3','TALK 7X Quad Core','2016 version of Galaxy','Tianjian T1','Saturn One','Maimang 3S','Pegasus','Lenovo VIBE X2','Mighty 3','Red pepper capricious version','Pepper 9','Grand View 4','Super Phone Max','Star 2','Thunder Fighter','Mighty 3C','Allure L3','Z9 mini Elite Edition','Chili S1','Pepper 5','T03 Frontier Edition','Fun 2C','Momada 3S','Lemeng K31','Golden Fighter S8 Play Edition','Fashionable phone'])

[ ]
# Device Model
fig, ax = plt.subplots(figsize = (12,6))

sns.barplot(x =phone_brand_device_model['device_model'].value_counts().keys()[:50], 
            y=phone_brand_device_model['device_model'].value_counts()[:50],
            color = 'b')
plt.xlabel('Model Name')
plt.ylabel('Counts')
plt.title('Model')
plt.xticks(rotation = 90)
plt.show()

[ ]
phone_brand_device_model.head(20)

Merging all 3 data set
[ ]
df1 = pd.merge(events_data_final, gender_age_train_final, how='inner', left_index=True, right_index=True)
df1

[ ]
df1.isnull().sum()
event_id       0
timestamp      0
city           0
state          0
device_id_x    0
longitude      0
latitude       0
device_id_y    0
gender         0
age            0
group          0
dtype: int64
[ ]
df2 = pd.merge(df1, phone_brand_device_model, how='inner',left_index=True, right_index=True)
df2

[ ]
df2.isnull().sum()
event_id        0
timestamp       0
city            0
state           0
device_id_x     0
longitude       0
latitude        0
device_id_y     0
gender          0
age             0
group           0
device_id       0
phone_brand     0
device_model    0
dtype: int64
[ ]
df2.columns
Index(['event_id', 'timestamp', 'city', 'state', 'device_id_x', 'longitude',
       'latitude', 'device_id_y', 'gender', 'age', 'group', 'device_id',
       'phone_brand', 'device_model'],
      dtype='object')
[ ]
cols_to_keep = ['event_id', 'device_id','timestamp', 'city', 'state', 'longitude',
       'latitude', 'gender', 'age', 'group','phone_brand', 'device_model']
df_final_data = df2[cols_to_keep]
df_final_data

[ ]
df_final_data['device_id'].nunique()
74645
[ ]
df_delhi_final = df_final_data[df_final_data['state']=='Delhi']
[ ]
df_delhi_final['device_id'].nunique()
17078

7. Data Post-Profiling
[ ]
#post_profile_Event = ProfileReport(events_data_final)
#post_profile_Event.to_file(output_file='Post Profiling Report_Event.html')
#post_profile_Event
#print('Accomplished!')
[ ]
#post_profile_Age_Gender = ProfileReport(gender_age_train_final)
#post_profile_Age_Gender.to_file(output_file='Post Profiling Report_Age_Gender.html')
#post_profile_Age_Gender
#print('Accomplished!')
[ ]
#post_profile_Phone_Brand = ProfileReport(phone_brand_device_model)
#post_profile_Phone_Brand.to_file(output_file='Post Profiling profile_Phone_Brand.html')
#post_profile_Phone_Brand
#print('Accomplished!')

8. Exploratory Data Analysis
Do an Analysis of the preprocessed data.

Here are the points about how Analysis should be done:

Distribution of Users(device_id) across States.
Distribution of Users across Phone Brands(Consider only 10 Most used Phone Brands).
Distribution of Users across Gender.
Distribution of Users across Age Segments.
Distribution of Phone Brands(Consider only 10 Most used Phone Brands) for each Age Segment, State, Gender.
Distribution of Gender for each State, Age Segment and Phone Brand(Consider only 10 Most used Phone Brands).
Distribution of Age Segments for each State, Gender and Phone Brand(Consider only 10 Most used Phone Brands).
Note: While doing analysis for the above points, consider only one instance of a particular User(device_id) as a User can do multiple phone calls and considering every instance of the same User can give misleading numbers.

Hourly distribution of Phone Calls.
Plot the Users on the Map using any suitable package.
1. Distribution of Users(device_id) across States.
[ ]
↳ 3 cells hidden
2. Distribution of Users across Phone Brands(Consider only 10 Most used Phone Brands).
[ ]
↳ 1 cell hidden
2. Distribution of Users across Phone Brands(Consider only 10 Most used Phone Brands)for delhi state
[ ]
plt.figure(figsize = (15,5))
plt.title("10 Most Used Device Model", size = 20, fontfamily = 'Arial')
plt.xlabel("", size = 15, fontfamily = 'Arial')
plt.ylabel("User", size = 15, fontfamily = 'Arial')
plt.yticks(ha = 'right', size = 12, fontfamily = 'Arial')
sns.barplot(x=df_delhi_final['device_model'].value_counts()[:10].index, y= df_delhi_final['device_model'].value_counts()[:10])

[ ]

[ ]
df_final_data['phone_brand'].value_counts()[0:10]
Xiaomi     18357
Samsung    16424
Huawei     12990
vivo        6475
OPPO        5650
Meizu       4686
Coolpad     3359
HTC         1089
Lenovo       849
LeTV         698
Name: phone_brand, dtype: int64
The top 10 most used brands are Xiaomi with 18357 users, Samsung with 16424, Huawei with 12990, vivo with 6475, OPPO with 5650, Meizu with 4686, Coolpad with 3359, HTC with 1089, Lenovo with 849, LeTV wtih 698 users.

3. Distribution of users across gender
[ ]
↳ 2 cells hidden
3.1 Distribution of users across gender for delhi state
[ ]
plt.figure(figsize=(10, 7))
plt.title("Distribution of users across gender ", size = 20, fontfamily = 'Arial')
sns.countplot(df_delhi_final['gender'],
                      data = df_delhi_final, palette = "dark")

[ ]
df_delhi_final['gender'].value_counts()
M    10985
F     6093
Name: gender, dtype: int64
[ ]
plt.figure(figsize=(10, 7))
plt.pie(x = df_delhi_final['gender'].value_counts(), autopct = '%.1f%%', 
             labels = df_delhi_final['gender'].value_counts().index)

[ ]
df_delhi_final['age'].min()
8.5
[ ]
df_delhi_final['age'].max()
52.5
We observered that male users are more as compared to female in the data set. Mare user are accounted 64% whereas 36% users are female.

4. Distribution of Users across Age Segments
[ ]
↳ 5 cells hidden
4.1 Distribution of Users across Age Segments for delhi state
[ ]
fig = plt.figure(figsize=(15, 8))
plt.title("Distribution of Users across age segments", size = 20, fontfamily = 'Arial')
sns.countplot(x = df_delhi_final['group'],
                      data = df_delhi_final, palette = "deep")

[ ]
df_delhi_final.groupby(['age'])['device_id'].count().sort_values(ascending=False).plot.bar(figsize=(16,6))

[ ]
# Age distribution for each gender
plt.figure(figsize=(15,5))
sns.set(style="whitegrid")
df_plot = df_delhi_final.drop_duplicates(subset = ["device_id"]).sort_values(by='device_id')

plt.title("Age distribution for each gender", size = 20, fontfamily = 'Arial')
Male_user = df_delhi_final[df_delhi_final['gender'] == 'M'].copy()
Male_user = Male_user['age']
Female_user = df_delhi_final[df_delhi_final['gender'] == 'F'].copy()
Female_user = Female_user['age']

plt.hist(Male_user, label='Male_user')
plt.hist(Female_user, label='Female_user')
plt.legend()

[ ]
import numpy as np

###pie chart for group

df_delhi_final['group'].value_counts()
group_count=df_delhi_final['group'].value_counts().reset_index().sort_values(by='index')
group_count.columns=['group','Count']

g_count=df_delhi_final['gender'].value_counts().reset_index().sort_values(by='index')
g_count.columns=['gender','Count']


# Create a trace
tag = (np.array(group_count.group))
sizes = (np.array((group_count['Count'] / group_count['Count'].sum())))
g_sizes = (np.array((g_count['Count'] / g_count['Count'].sum())))
g_tag = (np.array(g_count.gender))

grp_labels = zip(tag, sizes)
g_labels = zip(g_tag, g_sizes)


fig, ax = plt.subplots(figsize=(15,9))

size = 0.3
cmap = plt.get_cmap("tab20c")
outer_colors = cmap(np.arange(3)*4)
inner_colors = cmap(np.array([1, 2, 3,4,5, 6,7,8, 9, 10,11,12]))


ax.pie(g_sizes, radius=1, colors=outer_colors,autopct='%.2f%%',#labels=g_tag,
       wedgeprops=dict(width=size, edgecolor='w'))

ax.pie(sizes, radius=1-size, colors=inner_colors,#autopct='%3.1f%%',#labels=tag,
       wedgeprops=dict(width=size, edgecolor='w'))


ax.set(aspect="equal", title='Pie plot with `ax.pie`')
plt.title("Distribution of usage across Age Groups", size = 20, fontfamily = 'Arial')
#plt.legend(loc='right', prop=font_prop, numpoints=1)

plt.legend(
    loc='upper left',
    prop={'size': 12},
    labels=['%s, %1.1f%%' % (
        l, (float(s)) * 100) for l, s in grp_labels],
    bbox_to_anchor=(0.0, 1),
    bbox_transform=fig.transFigure
)


#plt.legend(loc='center', prop=font_prop, numpoints=1)
plt.show()

5. Distribution of Phone Brands(Consider only 10 Most used Phone Brands) for each Age Segment, State, Gender.
[ ]
↳ 7 cells hidden
5.1 Distribution of Phone Brands(Consider only 10 Most used Phone Brands) for each Age Segment, State, Gender.for delhi state
Distribution of 10 most used phone brand across Age Segment

[ ]
plt.figure(figsize=(15,5))
sns.countplot("group", hue="phone_brand", hue_order=df_delhi_final.phone_brand.value_counts().iloc[:10].index, order=df_delhi_final.group.value_counts().iloc[:].index, data=df_final_data)
plt.title("Distribution of 10 most used phone brand across Age Segment", size = 20, fontfamily = 'Arial')
plt.show()

Distribution of 10 most used phone brand across State

[ ]
plt.figure(figsize=(15,5))
sns.countplot("state", hue="phone_brand", hue_order=df_delhi_final.phone_brand.value_counts().iloc[:10].index, order=df_delhi_final.state.value_counts().iloc[:10].index, data=df_final_data)
plt.title("Distribution of 10 most used phone brand across State", size = 20, fontfamily = 'Arial')
plt.show()

Distribution of 10 most used phone brand across Gender for delhi state

[ ]
plt.figure(figsize=(15,5))
sns.countplot("gender", hue="phone_brand", hue_order=df_delhi_final.phone_brand.value_counts().iloc[:10].index, order=df_delhi_final.gender.value_counts().iloc[:].index, data=df_final_data)
plt.title("Distribution of 10 most used phone brand across Gender", size = 20, fontfamily = 'Arial')
plt.show()

6-Distribution of Gender for each State, Age Segment and Phone Brand(Consider only 10 Most used Phone Brands)?
[ ]
↳ 7 cells hidden
7. Distribution of Age Segments for each State, Gender and Phone Brand(Consider only 10 Most used Phone Brands).
[ ]
↳ 7 cells hidden
8. Hourly distribution of Phone Calls.
[ ]
↳ 6 cells hidden
9. Plot the Users on the Map using any suitable package.
[ ]
↳ 2 cells hidden
Showing Age Distribution using a Box plot
[ ]
↳ 5 cells hidden

9. Summarization
Observation:
From the given data set the below observation can be drawn:

Delhi has the maximum number of users with the count of 17078 followed by Maharashtra with 15509 and TamilNadu with 10137.

The top 10 most used brands are Xiaomi with 18357 users, Samsung with 16424, Huawei with 12990, vivo with 6475, OPPO with 5650, Meizu with 4686, Coolpad with 3359, HTC with 1089, Lenovo with 849, LeTV wtih 698 users.

Xiaomi is the most used phone brands across users.

Male users are more as compared to female in the data set. Male user are accounted 64% whereas 36% users are female.

The users whose age segment falls under 23-26, 32-38 are maximum. The usage of phone is more for the users age between 23 - 38.

Usage of phone brand for Male users with Age segment 23-26 are more against other age segments.

Usage of phone brand are Higher in Delhi followed by Maharashtra and TamilNadu.

Usage of phone brand are higher in Male users as compared to female users.

Male users are more than Female in across all the states.

Male users with Age group 23-26 and 32-38 records for the maximum counts. In Female Age group 33-42 records the maximum counts.

Xiaomi is the most favourite brand across gender followed by Samsung and Huawei.

In Delhi the Male users are with Age group 23-26 are more wheras in Maharashtra Male user with Age group 32-38 are more.

Male users with Age group 23-26 records for the maximum counts. In Female Age group 33-42 records the maximum counts.

Xiaomi is the most favourite brand for age group of Male 23-32. Vivo is the most favourite brand of Male age group 32-38. HTC is most used by the Male user with age is 39+.

We observed that at 10 am users are more active making a phone calls. Also, we observed at 20th, 21st and 22nd hours users are active making a calls.Male users are making more calls.

The users are more active in Delhi followed by Maharashtra and TamilNadu.

Conclusion:
INSAID Telecom needs to perform several activities to become a Market leader.

Introduced different plans for different states across different age group.
Provide some discounts to scale more productivity during non productive hours.
Must be special plans for the states where the usage are low.
To increase the usage in Delhi, provide them some privilege offers as Delhi is the most potential market for Business growth.
Tie up with the top Phone brands of the markets based on their usage for more productivity.
Special plans must be there for age group between 23-38 as they are the highest usage consumers.
Provide some kind of discount vouchers to Female users to scale their phone usage as it has the huge potential to scale the Business.
THANK YOU
[ ]

error
0s
completed at 10:16 PM

