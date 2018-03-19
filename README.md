

```python
import pandas as pd
import numpy as np
df_path = 'purchase_data.JSON'
df = pd.read_json(df_path)

```


```python
player_count = len(df['SN'].unique())

player_count_df = pd.DataFrame({
    'Total Players' : player_count
                                },
    index = [0]
)

player_count_df

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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
purchase_count = df['Price'].count()

total_revenue = df['Price'].sum()

avg_price = total_revenue / purchase_count

unique_items = len(df['Item Name'].unique())

purchasing_analysis = pd.DataFrame({
    'Number of Unique Items' : unique_items,
    'Average Price' : avg_price,
    'Number of Purchases' : purchase_count,
    'Total Revenue' : total_revenue
                                    },
    index = [0]
)

purchasing_analysis = purchasing_analysis[['Number of Unique Items', 'Average Price', 'Number of Purchases', 'Total Revenue']]

purchasing_analysis['Average Price'] = purchasing_analysis['Average Price'].map('$ {:,.2f}'.format)
purchasing_analysis['Total Revenue'] = purchasing_analysis['Total Revenue'].map('$ {:,.2f}'.format)


purchasing_analysis

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
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>179</td>
      <td>$ 2.93</td>
      <td>780</td>
      <td>$ 2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
unique_player_df = df.drop_duplicates(subset='SN', keep="last")

total_players_by_gender = unique_player_df['Gender'].value_counts().values

gender_list = df['Gender'].value_counts().keys()

percent_players_by_gender = (total_players_by_gender / player_count) * 100

gender_demographics = pd.DataFrame({
    'Percentage of Players' : percent_players_by_gender,
    'Total Count' : total_players_by_gender
                                    },
    index = [gender_list]
)

gender_demographics

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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.151832</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.452007</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.396161</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
male_purchases = df.loc[df['Gender'] == 'Male']
female_purchases = df.loc[df['Gender'] == 'Female']
other_purchases = df.loc[df['Gender'] == 'Other / Non-Disclosed']

purchase_count_gender = pd.Series([male_purchases['Price'].count(), 
                                female_purchases['Price'].count(),  
                                other_purchases['Price'].count()
                                ])

total_value_gender = pd.Series([male_purchases['Price'].sum(), 
                                female_purchases['Price'].sum(),  
                                other_purchases['Price'].sum()
                                ])

avg_price_gender = total_value_gender / purchase_count_gender

norm_price_gender = total_value_gender / total_players_by_gender

purchasing_analysis = pd.DataFrame({
    'Purchase Count' : purchase_count_gender,
    'Average Purchase Price' : avg_price_gender,
    'Total Purchase Value' : total_value_gender,
    'Normalized Totals' : norm_price_gender,
    'Gender' : ['Male', 'Female', 'Other / Non-Disclosed']
                                    })

purchasing_analysis.set_index('Gender', inplace=True)

purchasing_analysis['Average Purchase Price'] = purchasing_analysis['Average Purchase Price'].map('$ {:,.2f}'.format)
purchasing_analysis['Total Purchase Value'] = purchasing_analysis['Total Purchase Value'].map('$ {:,.2f}'.format)
purchasing_analysis['Normalized Totals'] = purchasing_analysis['Normalized Totals'].map('$ {:,.2f}'.format)

purchasing_analysis = purchasing_analysis[['Purchase Count', 'Average Purchase Price', 'Total Purchase Value', 'Normalized Totals']]

purchasing_analysis

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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$ 2.95</td>
      <td>$ 1,867.68</td>
      <td>$ 4.02</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>$ 2.82</td>
      <td>$ 382.91</td>
      <td>$ 3.83</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$ 3.25</td>
      <td>$ 35.74</td>
      <td>$ 4.47</td>
    </tr>
  </tbody>
</table>
</div>




```python


bins = [0, 10, 15, 20, 25, 30, 35, 40, 150]

group_labels = ["<10","10-14","15-19","20-24","25-29","30-34","35-39","40+"]

binned_unique_df = df.drop_duplicates(subset='SN', keep='last')
binned_unique_df['Total Count'] = pd.cut(binned_unique_df["Age"],bins,labels=group_labels)

age_demographics = pd.DataFrame(binned_unique_df['Total Count'].value_counts())

age_demographics['Percentage of Players'] = round((age_demographics['Total Count'] / player_count) * 100, 2)

age_demographics = age_demographics.reindex(["<10","10-14","15-19","20-24","25-29","30-34","35-39","40+"])

age_demographics = age_demographics[['Percentage of Players', 'Total Count']]

age_demographics

```

    C:\Users\timot\Anaconda3\envs\PythonData\lib\site-packages\ipykernel_launcher.py:8: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      
    




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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.84</td>
      <td>22</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>9.42</td>
      <td>54</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>24.26</td>
      <td>139</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>40.84</td>
      <td>234</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>9.08</td>
      <td>52</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>7.68</td>
      <td>44</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.36</td>
      <td>25</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>0.52</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
binned_purchase_df = df
binned_purchase_df['Total Count'] = pd.cut(binned_purchase_df["Age"],bins,labels=group_labels)

binned_purchase_df = binned_purchase_df.groupby('Total Count')

binned_purchase_count = binned_purchase_df['Price'].count()

binned_total_value = binned_purchase_df['Price'].sum()

binned_avg_price = binned_total_value / binned_purchase_count

binned_norm_price = binned_total_value / age_demographics['Total Count']

binned_purchase_analysis_df = pd.DataFrame(
            {'Purchase Count' : binned_purchase_count,
             'Average Purchase Price' : binned_avg_price,
             'Total Purchase Value' : binned_total_value,
             'Normalized Totals' : binned_norm_price
        }
)

binned_purchase_analysis_df['Average Purchase Price'] = binned_purchase_analysis_df['Average Purchase Price'].map('$ {:,.2f}'.format)
binned_purchase_analysis_df['Total Purchase Value'] = binned_purchase_analysis_df['Total Purchase Value'].map('$ {:,.2f}'.format)
binned_purchase_analysis_df['Normalized Totals'] = binned_purchase_analysis_df['Normalized Totals'].map('$ {:,.2f}'.format)

binned_purchase_analysis_df = binned_purchase_analysis_df[['Purchase Count', 'Average Purchase Price', 'Total Purchase Value', 'Normalized Totals']]

binned_purchase_analysis_df

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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Total Count</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>32</td>
      <td>$ 3.02</td>
      <td>$ 96.62</td>
      <td>$ 4.39</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>78</td>
      <td>$ 2.87</td>
      <td>$ 224.15</td>
      <td>$ 4.15</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>184</td>
      <td>$ 2.87</td>
      <td>$ 528.74</td>
      <td>$ 3.80</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>305</td>
      <td>$ 2.96</td>
      <td>$ 902.61</td>
      <td>$ 3.86</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>76</td>
      <td>$ 2.89</td>
      <td>$ 219.82</td>
      <td>$ 4.23</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>58</td>
      <td>$ 3.07</td>
      <td>$ 178.26</td>
      <td>$ 4.05</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>44</td>
      <td>$ 2.90</td>
      <td>$ 127.49</td>
      <td>$ 5.10</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>3</td>
      <td>$ 2.88</td>
      <td>$ 8.64</td>
      <td>$ 2.88</td>
    </tr>
  </tbody>
</table>
</div>




```python
spenders_df = df[['Item ID', 'Price', 'SN']]

spenders_count = spenders_df.groupby('SN')['Price'].count()
spenders_df = spenders_df.join(spenders_count, on='SN', lsuffix='_l', rsuffix='_r')

spenders_total = spenders_df.groupby('SN')['Price_l'].sum()
spenders_df = spenders_df.join(spenders_total, on='SN', lsuffix='_l', rsuffix='_r')

spenders_avg = spenders_df['Price_l_r'].values / spenders_df['Price_r']
spenders_df['Average Purchase Price'] = spenders_avg

spenders_df = spenders_df.rename(columns=
    {
        'Price_l_l' : 'Price',
        'Price_l_r' : 'Total Purchase Value',
        'Price_r' : 'Purchase Count'
    }
)

spenders_df.drop_duplicates('SN', inplace=True)
spenders_df.sort_values('Total Purchase Value', inplace=True, ascending=False)
spenders_df = spenders_df.iloc[0:5,:]

spenders_df['Price'] = spenders_df['Price'].map('$ {:,.2f}'.format)
spenders_df['Total Purchase Value'] = spenders_df['Total Purchase Value'].map('$ {:,.2f}'.format)
spenders_df['Average Purchase Price'] = spenders_df['Average Purchase Price'].map('$ {:,.2f}'.format)

spenders_df.set_index('SN', inplace=True)


spenders_df = spenders_df[['Purchase Count', 'Average Purchase Price', 'Total Purchase Value']]

spenders_df
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>5</td>
      <td>$ 3.41</td>
      <td>$ 17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$ 3.39</td>
      <td>$ 13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$ 3.18</td>
      <td>$ 12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$ 4.24</td>
      <td>$ 12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>$ 3.86</td>
      <td>$ 11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
item_group_df = df[['Item ID', 'Item Name', 'Price']]

item_count_df = item_group_df

most_item_count = item_group_df.groupby('Item ID').count()
most_item_count = most_item_count['Price']
item_count_df = item_count_df.join(most_item_count, on='Item ID',lsuffix='_left', rsuffix='_right')

item_count_df = item_count_df.rename(columns=
    {
        'Price_left' : 'Item Price',
        'Price_right' : 'Purchase Count'
    }
)

item_count_df = item_count_df.drop_duplicates('Item ID')
item_count_df['Total Purchase Value'] = item_count_df['Item Price'].values * item_count_df['Purchase Count']

item_count_df = item_count_df[['Item ID', 'Item Name', 'Purchase Count', 'Item Price', 'Total Purchase Value']]

item_count_df.set_index(['Item ID', 'Item Name'], inplace=True)

most_prof_item_count_df = item_count_df
most_pop_item_count_df = item_count_df

most_prof_item_count_df = most_prof_item_count_df.sort_values('Purchase Count', ascending=False)
most_prof_item_count_df = most_prof_item_count_df.iloc[0:5,:]

most_prof_item_count_df['Item Price'] = most_prof_item_count_df['Item Price'].map('$ {:,.2f}'.format)
most_prof_item_count_df['Total Purchase Value'] = most_prof_item_count_df['Total Purchase Value'].map('$ {:,.2f}'.format)


most_prof_item_count_df
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$ 2.35</td>
      <td>$ 25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$ 2.23</td>
      <td>$ 24.53</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$ 1.24</td>
      <td>$ 11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$ 1.49</td>
      <td>$ 13.41</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$ 2.07</td>
      <td>$ 18.63</td>
    </tr>
  </tbody>
</table>
</div>




```python
most_pop_item_count_df = item_count_df.sort_values('Total Purchase Value', ascending=False)
most_pop_item_count_df = most_pop_item_count_df.iloc[0:5,:]

most_pop_item_count_df['Item Price'] = most_pop_item_count_df['Item Price'].map('$ {:,.2f}'.format)
most_pop_item_count_df['Total Purchase Value'] = most_pop_item_count_df['Total Purchase Value'].map('$ {:,.2f}'.format)


most_pop_item_count_df
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$ 4.14</td>
      <td>$ 37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>$ 4.25</td>
      <td>$ 29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>$ 4.95</td>
      <td>$ 29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>$ 4.87</td>
      <td>$ 29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>$ 3.61</td>
      <td>$ 28.88</td>
    </tr>
  </tbody>
</table>
</div>


