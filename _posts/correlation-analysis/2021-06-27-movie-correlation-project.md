---
layout: post
title:  "Correlation Analysis in Python"
date:   2021-06-27 09:29:20 +0700
tags: [Python, GoogleColab, Pandas, seaborn, matplot]
---


# Import Libraries & Upload data


```python
import pandas as pd
import io
import numpy as np
import seaborn as sns

import matplotlib
import matplotlib.pyplot as plt
plt.style.use('ggplot')
from matplotlib.pyplot import figure

%matplotlib inline
matplotlib.rcParams['figure.figsize'] = (12,8)  # Adjusts the config of plots we will create
```

for local file : 


```python
#from google.colab import files
#uploaded = files.upload()

#df = pd.read_csv(io.BytesIO(uploaded['movies.csv']), encoding='latin-1')
```

For github csv file 


```python
df = pd.read_csv('https://raw.githubusercontent.com/saurabhgwari/Correlation-Analysis/main/movies.csv', encoding='latin-1')

df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>budget</th>
      <th>company</th>
      <th>country</th>
      <th>director</th>
      <th>genre</th>
      <th>gross</th>
      <th>name</th>
      <th>rating</th>
      <th>released</th>
      <th>runtime</th>
      <th>score</th>
      <th>star</th>
      <th>votes</th>
      <th>writer</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8000000.0</td>
      <td>Columbia Pictures Corporation</td>
      <td>USA</td>
      <td>Rob Reiner</td>
      <td>Adventure</td>
      <td>52287414.0</td>
      <td>Stand by Me</td>
      <td>R</td>
      <td>1986-08-22</td>
      <td>89</td>
      <td>8.1</td>
      <td>Wil Wheaton</td>
      <td>299174</td>
      <td>Stephen King</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6000000.0</td>
      <td>Paramount Pictures</td>
      <td>USA</td>
      <td>John Hughes</td>
      <td>Comedy</td>
      <td>70136369.0</td>
      <td>Ferris Bueller's Day Off</td>
      <td>PG-13</td>
      <td>1986-06-11</td>
      <td>103</td>
      <td>7.8</td>
      <td>Matthew Broderick</td>
      <td>264740</td>
      <td>John Hughes</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15000000.0</td>
      <td>Paramount Pictures</td>
      <td>USA</td>
      <td>Tony Scott</td>
      <td>Action</td>
      <td>179800601.0</td>
      <td>Top Gun</td>
      <td>PG</td>
      <td>1986-05-16</td>
      <td>110</td>
      <td>6.9</td>
      <td>Tom Cruise</td>
      <td>236909</td>
      <td>Jim Cash</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>3</th>
      <td>18500000.0</td>
      <td>Twentieth Century Fox Film Corporation</td>
      <td>USA</td>
      <td>James Cameron</td>
      <td>Action</td>
      <td>85160248.0</td>
      <td>Aliens</td>
      <td>R</td>
      <td>1986-07-18</td>
      <td>137</td>
      <td>8.4</td>
      <td>Sigourney Weaver</td>
      <td>540152</td>
      <td>James Cameron</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9000000.0</td>
      <td>Walt Disney Pictures</td>
      <td>USA</td>
      <td>Randal Kleiser</td>
      <td>Adventure</td>
      <td>18564613.0</td>
      <td>Flight of the Navigator</td>
      <td>PG</td>
      <td>1986-08-01</td>
      <td>90</td>
      <td>6.9</td>
      <td>Joey Cramer</td>
      <td>36636</td>
      <td>Mark H. Baker</td>
      <td>1986</td>
    </tr>
  </tbody>
</table>
</div>



# Cleaning & formating data


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>budget</th>
      <th>company</th>
      <th>country</th>
      <th>director</th>
      <th>genre</th>
      <th>gross</th>
      <th>name</th>
      <th>rating</th>
      <th>released</th>
      <th>runtime</th>
      <th>score</th>
      <th>star</th>
      <th>votes</th>
      <th>writer</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8000000.0</td>
      <td>Columbia Pictures Corporation</td>
      <td>USA</td>
      <td>Rob Reiner</td>
      <td>Adventure</td>
      <td>52287414.0</td>
      <td>Stand by Me</td>
      <td>R</td>
      <td>1986-08-22</td>
      <td>89</td>
      <td>8.1</td>
      <td>Wil Wheaton</td>
      <td>299174</td>
      <td>Stephen King</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6000000.0</td>
      <td>Paramount Pictures</td>
      <td>USA</td>
      <td>John Hughes</td>
      <td>Comedy</td>
      <td>70136369.0</td>
      <td>Ferris Bueller's Day Off</td>
      <td>PG-13</td>
      <td>1986-06-11</td>
      <td>103</td>
      <td>7.8</td>
      <td>Matthew Broderick</td>
      <td>264740</td>
      <td>John Hughes</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15000000.0</td>
      <td>Paramount Pictures</td>
      <td>USA</td>
      <td>Tony Scott</td>
      <td>Action</td>
      <td>179800601.0</td>
      <td>Top Gun</td>
      <td>PG</td>
      <td>1986-05-16</td>
      <td>110</td>
      <td>6.9</td>
      <td>Tom Cruise</td>
      <td>236909</td>
      <td>Jim Cash</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>3</th>
      <td>18500000.0</td>
      <td>Twentieth Century Fox Film Corporation</td>
      <td>USA</td>
      <td>James Cameron</td>
      <td>Action</td>
      <td>85160248.0</td>
      <td>Aliens</td>
      <td>R</td>
      <td>1986-07-18</td>
      <td>137</td>
      <td>8.4</td>
      <td>Sigourney Weaver</td>
      <td>540152</td>
      <td>James Cameron</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9000000.0</td>
      <td>Walt Disney Pictures</td>
      <td>USA</td>
      <td>Randal Kleiser</td>
      <td>Adventure</td>
      <td>18564613.0</td>
      <td>Flight of the Navigator</td>
      <td>PG</td>
      <td>1986-08-01</td>
      <td>90</td>
      <td>6.9</td>
      <td>Joey Cramer</td>
      <td>36636</td>
      <td>Mark H. Baker</td>
      <td>1986</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Loop to check if there is any missing data :

for col in df.columns:
    pct_missing = np.mean(df[col].isnull())
    print('{} - {}%'.format(col,pct_missing))
```

    budget - 0.0%
    company - 0.0%
    country - 0.0%
    director - 0.0%
    genre - 0.0%
    gross - 0.0%
    name - 0.0%
    rating - 0.0%
    released - 0.0%
    runtime - 0.0%
    score - 0.0%
    star - 0.0%
    votes - 0.0%
    writer - 0.0%
    year - 0.0%
    


```python
# Data Types for our columns 

df.dtypes
```




    budget      float64
    company      object
    country      object
    director     object
    genre        object
    gross       float64
    name         object
    rating       object
    released     object
    runtime       int64
    score       float64
    star         object
    votes         int64
    writer       object
    year          int64
    dtype: object




```python
# 'budget' & 'gross' columns are showing as float but we don't want decimal values so let's change to int

df['budget'] = df['budget'].astype('int64')
df['gross'] = df['gross'].astype('int64')
```


```python
# 'year' & 'released' are not same for some rows, let's extract year from 'released' column and drop 'year'

df['year_correct'] = df['released'].astype('str').str[:4]
df.drop(columns='year', inplace=True)
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>budget</th>
      <th>company</th>
      <th>country</th>
      <th>director</th>
      <th>genre</th>
      <th>gross</th>
      <th>name</th>
      <th>rating</th>
      <th>released</th>
      <th>runtime</th>
      <th>score</th>
      <th>star</th>
      <th>votes</th>
      <th>writer</th>
      <th>year_correct</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8000000</td>
      <td>Columbia Pictures Corporation</td>
      <td>USA</td>
      <td>Rob Reiner</td>
      <td>Adventure</td>
      <td>52287414</td>
      <td>Stand by Me</td>
      <td>R</td>
      <td>1986-08-22</td>
      <td>89</td>
      <td>8.1</td>
      <td>Wil Wheaton</td>
      <td>299174</td>
      <td>Stephen King</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6000000</td>
      <td>Paramount Pictures</td>
      <td>USA</td>
      <td>John Hughes</td>
      <td>Comedy</td>
      <td>70136369</td>
      <td>Ferris Bueller's Day Off</td>
      <td>PG-13</td>
      <td>1986-06-11</td>
      <td>103</td>
      <td>7.8</td>
      <td>Matthew Broderick</td>
      <td>264740</td>
      <td>John Hughes</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15000000</td>
      <td>Paramount Pictures</td>
      <td>USA</td>
      <td>Tony Scott</td>
      <td>Action</td>
      <td>179800601</td>
      <td>Top Gun</td>
      <td>PG</td>
      <td>1986-05-16</td>
      <td>110</td>
      <td>6.9</td>
      <td>Tom Cruise</td>
      <td>236909</td>
      <td>Jim Cash</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>3</th>
      <td>18500000</td>
      <td>Twentieth Century Fox Film Corporation</td>
      <td>USA</td>
      <td>James Cameron</td>
      <td>Action</td>
      <td>85160248</td>
      <td>Aliens</td>
      <td>R</td>
      <td>1986-07-18</td>
      <td>137</td>
      <td>8.4</td>
      <td>Sigourney Weaver</td>
      <td>540152</td>
      <td>James Cameron</td>
      <td>1986</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9000000</td>
      <td>Walt Disney Pictures</td>
      <td>USA</td>
      <td>Randal Kleiser</td>
      <td>Adventure</td>
      <td>18564613</td>
      <td>Flight of the Navigator</td>
      <td>PG</td>
      <td>1986-08-01</td>
      <td>90</td>
      <td>6.9</td>
      <td>Joey Cramer</td>
      <td>36636</td>
      <td>Mark H. Baker</td>
      <td>1986</td>
    </tr>
  </tbody>
</table>
</div>




```python
# incase we want to see all the data

pd.set_option('display.max_rows', None)
```


```python
# Drop any duplicates 

df.shape
```




    (6820, 15)




```python
# Get entire unique company list 

df['company'].drop_duplicates().sort_values(ascending=False).head()
```




    5288                    micro_scope
    3486                       i5 Films
    6084                           erbp
    3225                 double A Films
    2707    Zucker Brothers Productions
    Name: company, dtype: object




```python
# Get Top 10 companies by frequency

df['company'].value_counts().sort_values(ascending=False).head(10)
```




    Universal Pictures                        302
    Warner Bros.                              294
    Paramount Pictures                        259
    Twentieth Century Fox Film Corporation    205
    New Line Cinema                           172
    Columbia Pictures Corporation             166
    Touchstone Pictures                       131
    Columbia Pictures                         108
    Walt Disney Pictures                      102
    Metro-Goldwyn-Mayer (MGM)                 101
    Name: company, dtype: int64




```python
# Order entire dataset by 'Gross' column

df = df.sort_values(by=['gross'], ascending=False)
```

# Correlation Analysis


```python
# Scatter plot for Budget vs Gross

plt.scatter(x=df['budget'], y=df['gross'] )
plt.title('Budget vs Gross earnings')
plt.xlabel('Budget Earnings')
plt.ylabel('Gross Earnings')
plt.show()
```


![png](../assets/img/post_img/correlation-analysis/output_18_0.png)



```python
# Quick check the above scatter plot 

df[['name','company','budget','gross']].sort_values(by='gross', ascending=False).head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>company</th>
      <th>budget</th>
      <th>gross</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6380</th>
      <td>Star Wars: The Force Awakens</td>
      <td>Lucasfilm</td>
      <td>245000000</td>
      <td>936662225</td>
    </tr>
    <tr>
      <th>5061</th>
      <td>Avatar</td>
      <td>Twentieth Century Fox Film Corporation</td>
      <td>237000000</td>
      <td>760507625</td>
    </tr>
    <tr>
      <th>2420</th>
      <td>Titanic</td>
      <td>Twentieth Century Fox Film Corporation</td>
      <td>200000000</td>
      <td>658672302</td>
    </tr>
    <tr>
      <th>6391</th>
      <td>Jurassic World</td>
      <td>Universal Pictures</td>
      <td>150000000</td>
      <td>652270625</td>
    </tr>
    <tr>
      <th>5723</th>
      <td>The Avengers</td>
      <td>Marvel Studios</td>
      <td>220000000</td>
      <td>623357910</td>
    </tr>
    <tr>
      <th>4840</th>
      <td>The Dark Knight</td>
      <td>Warner Bros.</td>
      <td>185000000</td>
      <td>534858444</td>
    </tr>
    <tr>
      <th>6614</th>
      <td>Rogue One</td>
      <td>Lucasfilm</td>
      <td>200000000</td>
      <td>532177324</td>
    </tr>
    <tr>
      <th>6687</th>
      <td>Finding Dory</td>
      <td>Pixar Animation Studios</td>
      <td>200000000</td>
      <td>486295561</td>
    </tr>
    <tr>
      <th>2870</th>
      <td>Star Wars: Episode I - The Phantom Menace</td>
      <td>Lucasfilm</td>
      <td>115000000</td>
      <td>474544677</td>
    </tr>
    <tr>
      <th>6398</th>
      <td>Avengers: Age of Ultron</td>
      <td>Marvel Studios</td>
      <td>250000000</td>
      <td>459005868</td>
    </tr>
  </tbody>
</table>
</div>




```python
# now let's make a regression plot (budget vs gross) using seaborn

sns.regplot(x='budget', y='gross', data=df, 
            scatter_kws={"color":"grey"}, 
            line_kws={"color":"blue"})
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f5ea0bb45d0>




![png](../assets/img/post_img/correlation-analysis/output_20_1.png)



```python
# Let's look at correlation matrix now, but this is for numeric features only

df.corr(method="pearson")       # pearson(default), kendall, spearman
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>budget</th>
      <th>gross</th>
      <th>runtime</th>
      <th>score</th>
      <th>votes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>budget</th>
      <td>1.000000</td>
      <td>0.712196</td>
      <td>0.268226</td>
      <td>0.042145</td>
      <td>0.503924</td>
    </tr>
    <tr>
      <th>gross</th>
      <td>0.712196</td>
      <td>1.000000</td>
      <td>0.224579</td>
      <td>0.165693</td>
      <td>0.662457</td>
    </tr>
    <tr>
      <th>runtime</th>
      <td>0.268226</td>
      <td>0.224579</td>
      <td>1.000000</td>
      <td>0.395343</td>
      <td>0.317399</td>
    </tr>
    <tr>
      <th>score</th>
      <td>0.042145</td>
      <td>0.165693</td>
      <td>0.395343</td>
      <td>1.000000</td>
      <td>0.393607</td>
    </tr>
    <tr>
      <th>votes</th>
      <td>0.503924</td>
      <td>0.662457</td>
      <td>0.317399</td>
      <td>0.393607</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
correlation_matrix = df.corr(method='pearson')

sns.heatmap(data=correlation_matrix, annot=True)
plt.title('Correlation matrix for Numeric features')
plt.show()
```


![png](../assets/img/post_img/correlation-analysis/output_22_0.png)



```python
#  now let's also look at text features by mapping string values with numeric values

df_numerized = df.copy()

for col_name in df_numerized.columns:
    if(df_numerized[col_name].dtype == 'object'):
        df_numerized[col_name] = df_numerized[col_name].astype('category')
        df_numerized[col_name] = df_numerized[col_name].cat.codes


df_numerized.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>budget</th>
      <th>company</th>
      <th>country</th>
      <th>director</th>
      <th>genre</th>
      <th>gross</th>
      <th>name</th>
      <th>rating</th>
      <th>released</th>
      <th>runtime</th>
      <th>score</th>
      <th>star</th>
      <th>votes</th>
      <th>writer</th>
      <th>year_correct</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6380</th>
      <td>245000000</td>
      <td>1428</td>
      <td>54</td>
      <td>1037</td>
      <td>0</td>
      <td>936662225</td>
      <td>4679</td>
      <td>7</td>
      <td>2290</td>
      <td>136</td>
      <td>8.1</td>
      <td>475</td>
      <td>687192</td>
      <td>2356</td>
      <td>29</td>
    </tr>
    <tr>
      <th>5061</th>
      <td>237000000</td>
      <td>2062</td>
      <td>53</td>
      <td>1066</td>
      <td>0</td>
      <td>760507625</td>
      <td>501</td>
      <td>7</td>
      <td>1800</td>
      <td>162</td>
      <td>7.8</td>
      <td>2084</td>
      <td>954412</td>
      <td>1629</td>
      <td>23</td>
    </tr>
    <tr>
      <th>2420</th>
      <td>200000000</td>
      <td>2062</td>
      <td>54</td>
      <td>1066</td>
      <td>6</td>
      <td>658672302</td>
      <td>6177</td>
      <td>7</td>
      <td>910</td>
      <td>194</td>
      <td>7.8</td>
      <td>1444</td>
      <td>862554</td>
      <td>1629</td>
      <td>11</td>
    </tr>
    <tr>
      <th>6391</th>
      <td>150000000</td>
      <td>2085</td>
      <td>54</td>
      <td>466</td>
      <td>0</td>
      <td>652270625</td>
      <td>2721</td>
      <td>7</td>
      <td>2247</td>
      <td>124</td>
      <td>7.0</td>
      <td>404</td>
      <td>469200</td>
      <td>3310</td>
      <td>29</td>
    </tr>
    <tr>
      <th>5723</th>
      <td>220000000</td>
      <td>1491</td>
      <td>54</td>
      <td>1412</td>
      <td>0</td>
      <td>623357910</td>
      <td>4995</td>
      <td>7</td>
      <td>1987</td>
      <td>143</td>
      <td>8.1</td>
      <td>2001</td>
      <td>1064633</td>
      <td>2145</td>
      <td>26</td>
    </tr>
  </tbody>
</table>
</div>




```python
correlation_matrix = df_numerized.corr(method='pearson')

sns.heatmap(data=correlation_matrix, annot=True)
plt.title('Correlation matrix for Numeric features')
plt.show()
```


![png](../assets/img/post_img/correlation-analysis/output_24_0.png)



```python
correlation_matrix
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>budget</th>
      <th>company</th>
      <th>country</th>
      <th>director</th>
      <th>genre</th>
      <th>gross</th>
      <th>name</th>
      <th>rating</th>
      <th>released</th>
      <th>runtime</th>
      <th>score</th>
      <th>star</th>
      <th>votes</th>
      <th>writer</th>
      <th>year_correct</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>budget</th>
      <td>1.000000</td>
      <td>0.187205</td>
      <td>0.137635</td>
      <td>0.011602</td>
      <td>-0.346794</td>
      <td>0.712196</td>
      <td>0.028712</td>
      <td>-0.119660</td>
      <td>0.276635</td>
      <td>0.268226</td>
      <td>0.042145</td>
      <td>-0.015061</td>
      <td>0.503924</td>
      <td>-0.015611</td>
      <td>0.274820</td>
    </tr>
    <tr>
      <th>company</th>
      <td>0.187205</td>
      <td>1.000000</td>
      <td>0.107950</td>
      <td>0.004320</td>
      <td>-0.068330</td>
      <td>0.187220</td>
      <td>0.018098</td>
      <td>-0.062250</td>
      <td>0.027898</td>
      <td>0.033058</td>
      <td>-0.010426</td>
      <td>-0.003160</td>
      <td>0.138662</td>
      <td>-0.004032</td>
      <td>0.028012</td>
    </tr>
    <tr>
      <th>country</th>
      <td>0.137635</td>
      <td>0.107950</td>
      <td>1.000000</td>
      <td>0.003698</td>
      <td>-0.042793</td>
      <td>0.149988</td>
      <td>0.025020</td>
      <td>0.057979</td>
      <td>-0.062609</td>
      <td>-0.081796</td>
      <td>-0.174414</td>
      <td>-0.014566</td>
      <td>0.078657</td>
      <td>0.024981</td>
      <td>-0.062707</td>
    </tr>
    <tr>
      <th>director</th>
      <td>0.011602</td>
      <td>0.004320</td>
      <td>0.003698</td>
      <td>1.000000</td>
      <td>-0.027668</td>
      <td>-0.011429</td>
      <td>0.001905</td>
      <td>0.021926</td>
      <td>0.001440</td>
      <td>0.026779</td>
      <td>0.017130</td>
      <td>0.039813</td>
      <td>0.000639</td>
      <td>0.298997</td>
      <td>0.001822</td>
    </tr>
    <tr>
      <th>genre</th>
      <td>-0.346794</td>
      <td>-0.068330</td>
      <td>-0.042793</td>
      <td>-0.027668</td>
      <td>1.000000</td>
      <td>-0.242676</td>
      <td>0.018062</td>
      <td>0.100960</td>
      <td>-0.039179</td>
      <td>-0.041357</td>
      <td>0.056234</td>
      <td>0.008140</td>
      <td>-0.150519</td>
      <td>-0.000608</td>
      <td>-0.039014</td>
    </tr>
    <tr>
      <th>gross</th>
      <td>0.712196</td>
      <td>0.187220</td>
      <td>0.149988</td>
      <td>-0.011429</td>
      <td>-0.242676</td>
      <td>1.000000</td>
      <td>0.022768</td>
      <td>-0.135538</td>
      <td>0.178564</td>
      <td>0.224579</td>
      <td>0.165693</td>
      <td>0.008382</td>
      <td>0.662457</td>
      <td>-0.009455</td>
      <td>0.176879</td>
    </tr>
    <tr>
      <th>name</th>
      <td>0.028712</td>
      <td>0.018098</td>
      <td>0.025020</td>
      <td>0.001905</td>
      <td>0.018062</td>
      <td>0.022768</td>
      <td>1.000000</td>
      <td>0.001288</td>
      <td>0.024120</td>
      <td>0.013942</td>
      <td>0.023342</td>
      <td>-0.001910</td>
      <td>0.023665</td>
      <td>0.009821</td>
      <td>0.023411</td>
    </tr>
    <tr>
      <th>rating</th>
      <td>-0.119660</td>
      <td>-0.062250</td>
      <td>0.057979</td>
      <td>0.021926</td>
      <td>0.100960</td>
      <td>-0.135538</td>
      <td>0.001288</td>
      <td>1.000000</td>
      <td>0.016696</td>
      <td>0.079542</td>
      <td>0.019271</td>
      <td>0.007893</td>
      <td>0.011678</td>
      <td>0.010740</td>
      <td>0.017438</td>
    </tr>
    <tr>
      <th>released</th>
      <td>0.276635</td>
      <td>0.027898</td>
      <td>-0.062609</td>
      <td>0.001440</td>
      <td>-0.039179</td>
      <td>0.178564</td>
      <td>0.024120</td>
      <td>0.016696</td>
      <td>1.000000</td>
      <td>0.091102</td>
      <td>0.119577</td>
      <td>-0.025504</td>
      <td>0.221736</td>
      <td>-0.004635</td>
      <td>0.999389</td>
    </tr>
    <tr>
      <th>runtime</th>
      <td>0.268226</td>
      <td>0.033058</td>
      <td>-0.081796</td>
      <td>0.026779</td>
      <td>-0.041357</td>
      <td>0.224579</td>
      <td>0.013942</td>
      <td>0.079542</td>
      <td>0.091102</td>
      <td>1.000000</td>
      <td>0.395343</td>
      <td>0.016019</td>
      <td>0.317399</td>
      <td>0.000759</td>
      <td>0.088342</td>
    </tr>
    <tr>
      <th>score</th>
      <td>0.042145</td>
      <td>-0.010426</td>
      <td>-0.174414</td>
      <td>0.017130</td>
      <td>0.056234</td>
      <td>0.165693</td>
      <td>0.023342</td>
      <td>0.019271</td>
      <td>0.119577</td>
      <td>0.395343</td>
      <td>1.000000</td>
      <td>0.009482</td>
      <td>0.393607</td>
      <td>0.012223</td>
      <td>0.117679</td>
    </tr>
    <tr>
      <th>star</th>
      <td>-0.015061</td>
      <td>-0.003160</td>
      <td>-0.014566</td>
      <td>0.039813</td>
      <td>0.008140</td>
      <td>0.008382</td>
      <td>-0.001910</td>
      <td>0.007893</td>
      <td>-0.025504</td>
      <td>0.016019</td>
      <td>0.009482</td>
      <td>1.000000</td>
      <td>-0.011919</td>
      <td>0.035378</td>
      <td>-0.026050</td>
    </tr>
    <tr>
      <th>votes</th>
      <td>0.503924</td>
      <td>0.138662</td>
      <td>0.078657</td>
      <td>0.000639</td>
      <td>-0.150519</td>
      <td>0.662457</td>
      <td>0.023665</td>
      <td>0.011678</td>
      <td>0.221736</td>
      <td>0.317399</td>
      <td>0.393607</td>
      <td>-0.011919</td>
      <td>1.000000</td>
      <td>0.001154</td>
      <td>0.220797</td>
    </tr>
    <tr>
      <th>writer</th>
      <td>-0.015611</td>
      <td>-0.004032</td>
      <td>0.024981</td>
      <td>0.298997</td>
      <td>-0.000608</td>
      <td>-0.009455</td>
      <td>0.009821</td>
      <td>0.010740</td>
      <td>-0.004635</td>
      <td>0.000759</td>
      <td>0.012223</td>
      <td>0.035378</td>
      <td>0.001154</td>
      <td>1.000000</td>
      <td>-0.004546</td>
    </tr>
    <tr>
      <th>year_correct</th>
      <td>0.274820</td>
      <td>0.028012</td>
      <td>-0.062707</td>
      <td>0.001822</td>
      <td>-0.039014</td>
      <td>0.176879</td>
      <td>0.023411</td>
      <td>0.017438</td>
      <td>0.999389</td>
      <td>0.088342</td>
      <td>0.117679</td>
      <td>-0.026050</td>
      <td>0.220797</td>
      <td>-0.004546</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Unstacking 

corr_pairs = correlation_matrix.unstack()
corr_pairs.head()
```




    budget  budget      1.000000
            company     0.187205
            country     0.137635
            director    0.011602
            genre      -0.346794
    dtype: float64




```python
sort_pairs = corr_pairs.sort_values()
```


```python
sort_pairs.tail()
```




    director      director        1.0
    country       country         1.0
    company       company         1.0
    writer        writer          1.0
    year_correct  year_correct    1.0
    dtype: float64




```python
high_corr = sort_pairs[(sort_pairs) > 0.5]
high_corr
```




    votes         budget          0.503924
    budget        votes           0.503924
    gross         votes           0.662457
    votes         gross           0.662457
    gross         budget          0.712196
    budget        gross           0.712196
    released      year_correct    0.999389
    year_correct  released        0.999389
    budget        budget          1.000000
    rating        rating          1.000000
    votes         votes           1.000000
    star          star            1.000000
    score         score           1.000000
    runtime       runtime         1.000000
    released      released        1.000000
    name          name            1.000000
    gross         gross           1.000000
    genre         genre           1.000000
    director      director        1.000000
    country       country         1.000000
    company       company         1.000000
    writer        writer          1.000000
    year_correct  year_correct    1.000000
    dtype: float64



From above list, we can see Gross is correlated to budget and votes.

# Conclusion

In this analysis we wanted to understand on which factor does gross depends for a movie. In our hypothesis, we assumed Company would play and important factor however Correlation matrix showed otherwise.

Gross is correlated to Budget of movie and votes it received. For our analysis we used Pearson correlation factor.
