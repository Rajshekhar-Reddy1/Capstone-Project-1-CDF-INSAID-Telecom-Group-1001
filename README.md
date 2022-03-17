https://www.google.com/imgres?imgurl=https%3A%2F%2Fwww.xingular.in%2Fimages%2Fpostimage.jpg&imgrefurl=https%3A%2F%2Fwww.xingular.in%2Fabout.html&tbnid=86sBZxqlxA4tZM&vet=12ahUKEwjNwu21x832AhXyktgFHVbwCDAQMygAegUIARCIAQ..i&docid=cVWqSTGlI63IFM&w=628&h=250&itg=1&q=Telecom%20project&hl=en&ved=2ahUKEwjNwu21x832AhXyktgFHVbwCDAQMygAegUIARCIAQ

# Capstone-Project-1-CDF-INSAID-Telecom-Group-1001
Capstone Project 1 (CDF) INSAID Telecom Group 1001
Loading...
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
↳ 15 cells hidden
Phone_Brand_Device_Model Data details
[ ]
↳ 10 cells hidden

5. Data Pre-Profiling
[ ]
↳ 3 cells hidden

6. Data Pre-Processing
[ ]
↳ 94 cells hidden

7. Data Post-Profiling
[ ]
↳ 3 cells hidden

8. Exploratory Data Analysis
[ ]
↳ 7 cells hidden
2. Distribution of Users across Phone Brands(Consider only 10 Most used Phone Brands)for delhi state
[ ]
↳ 7 cells hidden
3.1 Distribution of users across gender for delhi state
[ ]
↳ 12 cells hidden
4.1 Distribution of Users across Age Segments for delhi state
[ ]
↳ 12 cells hidden
5.1 Distribution of Phone Brands(Consider only 10 Most used Phone Brands) for each Age Segment, State, Gender.for delhi state
[ ]
↳ 38 cells hidden

9. Summarization
↳ 2 cells hidden
THANK YOU
[ ]
↳ 1 cell hidden
