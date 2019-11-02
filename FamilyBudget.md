

```python
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns
import calendar
%matplotlib inline
```


```python
df = pd.read_excel(r'C:\Users\denis.gorev\Desktop\life\other\details.xlsx',encoding='cp1251')
df.drop(['Номер карты/счета/договора', 'Валюта счета', 'Валюта операции', 'Сумма пересчитанная в валюту счета', 'Дата обработки'], axis = 1, inplace = True)
df = df[df['Основание'] != 'Перевод на другую карту (Р2Р)']
df['Место траты'] = df['Основание'].str.split(' ').str[0]
df['День месяца'] = df['Дата операции'].dt.day
df['День недели'] = df['Дата операции'].dt.dayofweek
df = df[df['Сумма операции']<0]
#df.groupby('День месяца')['Сумма операции'].sum()
x1 = np.sort(df['День месяца'].unique())
x2 = np.sort(df['День недели'].unique())
day_of_month = df.groupby('День месяца')['Сумма операции'].sum()*(-1)
day_of_week = df.groupby('День недели')['Сумма операции'].sum()*(-1)

fig,axes = plt.subplots(figsize=(14,4), nrows = 1,ncols = 2)
plt.tight_layout()
axes[0].set_xticks(list(range(0,32)))
axes[0].bar(x1,day_of_month)
axes[0].set_title('Распределение трат по дням месяца')

axes[1].set_xticks(list(range(0,7)))
axes[1].bar(x2, day_of_week)
axes[1].set_title('Распределение трат по дням недели')
axes[1].set_xticklabels(list(calendar.day_name), fontsize=12)

#sns.barplot(x = x1, y = day_of_month)

```




    [Text(0.0, 0, 'Monday'),
     Text(0.2, 0, 'Tuesday'),
     Text(0.4, 0, 'Wednesday'),
     Text(0.6000000000000001, 0, 'Thursday'),
     Text(0.8, 0, 'Friday'),
     Text(1.0, 0, 'Saturday'),
     Text(0, 0, 'Sunday')]




![png](output_1_1.png)



```python
df[df['День месяца']==5]
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
      <th>Дата операции</th>
      <th>Сумма операции</th>
      <th>Основание</th>
      <th>Статус</th>
      <th>Место траты</th>
      <th>День месяца</th>
      <th>День недели</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>32</th>
      <td>2019-10-05 22:28:00</td>
      <td>-3354.0</td>
      <td>IKEA DOM 4 CASH LINE</td>
      <td>Исполнено</td>
      <td>IKEA</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>33</th>
      <td>2019-10-05 15:48:00</td>
      <td>-465.5</td>
      <td>PYATEROCHKA 198</td>
      <td>Исполнено</td>
      <td>PYATEROCHKA</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>34</th>
      <td>2019-10-05 15:34:00</td>
      <td>-70.0</td>
      <td>KONDITERMAG</td>
      <td>Исполнено</td>
      <td>KONDITERMAG</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>35</th>
      <td>2019-10-05 15:33:00</td>
      <td>-248.0</td>
      <td>KONDITERMAG</td>
      <td>Исполнено</td>
      <td>KONDITERMAG</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>36</th>
      <td>2019-10-05 01:06:00</td>
      <td>-56000.0</td>
      <td>Перевод на другую карту (Р2Р)</td>
      <td>Исполнено</td>
      <td>Перевод</td>
      <td>5</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
df = pd.read_excel(r'C:\Users\denis.gorev\Desktop\life\other\details_3.xlsx',encoding='cp1251')
df.drop(['Номер карты/счета/договора', 'Валюта счета', 'Валюта операции', 'Сумма пересчитанная в валюту счета', 'Дата обработки'], axis = 1, inplace = True)
df = df[df['Основание'] != 'Перевод на другую карту (Р2Р)']
#df['Место траты'] = df['Основание'].str.split(' ').str[0]
df['Месяцы'] = df['Дата операции'].dt.month
df['День месяца'] = df['Дата операции'].dt.day
df['День недели'] = df['Дата операции'].dt.dayofweek
df = df[df['Сумма операции']<0]
#df.groupby('День месяца')['Сумма операции'].sum()
x1 = np.sort(df['День месяца'].unique())
x2 = np.sort(df['День недели'].unique())
x3 = np.sort(df['Месяцы'].unique())
day_of_month = df.groupby('День месяца')['Сумма операции'].median()*(-1)
day_of_week = df.groupby('День недели')['Сумма операции'].median()*(-1)
month = df.groupby('Месяцы')['Сумма операции'].sum()*(-1)

fig,axes = plt.subplots(figsize=(16,4), nrows = 1,ncols = 3)
plt.tight_layout()

axes[2].set_xticks(list(range(3,11)))
axes[2].bar(x3,month)
axes[2].set_title('Распределение трат по месяцам')
axes[2].set_xticklabels(['мрт', 'апр', 'май', 'июн', 'июл', 'авг', 'снт', 'окт'], fontsize=12)

axes[0].set_xticks(list(range(1,32)))
axes[0].bar(x1,day_of_month)
axes[0].set_title('Распределение трат по дням месяца (средние)')

axes[1].set_xticks(list(range(0,7)))
axes[1].bar(x2, day_of_week)
axes[1].set_title('Распределение трат по дням недели (средние)')
axes[1].set_xticklabels([r'пн', 'вт', 'ср', 'чт', 'пт', 'cб', 'вс'], fontsize=12)
```




    [Text(0.0, 0, 'пн'),
     Text(0.2, 0, 'вт'),
     Text(0.4, 0, 'ср'),
     Text(0.6000000000000001, 0, 'чт'),
     Text(0.8, 0, 'пт'),
     Text(1.0, 0, 'cб'),
     Text(0, 0, 'вс')]




![png](output_3_1.png)



```python
#df[df['День месяца'] == 31]['Сумма операции'].mean()
df[df['День месяца'] == 16]
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
      <th>Дата операции</th>
      <th>Сумма операции</th>
      <th>Основание</th>
      <th>Статус</th>
      <th>Месяцы</th>
      <th>День месяца</th>
      <th>День недели</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>56</th>
      <td>2019-09-16 21:11:12</td>
      <td>-819.96</td>
      <td>VKUSVILL 1559_1</td>
      <td>Исполнено</td>
      <td>9</td>
      <td>16</td>
      <td>0</td>
    </tr>
    <tr>
      <th>90</th>
      <td>2019-08-16 19:55:20</td>
      <td>-716.67</td>
      <td>BILLA PARKOVAYA</td>
      <td>Исполнено</td>
      <td>8</td>
      <td>16</td>
      <td>4</td>
    </tr>
    <tr>
      <th>91</th>
      <td>2019-08-16 18:50:44</td>
      <td>-1233.00</td>
      <td>DELIVERY-CLUB</td>
      <td>Исполнено</td>
      <td>8</td>
      <td>16</td>
      <td>4</td>
    </tr>
    <tr>
      <th>127</th>
      <td>2019-07-16 20:37:58</td>
      <td>-784.00</td>
      <td>VKUSVILL 1122_3</td>
      <td>Исполнено</td>
      <td>7</td>
      <td>16</td>
      <td>1</td>
    </tr>
    <tr>
      <th>128</th>
      <td>2019-07-16 17:44:29</td>
      <td>-408.00</td>
      <td>KAFE PRAYM PR.MIRA</td>
      <td>Исполнено</td>
      <td>7</td>
      <td>16</td>
      <td>1</td>
    </tr>
    <tr>
      <th>129</th>
      <td>2019-07-16 17:25:54</td>
      <td>-18307.00</td>
      <td>"VF SERVISES"</td>
      <td>Исполнено</td>
      <td>7</td>
      <td>16</td>
      <td>1</td>
    </tr>
    <tr>
      <th>168</th>
      <td>2019-06-16 16:48:01</td>
      <td>-691.74</td>
      <td>VKUSVILL 1122_2</td>
      <td>Исполнено</td>
      <td>6</td>
      <td>16</td>
      <td>6</td>
    </tr>
    <tr>
      <th>169</th>
      <td>2019-06-16 00:00:02</td>
      <td>-223.00</td>
      <td>ORANGE</td>
      <td>Исполнено</td>
      <td>6</td>
      <td>16</td>
      <td>6</td>
    </tr>
    <tr>
      <th>209</th>
      <td>2019-05-16 21:56:03</td>
      <td>-333.32</td>
      <td>VKUSVILL 1122_1</td>
      <td>Исполнено</td>
      <td>5</td>
      <td>16</td>
      <td>3</td>
    </tr>
    <tr>
      <th>259</th>
      <td>2019-04-16 18:47:32</td>
      <td>-409.15</td>
      <td>STANEM DRUZYAMI</td>
      <td>Исполнено</td>
      <td>4</td>
      <td>16</td>
      <td>1</td>
    </tr>
    <tr>
      <th>260</th>
      <td>2019-04-16 12:13:17</td>
      <td>-868.57</td>
      <td>VKUSVILL 1303_2</td>
      <td>Исполнено</td>
      <td>4</td>
      <td>16</td>
      <td>1</td>
    </tr>
    <tr>
      <th>293</th>
      <td>2019-03-16 16:54:20</td>
      <td>-807.00</td>
      <td>ATLANTFARM</td>
      <td>Исполнено</td>
      <td>3</td>
      <td>16</td>
      <td>5</td>
    </tr>
    <tr>
      <th>294</th>
      <td>2019-03-16 10:16:47</td>
      <td>-583.67</td>
      <td>BILLA PARKOVAYA</td>
      <td>Исполнено</td>
      <td>3</td>
      <td>16</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>


