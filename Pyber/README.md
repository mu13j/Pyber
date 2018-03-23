
# Analysis

* Urban cities generally have both more riders and drivers, accounting for a majority in both number of drivers and total fare.
* Rural cities, while sparsely used, can be the most expensive.
* Suburban cities generally fall in between rural cities and urban cities in each of the measured variables




```python
#Dependencies
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
```


```python
#CSV Paths
csv= "raw_data/city_data.csv"
csv2='raw_data/ride_data.csv'
```


```python
#Import Each CSV into a dataframe, finding total fares
a=pd.read_csv(csv)
b=pd.read_csv(csv2)
c=b.groupby('city').sum()
c=c.reset_index()
```


```python
#Merging the dataframes
d=pd.merge(a,c,on='city',how="outer")
del d['ride_id']
d=d.rename(columns={'fare':'Total Fare'})
```


```python
#Counting number of rides in each city
c=b.groupby('city').count()
c=c.reset_index()
del c['ride_id']
del c['fare']
```


```python
#Creating a column in dataframe to help color the graphs
e=pd.merge(d,c,on='city',how="outer")
e=e.rename(columns={'date':'Total Number of Rides','driver_count':'Total Number of Drivers'})
e['Average Fare']=e['Total Fare']/e['Total Number of Rides']
colors=[]
for i in e['type']:
    if i=='Urban':
        colors.append('lightcoral')
    elif i=='Suburban':
        colors.append('lightskyblue')
    else:
        colors.append('gold')
e['color']=colors
groups=e.groupby('type')
```


```python
#Bubble Chart
labels=[]
sns.set(color_codes=True)
plt.grid()
for name, group in groups:
    plt.scatter(group['Total Number of Rides'],group['Average Fare'],s=5*group['Total Number of Drivers'],alpha=0.5,color=group['color'])
    labels.append(name)
plt.xlabel('Total Rides Per City')
plt.ylabel('Average Fare Per City ($)')
plt.title('Pyber Ride Sharing Data (2016)')
plt.grid(axis='both')
plt.legend(labels)
plt.annotate('    Circle size correlates with driver count of city',xy=(36,40))
plt.show()
```


![png](output_7_0.png)



```python
#Pie Chart for % Total Fares By City Types
plt.title("% of Total Fares By City Type")
explode = (0, 0, 0.1)
f=groups.sum()
plt.pie(f['Total Fare'],labels=labels,explode=explode, autopct="%1.1f%%",shadow=True,startangle=90, colors=['gold','lightskyblue','lightcoral'])
plt.axis("equal")
plt.show()
```


![png](output_8_0.png)



```python
#Pie Chart for Total Rides By City Types
plt.title("% of Total Rides By City Type")
plt.pie(f['Total Number of Rides'],labels=labels,explode=explode, autopct="%1.1f%%",shadow=True,startangle=90, colors=['gold','lightskyblue','lightcoral'])
plt.axis("equal")
plt.show()
```


![png](output_9_0.png)



```python
#Pie Chart for Total Drivers By City Types
plt.title("% of Total Drivers By City Type")
plt.pie(f['Total Number of Drivers'],labels=labels,explode=explode, autopct="%1.1f%%",shadow=True,startangle=90, colors=['gold','lightskyblue','lightcoral'])
plt.axis("equal")
plt.show()
```


![png](output_10_0.png)

