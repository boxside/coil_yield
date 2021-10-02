# COIL PRODUCTION ANALYSIS

## Background

![CRC](imagename.png)

   In Steel manufacturing company, there is cold rolling process. This process will produce cold rolled coil(CRC). CRC have better surface quality, thinner and with more precise size, also have good mechanical property and excellent durability and finishes, while hot rolled coils have excellent formability and weldability, CRC are used in a wide range of applications, such as automotive manufacturing, electrical products, rolling stock, aviation, precision instruments, and food cans. But in this process, have several challange that make reduce the efficiency of the process, this process can produce "byproduct material" a.k.a Scrap. this material caused by production parameter or defect after process. In this case we have to find what that make yield of process reduced.

COLUMN EXPLANATION :

    NO                     : No. of Coil
    HRC_THICKNESS          : Raw Material Thickness 
    HRC_WIDTH              : Raw Material Width 
    HRC_WEIGHT             : Raw Material Weight 
    XXXXX_DATE             : Date process of process's unit X
    XXXXX_TONNAGE_INPUT    : Weight Input of process's unit X
    XXXXX_TONNAGE_OUTPUT   : Weight Output of process's unit X
    XXXXX_SCRAP            : Weight Byproduct of process's unit X
    XXXXX_DF               : Efficiency of process's unit X
    PIKLING_LOSE           : Weight material that dissolved in acid
    SOURCE                 : Source of Raw Material
    ROLLING_LINE           : Unit of Cold Rolling Mill Line (6hi/bls)
    XXXXX_TIME             : Time process of process's unit X
    OUTPUT_THICKNESS       : Output Thickness from CRM process
    OUTPUT_WIDTH           : Output Width from CRM process
    CRC_SIZE               : Cold Rolled Coil Size
    SALES_ORDER            : No. Of Sales Order
    SCRAP_HRC_FINISH       : Total Byproduct from first process to the last
    YIELD(%)               : Efficiency process to finish by Coil
    REMARK                 : Additional Note
    
 Unit Of Measurement :
 
    Weight      : Metric Ton(MT) and Kilograms (Kg)
    Length      : milimeter (mm)
    Time        : minutes (m)
    Date Format : YYYY-MM-DD

## Code 

### Import Library


```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
pd.options.display.max_columns = 50
from jupyterthemes import jtplot
jtplot.style(theme='monokai', context='notebook', ticks=True, grid=False)
```

### Import and Check Dataset


```python
df = pd.read_csv(r'C:\Users\fikri\Desktop\pyproj\coil performance\coil_final.csv') ## import dataset
```


```python
print("There are {} rows and {} columns in dataset".format(df.shape[0],df.shape[1]))
df.head()
```

    There are 1457 rows and 35 columns in dataset
    




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NO</th>
      <th>HRC_THICKNESS</th>
      <th>HRC_WIDTH</th>
      <th>HRC_WEIGHT</th>
      <th>PICKLING_DATE</th>
      <th>SLITING_TONNAGE_INPUT</th>
      <th>SLITING_TONNAGE_OUTPUT</th>
      <th>SLITING_SCRAP</th>
      <th>SLITING_DF</th>
      <th>PICKLING_TONNAGE_INPUT</th>
      <th>PICKLING_TONNAGE_OUTPUT</th>
      <th>PIKLING_LOSE</th>
      <th>PICKLING_DF</th>
      <th>SOURCE</th>
      <th>ROLLING_LINE</th>
      <th>ROLLING_DATE</th>
      <th>ROLLING_TIME</th>
      <th>OUTPUT_THICKNESS</th>
      <th>OUTPUT_WIDTH</th>
      <th>CRC_SIZE</th>
      <th>ROLLING_TONNAGE_INPUT</th>
      <th>ROLLING_TONNAGE_OUTPUT</th>
      <th>ROLLING_SCRAP</th>
      <th>ROLLING_DF</th>
      <th>SALES_ORDER</th>
      <th>FINISHING_DATE</th>
      <th>FINISHING_TIME</th>
      <th>FINISHING_TONNAGE_INPUT</th>
      <th>FINISHING_TONNAGE_OUTPUT</th>
      <th>FINISHING_ID_SCRAP</th>
      <th>FINISHING_TRIMING_LOSE</th>
      <th>FINISHING_DF</th>
      <th>SCRAP_HRC_FINISH</th>
      <th>YIELD(%)</th>
      <th>REMARK</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2.6</td>
      <td>1240</td>
      <td>19780</td>
      <td>2020-12-10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>19780.0</td>
      <td>19560</td>
      <td>220.0</td>
      <td>0.988878</td>
      <td>GR</td>
      <td>6HI</td>
      <td>2020-12-12</td>
      <td>35.0</td>
      <td>0.7</td>
      <td>1219</td>
      <td>0.70 X 1219</td>
      <td>19560</td>
      <td>19400</td>
      <td>160</td>
      <td>0.991820</td>
      <td>SP0-2104-0107</td>
      <td>2021-01-03</td>
      <td>20</td>
      <td>19400</td>
      <td>18375</td>
      <td>140.0</td>
      <td>885</td>
      <td>0.947165</td>
      <td>1405</td>
      <td>0.928969</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2.2</td>
      <td>1240</td>
      <td>22840</td>
      <td>2020-12-10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>22840.0</td>
      <td>22635</td>
      <td>205.0</td>
      <td>0.991025</td>
      <td>KS</td>
      <td>6HI</td>
      <td>2020-12-12</td>
      <td>23.0</td>
      <td>0.7</td>
      <td>1219</td>
      <td>0.70 X 1219</td>
      <td>22635</td>
      <td>22520</td>
      <td>115</td>
      <td>0.994919</td>
      <td>SP0-2104-0107</td>
      <td>2021-01-03</td>
      <td>15</td>
      <td>22520</td>
      <td>21290</td>
      <td>280.0</td>
      <td>950</td>
      <td>0.945382</td>
      <td>1550</td>
      <td>0.932137</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2.2</td>
      <td>1240</td>
      <td>22820</td>
      <td>2020-12-10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>22820.0</td>
      <td>22585</td>
      <td>235.0</td>
      <td>0.989702</td>
      <td>KS</td>
      <td>6HI</td>
      <td>2020-12-12</td>
      <td>29.0</td>
      <td>0.7</td>
      <td>1219</td>
      <td>0.70 X 1219</td>
      <td>22585</td>
      <td>22450</td>
      <td>135</td>
      <td>0.994023</td>
      <td>SP0-2104-0107</td>
      <td>2021-01-03</td>
      <td>15</td>
      <td>22450</td>
      <td>21350</td>
      <td>145.0</td>
      <td>955</td>
      <td>0.951002</td>
      <td>1470</td>
      <td>0.935583</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>2.2</td>
      <td>1240</td>
      <td>22720</td>
      <td>2020-12-10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>22720.0</td>
      <td>22540</td>
      <td>180.0</td>
      <td>0.992077</td>
      <td>KS</td>
      <td>6HI</td>
      <td>2020-12-12</td>
      <td>22.0</td>
      <td>0.7</td>
      <td>1219</td>
      <td>0.70 X 1219</td>
      <td>22540</td>
      <td>22400</td>
      <td>140</td>
      <td>0.993789</td>
      <td>SP0-2104-0107</td>
      <td>2021-01-03</td>
      <td>20</td>
      <td>22400</td>
      <td>21445</td>
      <td>150.0</td>
      <td>805</td>
      <td>0.957366</td>
      <td>1275</td>
      <td>0.943882</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2.2</td>
      <td>1240</td>
      <td>22660</td>
      <td>2020-12-10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>22660.0</td>
      <td>22430</td>
      <td>230.0</td>
      <td>0.989850</td>
      <td>KS</td>
      <td>6HI</td>
      <td>2020-12-12</td>
      <td>19.0</td>
      <td>0.7</td>
      <td>1219</td>
      <td>0.70 X 1219</td>
      <td>22430</td>
      <td>22300</td>
      <td>130</td>
      <td>0.994204</td>
      <td>SP0-2104-0107</td>
      <td>2021-01-03</td>
      <td>15</td>
      <td>22300</td>
      <td>21285</td>
      <td>135.0</td>
      <td>880</td>
      <td>0.954484</td>
      <td>1375</td>
      <td>0.939320</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
print("Checking datatype every column")
print(df.info(verbose=True))
```

    Checking datatype every column
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1457 entries, 0 to 1456
    Data columns (total 35 columns):
     #   Column                    Non-Null Count  Dtype  
    ---  ------                    --------------  -----  
     0   NO                        1457 non-null   int64  
     1   HRC_THICKNESS             1457 non-null   float64
     2   HRC_WIDTH                 1457 non-null   int64  
     3   HRC_WEIGHT                1457 non-null   int64  
     4   PICKLING_DATE             1457 non-null   object 
     5   SLITING_TONNAGE_INPUT     680 non-null    float64
     6   SLITING_TONNAGE_OUTPUT    680 non-null    float64
     7   SLITING_SCRAP             680 non-null    float64
     8   SLITING_DF                680 non-null    float64
     9   PICKLING_TONNAGE_INPUT    1457 non-null   float64
     10  PICKLING_TONNAGE_OUTPUT   1457 non-null   int64  
     11  PIKLING_LOSE              1457 non-null   float64
     12  PICKLING_DF               1457 non-null   float64
     13  SOURCE                    1457 non-null   object 
     14  ROLLING_LINE              1457 non-null   object 
     15  ROLLING_DATE              1457 non-null   object 
     16  ROLLING_TIME              1456 non-null   float64
     17  OUTPUT_THICKNESS          1457 non-null   float64
     18  OUTPUT_WIDTH              1457 non-null   int64  
     19  CRC_SIZE                  1457 non-null   object 
     20  ROLLING_TONNAGE_INPUT     1457 non-null   int64  
     21  ROLLING_TONNAGE_OUTPUT    1457 non-null   int64  
     22  ROLLING_SCRAP             1457 non-null   int64  
     23  ROLLING_DF                1457 non-null   float64
     24  SALES_ORDER               1457 non-null   object 
     25  FINISHING_DATE            1457 non-null   object 
     26  FINISHING_TIME            1457 non-null   int64  
     27  FINISHING_TONNAGE_INPUT   1457 non-null   int64  
     28  FINISHING_TONNAGE_OUTPUT  1457 non-null   int64  
     29  FINISHING_ID_SCRAP        1455 non-null   float64
     30  FINISHING_TRIMING_LOSE    1457 non-null   int64  
     31  FINISHING_DF              1457 non-null   float64
     32  SCRAP_HRC_FINISH          1457 non-null   int64  
     33  YIELD(%)                  1457 non-null   float64
     34  REMARK                    92 non-null     object 
    dtypes: float64(14), int64(13), object(8)
    memory usage: 398.5+ KB
    None
    

#### checking null values


```python
df.isnull().sum()
```




    NO                             0
    HRC_THICKNESS                  0
    HRC_WIDTH                      0
    HRC_WEIGHT                     0
    PICKLING_DATE                  0
    SLITING_TONNAGE_INPUT        777
    SLITING_TONNAGE_OUTPUT       777
    SLITING_SCRAP                777
    SLITING_DF                   777
    PICKLING_TONNAGE_INPUT         0
    PICKLING_TONNAGE_OUTPUT        0
    PIKLING_LOSE                   0
    PICKLING_DF                    0
    SOURCE                         0
    ROLLING_LINE                   0
    ROLLING_DATE                   0
    ROLLING_TIME                   1
    OUTPUT_THICKNESS               0
    OUTPUT_WIDTH                   0
    CRC_SIZE                       0
    ROLLING_TONNAGE_INPUT          0
    ROLLING_TONNAGE_OUTPUT         0
    ROLLING_SCRAP                  0
    ROLLING_DF                     0
    SALES_ORDER                    0
    FINISHING_DATE                 0
    FINISHING_TIME                 0
    FINISHING_TONNAGE_INPUT        0
    FINISHING_TONNAGE_OUTPUT       0
    FINISHING_ID_SCRAP             2
    FINISHING_TRIMING_LOSE         0
    FINISHING_DF                   0
    SCRAP_HRC_FINISH               0
    YIELD(%)                       0
    REMARK                      1365
    dtype: int64




```python
# there 2 columns with null values
# check where row that has null value
df[df['ROLLING_TIME'].isnull()].index.tolist()
```




    [195]




```python
# fill null value 'FINISHING_ID_SCRAP' to 0
# fill null value 'ROLLING_TIME' with average time process with same size.
df['FINISHING_ID_SCRAP'].fillna(0,inplace=True)
df['ROLLING_TIME'].iloc[195] = df[df['CRC_SIZE'] == df['CRC_SIZE'].iloc[195]]['ROLLING_TIME'].mean()
print(df['ROLLING_TIME'].iloc[195])
print(df['FINISHING_ID_SCRAP'].isnull().sum())
```

    26.726027397260275
    0
    

    c:\users\fikri\appdata\local\programs\python\python37\lib\site-packages\pandas\core\indexing.py:1637: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      self._setitem_single_block(indexer, value, name)
    

### DATA WRANGLING


```python
## change datatype for date data
date_data = ['PICKLING_DATE','ROLLING_DATE','FINISHING_DATE']
for date in date_data :
    df[date] = pd.to_datetime(df[date])
    print(df[date].head())
```

    0   2020-12-10
    1   2020-12-10
    2   2020-12-10
    3   2020-12-10
    4   2020-12-10
    Name: PICKLING_DATE, dtype: datetime64[ns]
    0   2020-12-12
    1   2020-12-12
    2   2020-12-12
    3   2020-12-12
    4   2020-12-12
    Name: ROLLING_DATE, dtype: datetime64[ns]
    0   2021-01-03
    1   2021-01-03
    2   2021-01-03
    3   2021-01-03
    4   2021-01-03
    Name: FINISHING_DATE, dtype: datetime64[ns]
    


```python
# CHANGE OUTPUT_WIDTH AND OUTPUT_THICKNESS TO CATEGORICAL DATA RANGE

df['WIDTH_RANGE'] = pd.cut(df['OUTPUT_WIDTH'], [0, 929, 1000, 1300], labels=['<930', '930-1000', '1000-1300'])
df['THICKNESS_RANGE'] = pd.cut(df['OUTPUT_THICKNESS'], [0, 0.249,0.649,1,2], labels=['<0.250', '0.250-0.649', '0.650-1','>1'])

# THERE IS STANDARD FOR 'YIELD(%)' VALUE IF WANT TO SEND FINISH GOODS TO CUSTOMER,
# RANGE YIELD THRESHOLD IS 92%-97% THAT ACCEPTABLE
df['status'] = np.where((df['YIELD(%)'] <0.92),'Below Low Threshold',np.where(
                             (df['YIELD(%)'] >= 0.92)  & (df['YIELD(%)'] <=0.97) ,'Acceptable',np.where\
                             ((df['YIELD(%)'] >0.97) ,'Above Upper Threshold','out of parameter' )))

```

### Find product yield acceptance


```python
status_df1 = df['status'].value_counts().reset_index()
status_df2 = df['status'].value_counts(normalize=True).reset_index()

status_df = status_df1.merge(status_df2,how='inner',on='index')
status_df
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>status_x</th>
      <th>status_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Acceptable</td>
      <td>1303</td>
      <td>0.894303</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Below Low Threshold</td>
      <td>154</td>
      <td>0.105697</td>
    </tr>
  </tbody>
</table>
</div>



When categorize the coil in this dataset have 155 coil that below threshold it's about 10% Production have low Yield. in next section we need find the cause of this problem.


```python
# find top 5 of total low Yield Coil by CRC Size
case = df[df['status'] != 'Acceptable']['CRC_SIZE'].value_counts().reset_index().sort_values('CRC_SIZE',ascending=False).head()
case['ratio'] = (case['CRC_SIZE']/sum(case['CRC_SIZE']))*100
case
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>CRC_SIZE</th>
      <th>ratio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.20 X 762</td>
      <td>62</td>
      <td>45.925926</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.20 X 914</td>
      <td>57</td>
      <td>42.222222</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.90 X 1219</td>
      <td>9</td>
      <td>6.666667</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.75 X 914</td>
      <td>4</td>
      <td>2.962963</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.10 X 1219</td>
      <td>3</td>
      <td>2.222222</td>
    </tr>
  </tbody>
</table>
</div>



the result above show that CRC with size (0.20x762) and (0.20x914) have many product with low yield. it's over 40% of total low yield coil. then we need to explore every scrap in unit process for that 5 size


```python
size_req = case['index'][:5]
scrap_size = df[df['status'] != 'Acceptable'].groupby('CRC_SIZE')[['SLITING_SCRAP','PIKLING_LOSE','ROLLING_SCRAP','FINISHING_ID_SCRAP',\
                                               'FINISHING_TRIMING_LOSE']].mean().reset_index().round(2)
scrap_size[scrap_size['CRC_SIZE'].str.contains('|'.join(size_req))]
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CRC_SIZE</th>
      <th>SLITING_SCRAP</th>
      <th>PIKLING_LOSE</th>
      <th>ROLLING_SCRAP</th>
      <th>FINISHING_ID_SCRAP</th>
      <th>FINISHING_TRIMING_LOSE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0.20 X 762</td>
      <td>NaN</td>
      <td>74.84</td>
      <td>135.56</td>
      <td>325.65</td>
      <td>901.29</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.20 X 914</td>
      <td>350.0</td>
      <td>114.39</td>
      <td>230.61</td>
      <td>565.53</td>
      <td>510.09</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0.75 X 914</td>
      <td>400.0</td>
      <td>185.00</td>
      <td>133.75</td>
      <td>660.00</td>
      <td>200.00</td>
    </tr>
    <tr>
      <th>12</th>
      <td>0.90 X 1219</td>
      <td>NaN</td>
      <td>195.56</td>
      <td>157.22</td>
      <td>298.33</td>
      <td>1028.89</td>
    </tr>
    <tr>
      <th>17</th>
      <td>1.10 X 1219</td>
      <td>NaN</td>
      <td>158.33</td>
      <td>121.67</td>
      <td>228.33</td>
      <td>1451.67</td>
    </tr>
  </tbody>
</table>
</div>




```python
#if we compare with acceptable coil
scrap_size_acc = df[df['status'] == 'Acceptable'].groupby('CRC_SIZE')[['SLITING_SCRAP','PIKLING_LOSE','ROLLING_SCRAP','FINISHING_ID_SCRAP',\
                                               'FINISHING_TRIMING_LOSE']].mean().reset_index().round(2)
scrap_size_acc[scrap_size_acc['CRC_SIZE'].str.contains('|'.join(size_req))]
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CRC_SIZE</th>
      <th>SLITING_SCRAP</th>
      <th>PIKLING_LOSE</th>
      <th>ROLLING_SCRAP</th>
      <th>FINISHING_ID_SCRAP</th>
      <th>FINISHING_TRIMING_LOSE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0.20 X 762</td>
      <td>NaN</td>
      <td>74.38</td>
      <td>125.62</td>
      <td>339.38</td>
      <td>798.12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.20 X 914</td>
      <td>275.00</td>
      <td>104.54</td>
      <td>149.63</td>
      <td>398.60</td>
      <td>445.90</td>
    </tr>
    <tr>
      <th>29</th>
      <td>0.75 X 914</td>
      <td>401.25</td>
      <td>161.88</td>
      <td>112.36</td>
      <td>366.60</td>
      <td>8.33</td>
    </tr>
    <tr>
      <th>37</th>
      <td>0.90 X 1219</td>
      <td>NaN</td>
      <td>183.16</td>
      <td>129.47</td>
      <td>246.84</td>
      <td>901.58</td>
    </tr>
    <tr>
      <th>42</th>
      <td>1.10 X 1219</td>
      <td>NaN</td>
      <td>157.50</td>
      <td>150.71</td>
      <td>241.43</td>
      <td>1008.57</td>
    </tr>
  </tbody>
</table>
</div>



as the result above, the diffecence between scrap with low yield and acceptable yield is showed in rolling and finishing process. for low yield coil show that many scrap produced because of ID coil and triming process. ID coil occured because of coil with thin thickness (<0.350) it will produce ID in rolling process to avoid coil to collapse caused by no barrier for tension, so coil with thin thickness will need ID(Raw Material Thickness that wasn't rolled) to barrier the tension. and trimming process in finishing is used to cut the edge with standard width and to avoid wavy strip on edge or demage edge caused by handling. But, have rolling line have an impact for yield? we'll see below!

### Yield by Rolling Line


```python
case1 = df[df['status'] != 'Acceptable']
count_case = case1['ROLLING_LINE'].value_counts().reset_index()
yield_case = case1.groupby('ROLLING_LINE')['YIELD(%)'].mean().reset_index()

fig, (ax1, ax2) = plt.subplots(1, 2,figsize=(20,10))
fig.tight_layout(w_pad=10)
fig.suptitle('ROLLING LINE ',size=40)
fig.subplots_adjust(top=0.8)
ax1.set_title('Production Rate',size=26,pad=20)
ax2.set_title('Yield Difference',size=26,pad=20)
ax1.pie(count_case['ROLLING_LINE'], labels=count_case['index'], autopct='%1.1f%%',
        shadow=True, startangle=90,textprops={'color':'w','size': '20'})
ax1.axis('equal')
axx = ax2.bar(yield_case['ROLLING_LINE'], yield_case['YIELD(%)'], color ='g',
        width = 0.4)
ax2.axis(ymin=0.85,ymax=0.92)
ax2.set_xticklabels(yield_case['ROLLING_LINE'],size=18)
ax2.set_yticklabels(' ')
ax2.bar_label(axx,fmt='%.3f',padding=2, size=18)



```






    
![png](output_28_2.png)
    


diagrams above indices that 6HI Line have more production rate and higher yield than BLS, this result make BLS line have low capacity and low efficiency. but why this company still running this line? we must know characteristic of coil producted for each line first.


```python
group_yield = df.groupby(['ROLLING_LINE','WIDTH_RANGE','THICKNESS_RANGE'])['YIELD(%)'].mean().reset_index().sort_values(['ROLLING_LINE','WIDTH_RANGE','THICKNESS_RANGE'],ascending=False)

plt.figure(figsize=(25,10))
plt.suptitle('Yield(%) in Dimension Parameter Rolling Line', size=18)
for index, col in enumerate(group_yield['ROLLING_LINE'].unique(), start=1):
    plt.subplot(1,2,index)
    dat = (group_yield['ROLLING_LINE'] == col)
    plt.title(col,size=20)

    ax = sns.barplot(x='WIDTH_RANGE', y='YIELD(%)',hue='THICKNESS_RANGE', data=group_yield[dat],palette='gnuplot')
    plt.xlabel('Wide Range(mm)',size=14)
    plt.ylabel('Yield',size=14)
    plt.ylim(0.9,0.96)
    for container in ax.containers:
        ax.bar_label(container,fmt='%.3f',padding=2, size=12)


```


    
![png](output_30_0.png)
    


for characteristic product, 6HI still have more variance than BLS line, in graph above indices that BLS in this dataset, just process for CRC product with Characteristic less than 930 for width CRC and <0.250mm for thickness. Or can BLS line process the CRC with MILL EDGE characteristic? how Mill Edge coil effect to yield?

### Mill edge coil analysis


```python
df['MILL_EDGE'] = np.where(df['SLITING_TONNAGE_OUTPUT'].isna(),'No','Yes')
print(df.groupby('MILL_EDGE')['MILL_EDGE'].count())
print(df.groupby(['ROLLING_LINE','MILL_EDGE'])['MILL_EDGE'].count())
```

    MILL_EDGE
    No     777
    Yes    680
    Name: MILL_EDGE, dtype: int64
    ROLLING_LINE  MILL_EDGE
    6HI           No           570
                  Yes          680
    BLS           No           207
    Name: MILL_EDGE, dtype: int64
    


```python
edge_yield = df.groupby('MILL_EDGE')['YIELD(%)'].mean().reset_index()
x = pd.Series(edge_yield['YIELD(%)'])
y = edge_yield['MILL_EDGE']


plt.figure(figsize=(12, 8))
ax = x.plot(kind='barh', color='g')
ax.set_title('Mill Edge Yield',size=20)
ax.set_xlabel('Yield(%)')
ax.set_ylabel('Mill Edge')
ax.set_yticklabels(y)
ax.set_xlim(0.92, 0.96) 
rects = ax.patches


for rect in rects:

    x_value = rect.get_width()
    y_value = rect.get_y() + rect.get_height() / 2
    space = 5
    ha = 'left'
    if x_value < 0:
        space *= -1
        ha = 'right'
    label = "{:.3f}".format(x_value)


    plt.annotate(
        label,                      
        (x_value, y_value),         
        xytext=(space, 0),          
        textcoords="offset points", 
        va='center',                
        ha=ha)                      
                                    

```


    
![png](output_34_0.png)
    


Mill edge coil have significant impact for CRC yield, but BLS line still didn't produce Mill Edge CRC, that's why BLS line have lower efficient than 6hi, for distribution CRC size in Mill edge will show below.


```python
group_yield2 = df.groupby(['MILL_EDGE','WIDTH_RANGE','THICKNESS_RANGE'])['YIELD(%)'].mean().reset_index().sort_values(['MILL_EDGE','WIDTH_RANGE','THICKNESS_RANGE'],ascending=False)
ax = sns.barplot(x='THICKNESS_RANGE', y='YIELD(%)',hue='MILL_EDGE', data=group_yield2,palette='gnuplot',ci=None)
plt.xlabel('Wide Range(mm)',size=14)
plt.ylabel('Yield',size=14)
plt.ylim(0.92,0.96)
for container in ax.containers:
    ax.bar_label(container,fmt='%.3f',padding=2, size=12)


```


    
![png](output_36_0.png)
    


next, we need to know how about time rolling for every size in cold rolling process


```python
group_yield = df.groupby(['ROLLING_LINE','WIDTH_RANGE','THICKNESS_RANGE'])['ROLLING_TIME'].mean().reset_index().sort_values(['ROLLING_LINE','WIDTH_RANGE','THICKNESS_RANGE'],ascending=False)

plt.figure(figsize=(25,10))
plt.suptitle('Rolling Time in Dimension Parameter Rolling Line', size=18)
for index, col in enumerate(group_yield['ROLLING_LINE'].unique(), start=1):
    plt.subplot(1,2,index)
    dat = (group_yield['ROLLING_LINE'] == col)
    plt.title(col,size=20)

    ax = sns.barplot(x='WIDTH_RANGE', y='ROLLING_TIME',hue='THICKNESS_RANGE', data=group_yield[dat],palette='gnuplot')
    plt.xlabel('Wide Range(mm)',size=14)
    plt.ylabel('Rolling Time(minutes)',size=14)
    plt.ylim(15,96)
    for container in ax.containers:
        ax.bar_label(container,fmt='%.3f',padding=2, size=12)
```


    
![png](output_38_0.png)
    


   for rolling time in every CRC size show that time is more influenced by thickness. it's because we need more time to reduce thinner thickness, thickness reducted step by step, steel have %elongation that indice ability to deformation that occurs before a material eventually breaks, common HRC have elongation about 32%. if we reduce the thickness above that elongation. the coil will strip break.
   
   So we can decide that BLS still use because to distribute the <0.20 CRC Order from 6HI. the less 6HI process CRC with target <0.250, the more 6HI will process other size and make more productivity for company, example the time used for process **1 Coil** <0.250 with width <930 is almost same the time used for approx. **3 coil** with 0.650-1.000 thickness and <930mm width (1:3). this can be option if the company have full production, it can make time more efficient.

last but not least, some coil may not always low yield caused by process parameter, it also comes from raw material itself, the finishing line will note that if the coil produced more scrap than usual, it'll write in remark.

### defect data analysis


```python
# see the remark note
df[(df['YIELD(%)'] <0.92)]['REMARK'].value_counts()
```




    Scrap more due to Wrinkle                                                 9
    Scrap More Due To wrinkle                                                 7
    Scrap more due to wavy                                                    7
    Scrap more due to Kink Mark                                               4
    Scrap more due to R/R coil                                                4
    scrap more due to wavy & id more from blismill                            4
    Scrap more due to Wavy and crack                                          3
    scrap more due to crack from 6-himill                                     3
    Scarp more due to wavy                                                    3
    Scrap more due to kink mark                                               2
    Scrap more due to pinch mark and ridge                                    2
    Scrap more due to Wavy and colant mark                                    2
    Scrap  more due to wavy                                                   1
    Scrap more E/ CRACK ( BLISSMILL )                                         1
    Scrap more due to Crack ptpng 715 kg                                      1
    Scrap more due to over thicknes 0.24                                      1
    ID More due to wrinkle and tail wrinkle at both side                      1
    Scrap ID sleeve ada kink                                                  1
    Scrap more due to Scratch and chamber                                     1
    Scrap more due to Coil RR                                                 1
    Scrap more due to Wavy and kink mark                                      1
    scrap more due to rusty and Wavy                                          1
    Scrap Head Scratch Potong ( 300Kg )                                       1
    Scrap more due to id lamination and hole                                  1
    Scrap more due to id more blismill                                        1
    Scrap  more due to wavy and Crack                                         1
    Scrap ID more E/ CRACK ( BLISSMILL )                                      1
    Scrap naik head potong ada crack + Repair tail wve at O/S                 1
    Scrap more due to Crack 580 kg in rewending                               1
    Scrap more due to collant mark                                            1
    Scrap more CRACK ( Blismill )                                             1
    Scrap more due to tail Pinching                                           1
    Scrap more due to coil R/R                                                1
    Scrap More Due To big Crack from 6-himill                                 1
    Scrap more due to Wavy and Rol mark                                       1
    Scrap more due to wavy & Crack                                            1
    Scarp more due to crack & chamber                                         1
    Scrap More Due To wrinkle & Scratch                                       1
    Scrap more belt weaper sensor error ( rewinding ) coil rusak di potong    1
    Scrap more due crack                                                      1
    scrap more due to crack and collant mark                                  1
    Scrap naik ada crack ( Divert to catur )                                  1
    Tail cbu at both side ID more from blissmill                              1
    Scrap More Due to Problem listrik ( PLN )                                 1
    Scrap more due to R/R coil & Scrap more due to kink mark                  1
    Scrap more due to crack and hole                                          1
    Scrap more due to pinching                                                1
    Scrap more due to Wavy                                                    1
    Scrap Id More From Blissmill                                              1
    Scrap more due to coil R/R and kink                                       1
    Scrap wavy at o/s head Ǿ 830 ( Repair secrated edge damage )              1
    Scrap more due to pinching & Ridge                                        1
    Name: REMARK, dtype: int64




```python
# we counting the defect detected in coil
defects = ['wrinkle','mark','r/r','crack','cbu','kink','over','wavy','error','scratch','chamber','ridge'] # common defect
defect_count = []
for defect in defects :
    deftot = pd.DataFrame(columns=['defect', 'total'])
    deftot = deftot.append({'defect' : defect},ignore_index=True)
    deftot['total'] = df[(df['YIELD(%)'] <0.92)]['REMARK'].str.contains(defect,case=False).sum()
    defect_count.append(deftot)
    del deftot

defect_final = pd.concat(defect_count, ignore_index=False)
defect_final.reset_index(drop=True).sort_values('total',ascending=False)
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>defect</th>
      <th>total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7</th>
      <td>wavy</td>
      <td>27</td>
    </tr>
    <tr>
      <th>3</th>
      <td>crack</td>
      <td>20</td>
    </tr>
    <tr>
      <th>0</th>
      <td>wrinkle</td>
      <td>18</td>
    </tr>
    <tr>
      <th>1</th>
      <td>mark</td>
      <td>15</td>
    </tr>
    <tr>
      <th>5</th>
      <td>kink</td>
      <td>10</td>
    </tr>
    <tr>
      <th>2</th>
      <td>r/r</td>
      <td>7</td>
    </tr>
    <tr>
      <th>9</th>
      <td>scratch</td>
      <td>3</td>
    </tr>
    <tr>
      <th>11</th>
      <td>ridge</td>
      <td>3</td>
    </tr>
    <tr>
      <th>10</th>
      <td>chamber</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>cbu</td>
      <td>1</td>
    </tr>
    <tr>
      <th>6</th>
      <td>over</td>
      <td>1</td>
    </tr>
    <tr>
      <th>8</th>
      <td>error</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



so many defect that occurs in this dataset, but we have to simplified the defect to make easier to analize. defect in CRC commonly divided by 2. first, defect that occur at edge of the strip of coil, then the other occur at the center of the strip. the edge defect in the table above is : crack and wavy. if this defect occur, the weight loss will calculate in trimming lose, but if mill edge coil. it'll strip cut when edge defect occur or it'll rerolling to reduce the defect. then the other defect will calculate in ID scrap. 


## Conclusion


After we analyze the dataset of coil production, we can decide the conclusion as the point below:
    
 * Cold Rolling Mill is the one of process in Steel Manufacturing, that produced Cold Rolled Coil.
 * there are 1303 Coils that acceptable to send to customer, but there are 154 coil that below threshold.
 * lower yield caused by scrap or byproduct that producted from every process line
 * 6HI Line have more production rate  66.9% and higher yield 0.906 than BLS from total coil that below threshold
 * lower yield mostly occur for coil with thickness <0.250 and width <930mm for every cold rolling mill, its result approx  0.92%
 * lower yield occur because coil with target thickness thinner need ID from raw material thickness to avoid coil collapse caused by tension to inner diameter.
 * Mill edge coil is the coil that was edge cut from sliting line.
 * Mill edge coil have higher average yield 0.95% than non-Mill edge 0.93%.
 * BLS can be optional line for distribution coil with target thickness <0.250mm to maximize time efficient up to 30%
 * mostly defect occur is edge defect that make trimming lose in finishing line higher.


```python

```
