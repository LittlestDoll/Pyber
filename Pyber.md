

```python
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import os
import matplotlib.patches as mpatches
```


```python
city_data = os.path.join('Resources', 'city_data.csv')
city_df = pd.read_csv(city_data, encoding="utf-8")
city_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nguyenbury</td>
      <td>8</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>East Douglas</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Dawnfurt</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rodriguezburgh</td>
      <td>52</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
ride_data = os.path.join('Resources', 'ride_data.csv')
ride_df = pd.read_csv(ride_data, encoding="utf-8")
ride_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Roy</td>
      <td>2016-01-02 18:42:34</td>
      <td>17.49</td>
      <td>4036272335942</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wiseborough</td>
      <td>2016-01-21 17:35:29</td>
      <td>44.18</td>
      <td>3645042422587</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spencertown</td>
      <td>2016-07-31 14:53:22</td>
      <td>6.87</td>
      <td>2242596575892</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nguyenbury</td>
      <td>2016-07-09 04:42:44</td>
      <td>6.28</td>
      <td>1543057793673</td>
    </tr>
  </tbody>
</table>
</div>




```python
city_and_ride_df = pd.merge(city_df, ride_df, on="city", how="outer")
city_and_ride_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-08-19 04:27:52</td>
      <td>5.51</td>
      <td>6246006544795</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-04-17 06:59:50</td>
      <td>5.54</td>
      <td>7466473222333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-05-04 15:06:07</td>
      <td>30.54</td>
      <td>2140501382736</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-01-25 20:44:56</td>
      <td>12.08</td>
      <td>1896987891309</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-08-09 18:19:47</td>
      <td>17.91</td>
      <td>8784212854829</td>
    </tr>
  </tbody>
</table>
</div>




```python
city_group = city_and_ride_df.groupby('city')
avg_fare = city_group['fare'].mean()
number_rides = city_group['ride_id'].count()
drivers_city = city_group['driver_count'].unique().map(lambda x: x[0])
city_type = city_group['type'].unique().map(lambda x: x[0])

colors = []
for i, type in city_type.iteritems():
    if type == "Urban":
        colors.append("Gold")
    elif type == "Suburban":
        colors.append("Lightskyblue")
    elif type == "Rural":
        colors.append("Lightcoral")
    else:
        colors.append("Black")   
        
city_types = ['Urban','Suburban','Rural']
city_types_colors = ['Gold','Lightskyblue','Lightcoral']
recs = []
for i in range(0,len(city_types_colors)):
    recs.append(mpatches.Rectangle((0,0),1,1,fc=city_types_colors[i]))
```


```python
plt.scatter(number_rides, avg_fare, marker='o', 
            sizes=drivers_city, alpha=0.25, edgecolors="black",
            c=colors)

plt.xlim(0, 40)
plt.ylim(15, 45)

plt.xlabel("Total Number of Rides (Per City)")
plt.ylabel("Average Fare ($)")
plt.title("Pyber Ride Sharing Data")

plt.legend(recs, city_types ,loc="best")  

plt.show()
```


![png](output_5_0.png)



```python
city_type_group = city_and_ride_df.groupby('type')
colors = ['Lightcoral','Lightskyblue', 'Gold']
explode = [0, 0.02, 0.05]

count_fare = city_type_group['fare'].sum()
plt.title("% of Total Fares by City Type")
plt.pie(count_fare, explode=explode, labels=count_fare.keys(), 
        colors=colors, autopct="%1.1f%%", startangle=90)
plt.axis("equal")
plt.show()
```


![png](output_6_0.png)



```python
#% of Total Rides by City Type
count_rides = city_type_group['ride_id'].count()

plt.title("% of Total Rides by City Type")
plt.pie(count_rides, explode=explode, labels=count_rides.keys(), 
        colors=colors, autopct="%1.1f%%", startangle=90)
plt.axis("equal")
plt.show()

plt.show()
```


![png](output_7_0.png)



```python
#% of Total Drivers by City Type
count_drivers = city_type_group['driver_count'].sum()

plt.title("% of Total Drivers by City Type")
plt.pie(count_drivers, explode=explode, labels=count_drivers.keys(), 
        colors=colors, autopct="%1.1f%%", startangle=90)
plt.axis("equal")
plt.show()

plt.show()
```


![png](output_8_0.png)



```python
#Observable trends
# First: People in rural and suburban areas pay higher fares than people in urban areas. 
# Presumably, this would be because of distance travelled - likely further distances than those in urban areas.
# Second: There is a higher demand for rides the closer you get to the urban center.
# Third: Logically, there is a much higher concentration of drivers in urban areas.
# Fourth: The higher the demand for rides, the lower the cost per ride.
```
