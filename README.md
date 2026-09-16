# Nasa-planetary-mystery-analysis
#The purpose of this python data-analysis project is to use planetary weather observations to investigate temperature, atmospheric pressure, seasonal patterns, and data quality in order to identify an unknown planet.

# import pandas and plotly express libraries
import pandas as pd
import plotly.express as px

# load planet_weather.csv data from datasets folder
df = pd.read_csv("datasets/planet_weather.csv")

# preview the data
df.head()

id	terrestrial_date	sol	ls	month	min_temp	max_temp	pressure	wind_speed	atmo_opacity
0	1895	2018-02-27	1977	135	Month 5	-77.0	-10.0	727.0	NaN	Sunny
1	1893	2018-02-26	1976	135	Month 5	-77.0	-10.0	728.0	NaN	Sunny
2	1894	2018-02-25	1975	134	Month 5	-76.0	-16.0	729.0	NaN	Sunny
3	1892	2018-02-24	1974	134	Month 5	-77.0	-13.0	729.0	NaN	Sunny
4	1889	2018-02-23	1973	133	Month 5	-78.0	-18.0	730.0	NaN	Sunny

# Number of rows and columns in the dataset?
df.shape

(1894, 10)

# let's Find the column names
df.columns

Index(['id', 'terrestrial_date', 'sol', 'ls', 'month', 'min_temp', 'max_temp',
       'pressure', 'wind_speed', 'atmo_opacity'],
      dtype='object')

# data type of each column?
df.info()

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1894 entries, 0 to 1893
Data columns (total 10 columns):
 #   Column            Non-Null Count  Dtype  
---  ------            --------------  -----  
 0   id                1894 non-null   int64  
 1   terrestrial_date  1894 non-null   object 
 2   sol               1894 non-null   int64  
 3   ls                1894 non-null   int64  
 4   month             1894 non-null   object 
 5   min_temp          1867 non-null   float64
 6   max_temp          1867 non-null   float64
 7   pressure          1867 non-null   float64
 8   wind_speed        0 non-null      float64
 9   atmo_opacity      1894 non-null   object 
dtypes: float64(4), int64(3), object(3)
memory usage: 148.1+ KB


df.nunique()

id                  1894
terrestrial_date    1894
sol                 1894
ls                   360
month                 12
min_temp              29
max_temp              46
pressure             199
wind_speed             0
atmo_opacity           2
dtype: int64

# Provide a statistical summary of the DataFrame
df.describe()

id	sol	ls	min_temp	max_temp	pressure	wind_speed
count	1894.000000	1894.000000	1894.000000	1867.000000	1867.000000	1867.000000	0.0
mean	948.372228	1007.930306	169.180570	-76.121050	-12.510445	841.066417	NaN
std	547.088173	567.879561	105.738532	5.504098	10.699454	54.253226	NaN
min	1.000000	1.000000	0.000000	-90.000000	-35.000000	727.000000	NaN
25%	475.250000	532.250000	78.000000	-80.000000	-23.000000	800.000000	NaN
50%	948.500000	1016.500000	160.000000	-76.000000	-11.000000	853.000000	NaN
75%	1421.750000	1501.750000	259.000000	-72.000000	-3.000000	883.000000	NaN
max	1895.000000	1977.000000	359.000000	-62.000000	11.000000	925.000000	NaN

# Looks like the wind speed sensor on the Rover was broken
# Delete wind_speed column, which is filled with null values
df.drop(columns=["wind_speed"])

id	terrestrial_date	sol	ls	month	min_temp	max_temp	pressure	atmo_opacity
0	1895	2018-02-27	1977	135	Month 5	-77.0	-10.0	727.0	Sunny
1	1893	2018-02-26	1976	135	Month 5	-77.0	-10.0	728.0	Sunny
2	1894	2018-02-25	1975	134	Month 5	-76.0	-16.0	729.0	Sunny
3	1892	2018-02-24	1974	134	Month 5	-77.0	-13.0	729.0	Sunny
4	1889	2018-02-23	1973	133	Month 5	-78.0	-18.0	730.0	Sunny
...	...	...	...	...	...	...	...	...	...
1889	24	2012-08-18	12	156	Month 6	-76.0	-18.0	741.0	Sunny
1890	13	2012-08-17	11	156	Month 6	-76.0	-11.0	740.0	Sunny
1891	2	2012-08-16	10	155	Month 6	-75.0	-16.0	739.0	Sunny
1892	232	2012-08-15	9	155	Month 6	NaN	NaN	NaN	Sunny
1893	1	2012-08-07	1	150	Month 6	NaN	NaN	NaN	Sunny
1894 rows × 9 columns

# How many unique values are there in the atmo_opacity column?
df['atmo_opacity'].value_counts()

Sunny    1891
--          3
Name: atmo_opacity, dtype: int64

# The atmosphere sensors were faulty and did not capture accurate data
# Delete atmo_opacity column, which mostly contains identical values
df.drop(columns=["atmo_opacity"])

id	terrestrial_date	sol	ls	month	min_temp	max_temp	pressure	wind_speed
0	1895	2018-02-27	1977	135	Month 5	-77.0	-10.0	727.0	NaN
1	1893	2018-02-26	1976	135	Month 5	-77.0	-10.0	728.0	NaN
2	1894	2018-02-25	1975	134	Month 5	-76.0	-16.0	729.0	NaN
3	1892	2018-02-24	1974	134	Month 5	-77.0	-13.0	729.0	NaN
4	1889	2018-02-23	1973	133	Month 5	-78.0	-18.0	730.0	NaN
...	...	...	...	...	...	...	...	...	...
1889	24	2012-08-18	12	156	Month 6	-76.0	-18.0	741.0	NaN
1890	13	2012-08-17	11	156	Month 6	-76.0	-11.0	740.0	NaN
1891	2	2012-08-16	10	155	Month 6	-75.0	-16.0	739.0	NaN
1892	232	2012-08-15	9	155	Month 6	NaN	NaN	NaN	NaN
1893	1	2012-08-07	1	150	Month 6	NaN	NaN	NaN	NaN
1894 rows × 9 columns

# How many months are there on this planet?
df['month'].nunique()

12

# What is the average min_temp for each month?
avg_temp= df['min_temp'].mean()

-76.12104981253347

# Plot a bar chart of the average min_temp by month
px.histogram(df, x = 'min_temp', y = 'month', histfunc = 'avg')

# What is the average pressure for each month?
df['pressure'].mean()

841.0664167113016

# Create a bar chart of the average atmospheric pressure by month
px.histogram(df, x = 'pressure', y = 'month', histfunc = 'avg')

# Plot a line chart of the daily atmospheric pressure by terrestrial date

px.line(df,x='pressure',y='terrestrial_date')

# Plot a line chart the daily minimum temp

px.line(df,x='min_temp',y='terrestrial_date')






