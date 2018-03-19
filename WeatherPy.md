
<a id='top_cell'></a>

<h2>WeatherPy Analysis</h2>
<blockquote><p>For this project, I created a Python script to visualize the weather of 500+ cities across the world of varying distance from the equator. To accomplish this, I utilized a simple Python library called [citipy](https://pypi.python.org/pypi/citipy) and the [OpenWeatherMap API](https://openweathermap.org/api). </p></blockquote>

<strong>Analysis</strong>
<ul>
<li>From the [City Latitude vs. Max Temperature graph](#another_cell), I can see that a lower absolute value of latitudes show higher temperature. This means that areas closer to the equator receive higher temperatures, whereas areas farther away from the equator receive lower temperatures. </li>
<li>From the [Latitude vs. Humidity Plot](#hum_cell), it appears that the average humidity is higher where the latitude is higher (northern hemisphere). However, my data is takien from the middle of March 2018. This may be due to the seasons. A more accurate assessment would have to be taken with historical data from the different times of the year.</li>
<li>From the [Latitude vs Windspeed Plot](#wind_cell), the lower wind speeds appeared closer to the equator (lower absolute values of latitudes). By contrast, higher wind speeds appear farther from the equator.</li>
<li>There aren't any visible patterns in the [Latitude vs Cloudiness plot](#cloud_cell).</li>
</ul>


<p>Additional Resources:</p>
<ul>
<li>[geographic coordinate system](http://desktop.arcgis.com/en/arcmap/10.3/guide-books/map-projections/about-geographic-coordinate-systems.htm)</li>
<li>[citypy](https://github.com/wingchen/citipy)</li>
</ul>


```python
import requests
from citipy import citipy
import pandas as pd
import numpy as np
import os
import matplotlib.pyplot as plt
import logging #dir(logging)
import seaborn as sns
```


```python
api_address = "http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q="
api_key = '224f7b3f56aba61567ec7c77e410d506'
```


```python
# Creating coordinate points. A point is referenced by its longitude and latitude values.
#Utilize numpy.random to generate random latitudes and longitudes.
#Latitude values are measured relative to the equator and range from -90° at the South Pole to +90° at the North Pole. 
#Longitude values are measured relative to the prime meridian. They range from -180° when traveling west to 180° when traveling east.
#Since the size is 2500, 2,500 values will be generated.
#These points will append to the coordinates series/list.
cities = []
coordinates = []

lat = np.random.uniform(low=-90, high=90, size=2500)
lng = np.random.uniform(low=-180, high=180, size=2500)

for x in range(0,len(lat)):
    coordinates.append((lat[x], lng[x]))
    
for coordinate_pair in coordinates:
    lat, lon = coordinate_pair
    cities.append(citipy.nearest_city(lat, lon))

# Creating DataFrame
# Find the cities.
cities_df = pd.DataFrame(cities)
cities_df["City Name"] = ""
cities_df["Country Code"] = ""
cities_df.head()
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
      <th>0</th>
      <th>City Name</th>
      <th>Country Code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>&lt;citipy.citipy.City object at 0x0000029B032E84E0&gt;</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>&lt;citipy.citipy.City object at 0x0000029B04D676D8&gt;</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>&lt;citipy.citipy.City object at 0x0000029B03910748&gt;</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>&lt;citipy.citipy.City object at 0x0000029B04A26908&gt;</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>&lt;citipy.citipy.City object at 0x0000029B02FDFCF8&gt;</td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>




```python
print(coordinates[1])
print(cities[1])
```

    (-18.202419988366501, 179.80155485946676)
    <citipy.citipy.City object at 0x0000029B04D676D8>
    


```python
coordinates_df = pd.DataFrame({"Lat": lat,
                              "Lon": lng})
coordinates_df.head()
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
      <th>Lat</th>
      <th>Lon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-52.730814</td>
      <td>-112.630807</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-52.730814</td>
      <td>179.801555</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-52.730814</td>
      <td>135.652084</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-52.730814</td>
      <td>-178.948781</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-52.730814</td>
      <td>-74.257536</td>
    </tr>
  </tbody>
</table>
</div>




```python
coord_df = coordinates_df
coord_df.head()
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
      <th>Lat</th>
      <th>Lon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-52.730814</td>
      <td>-112.630807</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-52.730814</td>
      <td>179.801555</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-52.730814</td>
      <td>135.652084</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-52.730814</td>
      <td>-178.948781</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-52.730814</td>
      <td>-74.257536</td>
    </tr>
  </tbody>
</table>
</div>




```python
coord_df['City Name'] = ""
coord_df['Country'] = ""
coord_df['Temperature (F)'] = ""
coord_df['Humidity (%)'] = ""
coord_df['Cloudiness (%)'] = ""
coord_df['Wind Speed (mph)'] = ""

coord_df.head()
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
      <th>Lat</th>
      <th>Lon</th>
      <th>City Name</th>
      <th>Country</th>
      <th>Temperature (F)</th>
      <th>Humidity (%)</th>
      <th>Cloudiness (%)</th>
      <th>Wind Speed (mph)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-52.730814</td>
      <td>-112.630807</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>-52.730814</td>
      <td>179.801555</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>-52.730814</td>
      <td>135.652084</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>-52.730814</td>
      <td>-178.948781</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>-52.730814</td>
      <td>-74.257536</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>




```python
#pandas iloc 
for index, row in cities_df.iterrows():
    row["City Name"] = cities_df.iloc[index,0].city_name
    row["Country Code"] = cities_df.iloc[index,0].country_code
    #iloc[index,0]
print(cities_df)
# Drop duplicate cities.
cities_df.drop_duplicates(['City Name', 'Country Code'], inplace=True)
cities_df.reset_index(inplace=True)

# # Delete unnecessary columns
del cities_df[0]
del cities_df['index']

cities_df.head()
```

                                                          0         City Name  \
    0     <citipy.citipy.City object at 0x0000029B032E84E0>      punta arenas   
    1     <citipy.citipy.City object at 0x0000029B04D676D8>           isangel   
    2     <citipy.citipy.City object at 0x0000029B03910748>              biak   
    3     <citipy.citipy.City object at 0x0000029B04A26908>             kapaa   
    4     <citipy.citipy.City object at 0x0000029B02FDFCF8>           ushuaia   
    5     <citipy.citipy.City object at 0x0000029B02FF64E0>       neulengbach   
    6     <citipy.citipy.City object at 0x0000029B0409A8D0>           mataura   
    7     <citipy.citipy.City object at 0x0000029B045BF8D0>        chokurdakh   
    8     <citipy.citipy.City object at 0x0000029B03018D68>           ngukurr   
    9     <citipy.citipy.City object at 0x0000029B0323A080>       clyde river   
    10    <citipy.citipy.City object at 0x0000029B04A264E0>              hilo   
    11    <citipy.citipy.City object at 0x0000029B030047F0>            albany   
    12    <citipy.citipy.City object at 0x0000029B03E79A58>         hithadhoo   
    13    <citipy.citipy.City object at 0x0000029B0324A320>           halifax   
    14    <citipy.citipy.City object at 0x0000029B035DE208>          carballo   
    15    <citipy.citipy.City object at 0x0000029B0374FB00>  illoqqortoormiut   
    16    <citipy.citipy.City object at 0x0000029B03E3F748>          tsihombe   
    17    <citipy.citipy.City object at 0x0000029B04897A90>             vaini   
    18    <citipy.citipy.City object at 0x0000029B04D81C50>           margate   
    19    <citipy.citipy.City object at 0x0000029B04097D68>            atuona   
    20    <citipy.citipy.City object at 0x0000029B049922E8>        fort smith   
    21    <citipy.citipy.City object at 0x0000029B04D676D8>           isangel   
    22    <citipy.citipy.City object at 0x0000029B03E3F470>         taolanaro   
    23    <citipy.citipy.City object at 0x0000029B045D7048>        dzerzhinsk   
    24    <citipy.citipy.City object at 0x0000029B045BF8D0>        chokurdakh   
    25    <citipy.citipy.City object at 0x0000029B032E84E0>      punta arenas   
    26    <citipy.citipy.City object at 0x0000029B045939E8>     belushya guba   
    27    <citipy.citipy.City object at 0x0000029B0409E240>           rikitea   
    28    <citipy.citipy.City object at 0x0000029B02FDFCF8>           ushuaia   
    29    <citipy.citipy.City object at 0x0000029B0304CCC0>         bathsheba   
    ...                                                 ...               ...   
    2470  <citipy.citipy.City object at 0x0000029B047EBF60>            barbar   
    2471  <citipy.citipy.City object at 0x0000029B0374FF98>              nuuk   
    2472  <citipy.citipy.City object at 0x0000029B0409E240>           rikitea   
    2473  <citipy.citipy.City object at 0x0000029B037530B8>           qaanaaq   
    2474  <citipy.citipy.City object at 0x0000029B0409E240>           rikitea   
    2475  <citipy.citipy.City object at 0x0000029B04577358>             andra   
    2476  <citipy.citipy.City object at 0x0000029B03FA45C0>           magaria   
    2477  <citipy.citipy.City object at 0x0000029B03293208>       yellowknife   
    2478  <citipy.citipy.City object at 0x0000029B047EBB00>          victoria   
    2479  <citipy.citipy.City object at 0x0000029B03C80EF0>       mitsamiouli   
    2480  <citipy.citipy.City object at 0x0000029B043A4550>        abu samrah   
    2481  <citipy.citipy.City object at 0x0000029B03E646A0>           maghama   
    2482  <citipy.citipy.City object at 0x0000029B0409E240>           rikitea   
    2483  <citipy.citipy.City object at 0x0000029B03C087F0>         gushikawa   
    2484  <citipy.citipy.City object at 0x0000029B0409A8D0>           mataura   
    2485  <citipy.citipy.City object at 0x0000029B04D677F0>        luganville   
    2486  <citipy.citipy.City object at 0x0000029B0326F5C0>     prince george   
    2487  <citipy.citipy.City object at 0x0000029B03D4C278>          coos bay   
    2488  <citipy.citipy.City object at 0x0000029B047EB7B8>           honiara   
    2489  <citipy.citipy.City object at 0x0000029B030047F0>            albany   
    2490  <citipy.citipy.City object at 0x0000029B02FDFCF8>           ushuaia   
    2491  <citipy.citipy.City object at 0x0000029B04985710>            barrow   
    2492  <citipy.citipy.City object at 0x0000029B032E84E0>      punta arenas   
    2493  <citipy.citipy.City object at 0x0000029B032273C8>           aklavik   
    2494  <citipy.citipy.City object at 0x0000029B0388D828>      port-de-paix   
    2495  <citipy.citipy.City object at 0x0000029B03289128>       tuktoyaktuk   
    2496  <citipy.citipy.City object at 0x0000029B0409E240>           rikitea   
    2497  <citipy.citipy.City object at 0x0000029B045939E8>     belushya guba   
    2498  <citipy.citipy.City object at 0x0000029B040377F0>             bluff   
    2499  <citipy.citipy.City object at 0x0000029B032DBEF0>            avarua   
    
         Country Code  
    0              cl  
    1              vu  
    2              id  
    3              us  
    4              ar  
    5              at  
    6              pf  
    7              ru  
    8              au  
    9              ca  
    10             us  
    11             au  
    12             mv  
    13             ca  
    14             es  
    15             gl  
    16             mg  
    17             to  
    18             za  
    19             pf  
    20             us  
    21             vu  
    22             mg  
    23             ru  
    24             ru  
    25             cl  
    26             ru  
    27             pf  
    28             ar  
    29             bb  
    ...           ...  
    2470           sd  
    2471           gl  
    2472           pf  
    2473           gl  
    2474           pf  
    2475           ru  
    2476           ne  
    2477           ca  
    2478           sc  
    2479           km  
    2480           qa  
    2481           mr  
    2482           pf  
    2483           jp  
    2484           pf  
    2485           vu  
    2486           ca  
    2487           us  
    2488           sb  
    2489           au  
    2490           ar  
    2491           us  
    2492           cl  
    2493           ca  
    2494           ht  
    2495           ca  
    2496           pf  
    2497           ru  
    2498           nz  
    2499           ck  
    
    [2500 rows x 3 columns]
    




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
      <th>City Name</th>
      <th>Country Code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>punta arenas</td>
      <td>cl</td>
    </tr>
    <tr>
      <th>1</th>
      <td>isangel</td>
      <td>vu</td>
    </tr>
    <tr>
      <th>2</th>
      <td>biak</td>
      <td>id</td>
    </tr>
    <tr>
      <th>3</th>
      <td>kapaa</td>
      <td>us</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ushuaia</td>
      <td>ar</td>
    </tr>
  </tbody>
</table>
</div>




```python
cities_df.head()
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
      <th>City Name</th>
      <th>Country Code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>punta arenas</td>
      <td>cl</td>
    </tr>
    <tr>
      <th>1</th>
      <td>isangel</td>
      <td>vu</td>
    </tr>
    <tr>
      <th>2</th>
      <td>biak</td>
      <td>id</td>
    </tr>
    <tr>
      <th>3</th>
      <td>kapaa</td>
      <td>us</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ushuaia</td>
      <td>ar</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Adding columns for values for cities_df, not coordinates_df. 
cities_df['Latitude'] = ""
cities_df['Longitude'] = ""
cities_df['Temperature (F)'] = ""
cities_df['Humidity (%)'] = ""
cities_df['Cloudiness (%)'] = ""
cities_df['Wind Speed (mph)'] = ""

cities_df.head()
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
      <th>City Name</th>
      <th>Country Code</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature (F)</th>
      <th>Humidity (%)</th>
      <th>Cloudiness (%)</th>
      <th>Wind Speed (mph)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>punta arenas</td>
      <td>cl</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>isangel</td>
      <td>vu</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>biak</td>
      <td>id</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>kapaa</td>
      <td>us</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>ushuaia</td>
      <td>ar</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>




```python
# Save config information.
url = "http://api.openweathermap.org/data/2.5/weather?"
units = "imperial"

# Build partial query URL
#query_url = f"{url}appid={api_key}&units={units}&q="
query_url = f"{url}appid={api_key}&units={units}"
```


```python
api_key
```




    '224f7b3f56aba61567ec7c77e410d506'




```python
# #Logging option.
# LOG_FORMAT = "%(message)s"
# logging.basicConfig(filename="C:\\Users\\nisha\\Desktop\\Week7\\weatherpy.log" , level = logging.INFO, format = LOG_FORMAT, 
#                     filemode = 'w')
# #logger = getLogger()
# logger = logging.getLogger(__name__)
# logger.info("Beginning Data Retrieval")
# logger.info("---------------------------------")

# for index, row in cities_df.iterrows():
#     #for index, row in cities_df.iterrows():
# #for row in cities_df:
# #.items
#     #to test print row and two of the series from that row:
#     logger.info(index, row, row['City Name'], row['Country Code']) 
#     logger.info("Processing Record " + str(index) + "of Set 1 | " + str(row['City Name']))
#      #url = "http://api.openweathermap.org/data/2.5/weather?q=%s,%s&units=imperial&appid=%s" % (row['City Name'], row['Country Code'], api_key)
# #     params = {
# #         "key": api_key
# #     }
#     url = "http://api.openweathermap.org/data/2.5/weather?appid=%s&q=%s&units=imperial" % (api_key, row['City Name'])
#     #print(url)
#     weather_data = requests.get(url)
#     #.json() above
#     logger.info(weather_data.url)
#     weather_data = weather_data.json()
    
#     try:
#     #Append the data with exception handling in case data doesn't exist for a city. 
        
#         lati = weather_data['coord']['lat']
#         long = weather_data['coord']['lon']
#         temper = weather_data['main']['temp']
#         humi = weather_data['main']['humidity']
#         cloud = weather_data['clouds']['all']
#         wind = weather_data['wind']['speed']
        
#         cities_df.set_value(index, "Latitude", lati)
#         cities_df.set_value(index, "Longitude", long)
#         cities_df.set_value(index, "Temperature (F)", temper)
#         cities_df.set_value(index, "Humidity (%)", humi)
#         cities_df.set_value(index, "Cloudiness (%)", cloud)
#         cities_df.set_value(index, "Wind Speed (mph)", wind)
           
#     except:
#         logger.info("Error")
#         continue
    
    
#     #use exception handling in case the city doesn't match
# print("---------------------------------")
# print("Data Retrieval Complete")
# print("---------------------------------")
```


```python
print("Beginning Data Retrieval")
print("---------------------------------")

for index, row in cities_df.iterrows():
    #for index, row in cities_df.iterrows():
#for row in cities_df:
#.items
    #to test print row and two of the series from that row:
    print(index, row, row['City Name'], row['Country Code']) 
    print("Processing Record " + str(index) + "of Set 1 | " + str(row['City Name']))
     #url = "http://api.openweathermap.org/data/2.5/weather?q=%s,%s&units=imperial&appid=%s" % (row['City Name'], row['Country Code'], api_key)
#     params = {
#         "key": api_key
#     }
    url = "http://api.openweathermap.org/data/2.5/weather?appid=%s&q=%s&units=imperial" % (api_key, row['City Name'])
    print(url)
    weather_data = requests.get(url)
    #.json() above
    print(weather_data.url)
    weather_data = weather_data.json()
    #Append
    
    try:
    #Append the data with exception handling in case data doesn't exist for a city. 
#         cities_df.loc[index, 'Latitude'] = weather_data['coord']['lat']
#         cities_df.loc[index,'Longitude'] = weather_data['coord']['lon']
#         cities_df.loc[index,'Temperature (F)'] = weather_data['main']['temp']
#         cities_df.loc[index,'Humidity (%)'] = weather_data['main']['humidity']
#         cities_df.loc[index,'Cloudiness (%)'] = weather_data['clouds']['all']
#         cities_df.loc[index,'Wind Speed (mph)'] = weather_data['wind']['speed']
        
        lati = weather_data['coord']['lat']
        long = weather_data['coord']['lon']
        temper = weather_data['main']['temp']
        humi = weather_data['main']['humidity']
        cloud = weather_data['clouds']['all']
        wind = weather_data['wind']['speed']
        
        cities_df.set_value(index, "Latitude", lati)
        cities_df.set_value(index, "Longitude", long)
        cities_df.set_value(index, "Temperature (F)", temper)
        cities_df.set_value(index, "Humidity (%)", humi)
        cities_df.set_value(index, "Cloudiness (%)", cloud)
        cities_df.set_value(index, "Wind Speed (mph)", wind)
        
#Temperature (F)	Humidity (%)	Cloudiness (%)	Wind Speed (mph)
#         row['Longitude'] = weather_data['coord']['lon']
#         row['Temperature (F)'] = weather_data['main']['temp']
#         row['Humidity (%)'] = weather_data['main']['humidity']
#         row['Cloudiness (%)'] = weather_data['clouds']['all']
#         row['Wind Speed (mph)'] = weather_data['wind']['speed']
    
    except:
        print("Error")
        continue
    
    
    #use exception handling in case the city doesn't match
print("---------------------------------")
print("Data Retrieval Complete")
print("---------------------------------")
```

    Beginning Data Retrieval
    ---------------------------------
    0 City Name           punta arenas
    Country Code                  cl
    Latitude                  -53.16
    Longitude                 -70.91
    Temperature (F)               41
    Humidity (%)                  65
    Cloudiness (%)                40
    Wind Speed (mph)            6.93
    Name: 0, dtype: object punta arenas cl
    Processing Record 0of Set 1 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=punta arenas&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=punta%20arenas&units=imperial
    1 City Name           isangel
    Country Code             vu
    Latitude             -19.55
    Longitude            169.27
    Temperature (F)       80.36
    Humidity (%)            100
    Cloudiness (%)           36
    Wind Speed (mph)       9.46
    Name: 1, dtype: object isangel vu
    Processing Record 1of Set 1 | isangel
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=isangel&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=isangel&units=imperial
    2 City Name             biak
    Country Code            id
    Latitude             -0.91
    Longitude           122.88
    Temperature (F)      79.28
    Humidity (%)           100
    Cloudiness (%)           8
    Wind Speed (mph)       2.3
    Name: 2, dtype: object biak id
    Processing Record 2of Set 1 | biak
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=biak&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=biak&units=imperial
    3 City Name            kapaa
    Country Code            us
    Latitude             22.08
    Longitude          -159.32
    Temperature (F)      76.03
    Humidity (%)            78
    Cloudiness (%)          75
    Wind Speed (mph)     18.34
    Name: 3, dtype: object kapaa us
    Processing Record 3of Set 1 | kapaa
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kapaa&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kapaa&units=imperial
    4 City Name           ushuaia
    Country Code             ar
    Latitude             -54.81
    Longitude            -68.31
    Temperature (F)        37.4
    Humidity (%)             93
    Cloudiness (%)           75
    Wind Speed (mph)      27.51
    Name: 4, dtype: object ushuaia ar
    Processing Record 4of Set 1 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ushuaia&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ushuaia&units=imperial
    5 City Name           neulengbach
    Country Code                 at
    Latitude                   48.2
    Longitude                 15.91
    Temperature (F)              23
    Humidity (%)                 73
    Cloudiness (%)               90
    Wind Speed (mph)          11.41
    Name: 5, dtype: object neulengbach at
    Processing Record 5of Set 1 | neulengbach
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=neulengbach&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=neulengbach&units=imperial
    6 City Name           mataura
    Country Code             pf
    Latitude             -46.19
    Longitude            168.86
    Temperature (F)       72.71
    Humidity (%)             61
    Cloudiness (%)           36
    Wind Speed (mph)       20.2
    Name: 6, dtype: object mataura pf
    Processing Record 6of Set 1 | mataura
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mataura&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mataura&units=imperial
    7 City Name           chokurdakh
    Country Code                ru
    Latitude                 70.62
    Longitude                147.9
    Temperature (F)         -28.36
    Humidity (%)                46
    Cloudiness (%)               8
    Wind Speed (mph)          3.42
    Name: 7, dtype: object chokurdakh ru
    Processing Record 7of Set 1 | chokurdakh
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=chokurdakh&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=chokurdakh&units=imperial
    9 City Name           clyde river
    Country Code                 ca
    Latitude                  70.47
    Longitude                -68.59
    Temperature (F)          -14.81
    Humidity (%)                 68
    Cloudiness (%)               20
    Wind Speed (mph)          12.75
    Name: 9, dtype: object clyde river ca
    Processing Record 9of Set 1 | clyde river
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=clyde river&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=clyde%20river&units=imperial
    10 City Name             hilo
    Country Code            us
    Latitude             19.71
    Longitude          -155.08
    Temperature (F)      73.67
    Humidity (%)            69
    Cloudiness (%)          75
    Wind Speed (mph)     11.41
    Name: 10, dtype: object hilo us
    Processing Record 10of Set 1 | hilo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hilo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hilo&units=imperial
    11 City Name           albany
    Country Code            au
    Latitude             42.65
    Longitude           -73.75
    Temperature (F)      27.03
    Humidity (%)            42
    Cloudiness (%)          20
    Wind Speed (mph)     10.29
    Name: 11, dtype: object albany au
    Processing Record 11of Set 1 | albany
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=albany&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=albany&units=imperial
    12 City Name           hithadhoo
    Country Code               mv
    Latitude                 -0.6
    Longitude               73.08
    Temperature (F)         84.95
    Humidity (%)               99
    Cloudiness (%)             80
    Wind Speed (mph)         9.46
    Name: 12, dtype: object hithadhoo mv
    Processing Record 12of Set 1 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hithadhoo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hithadhoo&units=imperial
    13 City Name           halifax
    Country Code             ca
    Latitude              44.65
    Longitude            -63.58
    Temperature (F)          23
    Humidity (%)             57
    Cloudiness (%)           20
    Wind Speed (mph)      12.75
    Name: 13, dtype: object halifax ca
    Processing Record 13of Set 1 | halifax
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=halifax&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=halifax&units=imperial
    14 City Name           carballo
    Country Code              es
    Latitude               43.21
    Longitude              -8.69
    Temperature (F)        44.53
    Humidity (%)              81
    Cloudiness (%)            20
    Wind Speed (mph)       10.29
    Name: 14, dtype: object carballo es
    Processing Record 14of Set 1 | carballo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=carballo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=carballo&units=imperial
    17 City Name           vaini
    Country Code           to
    Latitude            15.34
    Longitude           74.49
    Temperature (F)     66.05
    Humidity (%)           91
    Cloudiness (%)          8
    Wind Speed (mph)     3.31
    Name: 17, dtype: object vaini to
    Processing Record 17of Set 1 | vaini
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vaini&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vaini&units=imperial
    18 City Name           margate
    Country Code             za
    Latitude             -43.03
    Longitude            147.26
    Temperature (F)        66.2
    Humidity (%)             55
    Cloudiness (%)           20
    Wind Speed (mph)      24.16
    Name: 18, dtype: object margate za
    Processing Record 18of Set 1 | margate
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=margate&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=margate&units=imperial
    19 City Name           atuona
    Country Code            pf
    Latitude              -9.8
    Longitude          -139.03
    Temperature (F)      82.07
    Humidity (%)            99
    Cloudiness (%)          56
    Wind Speed (mph)     19.64
    Name: 19, dtype: object atuona pf
    Processing Record 19of Set 1 | atuona
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=atuona&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=atuona&units=imperial
    20 City Name           fort smith
    Country Code                us
    Latitude                 35.39
    Longitude               -94.42
    Temperature (F)          60.82
    Humidity (%)                59
    Cloudiness (%)               1
    Wind Speed (mph)         11.41
    Name: 20, dtype: object fort smith us
    Processing Record 20of Set 1 | fort smith
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fort smith&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fort%20smith&units=imperial
    22 City Name           dzerzhinsk
    Country Code                ru
    Latitude                 56.24
    Longitude                43.46
    Temperature (F)            8.6
    Humidity (%)                65
    Cloudiness (%)               0
    Wind Speed (mph)          4.47
    Name: 22, dtype: object dzerzhinsk ru
    Processing Record 22of Set 1 | dzerzhinsk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dzerzhinsk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dzerzhinsk&units=imperial
    24 City Name           rikitea
    Country Code             pf
    Latitude             -23.12
    Longitude           -134.97
    Temperature (F)          80
    Humidity (%)            100
    Cloudiness (%)           68
    Wind Speed (mph)      14.72
    Name: 24, dtype: object rikitea pf
    Processing Record 24of Set 1 | rikitea
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rikitea&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rikitea&units=imperial
    25 City Name           bathsheba
    Country Code               bb
    Latitude                13.22
    Longitude              -59.52
    Temperature (F)            77
    Humidity (%)               78
    Cloudiness (%)             40
    Wind Speed (mph)        10.29
    Name: 25, dtype: object bathsheba bb
    Processing Record 25of Set 1 | bathsheba
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bathsheba&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bathsheba&units=imperial
    26 City Name           longyearbyen
    Country Code                  sj
    Latitude                   78.22
    Longitude                  15.63
    Temperature (F)              6.8
    Humidity (%)                  65
    Cloudiness (%)                20
    Wind Speed (mph)           19.46
    Name: 26, dtype: object longyearbyen sj
    Processing Record 26of Set 1 | longyearbyen
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=longyearbyen&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=longyearbyen&units=imperial
    27 City Name           souillac
    Country Code              mu
    Latitude                45.6
    Longitude               -0.6
    Temperature (F)        38.21
    Humidity (%)              86
    Cloudiness (%)            20
    Wind Speed (mph)         4.7
    Name: 27, dtype: object souillac mu
    Processing Record 27of Set 1 | souillac
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=souillac&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=souillac&units=imperial
    28 City Name           princeton
    Country Code               ca
    Latitude                41.37
    Longitude              -89.46
    Temperature (F)         48.18
    Humidity (%)               45
    Cloudiness (%)              1
    Wind Speed (mph)          4.7
    Name: 28, dtype: object princeton ca
    Processing Record 28of Set 1 | princeton
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=princeton&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=princeton&units=imperial
    29 City Name           lompoc
    Country Code            us
    Latitude             34.64
    Longitude          -120.46
    Temperature (F)      56.01
    Humidity (%)            62
    Cloudiness (%)           1
    Wind Speed (mph)      8.05
    Name: 29, dtype: object lompoc us
    Processing Record 29of Set 1 | lompoc
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lompoc&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lompoc&units=imperial
    30 City Name           turukhansk
    Country Code                ru
    Latitude                  65.8
    Longitude                87.96
    Temperature (F)           7.73
    Humidity (%)                75
    Cloudiness (%)              88
    Wind Speed (mph)          4.99
    Name: 30, dtype: object turukhansk ru
    Processing Record 30of Set 1 | turukhansk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=turukhansk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=turukhansk&units=imperial
    31 City Name           somerset
    Country Code              us
    Latitude               37.09
    Longitude              -84.6
    Temperature (F)        54.03
    Humidity (%)              58
    Cloudiness (%)             1
    Wind Speed (mph)        2.75
    Name: 31, dtype: object somerset us
    Processing Record 31of Set 1 | somerset
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=somerset&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=somerset&units=imperial
    33 City Name            mayo
    Country Code           ca
    Latitude            63.59
    Longitude          -135.9
    Temperature (F)        41
    Humidity (%)           48
    Cloudiness (%)         75
    Wind Speed (mph)     3.36
    Name: 33, dtype: object mayo ca
    Processing Record 33of Set 1 | mayo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mayo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mayo&units=imperial
    34 City Name           severnyy-kospashskiy
    Country Code                          ru
    Latitude                           59.09
    Longitude                           57.8
    Temperature (F)                    -1.09
    Humidity (%)                          76
    Cloudiness (%)                        32
    Wind Speed (mph)                   14.38
    Name: 34, dtype: object severnyy-kospashskiy ru
    Processing Record 34of Set 1 | severnyy-kospashskiy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=severnyy-kospashskiy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=severnyy-kospashskiy&units=imperial
    35 City Name           nikolskoye
    Country Code                ru
    Latitude                  59.7
    Longitude                30.79
    Temperature (F)           30.2
    Humidity (%)                74
    Cloudiness (%)              75
    Wind Speed (mph)         13.42
    Name: 35, dtype: object nikolskoye ru
    Processing Record 35of Set 1 | nikolskoye
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nikolskoye&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nikolskoye&units=imperial
    36 City Name           busselton
    Country Code               au
    Latitude               -33.64
    Longitude              115.35
    Temperature (F)         62.27
    Humidity (%)              100
    Cloudiness (%)              0
    Wind Speed (mph)        14.16
    Name: 36, dtype: object busselton au
    Processing Record 36of Set 1 | busselton
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=busselton&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=busselton&units=imperial
    37 City Name           jamestown
    Country Code               sh
    Latitude               -33.21
    Longitude               138.6
    Temperature (F)         64.25
    Humidity (%)               61
    Cloudiness (%)              0
    Wind Speed (mph)         8.12
    Name: 37, dtype: object jamestown sh
    Processing Record 37of Set 1 | jamestown
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=jamestown&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=jamestown&units=imperial
    38 City Name           bonthe
    Country Code            sl
    Latitude              7.53
    Longitude            -12.5
    Temperature (F)      78.83
    Humidity (%)            87
    Cloudiness (%)           0
    Wind Speed (mph)      4.65
    Name: 38, dtype: object bonthe sl
    Processing Record 38of Set 1 | bonthe
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bonthe&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bonthe&units=imperial
    39 City Name             bud
    Country Code           no
    Latitude            62.91
    Longitude            6.91
    Temperature (F)     36.82
    Humidity (%)           96
    Cloudiness (%)         92
    Wind Speed (mph)     26.4
    Name: 39, dtype: object bud no
    Processing Record 39of Set 1 | bud
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bud&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bud&units=imperial
    40 City Name           sakaiminato
    Country Code                 jp
    Latitude                  35.55
    Longitude                133.23
    Temperature (F)            53.6
    Humidity (%)                 87
    Cloudiness (%)               90
    Wind Speed (mph)           5.82
    Name: 40, dtype: object sakaiminato jp
    Processing Record 40of Set 1 | sakaiminato
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sakaiminato&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sakaiminato&units=imperial
    41 City Name           castro
    Country Code            cl
    Latitude            -42.48
    Longitude           -73.76
    Temperature (F)      52.82
    Humidity (%)            99
    Cloudiness (%)         100
    Wind Speed (mph)      5.21
    Name: 41, dtype: object castro cl
    Processing Record 41of Set 1 | castro
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=castro&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=castro&units=imperial
    44 City Name           kamenka
    Country Code             ru
    Latitude              53.19
    Longitude             44.05
    Temperature (F)       -9.37
    Humidity (%)             50
    Cloudiness (%)            0
    Wind Speed (mph)       2.86
    Name: 44, dtype: object kamenka ru
    Processing Record 44of Set 1 | kamenka
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kamenka&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kamenka&units=imperial
    45 City Name            luan
    Country Code           cn
    Latitude            46.36
    Longitude            6.98
    Temperature (F)     37.36
    Humidity (%)           80
    Cloudiness (%)         75
    Wind Speed (mph)     3.36
    Name: 45, dtype: object luan cn
    Processing Record 45of Set 1 | luan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=luan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=luan&units=imperial
    47 City Name           kruisfontein
    Country Code                  za
    Latitude                     -34
    Longitude                  24.73
    Temperature (F)            64.61
    Humidity (%)                  95
    Cloudiness (%)                44
    Wind Speed (mph)           18.86
    Name: 47, dtype: object kruisfontein za
    Processing Record 47of Set 1 | kruisfontein
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kruisfontein&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kruisfontein&units=imperial
    49 City Name           khatanga
    Country Code              ru
    Latitude               71.98
    Longitude             102.47
    Temperature (F)        -8.02
    Humidity (%)              63
    Cloudiness (%)            80
    Wind Speed (mph)       11.36
    Name: 49, dtype: object khatanga ru
    Processing Record 49of Set 1 | khatanga
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=khatanga&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=khatanga&units=imperial
    50 City Name           arraial do cabo
    Country Code                     br
    Latitude                     -22.97
    Longitude                    -42.02
    Temperature (F)               78.11
    Humidity (%)                     95
    Cloudiness (%)                   44
    Wind Speed (mph)              13.04
    Name: 50, dtype: object arraial do cabo br
    Processing Record 50of Set 1 | arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=arraial do cabo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=arraial%20do%20cabo&units=imperial
    51 City Name           manokwari
    Country Code               id
    Latitude                -0.87
    Longitude              134.08
    Temperature (F)         81.08
    Humidity (%)              100
    Cloudiness (%)             20
    Wind Speed (mph)         3.65
    Name: 51, dtype: object manokwari id
    Processing Record 51of Set 1 | manokwari
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=manokwari&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=manokwari&units=imperial
    53 City Name           qaanaaq
    Country Code             gl
    Latitude              77.48
    Longitude            -69.36
    Temperature (F)      -21.25
    Humidity (%)             91
    Cloudiness (%)            0
    Wind Speed (mph)       6.67
    Name: 53, dtype: object qaanaaq gl
    Processing Record 53of Set 1 | qaanaaq
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=qaanaaq&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=qaanaaq&units=imperial
    54 City Name           puerto ayora
    Country Code                  ec
    Latitude                   -0.74
    Longitude                 -90.35
    Temperature (F)             82.4
    Humidity (%)                  74
    Cloudiness (%)                20
    Wind Speed (mph)           10.29
    Name: 54, dtype: object puerto ayora ec
    Processing Record 54of Set 1 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=puerto ayora&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=puerto%20ayora&units=imperial
    55 City Name           guerrero negro
    Country Code                    mx
    Latitude                     27.97
    Longitude                  -114.04
    Temperature (F)               66.5
    Humidity (%)                    53
    Cloudiness (%)                   0
    Wind Speed (mph)             13.49
    Name: 55, dtype: object guerrero negro mx
    Processing Record 55of Set 1 | guerrero negro
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=guerrero negro&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=guerrero%20negro&units=imperial
    56 City Name           hualmay
    Country Code             pe
    Latitude              -11.1
    Longitude            -77.61
    Temperature (F)       71.18
    Humidity (%)             73
    Cloudiness (%)            0
    Wind Speed (mph)       3.42
    Name: 56, dtype: object hualmay pe
    Processing Record 56of Set 1 | hualmay
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hualmay&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hualmay&units=imperial
    57 City Name           thunder bay
    Country Code                 ca
    Latitude                  48.41
    Longitude                -89.26
    Temperature (F)            26.6
    Humidity (%)                 63
    Cloudiness (%)               75
    Wind Speed (mph)           8.05
    Name: 57, dtype: object thunder bay ca
    Processing Record 57of Set 1 | thunder bay
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=thunder bay&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=thunder%20bay&units=imperial
    58 City Name           bredasdorp
    Country Code                za
    Latitude                -34.53
    Longitude                20.04
    Temperature (F)           66.2
    Humidity (%)                88
    Cloudiness (%)              20
    Wind Speed (mph)          5.82
    Name: 58, dtype: object bredasdorp za
    Processing Record 58of Set 1 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bredasdorp&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bredasdorp&units=imperial
    59 City Name            lebu
    Country Code           cl
    Latitude             8.96
    Longitude           38.73
    Temperature (F)      57.2
    Humidity (%)           76
    Cloudiness (%)         75
    Wind Speed (mph)      4.7
    Name: 59, dtype: object lebu cl
    Processing Record 59of Set 1 | lebu
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lebu&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lebu&units=imperial
    60 City Name            bluff
    Country Code            nz
    Latitude            -23.58
    Longitude           149.07
    Temperature (F)      81.71
    Humidity (%)            71
    Cloudiness (%)          88
    Wind Speed (mph)      4.76
    Name: 60, dtype: object bluff nz
    Processing Record 60of Set 1 | bluff
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bluff&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bluff&units=imperial
    61 City Name           shingu
    Country Code            jp
    Latitude             33.72
    Longitude           135.99
    Temperature (F)       62.6
    Humidity (%)            77
    Cloudiness (%)          75
    Wind Speed (mph)      6.93
    Name: 61, dtype: object shingu jp
    Processing Record 61of Set 1 | shingu
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=shingu&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=shingu&units=imperial
    62 City Name            lima
    Country Code           pe
    Latitude           -12.06
    Longitude          -77.04
    Temperature (F)      65.6
    Humidity (%)           70
    Cloudiness (%)          8
    Wind Speed (mph)      2.3
    Name: 62, dtype: object lima pe
    Processing Record 62of Set 1 | lima
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lima&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lima&units=imperial
    63 City Name           arlit
    Country Code           ne
    Latitude            18.74
    Longitude            7.39
    Temperature (F)     64.97
    Humidity (%)           25
    Cloudiness (%)          0
    Wind Speed (mph)      3.2
    Name: 63, dtype: object arlit ne
    Processing Record 63of Set 1 | arlit
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=arlit&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=arlit&units=imperial
    64 City Name           sabinas hidalgo
    Country Code                     mx
    Latitude                      26.51
    Longitude                   -100.18
    Temperature (F)               85.67
    Humidity (%)                     31
    Cloudiness (%)                    0
    Wind Speed (mph)               6.67
    Name: 64, dtype: object sabinas hidalgo mx
    Processing Record 64of Set 1 | sabinas hidalgo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sabinas hidalgo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sabinas%20hidalgo&units=imperial
    65 City Name           port hardy
    Country Code                ca
    Latitude                  50.7
    Longitude              -127.42
    Temperature (F)             50
    Humidity (%)                71
    Cloudiness (%)              20
    Wind Speed (mph)          9.17
    Name: 65, dtype: object port hardy ca
    Processing Record 65of Set 1 | port hardy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port hardy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port%20hardy&units=imperial
    66 City Name           ajdabiya
    Country Code              ly
    Latitude               30.75
    Longitude              20.22
    Temperature (F)        49.94
    Humidity (%)              93
    Cloudiness (%)             0
    Wind Speed (mph)        3.53
    Name: 66, dtype: object ajdabiya ly
    Processing Record 66of Set 1 | ajdabiya
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ajdabiya&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ajdabiya&units=imperial
    67 City Name           ostrovnoy
    Country Code               ru
    Latitude                68.05
    Longitude               39.51
    Temperature (F)         20.69
    Humidity (%)               81
    Cloudiness (%)             80
    Wind Speed (mph)        14.38
    Name: 67, dtype: object ostrovnoy ru
    Processing Record 67of Set 1 | ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ostrovnoy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ostrovnoy&units=imperial
    68 City Name           namibe
    Country Code            ao
    Latitude            -15.19
    Longitude            12.15
    Temperature (F)      77.12
    Humidity (%)           100
    Cloudiness (%)          80
    Wind Speed (mph)      5.77
    Name: 68, dtype: object namibe ao
    Processing Record 68of Set 1 | namibe
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=namibe&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=namibe&units=imperial
    69 City Name           coquimbo
    Country Code              cl
    Latitude              -29.95
    Longitude             -71.34
    Temperature (F)         60.8
    Humidity (%)              77
    Cloudiness (%)             0
    Wind Speed (mph)        5.82
    Name: 69, dtype: object coquimbo cl
    Processing Record 69of Set 1 | coquimbo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=coquimbo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=coquimbo&units=imperial
    70 City Name           thompson
    Country Code              ca
    Latitude               55.74
    Longitude             -97.86
    Temperature (F)           14
    Humidity (%)              25
    Cloudiness (%)            20
    Wind Speed (mph)        5.82
    Name: 70, dtype: object thompson ca
    Processing Record 70of Set 1 | thompson
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=thompson&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=thompson&units=imperial
    71 City Name           podporozhye
    Country Code                 ru
    Latitude                  60.91
    Longitude                 34.16
    Temperature (F)           29.06
    Humidity (%)                 88
    Cloudiness (%)               80
    Wind Speed (mph)          16.06
    Name: 71, dtype: object podporozhye ru
    Processing Record 71of Set 1 | podporozhye
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=podporozhye&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=podporozhye&units=imperial
    72 City Name            airai
    Country Code            pw
    Latitude             -8.93
    Longitude           125.41
    Temperature (F)      77.48
    Humidity (%)            72
    Cloudiness (%)          20
    Wind Speed (mph)      0.96
    Name: 72, dtype: object airai pw
    Processing Record 72of Set 1 | airai
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=airai&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=airai&units=imperial
    73 City Name           saint-pierre
    Country Code                  pm
    Latitude                   48.95
    Longitude                   4.24
    Temperature (F)             28.4
    Humidity (%)                  92
    Cloudiness (%)                90
    Wind Speed (mph)            6.93
    Name: 73, dtype: object saint-pierre pm
    Processing Record 73of Set 1 | saint-pierre
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saint-pierre&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saint-pierre&units=imperial
    74 City Name           pasighat
    Country Code              in
    Latitude               28.06
    Longitude              95.33
    Temperature (F)        49.13
    Humidity (%)              86
    Cloudiness (%)             0
    Wind Speed (mph)        2.53
    Name: 74, dtype: object pasighat in
    Processing Record 74of Set 1 | pasighat
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pasighat&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pasighat&units=imperial
    75 City Name           putina
    Country Code            pe
    Latitude            -14.91
    Longitude           -69.87
    Temperature (F)       46.4
    Humidity (%)            81
    Cloudiness (%)          75
    Wind Speed (mph)     13.87
    Name: 75, dtype: object putina pe
    Processing Record 75of Set 1 | putina
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=putina&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=putina&units=imperial
    76 City Name           barrow
    Country Code            us
    Latitude            -38.31
    Longitude           -60.23
    Temperature (F)      56.51
    Humidity (%)            94
    Cloudiness (%)           0
    Wind Speed (mph)      9.46
    Name: 76, dtype: object barrow us
    Processing Record 76of Set 1 | barrow
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=barrow&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=barrow&units=imperial
    77 City Name           aklavik
    Country Code             ca
    Latitude              68.22
    Longitude           -135.01
    Temperature (F)       -0.41
    Humidity (%)             77
    Cloudiness (%)           90
    Wind Speed (mph)       5.82
    Name: 77, dtype: object aklavik ca
    Processing Record 77of Set 1 | aklavik
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=aklavik&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=aklavik&units=imperial
    78 City Name           tarime
    Country Code            tz
    Latitude             -1.35
    Longitude            34.38
    Temperature (F)      64.61
    Humidity (%)           100
    Cloudiness (%)           8
    Wind Speed (mph)      3.76
    Name: 78, dtype: object tarime tz
    Processing Record 78of Set 1 | tarime
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tarime&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tarime&units=imperial
    79 City Name           polyarnyy
    Country Code               ru
    Latitude                 69.2
    Longitude               33.45
    Temperature (F)          12.2
    Humidity (%)               85
    Cloudiness (%)              0
    Wind Speed (mph)         6.71
    Name: 79, dtype: object polyarnyy ru
    Processing Record 79of Set 1 | polyarnyy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=polyarnyy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=polyarnyy&units=imperial
    80 City Name           flinders
    Country Code              au
    Latitude              -34.58
    Longitude             150.85
    Temperature (F)         71.6
    Humidity (%)              40
    Cloudiness (%)             0
    Wind Speed (mph)       13.87
    Name: 80, dtype: object flinders au
    Processing Record 80of Set 1 | flinders
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=flinders&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=flinders&units=imperial
    81 City Name           manggar
    Country Code             id
    Latitude              -2.88
    Longitude            108.27
    Temperature (F)       78.47
    Humidity (%)             98
    Cloudiness (%)            0
    Wind Speed (mph)       2.64
    Name: 81, dtype: object manggar id
    Processing Record 81of Set 1 | manggar
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=manggar&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=manggar&units=imperial
    82 City Name            hofn
    Country Code           is
    Latitude            64.25
    Longitude          -15.21
    Temperature (F)      37.7
    Humidity (%)           96
    Cloudiness (%)          8
    Wind Speed (mph)     9.91
    Name: 82, dtype: object hofn is
    Processing Record 82of Set 1 | hofn
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hofn&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hofn&units=imperial
    83 City Name           saldanha
    Country Code              za
    Latitude               41.42
    Longitude              -6.55
    Temperature (F)        34.46
    Humidity (%)              92
    Cloudiness (%)            88
    Wind Speed (mph)        5.88
    Name: 83, dtype: object saldanha za
    Processing Record 83of Set 1 | saldanha
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saldanha&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saldanha&units=imperial
    84 City Name           beira
    Country Code           mz
    Latitude             43.2
    Longitude           -8.36
    Temperature (F)     44.49
    Humidity (%)           81
    Cloudiness (%)         20
    Wind Speed (mph)    10.29
    Name: 84, dtype: object beira mz
    Processing Record 84of Set 1 | beira
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=beira&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=beira&units=imperial
    85 City Name           kanungu
    Country Code             ug
    Latitude               -0.9
    Longitude             29.78
    Temperature (F)       58.31
    Humidity (%)            100
    Cloudiness (%)           80
    Wind Speed (mph)       2.75
    Name: 85, dtype: object kanungu ug
    Processing Record 85of Set 1 | kanungu
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kanungu&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kanungu&units=imperial
    86 City Name           half moon bay
    Country Code                   us
    Latitude                    37.46
    Longitude                 -122.43
    Temperature (F)              55.6
    Humidity (%)                   62
    Cloudiness (%)                 90
    Wind Speed (mph)             6.93
    Name: 86, dtype: object half moon bay us
    Processing Record 86of Set 1 | half moon bay
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=half moon bay&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=half%20moon%20bay&units=imperial
    87 City Name           paamiut
    Country Code             gl
    Latitude              61.99
    Longitude            -49.67
    Temperature (F)       37.07
    Humidity (%)             76
    Cloudiness (%)           44
    Wind Speed (mph)        9.8
    Name: 87, dtype: object paamiut gl
    Processing Record 87of Set 1 | paamiut
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=paamiut&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=paamiut&units=imperial
    89 City Name           meulaboh
    Country Code              id
    Latitude                4.14
    Longitude              96.13
    Temperature (F)        79.28
    Humidity (%)             100
    Cloudiness (%)            64
    Wind Speed (mph)        4.09
    Name: 89, dtype: object meulaboh id
    Processing Record 89of Set 1 | meulaboh
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=meulaboh&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=meulaboh&units=imperial
    90 City Name           avarua
    Country Code            ck
    Latitude            -21.21
    Longitude          -159.78
    Temperature (F)         86
    Humidity (%)            70
    Cloudiness (%)          75
    Wind Speed (mph)      6.93
    Name: 90, dtype: object avarua ck
    Processing Record 90of Set 1 | avarua
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=avarua&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=avarua&units=imperial
    91 City Name           port elizabeth
    Country Code                    za
    Latitude                     39.31
    Longitude                   -74.98
    Temperature (F)              42.87
    Humidity (%)                    44
    Cloudiness (%)                   1
    Wind Speed (mph)              3.36
    Name: 91, dtype: object port elizabeth za
    Processing Record 91of Set 1 | port elizabeth
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port elizabeth&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port%20elizabeth&units=imperial
    92 City Name           kubrat
    Country Code            bg
    Latitude              43.8
    Longitude             26.5
    Temperature (F)      30.95
    Humidity (%)            96
    Cloudiness (%)          44
    Wind Speed (mph)     10.25
    Name: 92, dtype: object kubrat bg
    Processing Record 92of Set 1 | kubrat
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kubrat&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kubrat&units=imperial
    93 City Name           amahai
    Country Code            id
    Latitude             -3.31
    Longitude              129
    Temperature (F)      80.63
    Humidity (%)           100
    Cloudiness (%)          20
    Wind Speed (mph)      3.65
    Name: 93, dtype: object amahai id
    Processing Record 93of Set 1 | amahai
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=amahai&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=amahai&units=imperial
    94 City Name           saint-philippe
    Country Code                    re
    Latitude                     45.36
    Longitude                   -73.48
    Temperature (F)              16.27
    Humidity (%)                    44
    Cloudiness (%)                   1
    Wind Speed (mph)             12.75
    Name: 94, dtype: object saint-philippe re
    Processing Record 94of Set 1 | saint-philippe
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saint-philippe&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saint-philippe&units=imperial
    95 City Name           la cruz
    Country Code             mx
    Latitude              23.92
    Longitude           -106.89
    Temperature (F)       86.48
    Humidity (%)             27
    Cloudiness (%)            0
    Wind Speed (mph)       6.22
    Name: 95, dtype: object la cruz mx
    Processing Record 95of Set 1 | la cruz
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=la cruz&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=la%20cruz&units=imperial
    96 City Name           avera
    Country Code           pf
    Latitude            33.19
    Longitude          -82.53
    Temperature (F)     71.62
    Humidity (%)           40
    Cloudiness (%)          1
    Wind Speed (mph)     4.99
    Name: 96, dtype: object avera pf
    Processing Record 96of Set 1 | avera
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=avera&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=avera&units=imperial
    97 City Name           bontang
    Country Code             id
    Latitude               0.12
    Longitude            117.47
    Temperature (F)       81.35
    Humidity (%)            100
    Cloudiness (%)           80
    Wind Speed (mph)       0.51
    Name: 97, dtype: object bontang id
    Processing Record 97of Set 1 | bontang
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bontang&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bontang&units=imperial
    98 City Name           san patricio
    Country Code                  mx
    Latitude                  -26.98
    Longitude                 -56.83
    Temperature (F)            89.99
    Humidity (%)                  53
    Cloudiness (%)                36
    Wind Speed (mph)            5.32
    Name: 98, dtype: object san patricio mx
    Processing Record 98of Set 1 | san patricio
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=san patricio&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=san%20patricio&units=imperial
    99 City Name           dunedin
    Country Code             nz
    Latitude             -45.87
    Longitude             170.5
    Temperature (F)       70.64
    Humidity (%)             62
    Cloudiness (%)           80
    Wind Speed (mph)      12.71
    Name: 99, dtype: object dunedin nz
    Processing Record 99of Set 1 | dunedin
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dunedin&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dunedin&units=imperial
    100 City Name           varhaug
    Country Code             no
    Latitude              58.61
    Longitude              5.65
    Temperature (F)          32
    Humidity (%)            100
    Cloudiness (%)            0
    Wind Speed (mph)      11.41
    Name: 100, dtype: object varhaug no
    Processing Record 100of Set 1 | varhaug
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=varhaug&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=varhaug&units=imperial
    101 City Name           hurricane
    Country Code               us
    Latitude                37.18
    Longitude             -113.29
    Temperature (F)         45.41
    Humidity (%)               16
    Cloudiness (%)             20
    Wind Speed (mph)        18.34
    Name: 101, dtype: object hurricane us
    Processing Record 101of Set 1 | hurricane
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hurricane&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hurricane&units=imperial
    102 City Name           husavik
    Country Code             is
    Latitude              50.56
    Longitude            -96.99
    Temperature (F)       30.77
    Humidity (%)             94
    Cloudiness (%)           56
    Wind Speed (mph)      11.36
    Name: 102, dtype: object husavik is
    Processing Record 102of Set 1 | husavik
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=husavik&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=husavik&units=imperial
    103 City Name           kendal
    Country Code            gb
    Latitude             -6.92
    Longitude            110.2
    Temperature (F)      73.79
    Humidity (%)           100
    Cloudiness (%)           0
    Wind Speed (mph)      3.76
    Name: 103, dtype: object kendal gb
    Processing Record 103of Set 1 | kendal
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kendal&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kendal&units=imperial
    104 City Name           kahului
    Country Code             us
    Latitude              20.89
    Longitude           -156.47
    Temperature (F)       74.66
    Humidity (%)             94
    Cloudiness (%)           90
    Wind Speed (mph)       20.8
    Name: 104, dtype: object kahului us
    Processing Record 104of Set 1 | kahului
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kahului&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kahului&units=imperial
    105 City Name           mar del plata
    Country Code                   ar
    Latitude                   -46.43
    Longitude                  -67.52
    Temperature (F)             62.63
    Humidity (%)                   48
    Cloudiness (%)                 20
    Wind Speed (mph)            27.58
    Name: 105, dtype: object mar del plata ar
    Processing Record 105of Set 1 | mar del plata
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mar del plata&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mar%20del%20plata&units=imperial
    106 City Name           dunmore town
    Country Code                  bs
    Latitude                    25.5
    Longitude                 -76.65
    Temperature (F)            74.51
    Humidity (%)                 100
    Cloudiness (%)                20
    Wind Speed (mph)            4.54
    Name: 106, dtype: object dunmore town bs
    Processing Record 106of Set 1 | dunmore town
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dunmore town&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dunmore%20town&units=imperial
    107 City Name            tulun
    Country Code            ru
    Latitude             54.56
    Longitude           100.58
    Temperature (F)      14.12
    Humidity (%)            71
    Cloudiness (%)           8
    Wind Speed (mph)      3.42
    Name: 107, dtype: object tulun ru
    Processing Record 107of Set 1 | tulun
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tulun&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tulun&units=imperial
    108 City Name           yumen
    Country Code           cn
    Latitude            40.29
    Longitude           97.04
    Temperature (F)     34.55
    Humidity (%)           64
    Cloudiness (%)          0
    Wind Speed (mph)     9.35
    Name: 108, dtype: object yumen cn
    Processing Record 108of Set 1 | yumen
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yumen&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yumen&units=imperial
    109 City Name           puerto leguizamo
    Country Code                      co
    Latitude                       -0.19
    Longitude                     -74.78
    Temperature (F)                82.16
    Humidity (%)                      73
    Cloudiness (%)                     8
    Wind Speed (mph)                3.31
    Name: 109, dtype: object puerto leguizamo co
    Processing Record 109of Set 1 | puerto leguizamo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=puerto leguizamo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=puerto%20leguizamo&units=imperial
    110 City Name           carnarvon
    Country Code               au
    Latitude               -30.97
    Longitude               22.13
    Temperature (F)         46.97
    Humidity (%)               36
    Cloudiness (%)              0
    Wind Speed (mph)         3.87
    Name: 110, dtype: object carnarvon au
    Processing Record 110of Set 1 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=carnarvon&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=carnarvon&units=imperial
    111 City Name           dzhebariki-khaya
    Country Code                      ru
    Latitude                       62.22
    Longitude                      135.8
    Temperature (F)               -12.43
    Humidity (%)                      59
    Cloudiness (%)                    44
    Wind Speed (mph)                3.65
    Name: 111, dtype: object dzhebariki-khaya ru
    Processing Record 111of Set 1 | dzhebariki-khaya
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dzhebariki-khaya&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dzhebariki-khaya&units=imperial
    112 City Name           tasiilaq
    Country Code              gl
    Latitude               65.61
    Longitude             -37.64
    Temperature (F)         35.6
    Humidity (%)              80
    Cloudiness (%)            92
    Wind Speed (mph)       24.16
    Name: 112, dtype: object tasiilaq gl
    Processing Record 112of Set 1 | tasiilaq
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tasiilaq&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tasiilaq&units=imperial
    113 City Name           port augusta
    Country Code                  au
    Latitude                  -32.49
    Longitude                 137.76
    Temperature (F)             66.2
    Humidity (%)                  59
    Cloudiness (%)                 0
    Wind Speed (mph)            9.17
    Name: 113, dtype: object port augusta au
    Processing Record 113of Set 1 | port augusta
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port augusta&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port%20augusta&units=imperial
    114 City Name           quatre cocos
    Country Code                  mu
    Latitude                  -20.21
    Longitude                  57.76
    Temperature (F)             80.6
    Humidity (%)                  83
    Cloudiness (%)                40
    Wind Speed (mph)            9.17
    Name: 114, dtype: object quatre cocos mu
    Processing Record 114of Set 1 | quatre cocos
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=quatre cocos&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=quatre%20cocos&units=imperial
    115 City Name           la ronge
    Country Code              ca
    Latitude                55.1
    Longitude             -105.3
    Temperature (F)         17.6
    Humidity (%)              52
    Cloudiness (%)            75
    Wind Speed (mph)        8.05
    Name: 115, dtype: object la ronge ca
    Processing Record 115of Set 1 | la ronge
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=la ronge&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=la%20ronge&units=imperial
    116 City Name           watsa
    Country Code           cd
    Latitude             3.04
    Longitude           29.53
    Temperature (F)     66.23
    Humidity (%)           99
    Cloudiness (%)         92
    Wind Speed (mph)     4.99
    Name: 116, dtype: object watsa cd
    Processing Record 116of Set 1 | watsa
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=watsa&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=watsa&units=imperial
    117 City Name           georgetown
    Country Code                sh
    Latitude                   6.8
    Longitude               -58.16
    Temperature (F)           73.4
    Humidity (%)                88
    Cloudiness (%)               0
    Wind Speed (mph)          6.93
    Name: 117, dtype: object georgetown sh
    Processing Record 117of Set 1 | georgetown
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=georgetown&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=georgetown&units=imperial
    118 City Name           khandyga
    Country Code              ru
    Latitude               62.65
    Longitude             135.58
    Temperature (F)       -12.43
    Humidity (%)              59
    Cloudiness (%)            44
    Wind Speed (mph)        3.65
    Name: 118, dtype: object khandyga ru
    Processing Record 118of Set 1 | khandyga
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=khandyga&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=khandyga&units=imperial
    119 City Name           cabo san lucas
    Country Code                    mx
    Latitude                     22.89
    Longitude                  -109.91
    Temperature (F)               78.8
    Humidity (%)                    44
    Cloudiness (%)                  40
    Wind Speed (mph)             11.41
    Name: 119, dtype: object cabo san lucas mx
    Processing Record 119of Set 1 | cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cabo san lucas&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cabo%20san%20lucas&units=imperial
    121 City Name           new norfolk
    Country Code                 au
    Latitude                 -42.78
    Longitude                147.06
    Temperature (F)            66.2
    Humidity (%)                 48
    Cloudiness (%)               75
    Wind Speed (mph)          23.04
    Name: 121, dtype: object new norfolk au
    Processing Record 121of Set 1 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=new norfolk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=new%20norfolk&units=imperial
    122 City Name           qandala
    Country Code             so
    Latitude              11.47
    Longitude             49.87
    Temperature (F)       59.48
    Humidity (%)            100
    Cloudiness (%)            0
    Wind Speed (mph)       2.64
    Name: 122, dtype: object qandala so
    Processing Record 122of Set 1 | qandala
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=qandala&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=qandala&units=imperial
    123 City Name           dwarka
    Country Code            in
    Latitude             28.58
    Longitude            77.04
    Temperature (F)       66.2
    Humidity (%)            59
    Cloudiness (%)          20
    Wind Speed (mph)      2.86
    Name: 123, dtype: object dwarka in
    Processing Record 123of Set 1 | dwarka
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dwarka&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dwarka&units=imperial
    124 City Name           nizip
    Country Code           tr
    Latitude            37.01
    Longitude           37.79
    Temperature (F)      44.6
    Humidity (%)           81
    Cloudiness (%)          0
    Wind Speed (mph)      4.7
    Name: 124, dtype: object nizip tr
    Processing Record 124of Set 1 | nizip
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nizip&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nizip&units=imperial
    125 City Name           ribeira grande
    Country Code                    pt
    Latitude                     38.52
    Longitude                    -28.7
    Temperature (F)              62.63
    Humidity (%)                    98
    Cloudiness (%)                  92
    Wind Speed (mph)             10.02
    Name: 125, dtype: object ribeira grande pt
    Processing Record 125of Set 1 | ribeira grande
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ribeira grande&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ribeira%20grande&units=imperial
    126 City Name           leningradskiy
    Country Code                   ru
    Latitude                    69.38
    Longitude                  178.42
    Temperature (F)             -5.95
    Humidity (%)                  100
    Cloudiness (%)                 76
    Wind Speed (mph)             9.69
    Name: 126, dtype: object leningradskiy ru
    Processing Record 126of Set 1 | leningradskiy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=leningradskiy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=leningradskiy&units=imperial
    127 City Name           okhtyrka
    Country Code              ua
    Latitude               50.31
    Longitude              34.89
    Temperature (F)        10.79
    Humidity (%)              78
    Cloudiness (%)            92
    Wind Speed (mph)       15.05
    Name: 127, dtype: object okhtyrka ua
    Processing Record 127of Set 1 | okhtyrka
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=okhtyrka&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=okhtyrka&units=imperial
    128 City Name           anamur
    Country Code            tr
    Latitude             36.08
    Longitude            32.84
    Temperature (F)      59.57
    Humidity (%)            80
    Cloudiness (%)           0
    Wind Speed (mph)      6.89
    Name: 128, dtype: object anamur tr
    Processing Record 128of Set 1 | anamur
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=anamur&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=anamur&units=imperial
    129 City Name           ust-nera
    Country Code              ru
    Latitude               64.57
    Longitude             143.24
    Temperature (F)       -16.93
    Humidity (%)              66
    Cloudiness (%)            12
    Wind Speed (mph)        2.64
    Name: 129, dtype: object ust-nera ru
    Processing Record 129of Set 1 | ust-nera
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ust-nera&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ust-nera&units=imperial
    130 City Name           hobart
    Country Code            au
    Latitude            -42.88
    Longitude           147.33
    Temperature (F)       66.2
    Humidity (%)            55
    Cloudiness (%)          20
    Wind Speed (mph)     24.16
    Name: 130, dtype: object hobart au
    Processing Record 130of Set 1 | hobart
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hobart&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hobart&units=imperial
    131 City Name           kholmsk
    Country Code             ru
    Latitude              47.05
    Longitude            142.05
    Temperature (F)        28.4
    Humidity (%)             58
    Cloudiness (%)           40
    Wind Speed (mph)      11.18
    Name: 131, dtype: object kholmsk ru
    Processing Record 131of Set 1 | kholmsk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kholmsk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kholmsk&units=imperial
    132 City Name           farafangana
    Country Code                 mg
    Latitude                 -22.82
    Longitude                 47.83
    Temperature (F)           75.14
    Humidity (%)                100
    Cloudiness (%)              100
    Wind Speed (mph)          21.77
    Name: 132, dtype: object farafangana mg
    Processing Record 132of Set 1 | farafangana
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=farafangana&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=farafangana&units=imperial
    133 City Name           luderitz
    Country Code              na
    Latitude              -26.65
    Longitude              15.16
    Temperature (F)           59
    Humidity (%)              87
    Cloudiness (%)             0
    Wind Speed (mph)        5.82
    Name: 133, dtype: object luderitz na
    Processing Record 133of Set 1 | luderitz
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=luderitz&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=luderitz&units=imperial
    134 City Name           dikson
    Country Code            ru
    Latitude             73.51
    Longitude            80.55
    Temperature (F)      -7.75
    Humidity (%)           100
    Cloudiness (%)          64
    Wind Speed (mph)     31.61
    Name: 134, dtype: object dikson ru
    Processing Record 134of Set 1 | dikson
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dikson&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dikson&units=imperial
    135 City Name           palmer
    Country Code            us
    Latitude            -34.85
    Longitude           139.16
    Temperature (F)         68
    Humidity (%)            64
    Cloudiness (%)          75
    Wind Speed (mph)     17.22
    Name: 135, dtype: object palmer us
    Processing Record 135of Set 1 | palmer
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=palmer&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=palmer&units=imperial
    136 City Name           kodiak
    Country Code            us
    Latitude             39.95
    Longitude           -94.76
    Temperature (F)       44.6
    Humidity (%)            75
    Cloudiness (%)          90
    Wind Speed (mph)     11.41
    Name: 136, dtype: object kodiak us
    Processing Record 136of Set 1 | kodiak
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kodiak&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kodiak&units=imperial
    137 City Name           east london
    Country Code                 za
    Latitude                 -33.02
    Longitude                 27.91
    Temperature (F)           73.79
    Humidity (%)                 94
    Cloudiness (%)                0
    Wind Speed (mph)          11.81
    Name: 137, dtype: object east london za
    Processing Record 137of Set 1 | east london
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=east london&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=east%20london&units=imperial
    138 City Name           kalianget
    Country Code               id
    Latitude                -7.35
    Longitude              109.91
    Temperature (F)         70.37
    Humidity (%)               90
    Cloudiness (%)              0
    Wind Speed (mph)          3.2
    Name: 138, dtype: object kalianget id
    Processing Record 138of Set 1 | kalianget
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kalianget&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kalianget&units=imperial
    139 City Name           ahipara
    Country Code             nz
    Latitude             -35.17
    Longitude            173.16
    Temperature (F)       72.71
    Humidity (%)             79
    Cloudiness (%)           56
    Wind Speed (mph)      14.16
    Name: 139, dtype: object ahipara nz
    Processing Record 139of Set 1 | ahipara
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ahipara&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ahipara&units=imperial
    140 City Name           san carlos de bariloche
    Country Code                             ar
    Latitude                             -41.13
    Longitude                            -71.31
    Temperature (F)                        46.4
    Humidity (%)                             75
    Cloudiness (%)                           20
    Wind Speed (mph)                      27.51
    Name: 140, dtype: object san carlos de bariloche ar
    Processing Record 140of Set 1 | san carlos de bariloche
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=san carlos de bariloche&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=san%20carlos%20de%20bariloche&units=imperial
    141 City Name           aktau
    Country Code           kz
    Latitude            43.65
    Longitude           51.16
    Temperature (F)      42.8
    Humidity (%)          100
    Cloudiness (%)          8
    Wind Speed (mph)     8.95
    Name: 141, dtype: object aktau kz
    Processing Record 141of Set 1 | aktau
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=aktau&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=aktau&units=imperial
    142 City Name           port shepstone
    Country Code                    za
    Latitude                    -30.74
    Longitude                    30.45
    Temperature (F)              72.89
    Humidity (%)                    91
    Cloudiness (%)                   8
    Wind Speed (mph)              18.3
    Name: 142, dtype: object port shepstone za
    Processing Record 142of Set 1 | port shepstone
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port shepstone&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port%20shepstone&units=imperial
    143 City Name           norman wells
    Country Code                  ca
    Latitude                   65.28
    Longitude                -126.83
    Temperature (F)             15.8
    Humidity (%)                  92
    Cloudiness (%)                90
    Wind Speed (mph)            8.05
    Name: 143, dtype: object norman wells ca
    Processing Record 143of Set 1 | norman wells
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=norman wells&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=norman%20wells&units=imperial
    144 City Name           mardin
    Country Code            tr
    Latitude             37.32
    Longitude            40.72
    Temperature (F)      44.45
    Humidity (%)            70
    Cloudiness (%)          56
    Wind Speed (mph)      4.88
    Name: 144, dtype: object mardin tr
    Processing Record 144of Set 1 | mardin
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mardin&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mardin&units=imperial
    145 City Name            jalu
    Country Code           ly
    Latitude            29.03
    Longitude           21.55
    Temperature (F)     57.05
    Humidity (%)           54
    Cloudiness (%)          0
    Wind Speed (mph)     8.46
    Name: 145, dtype: object jalu ly
    Processing Record 145of Set 1 | jalu
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=jalu&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=jalu&units=imperial
    146 City Name           arani
    Country Code           bo
    Latitude            13.33
    Longitude           80.08
    Temperature (F)      75.2
    Humidity (%)           94
    Cloudiness (%)         20
    Wind Speed (mph)     3.36
    Name: 146, dtype: object arani bo
    Processing Record 146of Set 1 | arani
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=arani&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=arani&units=imperial
    147 City Name            tiksi
    Country Code            ru
    Latitude             71.64
    Longitude           128.87
    Temperature (F)     -35.02
    Humidity (%)            65
    Cloudiness (%)           8
    Wind Speed (mph)      2.86
    Name: 147, dtype: object tiksi ru
    Processing Record 147of Set 1 | tiksi
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tiksi&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tiksi&units=imperial
    148 City Name           portland
    Country Code              au
    Latitude               45.52
    Longitude            -122.67
    Temperature (F)        49.06
    Humidity (%)              54
    Cloudiness (%)            90
    Wind Speed (mph)        3.36
    Name: 148, dtype: object portland au
    Processing Record 148of Set 1 | portland
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=portland&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=portland&units=imperial
    149 City Name           severo-kurilsk
    Country Code                    ru
    Latitude                     50.68
    Longitude                   156.12
    Temperature (F)              26.27
    Humidity (%)                   100
    Cloudiness (%)                   0
    Wind Speed (mph)              4.09
    Name: 149, dtype: object severo-kurilsk ru
    Processing Record 149of Set 1 | severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=severo-kurilsk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=severo-kurilsk&units=imperial
    150 City Name           kikwit
    Country Code            cd
    Latitude             -5.04
    Longitude            18.82
    Temperature (F)      71.09
    Humidity (%)            93
    Cloudiness (%)          88
    Wind Speed (mph)      5.66
    Name: 150, dtype: object kikwit cd
    Processing Record 150of Set 1 | kikwit
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kikwit&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kikwit&units=imperial
    151 City Name           pendleton
    Country Code               us
    Latitude                34.65
    Longitude              -82.78
    Temperature (F)         65.86
    Humidity (%)               48
    Cloudiness (%)              1
    Wind Speed (mph)         3.76
    Name: 151, dtype: object pendleton us
    Processing Record 151of Set 1 | pendleton
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pendleton&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pendleton&units=imperial
    152 City Name           damietta
    Country Code              eg
    Latitude               31.42
    Longitude              31.81
    Temperature (F)         66.2
    Humidity (%)              82
    Cloudiness (%)             0
    Wind Speed (mph)        5.82
    Name: 152, dtype: object damietta eg
    Processing Record 152of Set 1 | damietta
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=damietta&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=damietta&units=imperial
    153 City Name           tiarei
    Country Code            pf
    Latitude            -17.53
    Longitude          -149.33
    Temperature (F)       78.8
    Humidity (%)            78
    Cloudiness (%)          75
    Wind Speed (mph)     16.11
    Name: 153, dtype: object tiarei pf
    Processing Record 153of Set 1 | tiarei
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tiarei&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tiarei&units=imperial
    155 City Name            pevek
    Country Code            ru
    Latitude              69.7
    Longitude           170.27
    Temperature (F)     -12.97
    Humidity (%)           100
    Cloudiness (%)          24
    Wind Speed (mph)      9.24
    Name: 155, dtype: object pevek ru
    Processing Record 155of Set 1 | pevek
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pevek&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pevek&units=imperial
    156 City Name           mumford
    Country Code             gh
    Latitude               5.26
    Longitude             -0.76
    Temperature (F)       78.92
    Humidity (%)             88
    Cloudiness (%)            0
    Wind Speed (mph)       9.35
    Name: 156, dtype: object mumford gh
    Processing Record 156of Set 1 | mumford
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mumford&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mumford&units=imperial
    157 City Name           champasak
    Country Code               la
    Latitude                14.89
    Longitude              105.88
    Temperature (F)          80.6
    Humidity (%)               74
    Cloudiness (%)             40
    Wind Speed (mph)         2.24
    Name: 157, dtype: object champasak la
    Processing Record 157of Set 1 | champasak
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=champasak&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=champasak&units=imperial
    159 City Name           longfeng
    Country Code              cn
    Latitude               31.05
    Longitude             103.87
    Temperature (F)         55.4
    Humidity (%)              93
    Cloudiness (%)             0
    Wind Speed (mph)        4.47
    Name: 159, dtype: object longfeng cn
    Processing Record 159of Set 1 | longfeng
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=longfeng&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=longfeng&units=imperial
    160 City Name           cape town
    Country Code               za
    Latitude               -33.93
    Longitude               18.42
    Temperature (F)          64.4
    Humidity (%)               88
    Cloudiness (%)             20
    Wind Speed (mph)        16.11
    Name: 160, dtype: object cape town za
    Processing Record 160of Set 1 | cape town
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cape town&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cape%20town&units=imperial
    162 City Name            gizo
    Country Code           sb
    Latitude             31.8
    Longitude           34.94
    Temperature (F)        59
    Humidity (%)           51
    Cloudiness (%)          0
    Wind Speed (mph)     3.36
    Name: 162, dtype: object gizo sb
    Processing Record 162of Set 1 | gizo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gizo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gizo&units=imperial
    163 City Name           ituango
    Country Code             co
    Latitude               7.17
    Longitude            -75.76
    Temperature (F)       65.78
    Humidity (%)            100
    Cloudiness (%)           20
    Wind Speed (mph)       1.63
    Name: 163, dtype: object ituango co
    Processing Record 163of Set 1 | ituango
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ituango&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ituango&units=imperial
    164 City Name           penzance
    Country Code              gb
    Latitude               50.12
    Longitude              -5.53
    Temperature (F)         30.2
    Humidity (%)              86
    Cloudiness (%)            92
    Wind Speed (mph)       24.16
    Name: 164, dtype: object penzance gb
    Processing Record 164of Set 1 | penzance
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=penzance&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=penzance&units=imperial
    165 City Name           hermanus
    Country Code              za
    Latitude              -34.42
    Longitude              19.24
    Temperature (F)         59.3
    Humidity (%)              92
    Cloudiness (%)            68
    Wind Speed (mph)        2.53
    Name: 165, dtype: object hermanus za
    Processing Record 165of Set 1 | hermanus
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hermanus&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hermanus&units=imperial
    166 City Name           banjar
    Country Code            id
    Latitude             -7.37
    Longitude           108.54
    Temperature (F)      72.35
    Humidity (%)            90
    Cloudiness (%)           0
    Wind Speed (mph)      2.42
    Name: 166, dtype: object banjar id
    Processing Record 166of Set 1 | banjar
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=banjar&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=banjar&units=imperial
    167 City Name           bayburt
    Country Code             tr
    Latitude              40.26
    Longitude             40.22
    Temperature (F)        31.4
    Humidity (%)             81
    Cloudiness (%)           80
    Wind Speed (mph)       2.64
    Name: 167, dtype: object bayburt tr
    Processing Record 167of Set 1 | bayburt
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bayburt&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bayburt&units=imperial
    168 City Name           ochakiv
    Country Code             ua
    Latitude              46.61
    Longitude             31.55
    Temperature (F)        21.2
    Humidity (%)             85
    Cloudiness (%)           90
    Wind Speed (mph)      15.66
    Name: 168, dtype: object ochakiv ua
    Processing Record 168of Set 1 | ochakiv
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ochakiv&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ochakiv&units=imperial
    170 City Name           belogorsk
    Country Code               ru
    Latitude                45.06
    Longitude               34.59
    Temperature (F)         44.18
    Humidity (%)               99
    Cloudiness (%)             92
    Wind Speed (mph)         3.87
    Name: 170, dtype: object belogorsk ru
    Processing Record 170of Set 1 | belogorsk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=belogorsk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=belogorsk&units=imperial
    171 City Name           balkanabat
    Country Code                tm
    Latitude                 39.51
    Longitude                54.36
    Temperature (F)          39.14
    Humidity (%)                96
    Cloudiness (%)              12
    Wind Speed (mph)          1.63
    Name: 171, dtype: object balkanabat tm
    Processing Record 171of Set 1 | balkanabat
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=balkanabat&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=balkanabat&units=imperial
    172 City Name           bambous virieux
    Country Code                     mu
    Latitude                     -20.34
    Longitude                     57.76
    Temperature (F)                80.6
    Humidity (%)                     83
    Cloudiness (%)                   40
    Wind Speed (mph)               9.17
    Name: 172, dtype: object bambous virieux mu
    Processing Record 172of Set 1 | bambous virieux
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bambous virieux&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bambous%20virieux&units=imperial
    173 City Name           vaitape
    Country Code             pf
    Latitude             -16.52
    Longitude           -151.75
    Temperature (F)       81.71
    Humidity (%)            100
    Cloudiness (%)           44
    Wind Speed (mph)      12.37
    Name: 173, dtype: object vaitape pf
    Processing Record 173of Set 1 | vaitape
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vaitape&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vaitape&units=imperial
    174 City Name           pangnirtung
    Country Code                 ca
    Latitude                  66.15
    Longitude                -65.72
    Temperature (F)            3.05
    Humidity (%)                 74
    Cloudiness (%)               48
    Wind Speed (mph)           2.75
    Name: 174, dtype: object pangnirtung ca
    Processing Record 174of Set 1 | pangnirtung
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pangnirtung&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pangnirtung&units=imperial
    176 City Name           muros
    Country Code           es
    Latitude            42.77
    Longitude           -9.06
    Temperature (F)      42.8
    Humidity (%)           87
    Cloudiness (%)         40
    Wind Speed (mph)     5.82
    Name: 176, dtype: object muros es
    Processing Record 176of Set 1 | muros
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=muros&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=muros&units=imperial
    177 City Name           vostok
    Country Code            ru
    Latitude             46.45
    Longitude           135.83
    Temperature (F)       9.98
    Humidity (%)            58
    Cloudiness (%)          56
    Wind Speed (mph)      9.46
    Name: 177, dtype: object vostok ru
    Processing Record 177of Set 1 | vostok
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vostok&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vostok&units=imperial
    178 City Name           katsuura
    Country Code              jp
    Latitude               33.93
    Longitude              134.5
    Temperature (F)         53.6
    Humidity (%)              93
    Cloudiness (%)            75
    Wind Speed (mph)        2.24
    Name: 178, dtype: object katsuura jp
    Processing Record 178of Set 1 | katsuura
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=katsuura&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=katsuura&units=imperial
    179 City Name           rio gallegos
    Country Code                  ar
    Latitude                  -51.62
    Longitude                 -69.22
    Temperature (F)             44.6
    Humidity (%)                  90
    Cloudiness (%)                 0
    Wind Speed (mph)           14.99
    Name: 179, dtype: object rio gallegos ar
    Processing Record 179of Set 1 | rio gallegos
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rio gallegos&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rio%20gallegos&units=imperial
    180 City Name           chingola
    Country Code              zm
    Latitude              -12.55
    Longitude              27.86
    Temperature (F)        63.53
    Humidity (%)              80
    Cloudiness (%)             0
    Wind Speed (mph)        5.88
    Name: 180, dtype: object chingola zm
    Processing Record 180of Set 1 | chingola
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=chingola&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=chingola&units=imperial
    181 City Name           shostka
    Country Code             ua
    Latitude              51.86
    Longitude             33.47
    Temperature (F)        5.03
    Humidity (%)             76
    Cloudiness (%)           76
    Wind Speed (mph)       10.8
    Name: 181, dtype: object shostka ua
    Processing Record 181of Set 1 | shostka
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=shostka&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=shostka&units=imperial
    182 City Name           iskitim
    Country Code             ru
    Latitude              54.64
    Longitude              83.3
    Temperature (F)        17.6
    Humidity (%)             72
    Cloudiness (%)            0
    Wind Speed (mph)      13.42
    Name: 182, dtype: object iskitim ru
    Processing Record 182of Set 1 | iskitim
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=iskitim&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=iskitim&units=imperial
    183 City Name           koslan
    Country Code            ru
    Latitude             63.46
    Longitude             48.9
    Temperature (F)       0.17
    Humidity (%)            73
    Cloudiness (%)          80
    Wind Speed (mph)      6.55
    Name: 183, dtype: object koslan ru
    Processing Record 183of Set 1 | koslan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=koslan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=koslan&units=imperial
    184 City Name           parana
    Country Code            br
    Latitude             -7.52
    Longitude           -72.89
    Temperature (F)       80.6
    Humidity (%)            78
    Cloudiness (%)          40
    Wind Speed (mph)      2.98
    Name: 184, dtype: object parana br
    Processing Record 184of Set 1 | parana
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=parana&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=parana&units=imperial
    185 City Name           atbasar
    Country Code             kz
    Latitude              51.81
    Longitude             68.36
    Temperature (F)       28.16
    Humidity (%)             87
    Cloudiness (%)           44
    Wind Speed (mph)      15.84
    Name: 185, dtype: object atbasar kz
    Processing Record 185of Set 1 | atbasar
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=atbasar&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=atbasar&units=imperial
    186 City Name             leh
    Country Code           in
    Latitude            34.16
    Longitude           77.58
    Temperature (F)    -11.17
    Humidity (%)           78
    Cloudiness (%)         48
    Wind Speed (mph)     1.74
    Name: 186, dtype: object leh in
    Processing Record 186of Set 1 | leh
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=leh&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=leh&units=imperial
    187 City Name           sehithwa
    Country Code              bw
    Latitude              -20.47
    Longitude               22.7
    Temperature (F)        73.07
    Humidity (%)              73
    Cloudiness (%)            88
    Wind Speed (mph)        1.74
    Name: 187, dtype: object sehithwa bw
    Processing Record 187of Set 1 | sehithwa
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sehithwa&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sehithwa&units=imperial
    188 City Name           yellowknife
    Country Code                 ca
    Latitude                  62.45
    Longitude               -114.38
    Temperature (F)              23
    Humidity (%)                 62
    Cloudiness (%)               75
    Wind Speed (mph)          12.75
    Name: 188, dtype: object yellowknife ca
    Processing Record 188of Set 1 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yellowknife&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yellowknife&units=imperial
    189 City Name           vardo
    Country Code           no
    Latitude            39.62
    Longitude          -77.74
    Temperature (F)     47.95
    Humidity (%)           29
    Cloudiness (%)          1
    Wind Speed (mph)     9.17
    Name: 189, dtype: object vardo no
    Processing Record 189of Set 1 | vardo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vardo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vardo&units=imperial
    190 City Name           butaritari
    Country Code                ki
    Latitude                  3.07
    Longitude               172.79
    Temperature (F)           81.8
    Humidity (%)               100
    Cloudiness (%)              32
    Wind Speed (mph)          8.23
    Name: 190, dtype: object butaritari ki
    Processing Record 190of Set 1 | butaritari
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=butaritari&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=butaritari&units=imperial
    191 City Name           acapulco
    Country Code              mx
    Latitude               16.86
    Longitude             -99.88
    Temperature (F)         78.8
    Humidity (%)              78
    Cloudiness (%)            75
    Wind Speed (mph)       13.87
    Name: 191, dtype: object acapulco mx
    Processing Record 191of Set 1 | acapulco
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=acapulco&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=acapulco&units=imperial
    193 City Name           nanortalik
    Country Code                gl
    Latitude                 60.14
    Longitude               -45.24
    Temperature (F)          34.37
    Humidity (%)                88
    Cloudiness (%)               8
    Wind Speed (mph)         46.93
    Name: 193, dtype: object nanortalik gl
    Processing Record 193of Set 1 | nanortalik
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nanortalik&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nanortalik&units=imperial
    194 City Name           talavera
    Country Code              pe
    Latitude               15.58
    Longitude             120.92
    Temperature (F)         78.8
    Humidity (%)              69
    Cloudiness (%)            40
    Wind Speed (mph)         4.7
    Name: 194, dtype: object talavera pe
    Processing Record 194of Set 1 | talavera
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=talavera&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=talavera&units=imperial
    196 City Name            inzai
    Country Code            jp
    Latitude              35.8
    Longitude           140.18
    Temperature (F)      56.07
    Humidity (%)            67
    Cloudiness (%)          75
    Wind Speed (mph)      5.82
    Name: 196, dtype: object inzai jp
    Processing Record 196of Set 1 | inzai
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=inzai&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=inzai&units=imperial
    197 City Name           kutum
    Country Code           sd
    Latitude             14.2
    Longitude           24.66
    Temperature (F)     52.91
    Humidity (%)           40
    Cloudiness (%)          0
    Wind Speed (mph)     3.76
    Name: 197, dtype: object kutum sd
    Processing Record 197of Set 1 | kutum
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kutum&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kutum&units=imperial
    198 City Name           mahebourg
    Country Code               mu
    Latitude               -20.41
    Longitude                57.7
    Temperature (F)          80.6
    Humidity (%)               83
    Cloudiness (%)             40
    Wind Speed (mph)         9.17
    Name: 198, dtype: object mahebourg mu
    Processing Record 198of Set 1 | mahebourg
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mahebourg&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mahebourg&units=imperial
    199 City Name           ponta do sol
    Country Code                  cv
    Latitude                  -20.63
    Longitude                    -46
    Temperature (F)            69.11
    Humidity (%)                  88
    Cloudiness (%)                 8
    Wind Speed (mph)            2.75
    Name: 199, dtype: object ponta do sol cv
    Processing Record 199of Set 1 | ponta do sol
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ponta do sol&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ponta%20do%20sol&units=imperial
    200 City Name           garowe
    Country Code            so
    Latitude              8.41
    Longitude            48.48
    Temperature (F)       64.7
    Humidity (%)            89
    Cloudiness (%)           0
    Wind Speed (mph)      6.89
    Name: 200, dtype: object garowe so
    Processing Record 200of Set 1 | garowe
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=garowe&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=garowe&units=imperial
    202 City Name           montepulciano
    Country Code                   it
    Latitude                    43.09
    Longitude                   11.78
    Temperature (F)              42.8
    Humidity (%)                   87
    Cloudiness (%)                 40
    Wind Speed (mph)             6.93
    Name: 202, dtype: object montepulciano it
    Processing Record 202of Set 1 | montepulciano
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=montepulciano&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=montepulciano&units=imperial
    203 City Name           bandarbeyla
    Country Code                 so
    Latitude                   9.49
    Longitude                 50.81
    Temperature (F)           77.66
    Humidity (%)                100
    Cloudiness (%)                0
    Wind Speed (mph)            8.9
    Name: 203, dtype: object bandarbeyla so
    Processing Record 203of Set 1 | bandarbeyla
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bandarbeyla&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bandarbeyla&units=imperial
    204 City Name           luanda
    Country Code            ao
    Latitude             -8.83
    Longitude            13.24
    Temperature (F)       78.8
    Humidity (%)            88
    Cloudiness (%)          75
    Wind Speed (mph)      3.36
    Name: 204, dtype: object luanda ao
    Processing Record 204of Set 1 | luanda
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=luanda&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=luanda&units=imperial
    205 City Name           yarada
    Country Code            in
    Latitude             17.65
    Longitude            83.27
    Temperature (F)      81.35
    Humidity (%)           100
    Cloudiness (%)           8
    Wind Speed (mph)      8.23
    Name: 205, dtype: object yarada in
    Processing Record 205of Set 1 | yarada
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yarada&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yarada&units=imperial
    206 City Name           kangaatsiaq
    Country Code                 gl
    Latitude                  68.31
    Longitude                -53.46
    Temperature (F)           15.47
    Humidity (%)                 91
    Cloudiness (%)                0
    Wind Speed (mph)           9.57
    Name: 206, dtype: object kangaatsiaq gl
    Processing Record 206of Set 1 | kangaatsiaq
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kangaatsiaq&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kangaatsiaq&units=imperial
    207 City Name           komsomolskiy
    Country Code                  ru
    Latitude                   67.55
    Longitude                  63.78
    Temperature (F)           -12.61
    Humidity (%)                  77
    Cloudiness (%)                44
    Wind Speed (mph)            9.91
    Name: 207, dtype: object komsomolskiy ru
    Processing Record 207of Set 1 | komsomolskiy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=komsomolskiy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=komsomolskiy&units=imperial
    208 City Name           rodrigues alves
    Country Code                     br
    Latitude                      -7.74
    Longitude                    -72.65
    Temperature (F)                80.6
    Humidity (%)                     78
    Cloudiness (%)                   40
    Wind Speed (mph)               2.98
    Name: 208, dtype: object rodrigues alves br
    Processing Record 208of Set 1 | rodrigues alves
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rodrigues alves&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rodrigues%20alves&units=imperial
    209 City Name           dongsheng
    Country Code               cn
    Latitude                29.72
    Longitude              112.52
    Temperature (F)         45.26
    Humidity (%)               93
    Cloudiness (%)             88
    Wind Speed (mph)        11.81
    Name: 209, dtype: object dongsheng cn
    Processing Record 209of Set 1 | dongsheng
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dongsheng&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dongsheng&units=imperial
    210 City Name           esperance
    Country Code               au
    Latitude                10.24
    Longitude              -61.45
    Temperature (F)          78.8
    Humidity (%)               65
    Cloudiness (%)             20
    Wind Speed (mph)          4.7
    Name: 210, dtype: object esperance au
    Processing Record 210of Set 1 | esperance
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=esperance&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=esperance&units=imperial
    211 City Name           salalah
    Country Code             om
    Latitude              17.01
    Longitude              54.1
    Temperature (F)        73.4
    Humidity (%)             73
    Cloudiness (%)            0
    Wind Speed (mph)        4.7
    Name: 211, dtype: object salalah om
    Processing Record 211of Set 1 | salalah
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=salalah&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=salalah&units=imperial
    212 City Name           san rafael
    Country Code                ar
    Latitude                -34.61
    Longitude               -68.33
    Temperature (F)          63.08
    Humidity (%)                14
    Cloudiness (%)               0
    Wind Speed (mph)          8.57
    Name: 212, dtype: object san rafael ar
    Processing Record 212of Set 1 | san rafael
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=san rafael&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=san%20rafael&units=imperial
    214 City Name           grand gaube
    Country Code                 mu
    Latitude                 -20.01
    Longitude                 57.66
    Temperature (F)            80.6
    Humidity (%)                 83
    Cloudiness (%)               40
    Wind Speed (mph)           9.17
    Name: 214, dtype: object grand gaube mu
    Processing Record 214of Set 1 | grand gaube
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=grand gaube&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=grand%20gaube&units=imperial
    215 City Name           saskylakh
    Country Code               ru
    Latitude                71.97
    Longitude              114.09
    Temperature (F)        -23.95
    Humidity (%)               71
    Cloudiness (%)             32
    Wind Speed (mph)        18.07
    Name: 215, dtype: object saskylakh ru
    Processing Record 215of Set 1 | saskylakh
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saskylakh&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saskylakh&units=imperial
    216 City Name           bo rai
    Country Code            th
    Latitude             12.57
    Longitude           102.54
    Temperature (F)      73.07
    Humidity (%)            91
    Cloudiness (%)          36
    Wind Speed (mph)      1.86
    Name: 216, dtype: object bo rai th
    Processing Record 216of Set 1 | bo rai
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bo rai&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bo%20rai&units=imperial
    217 City Name           robertsport
    Country Code                 lr
    Latitude                   6.75
    Longitude                -11.37
    Temperature (F)           79.82
    Humidity (%)                 98
    Cloudiness (%)               20
    Wind Speed (mph)           8.79
    Name: 217, dtype: object robertsport lr
    Processing Record 217of Set 1 | robertsport
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=robertsport&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=robertsport&units=imperial
    218 City Name           usinsk
    Country Code            ru
    Latitude                66
    Longitude            57.56
    Temperature (F)        -10
    Humidity (%)            69
    Cloudiness (%)          48
    Wind Speed (mph)      5.21
    Name: 218, dtype: object usinsk ru
    Processing Record 218of Set 1 | usinsk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=usinsk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=usinsk&units=imperial
    219 City Name           kaitangata
    Country Code                nz
    Latitude                -46.28
    Longitude               169.85
    Temperature (F)          71.81
    Humidity (%)                57
    Cloudiness (%)              92
    Wind Speed (mph)           9.8
    Name: 219, dtype: object kaitangata nz
    Processing Record 219of Set 1 | kaitangata
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kaitangata&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kaitangata&units=imperial
    220 City Name           xocali
    Country Code            az
    Latitude             39.91
    Longitude            46.79
    Temperature (F)       38.6
    Humidity (%)            72
    Cloudiness (%)           8
    Wind Speed (mph)      2.19
    Name: 220, dtype: object xocali az
    Processing Record 220of Set 1 | xocali
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=xocali&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=xocali&units=imperial
    221 City Name           sistranda
    Country Code               no
    Latitude                63.73
    Longitude                8.83
    Temperature (F)          39.2
    Humidity (%)               80
    Cloudiness (%)             75
    Wind Speed (mph)         26.4
    Name: 221, dtype: object sistranda no
    Processing Record 221of Set 1 | sistranda
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sistranda&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sistranda&units=imperial
    223 City Name           hastings
    Country Code              us
    Latitude              -39.64
    Longitude             176.84
    Temperature (F)        65.33
    Humidity (%)              96
    Cloudiness (%)            92
    Wind Speed (mph)       11.48
    Name: 223, dtype: object hastings us
    Processing Record 223of Set 1 | hastings
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hastings&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hastings&units=imperial
    224 City Name           los llanos de aridane
    Country Code                           es
    Latitude                            28.66
    Longitude                          -17.92
    Temperature (F)                      64.4
    Humidity (%)                           72
    Cloudiness (%)                         44
    Wind Speed (mph)                     3.36
    Name: 224, dtype: object los llanos de aridane es
    Processing Record 224of Set 1 | los llanos de aridane
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=los llanos de aridane&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=los%20llanos%20de%20aridane&units=imperial
    225 City Name           hirara
    Country Code            jp
    Latitude              24.8
    Longitude           125.28
    Temperature (F)      74.32
    Humidity (%)            78
    Cloudiness (%)          20
    Wind Speed (mph)      9.17
    Name: 225, dtype: object hirara jp
    Processing Record 225of Set 1 | hirara
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hirara&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hirara&units=imperial
    226 City Name           coihaique
    Country Code               cl
    Latitude               -45.58
    Longitude              -72.07
    Temperature (F)          51.8
    Humidity (%)               62
    Cloudiness (%)             75
    Wind Speed (mph)         6.93
    Name: 226, dtype: object coihaique cl
    Processing Record 226of Set 1 | coihaique
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=coihaique&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=coihaique&units=imperial
    227 City Name           lagoa
    Country Code           pt
    Latitude            37.14
    Longitude           -8.45
    Temperature (F)        50
    Humidity (%)           87
    Cloudiness (%)         40
    Wind Speed (mph)     5.82
    Name: 227, dtype: object lagoa pt
    Processing Record 227of Set 1 | lagoa
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lagoa&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lagoa&units=imperial
    228 City Name           cidreira
    Country Code              br
    Latitude              -30.17
    Longitude             -50.22
    Temperature (F)        79.28
    Humidity (%)              82
    Cloudiness (%)            64
    Wind Speed (mph)        9.13
    Name: 228, dtype: object cidreira br
    Processing Record 228of Set 1 | cidreira
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cidreira&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cidreira&units=imperial
    230 City Name           loandjili
    Country Code               cg
    Latitude                -4.77
    Longitude               11.87
    Temperature (F)          82.4
    Humidity (%)               78
    Cloudiness (%)             40
    Wind Speed (mph)          4.7
    Name: 230, dtype: object loandjili cg
    Processing Record 230of Set 1 | loandjili
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=loandjili&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=loandjili&units=imperial
    231 City Name           noyabrsk
    Country Code              ru
    Latitude                63.2
    Longitude              75.45
    Temperature (F)        16.01
    Humidity (%)              92
    Cloudiness (%)            92
    Wind Speed (mph)       14.16
    Name: 231, dtype: object noyabrsk ru
    Processing Record 231of Set 1 | noyabrsk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=noyabrsk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=noyabrsk&units=imperial
    232 City Name           richards bay
    Country Code                  za
    Latitude                  -28.77
    Longitude                  32.06
    Temperature (F)            67.31
    Humidity (%)                  90
    Cloudiness (%)                 0
    Wind Speed (mph)            7.45
    Name: 232, dtype: object richards bay za
    Processing Record 232of Set 1 | richards bay
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=richards bay&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=richards%20bay&units=imperial
    233 City Name           port alfred
    Country Code                 za
    Latitude                 -33.59
    Longitude                 26.89
    Temperature (F)           68.48
    Humidity (%)                 94
    Cloudiness (%)                0
    Wind Speed (mph)           8.68
    Name: 233, dtype: object port alfred za
    Processing Record 233of Set 1 | port alfred
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port alfred&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port%20alfred&units=imperial
    234 City Name           naze
    Country Code          jp
    Latitude            5.43
    Longitude           7.07
    Temperature (F)     80.6
    Humidity (%)          83
    Cloudiness (%)        40
    Wind Speed (mph)    4.43
    Name: 234, dtype: object naze jp
    Processing Record 234of Set 1 | naze
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=naze&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=naze&units=imperial
    235 City Name           zhigalovo
    Country Code               ru
    Latitude                54.81
    Longitude              105.15
    Temperature (F)         12.32
    Humidity (%)               69
    Cloudiness (%)             36
    Wind Speed (mph)         2.19
    Name: 235, dtype: object zhigalovo ru
    Processing Record 235of Set 1 | zhigalovo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=zhigalovo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=zhigalovo&units=imperial
    236 City Name           clearlake
    Country Code               us
    Latitude                38.96
    Longitude             -122.63
    Temperature (F)         56.32
    Humidity (%)               47
    Cloudiness (%)              1
    Wind Speed (mph)          4.7
    Name: 236, dtype: object clearlake us
    Processing Record 236of Set 1 | clearlake
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=clearlake&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=clearlake&units=imperial
    237 City Name           necochea
    Country Code              ar
    Latitude              -38.55
    Longitude             -58.74
    Temperature (F)         59.3
    Humidity (%)              89
    Cloudiness (%)           100
    Wind Speed (mph)       14.27
    Name: 237, dtype: object necochea ar
    Processing Record 237of Set 1 | necochea
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=necochea&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=necochea&units=imperial
    238 City Name           tuatapere
    Country Code               nz
    Latitude               -46.13
    Longitude              167.69
    Temperature (F)         68.48
    Humidity (%)               74
    Cloudiness (%)             32
    Wind Speed (mph)        19.53
    Name: 238, dtype: object tuatapere nz
    Processing Record 238of Set 1 | tuatapere
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tuatapere&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tuatapere&units=imperial
    239 City Name           nadvoitsy
    Country Code               ru
    Latitude                63.89
    Longitude               34.27
    Temperature (F)         22.94
    Humidity (%)               68
    Cloudiness (%)              0
    Wind Speed (mph)         8.34
    Name: 239, dtype: object nadvoitsy ru
    Processing Record 239of Set 1 | nadvoitsy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nadvoitsy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nadvoitsy&units=imperial
    240 City Name           torbay
    Country Code            ca
    Latitude             47.66
    Longitude           -52.73
    Temperature (F)       30.2
    Humidity (%)            86
    Cloudiness (%)          75
    Wind Speed (mph)     23.04
    Name: 240, dtype: object torbay ca
    Processing Record 240of Set 1 | torbay
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=torbay&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=torbay&units=imperial
    241 City Name           prokopyevsk
    Country Code                 ru
    Latitude                  53.89
    Longitude                 86.74
    Temperature (F)            7.01
    Humidity (%)                 61
    Cloudiness (%)                0
    Wind Speed (mph)           2.75
    Name: 241, dtype: object prokopyevsk ru
    Processing Record 241of Set 1 | prokopyevsk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=prokopyevsk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=prokopyevsk&units=imperial
    242 City Name           yerbogachen
    Country Code                 ru
    Latitude                  61.28
    Longitude                108.01
    Temperature (F)           23.84
    Humidity (%)                 91
    Cloudiness (%)               92
    Wind Speed (mph)           7.45
    Name: 242, dtype: object yerbogachen ru
    Processing Record 242of Set 1 | yerbogachen
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yerbogachen&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yerbogachen&units=imperial
    243 City Name           north bend
    Country Code                us
    Latitude                 43.41
    Longitude              -124.22
    Temperature (F)          50.36
    Humidity (%)                66
    Cloudiness (%)              75
    Wind Speed (mph)          3.36
    Name: 243, dtype: object north bend us
    Processing Record 243of Set 1 | north bend
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=north bend&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=north%20bend&units=imperial
    244 City Name            tura
    Country Code           ru
    Latitude            25.52
    Longitude           90.21
    Temperature (F)     54.17
    Humidity (%)           74
    Cloudiness (%)          0
    Wind Speed (mph)     4.65
    Name: 244, dtype: object tura ru
    Processing Record 244of Set 1 | tura
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tura&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tura&units=imperial
    245 City Name            lata
    Country Code           sb
    Latitude            30.78
    Longitude           78.62
    Temperature (F)     42.47
    Humidity (%)           56
    Cloudiness (%)          8
    Wind Speed (mph)      3.2
    Name: 245, dtype: object lata sb
    Processing Record 245of Set 1 | lata
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lata&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lata&units=imperial
    246 City Name           praia da vitoria
    Country Code                      pt
    Latitude                       38.73
    Longitude                     -27.07
    Temperature (F)                 60.8
    Humidity (%)                      93
    Cloudiness (%)                    20
    Wind Speed (mph)               10.69
    Name: 246, dtype: object praia da vitoria pt
    Processing Record 246of Set 1 | praia da vitoria
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=praia da vitoria&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=praia%20da%20vitoria&units=imperial
    248 City Name            emba
    Country Code           kz
    Latitude            34.81
    Longitude           32.42
    Temperature (F)     57.78
    Humidity (%)           62
    Cloudiness (%)          0
    Wind Speed (mph)     5.82
    Name: 248, dtype: object emba kz
    Processing Record 248of Set 1 | emba
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=emba&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=emba&units=imperial
    249 City Name           thai binh
    Country Code               vn
    Latitude                20.45
    Longitude              106.33
    Temperature (F)         72.17
    Humidity (%)               97
    Cloudiness (%)             92
    Wind Speed (mph)         8.01
    Name: 249, dtype: object thai binh vn
    Processing Record 249of Set 1 | thai binh
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=thai binh&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=thai%20binh&units=imperial
    250 City Name           faanui
    Country Code            pf
    Latitude            -16.48
    Longitude          -151.75
    Temperature (F)      82.25
    Humidity (%)           100
    Cloudiness (%)          48
    Wind Speed (mph)     13.82
    Name: 250, dtype: object faanui pf
    Processing Record 250of Set 1 | faanui
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=faanui&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=faanui&units=imperial
    251 City Name           quetta
    Country Code            pk
    Latitude             30.19
    Longitude            67.02
    Temperature (F)      34.37
    Humidity (%)            72
    Cloudiness (%)           0
    Wind Speed (mph)      1.74
    Name: 251, dtype: object quetta pk
    Processing Record 251of Set 1 | quetta
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=quetta&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=quetta&units=imperial
    252 City Name           sibolga
    Country Code             id
    Latitude               1.74
    Longitude             98.78
    Temperature (F)       68.48
    Humidity (%)             99
    Cloudiness (%)           24
    Wind Speed (mph)       2.19
    Name: 252, dtype: object sibolga id
    Processing Record 252of Set 1 | sibolga
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sibolga&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sibolga&units=imperial
    254 City Name           bensonville
    Country Code                 lr
    Latitude                   6.45
    Longitude                -10.61
    Temperature (F)            78.8
    Humidity (%)                 94
    Cloudiness (%)               75
    Wind Speed (mph)           6.78
    Name: 254, dtype: object bensonville lr
    Processing Record 254of Set 1 | bensonville
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bensonville&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bensonville&units=imperial
    255 City Name           nongan
    Country Code            cn
    Latitude             44.43
    Longitude           125.17
    Temperature (F)       24.8
    Humidity (%)            32
    Cloudiness (%)           0
    Wind Speed (mph)     11.18
    Name: 255, dtype: object nongan cn
    Processing Record 255of Set 1 | nongan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nongan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nongan&units=imperial
    256 City Name           christchurch
    Country Code                  nz
    Latitude                  -43.53
    Longitude                 172.64
    Temperature (F)             71.6
    Humidity (%)                  64
    Cloudiness (%)                44
    Wind Speed (mph)            9.17
    Name: 256, dtype: object christchurch nz
    Processing Record 256of Set 1 | christchurch
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=christchurch&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=christchurch&units=imperial
    257 City Name           ballina
    Country Code             au
    Latitude              54.11
    Longitude             -9.15
    Temperature (F)       32.21
    Humidity (%)             85
    Cloudiness (%)            8
    Wind Speed (mph)      12.71
    Name: 257, dtype: object ballina au
    Processing Record 257of Set 1 | ballina
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ballina&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ballina&units=imperial
    258 City Name           riyadh
    Country Code            sa
    Latitude             24.63
    Longitude            46.72
    Temperature (F)       62.6
    Humidity (%)            21
    Cloudiness (%)           0
    Wind Speed (mph)      3.36
    Name: 258, dtype: object riyadh sa
    Processing Record 258of Set 1 | riyadh
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=riyadh&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=riyadh&units=imperial
    259 City Name           cherskiy
    Country Code              ru
    Latitude               68.75
    Longitude              161.3
    Temperature (F)        -6.49
    Humidity (%)              59
    Cloudiness (%)            24
    Wind Speed (mph)         9.8
    Name: 259, dtype: object cherskiy ru
    Processing Record 259of Set 1 | cherskiy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cherskiy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cherskiy&units=imperial
    260 City Name             vao
    Country Code           nc
    Latitude             59.1
    Longitude           26.19
    Temperature (F)     27.17
    Humidity (%)           84
    Cloudiness (%)         44
    Wind Speed (mph)    13.04
    Name: 260, dtype: object vao nc
    Processing Record 260of Set 1 | vao
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vao&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vao&units=imperial
    261 City Name           saint george
    Country Code                  bm
    Latitude                   39.45
    Longitude                  22.34
    Temperature (F)               59
    Humidity (%)                  38
    Cloudiness (%)                20
    Wind Speed (mph)            6.93
    Name: 261, dtype: object saint george bm
    Processing Record 261of Set 1 | saint george
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saint george&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saint%20george&units=imperial
    262 City Name           matamoros
    Country Code               mx
    Latitude                25.87
    Longitude              -97.51
    Temperature (F)          82.4
    Humidity (%)               74
    Cloudiness (%)              1
    Wind Speed (mph)        13.87
    Name: 262, dtype: object matamoros mx
    Processing Record 262of Set 1 | matamoros
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=matamoros&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=matamoros&units=imperial
    263 City Name           ponta delgada
    Country Code                   pt
    Latitude                    37.73
    Longitude                  -25.67
    Temperature (F)              60.8
    Humidity (%)                  100
    Cloudiness (%)                 75
    Wind Speed (mph)            10.29
    Name: 263, dtype: object ponta delgada pt
    Processing Record 263of Set 1 | ponta delgada
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ponta delgada&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ponta%20delgada&units=imperial
    264 City Name           te anau
    Country Code             nz
    Latitude             -45.41
    Longitude            167.72
    Temperature (F)       63.89
    Humidity (%)             74
    Cloudiness (%)           56
    Wind Speed (mph)      18.86
    Name: 264, dtype: object te anau nz
    Processing Record 264of Set 1 | te anau
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=te anau&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=te%20anau&units=imperial
    265 City Name           felipe carrillo puerto
    Country Code                            mx
    Latitude                             19.58
    Longitude                           -88.05
    Temperature (F)                      82.88
    Humidity (%)                            58
    Cloudiness (%)                           0
    Wind Speed (mph)                      8.23
    Name: 265, dtype: object felipe carrillo puerto mx
    Processing Record 265of Set 1 | felipe carrillo puerto
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=felipe carrillo puerto&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=felipe%20carrillo%20puerto&units=imperial
    266 City Name           aripuana
    Country Code              br
    Latitude               -9.17
    Longitude             -60.63
    Temperature (F)        79.37
    Humidity (%)              84
    Cloudiness (%)            44
    Wind Speed (mph)        2.64
    Name: 266, dtype: object aripuana br
    Processing Record 266of Set 1 | aripuana
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=aripuana&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=aripuana&units=imperial
    268 City Name           comodoro rivadavia
    Country Code                        ar
    Latitude                        -45.87
    Longitude                       -67.48
    Temperature (F)                   66.2
    Humidity (%)                        32
    Cloudiness (%)                       0
    Wind Speed (mph)                 28.86
    Name: 268, dtype: object comodoro rivadavia ar
    Processing Record 268of Set 1 | comodoro rivadavia
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=comodoro rivadavia&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=comodoro%20rivadavia&units=imperial
    269 City Name           athabasca
    Country Code               ca
    Latitude                54.72
    Longitude             -113.29
    Temperature (F)          28.7
    Humidity (%)               86
    Cloudiness (%)             24
    Wind Speed (mph)         3.98
    Name: 269, dtype: object athabasca ca
    Processing Record 269of Set 1 | athabasca
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=athabasca&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=athabasca&units=imperial
    270 City Name           birur
    Country Code           in
    Latitude            13.62
    Longitude           75.97
    Temperature (F)     62.18
    Humidity (%)           95
    Cloudiness (%)         12
    Wind Speed (mph)     1.63
    Name: 270, dtype: object birur in
    Processing Record 270of Set 1 | birur
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=birur&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=birur&units=imperial
    271 City Name           nishihara
    Country Code               jp
    Latitude                35.74
    Longitude              139.53
    Temperature (F)          54.1
    Humidity (%)               66
    Cloudiness (%)             75
    Wind Speed (mph)          4.7
    Name: 271, dtype: object nishihara jp
    Processing Record 271of Set 1 | nishihara
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nishihara&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nishihara&units=imperial
    272 City Name           basco
    Country Code           ph
    Latitude            40.33
    Longitude           -91.2
    Temperature (F)     47.48
    Humidity (%)           57
    Cloudiness (%)          1
    Wind Speed (mph)      4.7
    Name: 272, dtype: object basco ph
    Processing Record 272of Set 1 | basco
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=basco&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=basco&units=imperial
    273 City Name           zavetnoye
    Country Code               ru
    Latitude                47.12
    Longitude               43.89
    Temperature (F)         22.58
    Humidity (%)               90
    Cloudiness (%)             80
    Wind Speed (mph)        19.08
    Name: 273, dtype: object zavetnoye ru
    Processing Record 273of Set 1 | zavetnoye
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=zavetnoye&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=zavetnoye&units=imperial
    274 City Name           caravelas
    Country Code               br
    Latitude               -17.73
    Longitude              -39.27
    Temperature (F)         82.25
    Humidity (%)              100
    Cloudiness (%)              0
    Wind Speed (mph)         13.6
    Name: 274, dtype: object caravelas br
    Processing Record 274of Set 1 | caravelas
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=caravelas&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=caravelas&units=imperial
    275 City Name           baykit
    Country Code            ru
    Latitude             61.68
    Longitude            96.39
    Temperature (F)      22.67
    Humidity (%)            78
    Cloudiness (%)          56
    Wind Speed (mph)      6.11
    Name: 275, dtype: object baykit ru
    Processing Record 275of Set 1 | baykit
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=baykit&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=baykit&units=imperial
    276 City Name           mabaruma
    Country Code              gy
    Latitude                 8.2
    Longitude             -59.78
    Temperature (F)        77.03
    Humidity (%)              83
    Cloudiness (%)            12
    Wind Speed (mph)        7.78
    Name: 276, dtype: object mabaruma gy
    Processing Record 276of Set 1 | mabaruma
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mabaruma&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mabaruma&units=imperial
    278 City Name           kavieng
    Country Code             pg
    Latitude              -2.57
    Longitude             150.8
    Temperature (F)       82.61
    Humidity (%)            100
    Cloudiness (%)           92
    Wind Speed (mph)      12.37
    Name: 278, dtype: object kavieng pg
    Processing Record 278of Set 1 | kavieng
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kavieng&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kavieng&units=imperial
    279 City Name           ilulissat
    Country Code               gl
    Latitude                69.22
    Longitude               -51.1
    Temperature (F)          26.6
    Humidity (%)               68
    Cloudiness (%)              0
    Wind Speed (mph)         5.82
    Name: 279, dtype: object ilulissat gl
    Processing Record 279of Set 1 | ilulissat
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ilulissat&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ilulissat&units=imperial
    280 City Name           beyneu
    Country Code            kz
    Latitude             45.32
    Longitude            55.19
    Temperature (F)       39.5
    Humidity (%)            93
    Cloudiness (%)          68
    Wind Speed (mph)         7
    Name: 280, dtype: object beyneu kz
    Processing Record 280of Set 1 | beyneu
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=beyneu&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=beyneu&units=imperial
    281 City Name           victoria
    Country Code              sc
    Latitude                5.28
    Longitude             115.24
    Temperature (F)        79.72
    Humidity (%)             100
    Cloudiness (%)            75
    Wind Speed (mph)           7
    Name: 281, dtype: object victoria sc
    Processing Record 281of Set 1 | victoria
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=victoria&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=victoria&units=imperial
    282 City Name            umea
    Country Code           se
    Latitude            63.83
    Longitude           20.26
    Temperature (F)      33.8
    Humidity (%)           64
    Cloudiness (%)         75
    Wind Speed (mph)     5.82
    Name: 282, dtype: object umea se
    Processing Record 282of Set 1 | umea
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=umea&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=umea&units=imperial
    283 City Name           san cristobal
    Country Code                   ec
    Latitude                    -0.39
    Longitude                  -78.55
    Temperature (F)              60.8
    Humidity (%)                   63
    Cloudiness (%)                 75
    Wind Speed (mph)             3.36
    Name: 283, dtype: object san cristobal ec
    Processing Record 283of Set 1 | san cristobal
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=san cristobal&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=san%20cristobal&units=imperial
    284 City Name           port blair
    Country Code                in
    Latitude                 11.67
    Longitude                92.75
    Temperature (F)          82.25
    Humidity (%)               100
    Cloudiness (%)               0
    Wind Speed (mph)          9.24
    Name: 284, dtype: object port blair in
    Processing Record 284of Set 1 | port blair
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port blair&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port%20blair&units=imperial
    285 City Name           nhulunbuy
    Country Code               au
    Latitude               -12.18
    Longitude              136.78
    Temperature (F)          78.8
    Humidity (%)               88
    Cloudiness (%)              0
    Wind Speed (mph)         5.82
    Name: 285, dtype: object nhulunbuy au
    Processing Record 285of Set 1 | nhulunbuy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nhulunbuy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nhulunbuy&units=imperial
    286 City Name            kieta
    Country Code            pg
    Latitude             -6.22
    Longitude           155.63
    Temperature (F)      77.12
    Humidity (%)           100
    Cloudiness (%)          80
    Wind Speed (mph)      2.98
    Name: 286, dtype: object kieta pg
    Processing Record 286of Set 1 | kieta
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kieta&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kieta&units=imperial
    287 City Name           road town
    Country Code               vg
    Latitude                18.42
    Longitude              -64.62
    Temperature (F)         76.59
    Humidity (%)               94
    Cloudiness (%)              1
    Wind Speed (mph)         6.93
    Name: 287, dtype: object road town vg
    Processing Record 287of Set 1 | road town
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=road town&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=road%20town&units=imperial
    288 City Name           olinda
    Country Code            br
    Latitude             -2.03
    Longitude           -79.75
    Temperature (F)       82.4
    Humidity (%)            74
    Cloudiness (%)          20
    Wind Speed (mph)      3.36
    Name: 288, dtype: object olinda br
    Processing Record 288of Set 1 | olinda
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=olinda&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=olinda&units=imperial
    289 City Name           orange cove
    Country Code                 us
    Latitude                  36.62
    Longitude               -119.31
    Temperature (F)           57.56
    Humidity (%)                 50
    Cloudiness (%)                1
    Wind Speed (mph)           8.05
    Name: 289, dtype: object orange cove us
    Processing Record 289of Set 1 | orange cove
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=orange cove&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=orange%20cove&units=imperial
    290 City Name           swan river
    Country Code                ca
    Latitude                 52.11
    Longitude              -101.27
    Temperature (F)          18.89
    Humidity (%)                88
    Cloudiness (%)              76
    Wind Speed (mph)          7.67
    Name: 290, dtype: object swan river ca
    Processing Record 290of Set 1 | swan river
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=swan river&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=swan%20river&units=imperial
    293 City Name           cayenne
    Country Code             gf
    Latitude               4.94
    Longitude            -52.33
    Temperature (F)          77
    Humidity (%)             94
    Cloudiness (%)           76
    Wind Speed (mph)      10.29
    Name: 293, dtype: object cayenne gf
    Processing Record 293of Set 1 | cayenne
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cayenne&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cayenne&units=imperial
    295 City Name             sur
    Country Code           om
    Latitude            22.57
    Longitude           59.53
    Temperature (F)     72.98
    Humidity (%)           99
    Cloudiness (%)          0
    Wind Speed (mph)     7.23
    Name: 295, dtype: object sur om
    Processing Record 295of Set 1 | sur
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sur&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sur&units=imperial
    296 City Name           progreso
    Country Code              mx
    Latitude              -34.68
    Longitude             -56.22
    Temperature (F)         66.2
    Humidity (%)              72
    Cloudiness (%)            75
    Wind Speed (mph)        20.8
    Name: 296, dtype: object progreso mx
    Processing Record 296of Set 1 | progreso
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=progreso&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=progreso&units=imperial
    297 City Name           kloulklubed
    Country Code                 pw
    Latitude                   7.04
    Longitude                134.26
    Temperature (F)           83.26
    Humidity (%)                 79
    Cloudiness (%)               75
    Wind Speed (mph)            4.7
    Name: 297, dtype: object kloulklubed pw
    Processing Record 297of Set 1 | kloulklubed
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kloulklubed&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kloulklubed&units=imperial
    298 City Name           deming
    Country Code            us
    Latitude             32.27
    Longitude          -107.76
    Temperature (F)      52.54
    Humidity (%)            28
    Cloudiness (%)           1
    Wind Speed (mph)     28.86
    Name: 298, dtype: object deming us
    Processing Record 298of Set 1 | deming
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=deming&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=deming&units=imperial
    299 City Name           eenhana
    Country Code             na
    Latitude             -17.48
    Longitude             16.34
    Temperature (F)       67.31
    Humidity (%)             93
    Cloudiness (%)           68
    Wind Speed (mph)       5.66
    Name: 299, dtype: object eenhana na
    Processing Record 299of Set 1 | eenhana
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=eenhana&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=eenhana&units=imperial
    301 City Name           saint-joseph
    Country Code                  re
    Latitude                   43.56
    Longitude                   6.97
    Temperature (F)            46.47
    Humidity (%)                  93
    Cloudiness (%)                 0
    Wind Speed (mph)            2.24
    Name: 301, dtype: object saint-joseph re
    Processing Record 301of Set 1 | saint-joseph
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saint-joseph&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saint-joseph&units=imperial
    302 City Name           urubamba
    Country Code              pe
    Latitude              -13.31
    Longitude             -72.12
    Temperature (F)           59
    Humidity (%)              58
    Cloudiness (%)            90
    Wind Speed (mph)        5.82
    Name: 302, dtype: object urubamba pe
    Processing Record 302of Set 1 | urubamba
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=urubamba&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=urubamba&units=imperial
    303 City Name           upernavik
    Country Code               gl
    Latitude                72.79
    Longitude              -56.15
    Temperature (F)          4.31
    Humidity (%)               98
    Cloudiness (%)             32
    Wind Speed (mph)        20.65
    Name: 303, dtype: object upernavik gl
    Processing Record 303of Set 1 | upernavik
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=upernavik&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=upernavik&units=imperial
    304 City Name           yulara
    Country Code            au
    Latitude            -25.24
    Longitude           130.99
    Temperature (F)       80.6
    Humidity (%)            24
    Cloudiness (%)           0
    Wind Speed (mph)      20.8
    Name: 304, dtype: object yulara au
    Processing Record 304of Set 1 | yulara
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yulara&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yulara&units=imperial
    305 City Name           bethel
    Country Code            us
    Latitude             60.79
    Longitude          -161.76
    Temperature (F)       30.2
    Humidity (%)           100
    Cloudiness (%)          90
    Wind Speed (mph)     10.29
    Name: 305, dtype: object bethel us
    Processing Record 305of Set 1 | bethel
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bethel&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bethel&units=imperial
    306 City Name           iqaluit
    Country Code             ca
    Latitude              63.75
    Longitude            -68.52
    Temperature (F)         8.6
    Humidity (%)             78
    Cloudiness (%)           40
    Wind Speed (mph)       6.93
    Name: 306, dtype: object iqaluit ca
    Processing Record 306of Set 1 | iqaluit
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=iqaluit&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=iqaluit&units=imperial
    308 City Name           vila velha
    Country Code                br
    Latitude                 -3.71
    Longitude                -38.6
    Temperature (F)           82.4
    Humidity (%)                78
    Cloudiness (%)               0
    Wind Speed (mph)          9.17
    Name: 308, dtype: object vila velha br
    Processing Record 308of Set 1 | vila velha
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vila velha&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vila%20velha&units=imperial
    309 City Name            hovd
    Country Code           mn
    Latitude            63.83
    Longitude            10.7
    Temperature (F)     37.47
    Humidity (%)           93
    Cloudiness (%)         90
    Wind Speed (mph)    24.16
    Name: 309, dtype: object hovd mn
    Processing Record 309of Set 1 | hovd
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hovd&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hovd&units=imperial
    310 City Name           torrington
    Country Code                us
    Latitude                 42.06
    Longitude              -104.18
    Temperature (F)          43.27
    Humidity (%)                80
    Cloudiness (%)              90
    Wind Speed (mph)         19.46
    Name: 310, dtype: object torrington us
    Processing Record 310of Set 1 | torrington
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=torrington&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=torrington&units=imperial
    311 City Name           kunnamangalam
    Country Code                   in
    Latitude                     11.3
    Longitude                   75.88
    Temperature (F)                77
    Humidity (%)                   83
    Cloudiness (%)                 40
    Wind Speed (mph)             2.24
    Name: 311, dtype: object kunnamangalam in
    Processing Record 311of Set 1 | kunnamangalam
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kunnamangalam&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kunnamangalam&units=imperial
    312 City Name           tarakan
    Country Code             id
    Latitude                3.3
    Longitude            117.63
    Temperature (F)       78.83
    Humidity (%)            100
    Cloudiness (%)           48
    Wind Speed (mph)       3.65
    Name: 312, dtype: object tarakan id
    Processing Record 312of Set 1 | tarakan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tarakan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tarakan&units=imperial
    313 City Name           prince rupert
    Country Code                   ca
    Latitude                    54.32
    Longitude                 -130.32
    Temperature (F)              44.6
    Humidity (%)                   87
    Cloudiness (%)                 75
    Wind Speed (mph)             6.93
    Name: 313, dtype: object prince rupert ca
    Processing Record 313of Set 1 | prince rupert
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=prince rupert&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=prince%20rupert&units=imperial
    314 City Name           fortuna
    Country Code             us
    Latitude              38.18
    Longitude             -1.13
    Temperature (F)        48.2
    Humidity (%)             61
    Cloudiness (%)            0
    Wind Speed (mph)       3.36
    Name: 314, dtype: object fortuna us
    Processing Record 314of Set 1 | fortuna
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fortuna&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fortuna&units=imperial
    315 City Name           dankov
    Country Code            ru
    Latitude             53.25
    Longitude            39.16
    Temperature (F)     -10.63
    Humidity (%)            54
    Cloudiness (%)           0
    Wind Speed (mph)       3.2
    Name: 315, dtype: object dankov ru
    Processing Record 315of Set 1 | dankov
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dankov&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dankov&units=imperial
    316 City Name           tuktoyaktuk
    Country Code                 ca
    Latitude                  69.44
    Longitude               -133.03
    Temperature (F)           -8.34
    Humidity (%)                 76
    Cloudiness (%)               75
    Wind Speed (mph)           9.17
    Name: 316, dtype: object tuktoyaktuk ca
    Processing Record 316of Set 1 | tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tuktoyaktuk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tuktoyaktuk&units=imperial
    317 City Name           north platte
    Country Code                  us
    Latitude                   41.12
    Longitude                -100.77
    Temperature (F)             39.2
    Humidity (%)                  80
    Cloudiness (%)                90
    Wind Speed (mph)           12.75
    Name: 317, dtype: object north platte us
    Processing Record 317of Set 1 | north platte
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=north platte&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=north%20platte&units=imperial
    318 City Name           alice springs
    Country Code                   au
    Latitude                    -23.7
    Longitude                  133.88
    Temperature (F)              78.8
    Humidity (%)                   29
    Cloudiness (%)                  0
    Wind Speed (mph)            13.87
    Name: 318, dtype: object alice springs au
    Processing Record 318of Set 1 | alice springs
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=alice springs&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=alice%20springs&units=imperial
    319 City Name           payakumbuh
    Country Code                id
    Latitude                 -0.23
    Longitude               100.63
    Temperature (F)          69.83
    Humidity (%)                97
    Cloudiness (%)              92
    Wind Speed (mph)          2.42
    Name: 319, dtype: object payakumbuh id
    Processing Record 319of Set 1 | payakumbuh
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=payakumbuh&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=payakumbuh&units=imperial
    322 City Name           sandnessjoen
    Country Code                  no
    Latitude                   66.02
    Longitude                  12.63
    Temperature (F)               32
    Humidity (%)                  99
    Cloudiness (%)                88
    Wind Speed (mph)           14.99
    Name: 322, dtype: object sandnessjoen no
    Processing Record 322of Set 1 | sandnessjoen
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sandnessjoen&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sandnessjoen&units=imperial
    323 City Name           maumere
    Country Code             id
    Latitude              -8.63
    Longitude            122.22
    Temperature (F)       81.71
    Humidity (%)            100
    Cloudiness (%)           24
    Wind Speed (mph)       2.64
    Name: 323, dtype: object maumere id
    Processing Record 323of Set 1 | maumere
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=maumere&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=maumere&units=imperial
    324 City Name           kavaratti
    Country Code               in
    Latitude                10.57
    Longitude               72.64
    Temperature (F)         81.98
    Humidity (%)              100
    Cloudiness (%)              0
    Wind Speed (mph)         5.21
    Name: 324, dtype: object kavaratti in
    Processing Record 324of Set 1 | kavaratti
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kavaratti&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kavaratti&units=imperial
    325 City Name           mareeba
    Country Code             au
    Latitude             -16.99
    Longitude            145.42
    Temperature (F)        84.2
    Humidity (%)             70
    Cloudiness (%)           75
    Wind Speed (mph)      16.11
    Name: 325, dtype: object mareeba au
    Processing Record 325of Set 1 | mareeba
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mareeba&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mareeba&units=imperial
    326 City Name           aracuai
    Country Code             br
    Latitude             -16.85
    Longitude            -42.07
    Temperature (F)       77.21
    Humidity (%)             67
    Cloudiness (%)           88
    Wind Speed (mph)       8.34
    Name: 326, dtype: object aracuai br
    Processing Record 326of Set 1 | aracuai
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=aracuai&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=aracuai&units=imperial
    327 City Name            faya
    Country Code           td
    Latitude            18.39
    Longitude           42.45
    Temperature (F)     59.04
    Humidity (%)           54
    Cloudiness (%)          0
    Wind Speed (mph)     3.36
    Name: 327, dtype: object faya td
    Processing Record 327of Set 1 | faya
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=faya&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=faya&units=imperial
    329 City Name           pangody
    Country Code             ru
    Latitude              65.85
    Longitude             74.49
    Temperature (F)       -7.39
    Humidity (%)             61
    Cloudiness (%)           48
    Wind Speed (mph)       6.67
    Name: 329, dtype: object pangody ru
    Processing Record 329of Set 1 | pangody
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pangody&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pangody&units=imperial
    330 City Name           kiunga
    Country Code            pg
    Latitude             -6.12
    Longitude            141.3
    Temperature (F)      76.58
    Humidity (%)            94
    Cloudiness (%)          92
    Wind Speed (mph)      1.97
    Name: 330, dtype: object kiunga pg
    Processing Record 330of Set 1 | kiunga
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kiunga&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kiunga&units=imperial
    331 City Name           lavrentiya
    Country Code                ru
    Latitude                 65.58
    Longitude              -170.99
    Temperature (F)          27.08
    Humidity (%)               100
    Cloudiness (%)              48
    Wind Speed (mph)          9.35
    Name: 331, dtype: object lavrentiya ru
    Processing Record 331of Set 1 | lavrentiya
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lavrentiya&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lavrentiya&units=imperial
    332 City Name           hambantota
    Country Code                lk
    Latitude                  6.12
    Longitude                81.12
    Temperature (F)          80.18
    Humidity (%)               100
    Cloudiness (%)               0
    Wind Speed (mph)         10.58
    Name: 332, dtype: object hambantota lk
    Processing Record 332of Set 1 | hambantota
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hambantota&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hambantota&units=imperial
    333 City Name            tigil
    Country Code            ru
    Latitude              57.8
    Longitude           158.67
    Temperature (F)      14.12
    Humidity (%)            90
    Cloudiness (%)          68
    Wind Speed (mph)      5.32
    Name: 333, dtype: object tigil ru
    Processing Record 333of Set 1 | tigil
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tigil&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tigil&units=imperial
    335 City Name            buala
    Country Code            sb
    Latitude             -8.15
    Longitude           159.59
    Temperature (F)      84.41
    Humidity (%)            90
    Cloudiness (%)          24
    Wind Speed (mph)      2.98
    Name: 335, dtype: object buala sb
    Processing Record 335of Set 1 | buala
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=buala&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=buala&units=imperial
    336 City Name           forio
    Country Code           it
    Latitude            40.73
    Longitude           13.86
    Temperature (F)     50.86
    Humidity (%)           87
    Cloudiness (%)         40
    Wind Speed (mph)    18.52
    Name: 336, dtype: object forio it
    Processing Record 336of Set 1 | forio
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=forio&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=forio&units=imperial
    337 City Name           jining
    Country Code            cn
    Latitude             35.41
    Longitude           116.58
    Temperature (F)      42.47
    Humidity (%)            93
    Cloudiness (%)         100
    Wind Speed (mph)      7.45
    Name: 337, dtype: object jining cn
    Processing Record 337of Set 1 | jining
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=jining&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=jining&units=imperial
    338 City Name           vermilion
    Country Code               ca
    Latitude                53.35
    Longitude             -110.85
    Temperature (F)          24.8
    Humidity (%)               92
    Cloudiness (%)             90
    Wind Speed (mph)        10.29
    Name: 338, dtype: object vermilion ca
    Processing Record 338of Set 1 | vermilion
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vermilion&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vermilion&units=imperial
    340 City Name           oranjemund
    Country Code                na
    Latitude                -28.55
    Longitude                16.43
    Temperature (F)          61.55
    Humidity (%)                96
    Cloudiness (%)              92
    Wind Speed (mph)         18.41
    Name: 340, dtype: object oranjemund na
    Processing Record 340of Set 1 | oranjemund
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=oranjemund&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=oranjemund&units=imperial
    341 City Name           beaverton
    Country Code               ca
    Latitude                45.49
    Longitude              -122.8
    Temperature (F)         49.53
    Humidity (%)               87
    Cloudiness (%)             90
    Wind Speed (mph)         6.93
    Name: 341, dtype: object beaverton ca
    Processing Record 341of Set 1 | beaverton
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=beaverton&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=beaverton&units=imperial
    342 City Name           camacha
    Country Code             pt
    Latitude              33.08
    Longitude            -16.33
    Temperature (F)        62.6
    Humidity (%)             88
    Cloudiness (%)           40
    Wind Speed (mph)       9.17
    Name: 342, dtype: object camacha pt
    Processing Record 342of Set 1 | camacha
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=camacha&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=camacha&units=imperial
    343 City Name           kirakira
    Country Code              sb
    Latitude              -10.46
    Longitude             161.92
    Temperature (F)        78.56
    Humidity (%)             100
    Cloudiness (%)            92
    Wind Speed (mph)        2.64
    Name: 343, dtype: object kirakira sb
    Processing Record 343of Set 1 | kirakira
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kirakira&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kirakira&units=imperial
    344 City Name           guilin
    Country Code            cn
    Latitude             25.28
    Longitude           110.29
    Temperature (F)       51.8
    Humidity (%)            93
    Cloudiness (%)          75
    Wind Speed (mph)      8.95
    Name: 344, dtype: object guilin cn
    Processing Record 344of Set 1 | guilin
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=guilin&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=guilin&units=imperial
    345 City Name           port lincoln
    Country Code                  au
    Latitude                  -34.72
    Longitude                 135.86
    Temperature (F)            68.66
    Humidity (%)                  82
    Cloudiness (%)                76
    Wind Speed (mph)           16.96
    Name: 345, dtype: object port lincoln au
    Processing Record 345of Set 1 | port lincoln
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port lincoln&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port%20lincoln&units=imperial
    346 City Name           havelock
    Country Code              us
    Latitude               34.88
    Longitude              -76.9
    Temperature (F)         49.3
    Humidity (%)              61
    Cloudiness (%)             1
    Wind Speed (mph)       11.41
    Name: 346, dtype: object havelock us
    Processing Record 346of Set 1 | havelock
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=havelock&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=havelock&units=imperial
    347 City Name           sijunjung
    Country Code               id
    Latitude                -0.69
    Longitude              100.95
    Temperature (F)         69.29
    Humidity (%)               98
    Cloudiness (%)             88
    Wind Speed (mph)         2.53
    Name: 347, dtype: object sijunjung id
    Processing Record 347of Set 1 | sijunjung
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sijunjung&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sijunjung&units=imperial
    348 City Name           kyaikkami
    Country Code               mm
    Latitude                16.08
    Longitude               97.57
    Temperature (F)          68.3
    Humidity (%)               91
    Cloudiness (%)              0
    Wind Speed (mph)          3.2
    Name: 348, dtype: object kyaikkami mm
    Processing Record 348of Set 1 | kyaikkami
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kyaikkami&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kyaikkami&units=imperial
    349 City Name           cortez
    Country Code            us
    Latitude             37.35
    Longitude          -108.58
    Temperature (F)       33.8
    Humidity (%)            80
    Cloudiness (%)          75
    Wind Speed (mph)      9.17
    Name: 349, dtype: object cortez us
    Processing Record 349of Set 1 | cortez
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cortez&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cortez&units=imperial
    350 City Name           samarai
    Country Code             pg
    Latitude             -10.62
    Longitude            150.67
    Temperature (F)       76.94
    Humidity (%)            100
    Cloudiness (%)           92
    Wind Speed (mph)       6.78
    Name: 350, dtype: object samarai pg
    Processing Record 350of Set 1 | samarai
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=samarai&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=samarai&units=imperial
    351 City Name           yuanping
    Country Code              cn
    Latitude               38.72
    Longitude             112.71
    Temperature (F)        32.21
    Humidity (%)              92
    Cloudiness (%)            56
    Wind Speed (mph)        0.96
    Name: 351, dtype: object yuanping cn
    Processing Record 351of Set 1 | yuanping
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yuanping&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yuanping&units=imperial
    352 City Name           kahta
    Country Code           tr
    Latitude            37.78
    Longitude           38.62
    Temperature (F)      55.4
    Humidity (%)           50
    Cloudiness (%)          0
    Wind Speed (mph)     3.36
    Name: 352, dtype: object kahta tr
    Processing Record 352of Set 1 | kahta
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kahta&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kahta&units=imperial
    353 City Name           kunstat
    Country Code             cz
    Latitude              49.51
    Longitude             16.52
    Temperature (F)       17.58
    Humidity (%)             67
    Cloudiness (%)           20
    Wind Speed (mph)       8.05
    Name: 353, dtype: object kunstat cz
    Processing Record 353of Set 1 | kunstat
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kunstat&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kunstat&units=imperial
    354 City Name           virginia beach
    Country Code                    us
    Latitude                     36.85
    Longitude                   -75.98
    Temperature (F)              44.24
    Humidity (%)                    60
    Cloudiness (%)                  20
    Wind Speed (mph)               4.7
    Name: 354, dtype: object virginia beach us
    Processing Record 354of Set 1 | virginia beach
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=virginia beach&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=virginia%20beach&units=imperial
    355 City Name           nouadhibou
    Country Code                mr
    Latitude                 20.93
    Longitude               -17.03
    Temperature (F)           64.4
    Humidity (%)                77
    Cloudiness (%)              44
    Wind Speed (mph)         23.04
    Name: 355, dtype: object nouadhibou mr
    Processing Record 355of Set 1 | nouadhibou
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nouadhibou&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nouadhibou&units=imperial
    356 City Name           sandovo
    Country Code             ru
    Latitude              58.46
    Longitude             36.41
    Temperature (F)       23.66
    Humidity (%)             89
    Cloudiness (%)           76
    Wind Speed (mph)      12.59
    Name: 356, dtype: object sandovo ru
    Processing Record 356of Set 1 | sandovo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sandovo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sandovo&units=imperial
    357 City Name           safford
    Country Code             us
    Latitude              32.83
    Longitude           -109.71
    Temperature (F)        57.2
    Humidity (%)             20
    Cloudiness (%)            1
    Wind Speed (mph)      21.92
    Name: 357, dtype: object safford us
    Processing Record 357of Set 1 | safford
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=safford&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=safford&units=imperial
    358 City Name           laguna
    Country Code            br
    Latitude             27.52
    Longitude          -110.01
    Temperature (F)       71.6
    Humidity (%)            40
    Cloudiness (%)          90
    Wind Speed (mph)      9.17
    Name: 358, dtype: object laguna br
    Processing Record 358of Set 1 | laguna
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=laguna&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=laguna&units=imperial
    359 City Name           qorveh
    Country Code            ir
    Latitude             35.17
    Longitude             47.8
    Temperature (F)       23.3
    Humidity (%)            68
    Cloudiness (%)           0
    Wind Speed (mph)      2.86
    Name: 359, dtype: object qorveh ir
    Processing Record 359of Set 1 | qorveh
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=qorveh&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=qorveh&units=imperial
    360 City Name           carmen
    Country Code            mx
    Latitude               7.2
    Longitude            124.8
    Temperature (F)      80.72
    Humidity (%)            68
    Cloudiness (%)          76
    Wind Speed (mph)      3.76
    Name: 360, dtype: object carmen mx
    Processing Record 360of Set 1 | carmen
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=carmen&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=carmen&units=imperial
    361 City Name           ponta do sol
    Country Code                  pt
    Latitude                  -20.63
    Longitude                    -46
    Temperature (F)            69.11
    Humidity (%)                  88
    Cloudiness (%)                 8
    Wind Speed (mph)            2.75
    Name: 361, dtype: object ponta do sol pt
    Processing Record 361of Set 1 | ponta do sol
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ponta do sol&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ponta%20do%20sol&units=imperial
    362 City Name           mantamados
    Country Code                gr
    Latitude                 39.31
    Longitude                26.33
    Temperature (F)          55.97
    Humidity (%)               100
    Cloudiness (%)              36
    Wind Speed (mph)         11.48
    Name: 362, dtype: object mantamados gr
    Processing Record 362of Set 1 | mantamados
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mantamados&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mantamados&units=imperial
    363 City Name           puerto carreno
    Country Code                    co
    Latitude                      6.19
    Longitude                   -67.49
    Temperature (F)               93.2
    Humidity (%)                    41
    Cloudiness (%)                  40
    Wind Speed (mph)              5.82
    Name: 363, dtype: object puerto carreno co
    Processing Record 363of Set 1 | puerto carreno
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=puerto carreno&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=puerto%20carreno&units=imperial
    364 City Name           bocas del toro
    Country Code                    pa
    Latitude                      9.33
    Longitude                   -82.25
    Temperature (F)              80.63
    Humidity (%)                   100
    Cloudiness (%)                   0
    Wind Speed (mph)              7.78
    Name: 364, dtype: object bocas del toro pa
    Processing Record 364of Set 1 | bocas del toro
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bocas del toro&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bocas%20del%20toro&units=imperial
    365 City Name           ciudad bolivar
    Country Code                    ve
    Latitude                      8.12
    Longitude                   -63.55
    Temperature (F)              87.11
    Humidity (%)                    37
    Cloudiness (%)                   0
    Wind Speed (mph)             14.16
    Name: 365, dtype: object ciudad bolivar ve
    Processing Record 365of Set 1 | ciudad bolivar
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ciudad bolivar&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ciudad%20bolivar&units=imperial
    366 City Name            kenai
    Country Code            us
    Latitude             60.55
    Longitude          -151.26
    Temperature (F)      36.52
    Humidity (%)           100
    Cloudiness (%)          90
    Wind Speed (mph)      6.93
    Name: 366, dtype: object kenai us
    Processing Record 366of Set 1 | kenai
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kenai&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kenai&units=imperial
    367 City Name           buzdyak
    Country Code             ru
    Latitude              54.57
    Longitude             54.53
    Temperature (F)        1.88
    Humidity (%)             77
    Cloudiness (%)           44
    Wind Speed (mph)      15.39
    Name: 367, dtype: object buzdyak ru
    Processing Record 367of Set 1 | buzdyak
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=buzdyak&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=buzdyak&units=imperial
    368 City Name             gat
    Country Code           ly
    Latitude            14.69
    Longitude          -16.54
    Temperature (F)      78.8
    Humidity (%)           39
    Cloudiness (%)          0
    Wind Speed (mph)      4.7
    Name: 368, dtype: object gat ly
    Processing Record 368of Set 1 | gat
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gat&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gat&units=imperial
    369 City Name           geraldton
    Country Code               au
    Latitude                49.72
    Longitude              -86.95
    Temperature (F)          17.6
    Humidity (%)               26
    Cloudiness (%)             75
    Wind Speed (mph)         9.17
    Name: 369, dtype: object geraldton au
    Processing Record 369of Set 1 | geraldton
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=geraldton&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=geraldton&units=imperial
    370 City Name            chuy
    Country Code           uy
    Latitude           -33.69
    Longitude          -53.46
    Temperature (F)     70.55
    Humidity (%)           94
    Cloudiness (%)         44
    Wind Speed (mph)    27.13
    Name: 370, dtype: object chuy uy
    Processing Record 370of Set 1 | chuy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=chuy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=chuy&units=imperial
    371 City Name           yatou
    Country Code           cn
    Latitude             3.63
    Longitude            9.81
    Temperature (F)      80.6
    Humidity (%)           94
    Cloudiness (%)         75
    Wind Speed (mph)     3.36
    Name: 371, dtype: object yatou cn
    Processing Record 371of Set 1 | yatou
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yatou&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yatou&units=imperial
    372 City Name           pavlodar
    Country Code              kz
    Latitude               52.28
    Longitude              76.96
    Temperature (F)         21.2
    Humidity (%)              92
    Cloudiness (%)             0
    Wind Speed (mph)        8.95
    Name: 372, dtype: object pavlodar kz
    Processing Record 372of Set 1 | pavlodar
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pavlodar&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pavlodar&units=imperial
    373 City Name           morro bay
    Country Code               us
    Latitude                35.37
    Longitude             -120.85
    Temperature (F)         56.64
    Humidity (%)               62
    Cloudiness (%)              1
    Wind Speed (mph)        13.87
    Name: 373, dtype: object morro bay us
    Processing Record 373of Set 1 | morro bay
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=morro bay&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=morro%20bay&units=imperial
    374 City Name           porto novo
    Country Code                cv
    Latitude                -23.68
    Longitude               -45.44
    Temperature (F)           73.4
    Humidity (%)                88
    Cloudiness (%)               0
    Wind Speed (mph)          5.82
    Name: 374, dtype: object porto novo cv
    Processing Record 374of Set 1 | porto novo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=porto novo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=porto%20novo&units=imperial
    376 City Name           pontes e lacerda
    Country Code                      br
    Latitude                      -15.23
    Longitude                     -59.33
    Temperature (F)                77.93
    Humidity (%)                      81
    Cloudiness (%)                    20
    Wind Speed (mph)                5.44
    Name: 376, dtype: object pontes e lacerda br
    Processing Record 376of Set 1 | pontes e lacerda
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pontes e lacerda&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pontes%20e%20lacerda&units=imperial
    377 City Name           mount gambier
    Country Code                   au
    Latitude                   -37.83
    Longitude                  140.78
    Temperature (F)             64.88
    Humidity (%)                   81
    Cloudiness (%)                 92
    Wind Speed (mph)            10.92
    Name: 377, dtype: object mount gambier au
    Processing Record 377of Set 1 | mount gambier
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mount gambier&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mount%20gambier&units=imperial
    378 City Name           svetlaya
    Country Code              ru
    Latitude               46.54
    Longitude             138.33
    Temperature (F)        23.93
    Humidity (%)             100
    Cloudiness (%)            36
    Wind Speed (mph)       23.44
    Name: 378, dtype: object svetlaya ru
    Processing Record 378of Set 1 | svetlaya
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=svetlaya&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=svetlaya&units=imperial
    380 City Name           fulton
    Country Code            us
    Latitude             43.32
    Longitude           -76.42
    Temperature (F)      30.96
    Humidity (%)            42
    Cloudiness (%)          90
    Wind Speed (mph)      5.82
    Name: 380, dtype: object fulton us
    Processing Record 380of Set 1 | fulton
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fulton&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fulton&units=imperial
    381 City Name           ampanihy
    Country Code              mg
    Latitude              -24.69
    Longitude              44.75
    Temperature (F)        73.25
    Humidity (%)              81
    Cloudiness (%)             0
    Wind Speed (mph)       11.48
    Name: 381, dtype: object ampanihy mg
    Processing Record 381of Set 1 | ampanihy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ampanihy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ampanihy&units=imperial
    382 City Name            kumo
    Country Code           ng
    Latitude            10.05
    Longitude           11.21
    Temperature (F)     69.29
    Humidity (%)           38
    Cloudiness (%)          0
    Wind Speed (mph)     3.98
    Name: 382, dtype: object kumo ng
    Processing Record 382of Set 1 | kumo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kumo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kumo&units=imperial
    383 City Name           ikalamavony
    Country Code                 mg
    Latitude                 -21.17
    Longitude                  46.6
    Temperature (F)           62.45
    Humidity (%)                 99
    Cloudiness (%)               92
    Wind Speed (mph)           3.98
    Name: 383, dtype: object ikalamavony mg
    Processing Record 383of Set 1 | ikalamavony
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ikalamavony&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ikalamavony&units=imperial
    384 City Name           mortka
    Country Code            ru
    Latitude             59.33
    Longitude            66.02
    Temperature (F)      31.22
    Humidity (%)            93
    Cloudiness (%)          88
    Wind Speed (mph)      4.88
    Name: 384, dtype: object mortka ru
    Processing Record 384of Set 1 | mortka
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mortka&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mortka&units=imperial
    386 City Name           roald
    Country Code           no
    Latitude            62.58
    Longitude            6.12
    Temperature (F)     36.36
    Humidity (%)          100
    Cloudiness (%)         75
    Wind Speed (mph)     26.4
    Name: 386, dtype: object roald no
    Processing Record 386of Set 1 | roald
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=roald&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=roald&units=imperial
    387 City Name           opuwo
    Country Code           na
    Latitude           -18.06
    Longitude           13.84
    Temperature (F)     63.89
    Humidity (%)           85
    Cloudiness (%)         36
    Wind Speed (mph)     3.09
    Name: 387, dtype: object opuwo na
    Processing Record 387of Set 1 | opuwo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=opuwo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=opuwo&units=imperial
    388 City Name           bowral
    Country Code            au
    Latitude            -34.48
    Longitude           150.42
    Temperature (F)         77
    Humidity (%)            27
    Cloudiness (%)           0
    Wind Speed (mph)     13.87
    Name: 388, dtype: object bowral au
    Processing Record 388of Set 1 | bowral
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bowral&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bowral&units=imperial
    389 City Name           cockburn town
    Country Code                   tc
    Latitude                    21.46
    Longitude                  -71.14
    Temperature (F)             74.78
    Humidity (%)                  100
    Cloudiness (%)                 32
    Wind Speed (mph)             6.89
    Name: 389, dtype: object cockburn town tc
    Processing Record 389of Set 1 | cockburn town
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cockburn town&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cockburn%20town&units=imperial
    390 City Name           vievis
    Country Code            lt
    Latitude             54.77
    Longitude            24.82
    Temperature (F)      17.69
    Humidity (%)            78
    Cloudiness (%)           0
    Wind Speed (mph)      5.82
    Name: 390, dtype: object vievis lt
    Processing Record 390of Set 1 | vievis
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vievis&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vievis&units=imperial
    392 City Name           hanyang
    Country Code             cn
    Latitude              32.14
    Longitude            105.51
    Temperature (F)       50.84
    Humidity (%)             63
    Cloudiness (%)           92
    Wind Speed (mph)       4.99
    Name: 392, dtype: object hanyang cn
    Processing Record 392of Set 1 | hanyang
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hanyang&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hanyang&units=imperial
    393 City Name            tabat
    Country Code            sd
    Latitude             -2.64
    Longitude           115.23
    Temperature (F)      77.03
    Humidity (%)            91
    Cloudiness (%)          76
    Wind Speed (mph)      2.98
    Name: 393, dtype: object tabat sd
    Processing Record 393of Set 1 | tabat
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tabat&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tabat&units=imperial
    394 City Name           buraydah
    Country Code              sa
    Latitude               26.33
    Longitude              43.97
    Temperature (F)         64.4
    Humidity (%)              34
    Cloudiness (%)             0
    Wind Speed (mph)         4.7
    Name: 394, dtype: object buraydah sa
    Processing Record 394of Set 1 | buraydah
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=buraydah&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=buraydah&units=imperial
    395 City Name           sesvete
    Country Code             hr
    Latitude              45.83
    Longitude             16.11
    Temperature (F)        30.2
    Humidity (%)             92
    Cloudiness (%)           90
    Wind Speed (mph)      12.75
    Name: 395, dtype: object sesvete hr
    Processing Record 395of Set 1 | sesvete
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sesvete&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sesvete&units=imperial
    396 City Name           zhangye
    Country Code             cn
    Latitude              38.94
    Longitude            100.46
    Temperature (F)       27.08
    Humidity (%)             59
    Cloudiness (%)            0
    Wind Speed (mph)       6.44
    Name: 396, dtype: object zhangye cn
    Processing Record 396of Set 1 | zhangye
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=zhangye&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=zhangye&units=imperial
    397 City Name           moose factory
    Country Code                   ca
    Latitude                    51.26
    Longitude                  -80.61
    Temperature (F)               1.4
    Humidity (%)                   64
    Cloudiness (%)                  1
    Wind Speed (mph)              4.7
    Name: 397, dtype: object moose factory ca
    Processing Record 397of Set 1 | moose factory
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=moose factory&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=moose%20factory&units=imperial
    398 City Name           jinka
    Country Code           et
    Latitude             5.79
    Longitude           36.57
    Temperature (F)     60.92
    Humidity (%)           94
    Cloudiness (%)         20
    Wind Speed (mph)     2.53
    Name: 398, dtype: object jinka et
    Processing Record 398of Set 1 | jinka
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=jinka&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=jinka&units=imperial
    399 City Name           hasaki
    Country Code            jp
    Latitude             35.73
    Longitude           140.83
    Temperature (F)      56.07
    Humidity (%)            67
    Cloudiness (%)          75
    Wind Speed (mph)      5.82
    Name: 399, dtype: object hasaki jp
    Processing Record 399of Set 1 | hasaki
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hasaki&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hasaki&units=imperial
    400 City Name           hastings
    Country Code              nz
    Latitude              -39.64
    Longitude             176.84
    Temperature (F)        65.33
    Humidity (%)              96
    Cloudiness (%)            92
    Wind Speed (mph)       11.48
    Name: 400, dtype: object hastings nz
    Processing Record 400of Set 1 | hastings
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hastings&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hastings&units=imperial
    402 City Name           beringovskiy
    Country Code                  ru
    Latitude                   63.05
    Longitude                 179.32
    Temperature (F)            10.07
    Humidity (%)                 100
    Cloudiness (%)                88
    Wind Speed (mph)           22.66
    Name: 402, dtype: object beringovskiy ru
    Processing Record 402of Set 1 | beringovskiy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=beringovskiy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=beringovskiy&units=imperial
    403 City Name           alugan
    Country Code            ph
    Latitude             12.22
    Longitude           125.48
    Temperature (F)      79.82
    Humidity (%)           100
    Cloudiness (%)          92
    Wind Speed (mph)      6.55
    Name: 403, dtype: object alugan ph
    Processing Record 403of Set 1 | alugan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=alugan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=alugan&units=imperial
    404 City Name           san-pedro
    Country Code               ci
    Latitude                 4.75
    Longitude               -6.64
    Temperature (F)         77.93
    Humidity (%)               89
    Cloudiness (%)              0
    Wind Speed (mph)         8.12
    Name: 404, dtype: object san-pedro ci
    Processing Record 404of Set 1 | san-pedro
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=san-pedro&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=san-pedro&units=imperial
    405 City Name           tymovskoye
    Country Code                ru
    Latitude                 50.85
    Longitude               142.66
    Temperature (F)          11.51
    Humidity (%)                61
    Cloudiness (%)              68
    Wind Speed (mph)         11.48
    Name: 405, dtype: object tymovskoye ru
    Processing Record 405of Set 1 | tymovskoye
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tymovskoye&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tymovskoye&units=imperial
    406 City Name           nantucket
    Country Code               us
    Latitude                41.28
    Longitude               -70.1
    Temperature (F)          30.2
    Humidity (%)               30
    Cloudiness (%)              1
    Wind Speed (mph)        14.99
    Name: 406, dtype: object nantucket us
    Processing Record 406of Set 1 | nantucket
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nantucket&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nantucket&units=imperial
    407 City Name           bosaso
    Country Code            so
    Latitude             11.28
    Longitude            49.18
    Temperature (F)      76.31
    Humidity (%)           100
    Cloudiness (%)           0
    Wind Speed (mph)      3.09
    Name: 407, dtype: object bosaso so
    Processing Record 407of Set 1 | bosaso
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bosaso&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bosaso&units=imperial
    408 City Name           ust-kuyga
    Country Code               ru
    Latitude                   70
    Longitude              135.55
    Temperature (F)        -29.89
    Humidity (%)               49
    Cloudiness (%)             12
    Wind Speed (mph)         2.75
    Name: 408, dtype: object ust-kuyga ru
    Processing Record 408of Set 1 | ust-kuyga
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ust-kuyga&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ust-kuyga&units=imperial
    409 City Name           charters towers
    Country Code                     au
    Latitude                     -20.07
    Longitude                    146.27
    Temperature (F)               77.21
    Humidity (%)                     94
    Cloudiness (%)                   24
    Wind Speed (mph)               7.45
    Name: 409, dtype: object charters towers au
    Processing Record 409of Set 1 | charters towers
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=charters towers&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=charters%20towers&units=imperial
    410 City Name           srivardhan
    Country Code                in
    Latitude                 18.03
    Longitude                73.02
    Temperature (F)          72.89
    Humidity (%)                89
    Cloudiness (%)              20
    Wind Speed (mph)          2.98
    Name: 410, dtype: object srivardhan in
    Processing Record 410of Set 1 | srivardhan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=srivardhan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=srivardhan&units=imperial
    411 City Name           la tuque
    Country Code              ca
    Latitude               47.44
    Longitude             -72.79
    Temperature (F)        11.24
    Humidity (%)              56
    Cloudiness (%)            44
    Wind Speed (mph)        8.23
    Name: 411, dtype: object la tuque ca
    Processing Record 411of Set 1 | la tuque
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=la tuque&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=la%20tuque&units=imperial
    412 City Name           talnakh
    Country Code             ru
    Latitude              69.49
    Longitude             88.39
    Temperature (F)       -4.42
    Humidity (%)             78
    Cloudiness (%)           36
    Wind Speed (mph)      14.72
    Name: 412, dtype: object talnakh ru
    Processing Record 412of Set 1 | talnakh
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=talnakh&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=talnakh&units=imperial
    413 City Name           sabha
    Country Code           ly
    Latitude            27.03
    Longitude           14.43
    Temperature (F)      51.2
    Humidity (%)           22
    Cloudiness (%)          0
    Wind Speed (mph)     2.75
    Name: 413, dtype: object sabha ly
    Processing Record 413of Set 1 | sabha
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sabha&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sabha&units=imperial
    414 City Name           sakhnovshchyna
    Country Code                    ua
    Latitude                     49.15
    Longitude                    35.87
    Temperature (F)              15.47
    Humidity (%)                    90
    Cloudiness (%)                  92
    Wind Speed (mph)              20.2
    Name: 414, dtype: object sakhnovshchyna ua
    Processing Record 414of Set 1 | sakhnovshchyna
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sakhnovshchyna&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sakhnovshchyna&units=imperial
    415 City Name            aksha
    Country Code            ru
    Latitude             50.28
    Longitude           113.29
    Temperature (F)      19.16
    Humidity (%)            67
    Cloudiness (%)           0
    Wind Speed (mph)      2.42
    Name: 415, dtype: object aksha ru
    Processing Record 415of Set 1 | aksha
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=aksha&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=aksha&units=imperial
    416 City Name           umm lajj
    Country Code              sa
    Latitude               25.02
    Longitude              37.27
    Temperature (F)        63.17
    Humidity (%)              99
    Cloudiness (%)             0
    Wind Speed (mph)        1.74
    Name: 416, dtype: object umm lajj sa
    Processing Record 416of Set 1 | umm lajj
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=umm lajj&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=umm%20lajj&units=imperial
    418 City Name           broken hill
    Country Code                 au
    Latitude                 -31.97
    Longitude                141.45
    Temperature (F)           69.56
    Humidity (%)                 38
    Cloudiness (%)                0
    Wind Speed (mph)          10.47
    Name: 418, dtype: object broken hill au
    Processing Record 418of Set 1 | broken hill
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=broken hill&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=broken%20hill&units=imperial
    419 City Name           codrington
    Country Code                ag
    Latitude                -28.95
    Longitude               153.24
    Temperature (F)          79.19
    Humidity (%)                87
    Cloudiness (%)               0
    Wind Speed (mph)          3.53
    Name: 419, dtype: object codrington ag
    Processing Record 419of Set 1 | codrington
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=codrington&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=codrington&units=imperial
    420 City Name           forbes
    Country Code            au
    Latitude            -33.38
    Longitude           148.01
    Temperature (F)      72.53
    Humidity (%)            31
    Cloudiness (%)           0
    Wind Speed (mph)      5.44
    Name: 420, dtype: object forbes au
    Processing Record 420of Set 1 | forbes
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=forbes&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=forbes&units=imperial
    421 City Name           pontianak
    Country Code               id
    Latitude                -0.02
    Longitude              109.34
    Temperature (F)         78.11
    Humidity (%)              100
    Cloudiness (%)             68
    Wind Speed (mph)         3.53
    Name: 421, dtype: object pontianak id
    Processing Record 421of Set 1 | pontianak
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pontianak&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pontianak&units=imperial
    422 City Name           xining
    Country Code            cn
    Latitude             36.62
    Longitude           101.77
    Temperature (F)      22.67
    Humidity (%)            75
    Cloudiness (%)          44
    Wind Speed (mph)      0.74
    Name: 422, dtype: object xining cn
    Processing Record 422of Set 1 | xining
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=xining&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=xining&units=imperial
    423 City Name           saint-georges
    Country Code                   gf
    Latitude                    46.12
    Longitude                  -70.67
    Temperature (F)              14.3
    Humidity (%)                   54
    Cloudiness (%)                 68
    Wind Speed (mph)             8.68
    Name: 423, dtype: object saint-georges gf
    Processing Record 423of Set 1 | saint-georges
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saint-georges&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saint-georges&units=imperial
    424 City Name           kathu
    Country Code           th
    Latitude            -27.7
    Longitude           23.05
    Temperature (F)      60.2
    Humidity (%)           94
    Cloudiness (%)         24
    Wind Speed (mph)     3.87
    Name: 424, dtype: object kathu th
    Processing Record 424of Set 1 | kathu
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kathu&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kathu&units=imperial
    425 City Name           eufaula
    Country Code             us
    Latitude              31.89
    Longitude            -85.15
    Temperature (F)       66.58
    Humidity (%)             83
    Cloudiness (%)           90
    Wind Speed (mph)       5.82
    Name: 425, dtype: object eufaula us
    Processing Record 425of Set 1 | eufaula
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=eufaula&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=eufaula&units=imperial
    426 City Name             hit
    Country Code           iq
    Latitude            33.64
    Longitude           42.83
    Temperature (F)     49.22
    Humidity (%)           38
    Cloudiness (%)         12
    Wind Speed (mph)     2.75
    Name: 426, dtype: object hit iq
    Processing Record 426of Set 1 | hit
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hit&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hit&units=imperial
    427 City Name           sokoto
    Country Code            ng
    Latitude             13.06
    Longitude             5.24
    Temperature (F)      68.21
    Humidity (%)            57
    Cloudiness (%)           0
    Wind Speed (mph)      2.75
    Name: 427, dtype: object sokoto ng
    Processing Record 427of Set 1 | sokoto
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sokoto&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sokoto&units=imperial
    428 City Name           ulaangom
    Country Code              mn
    Latitude               49.98
    Longitude              92.07
    Temperature (F)         0.62
    Humidity (%)              76
    Cloudiness (%)             0
    Wind Speed (mph)        1.97
    Name: 428, dtype: object ulaangom mn
    Processing Record 428of Set 1 | ulaangom
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ulaangom&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ulaangom&units=imperial
    429 City Name           chicama
    Country Code             pe
    Latitude              -7.84
    Longitude            -79.15
    Temperature (F)        69.8
    Humidity (%)             94
    Cloudiness (%)           75
    Wind Speed (mph)       8.05
    Name: 429, dtype: object chicama pe
    Processing Record 429of Set 1 | chicama
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=chicama&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=chicama&units=imperial
    430 City Name           upata
    Country Code           ve
    Latitude             8.02
    Longitude          -62.41
    Temperature (F)     81.71
    Humidity (%)           41
    Cloudiness (%)          0
    Wind Speed (mph)     9.91
    Name: 430, dtype: object upata ve
    Processing Record 430of Set 1 | upata
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=upata&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=upata&units=imperial
    431 City Name            sale
    Country Code           au
    Latitude            53.42
    Longitude           -2.32
    Temperature (F)     31.95
    Humidity (%)           78
    Cloudiness (%)         90
    Wind Speed (mph)     9.17
    Name: 431, dtype: object sale au
    Processing Record 431of Set 1 | sale
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sale&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sale&units=imperial
    432 City Name           langarud
    Country Code              ir
    Latitude                37.2
    Longitude              50.15
    Temperature (F)        43.72
    Humidity (%)              93
    Cloudiness (%)             0
    Wind Speed (mph)        4.99
    Name: 432, dtype: object langarud ir
    Processing Record 432of Set 1 | langarud
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=langarud&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=langarud&units=imperial
    433 City Name           abadan
    Country Code            ir
    Latitude             30.36
    Longitude            48.26
    Temperature (F)      60.53
    Humidity (%)            55
    Cloudiness (%)           0
    Wind Speed (mph)      9.17
    Name: 433, dtype: object abadan ir
    Processing Record 433of Set 1 | abadan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=abadan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=abadan&units=imperial
    434 City Name           dobryanka
    Country Code               ua
    Latitude                58.45
    Longitude               56.42
    Temperature (F)         -2.21
    Humidity (%)               70
    Cloudiness (%)              0
    Wind Speed (mph)        11.18
    Name: 434, dtype: object dobryanka ua
    Processing Record 434of Set 1 | dobryanka
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dobryanka&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dobryanka&units=imperial
    435 City Name            lasa
    Country Code           cn
    Latitude            34.92
    Longitude           32.53
    Temperature (F)     57.67
    Humidity (%)           62
    Cloudiness (%)          0
    Wind Speed (mph)     5.82
    Name: 435, dtype: object lasa cn
    Processing Record 435of Set 1 | lasa
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lasa&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lasa&units=imperial
    436 City Name            tabuk
    Country Code            sa
    Latitude             17.41
    Longitude           121.44
    Temperature (F)      67.31
    Humidity (%)            93
    Cloudiness (%)          32
    Wind Speed (mph)      2.42
    Name: 436, dtype: object tabuk sa
    Processing Record 436of Set 1 | tabuk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tabuk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tabuk&units=imperial
    438 City Name           batticaloa
    Country Code                lk
    Latitude                  7.71
    Longitude                81.69
    Temperature (F)          80.54
    Humidity (%)               100
    Cloudiness (%)              88
    Wind Speed (mph)          6.78
    Name: 438, dtype: object batticaloa lk
    Processing Record 438of Set 1 | batticaloa
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=batticaloa&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=batticaloa&units=imperial
    439 City Name           at-bashi
    Country Code              kg
    Latitude               41.17
    Longitude              75.81
    Temperature (F)         5.84
    Humidity (%)              74
    Cloudiness (%)            12
    Wind Speed (mph)         2.3
    Name: 439, dtype: object at-bashi kg
    Processing Record 439of Set 1 | at-bashi
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=at-bashi&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=at-bashi&units=imperial
    440 City Name           gornopravdinsk
    Country Code                    ru
    Latitude                     60.06
    Longitude                    69.92
    Temperature (F)              32.21
    Humidity (%)                    82
    Cloudiness (%)                  88
    Wind Speed (mph)             12.71
    Name: 440, dtype: object gornopravdinsk ru
    Processing Record 440of Set 1 | gornopravdinsk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gornopravdinsk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gornopravdinsk&units=imperial
    441 City Name           lucapa
    Country Code            ao
    Latitude             -8.42
    Longitude            20.74
    Temperature (F)      65.33
    Humidity (%)            99
    Cloudiness (%)          92
    Wind Speed (mph)      4.21
    Name: 441, dtype: object lucapa ao
    Processing Record 441of Set 1 | lucapa
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lucapa&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lucapa&units=imperial
    442 City Name           shieli
    Country Code            kz
    Latitude             44.18
    Longitude            66.74
    Temperature (F)      32.66
    Humidity (%)            90
    Cloudiness (%)          36
    Wind Speed (mph)      6.33
    Name: 442, dtype: object shieli kz
    Processing Record 442of Set 1 | shieli
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=shieli&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=shieli&units=imperial
    443 City Name           molchanovo
    Country Code                ru
    Latitude                 57.58
    Longitude                83.76
    Temperature (F)          17.36
    Humidity (%)                84
    Cloudiness (%)               0
    Wind Speed (mph)         11.59
    Name: 443, dtype: object molchanovo ru
    Processing Record 443of Set 1 | molchanovo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=molchanovo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=molchanovo&units=imperial
    444 City Name           mombetsu
    Country Code              jp
    Latitude               44.35
    Longitude             143.35
    Temperature (F)         37.4
    Humidity (%)              74
    Cloudiness (%)            75
    Wind Speed (mph)       14.99
    Name: 444, dtype: object mombetsu jp
    Processing Record 444of Set 1 | mombetsu
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mombetsu&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mombetsu&units=imperial
    445 City Name           bartlesville
    Country Code                  us
    Latitude                   36.75
    Longitude                 -95.98
    Temperature (F)            55.44
    Humidity (%)                  87
    Cloudiness (%)                90
    Wind Speed (mph)            3.36
    Name: 445, dtype: object bartlesville us
    Processing Record 445of Set 1 | bartlesville
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bartlesville&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bartlesville&units=imperial
    446 City Name           fort saint james
    Country Code                      ca
    Latitude                       54.43
    Longitude                    -124.25
    Temperature (F)                42.02
    Humidity (%)                      61
    Cloudiness (%)                    20
    Wind Speed (mph)                 3.2
    Name: 446, dtype: object fort saint james ca
    Processing Record 446of Set 1 | fort saint james
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fort saint james&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fort%20saint%20james&units=imperial
    447 City Name           novyy urengoy
    Country Code                   ru
    Latitude                    66.08
    Longitude                   76.63
    Temperature (F)             -5.05
    Humidity (%)                   68
    Cloudiness (%)                 68
    Wind Speed (mph)             3.42
    Name: 447, dtype: object novyy urengoy ru
    Processing Record 447of Set 1 | novyy urengoy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=novyy urengoy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=novyy%20urengoy&units=imperial
    448 City Name           qasigiannguit
    Country Code                   gl
    Latitude                    68.82
    Longitude                  -51.19
    Temperature (F)              26.6
    Humidity (%)                   68
    Cloudiness (%)                  0
    Wind Speed (mph)             5.82
    Name: 448, dtype: object qasigiannguit gl
    Processing Record 448of Set 1 | qasigiannguit
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=qasigiannguit&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=qasigiannguit&units=imperial
    449 City Name           mandan
    Country Code            us
    Latitude             46.83
    Longitude          -100.89
    Temperature (F)      32.99
    Humidity (%)            99
    Cloudiness (%)          90
    Wind Speed (mph)      5.82
    Name: 449, dtype: object mandan us
    Processing Record 449of Set 1 | mandan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mandan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mandan&units=imperial
    450 City Name           sorland
    Country Code             no
    Latitude              67.67
    Longitude             12.69
    Temperature (F)       37.43
    Humidity (%)            100
    Cloudiness (%)           88
    Wind Speed (mph)      37.31
    Name: 450, dtype: object sorland no
    Processing Record 450of Set 1 | sorland
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sorland&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sorland&units=imperial
    452 City Name           yeppoon
    Country Code             au
    Latitude             -23.13
    Longitude            150.74
    Temperature (F)          86
    Humidity (%)             58
    Cloudiness (%)           75
    Wind Speed (mph)      12.75
    Name: 452, dtype: object yeppoon au
    Processing Record 452of Set 1 | yeppoon
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yeppoon&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yeppoon&units=imperial
    453 City Name           shenjiamen
    Country Code                cn
    Latitude                 29.96
    Longitude                122.3
    Temperature (F)          53.45
    Humidity (%)                99
    Cloudiness (%)              92
    Wind Speed (mph)         17.74
    Name: 453, dtype: object shenjiamen cn
    Processing Record 453of Set 1 | shenjiamen
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=shenjiamen&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=shenjiamen&units=imperial
    454 City Name           bucksport
    Country Code               us
    Latitude                33.66
    Longitude               -79.1
    Temperature (F)         54.21
    Humidity (%)               87
    Cloudiness (%)              1
    Wind Speed (mph)         3.36
    Name: 454, dtype: object bucksport us
    Processing Record 454of Set 1 | bucksport
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bucksport&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bucksport&units=imperial
    455 City Name           grande-synthe
    Country Code                   fr
    Latitude                    51.01
    Longitude                     2.3
    Temperature (F)             29.23
    Humidity (%)                   63
    Cloudiness (%)                 80
    Wind Speed (mph)            12.75
    Name: 455, dtype: object grande-synthe fr
    Processing Record 455of Set 1 | grande-synthe
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=grande-synthe&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=grande-synthe&units=imperial
    456 City Name            nuuk
    Country Code           gl
    Latitude            64.17
    Longitude          -51.74
    Temperature (F)      44.6
    Humidity (%)           45
    Cloudiness (%)          8
    Wind Speed (mph)    19.46
    Name: 456, dtype: object nuuk gl
    Processing Record 456of Set 1 | nuuk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nuuk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nuuk&units=imperial
    457 City Name           mount isa
    Country Code               au
    Latitude               -20.73
    Longitude              139.49
    Temperature (F)          91.4
    Humidity (%)               29
    Cloudiness (%)              0
    Wind Speed (mph)         9.17
    Name: 457, dtype: object mount isa au
    Processing Record 457of Set 1 | mount isa
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mount isa&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mount%20isa&units=imperial
    458 City Name           westerland
    Country Code                de
    Latitude                 52.89
    Longitude                 4.93
    Temperature (F)          29.82
    Humidity (%)                54
    Cloudiness (%)               0
    Wind Speed (mph)         23.04
    Name: 458, dtype: object westerland de
    Processing Record 458of Set 1 | westerland
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=westerland&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=westerland&units=imperial
    459 City Name           massena
    Country Code             us
    Latitude              44.93
    Longitude            -74.89
    Temperature (F)       18.23
    Humidity (%)             47
    Cloudiness (%)           75
    Wind Speed (mph)       3.87
    Name: 459, dtype: object massena us
    Processing Record 459of Set 1 | massena
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=massena&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=massena&units=imperial
    460 City Name           touros
    Country Code            br
    Latitude              -5.2
    Longitude           -35.46
    Temperature (F)      81.26
    Humidity (%)            83
    Cloudiness (%)          88
    Wind Speed (mph)      9.57
    Name: 460, dtype: object touros br
    Processing Record 460of Set 1 | touros
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=touros&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=touros&units=imperial
    461 City Name           klamath falls
    Country Code                   us
    Latitude                    42.22
    Longitude                 -121.78
    Temperature (F)              44.6
    Humidity (%)                   39
    Cloudiness (%)                 90
    Wind Speed (mph)             3.36
    Name: 461, dtype: object klamath falls us
    Processing Record 461of Set 1 | klamath falls
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=klamath falls&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=klamath%20falls&units=imperial
    463 City Name           cap malheureux
    Country Code                    mu
    Latitude                    -19.98
    Longitude                    57.61
    Temperature (F)               80.6
    Humidity (%)                    83
    Cloudiness (%)                  40
    Wind Speed (mph)              9.17
    Name: 463, dtype: object cap malheureux mu
    Processing Record 463of Set 1 | cap malheureux
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cap malheureux&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cap%20malheureux&units=imperial
    464 City Name           sao filipe
    Country Code                cv
    Latitude                  14.9
    Longitude                -24.5
    Temperature (F)          71.63
    Humidity (%)                91
    Cloudiness (%)               0
    Wind Speed (mph)         17.96
    Name: 464, dtype: object sao filipe cv
    Processing Record 464of Set 1 | sao filipe
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sao filipe&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sao%20filipe&units=imperial
    465 City Name           allapalli
    Country Code               in
    Latitude                19.43
    Longitude               80.06
    Temperature (F)         70.01
    Humidity (%)               83
    Cloudiness (%)             36
    Wind Speed (mph)         2.75
    Name: 465, dtype: object allapalli in
    Processing Record 465of Set 1 | allapalli
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=allapalli&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=allapalli&units=imperial
    466 City Name           broome
    Country Code            au
    Latitude             52.47
    Longitude             1.45
    Temperature (F)         32
    Humidity (%)            79
    Cloudiness (%)          76
    Wind Speed (mph)     13.87
    Name: 466, dtype: object broome au
    Processing Record 466of Set 1 | broome
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=broome&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=broome&units=imperial
    467 City Name           najran
    Country Code            sa
    Latitude             17.54
    Longitude            44.22
    Temperature (F)       66.2
    Humidity (%)            24
    Cloudiness (%)           0
    Wind Speed (mph)      2.86
    Name: 467, dtype: object najran sa
    Processing Record 467of Set 1 | najran
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=najran&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=najran&units=imperial
    468 City Name           monticello
    Country Code                us
    Latitude                 45.31
    Longitude               -93.79
    Temperature (F)          39.52
    Humidity (%)                64
    Cloudiness (%)              90
    Wind Speed (mph)          3.42
    Name: 468, dtype: object monticello us
    Processing Record 468of Set 1 | monticello
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=monticello&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=monticello&units=imperial
    469 City Name           fillmore
    Country Code              us
    Latitude                34.4
    Longitude            -118.91
    Temperature (F)           59
    Humidity (%)              44
    Cloudiness (%)             1
    Wind Speed (mph)        8.05
    Name: 469, dtype: object fillmore us
    Processing Record 469of Set 1 | fillmore
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fillmore&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fillmore&units=imperial
    470 City Name           nizhniy ingash
    Country Code                    ru
    Latitude                      56.2
    Longitude                    96.53
    Temperature (F)               9.62
    Humidity (%)                    68
    Cloudiness (%)                   0
    Wind Speed (mph)              2.64
    Name: 470, dtype: object nizhniy ingash ru
    Processing Record 470of Set 1 | nizhniy ingash
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nizhniy ingash&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nizhniy%20ingash&units=imperial
    471 City Name           plettenberg bay
    Country Code                     za
    Latitude                     -34.05
    Longitude                     23.37
    Temperature (F)               65.06
    Humidity (%)                    100
    Cloudiness (%)                   44
    Wind Speed (mph)              16.62
    Name: 471, dtype: object plettenberg bay za
    Processing Record 471of Set 1 | plettenberg bay
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=plettenberg bay&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=plettenberg%20bay&units=imperial
    472 City Name           tautira
    Country Code             pf
    Latitude             -17.73
    Longitude           -149.15
    Temperature (F)        82.4
    Humidity (%)             65
    Cloudiness (%)           75
    Wind Speed (mph)       5.82
    Name: 472, dtype: object tautira pf
    Processing Record 472of Set 1 | tautira
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tautira&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tautira&units=imperial
    473 City Name           westport
    Country Code              nz
    Latitude                53.8
    Longitude              -9.52
    Temperature (F)        33.92
    Humidity (%)              89
    Cloudiness (%)             8
    Wind Speed (mph)       13.71
    Name: 473, dtype: object westport nz
    Processing Record 473of Set 1 | westport
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=westport&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=westport&units=imperial
    474 City Name           moyale
    Country Code            et
    Latitude              3.52
    Longitude            39.05
    Temperature (F)      61.19
    Humidity (%)            88
    Cloudiness (%)          12
    Wind Speed (mph)      2.75
    Name: 474, dtype: object moyale et
    Processing Record 474of Set 1 | moyale
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=moyale&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=moyale&units=imperial
    475 City Name           sindor
    Country Code            ru
    Latitude             62.87
    Longitude             51.9
    Temperature (F)      -9.91
    Humidity (%)            68
    Cloudiness (%)          48
    Wind Speed (mph)      8.57
    Name: 475, dtype: object sindor ru
    Processing Record 475of Set 1 | sindor
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sindor&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sindor&units=imperial
    476 City Name           sitka
    Country Code           us
    Latitude            37.17
    Longitude          -99.65
    Temperature (F)     60.38
    Humidity (%)           63
    Cloudiness (%)         20
    Wind Speed (mph)     15.5
    Name: 476, dtype: object sitka us
    Processing Record 476of Set 1 | sitka
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sitka&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sitka&units=imperial
    477 City Name           koumac
    Country Code            nc
    Latitude            -20.56
    Longitude           164.28
    Temperature (F)      83.24
    Humidity (%)            78
    Cloudiness (%)          12
    Wind Speed (mph)      1.97
    Name: 477, dtype: object koumac nc
    Processing Record 477of Set 1 | koumac
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=koumac&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=koumac&units=imperial
    478 City Name            tezu
    Country Code           in
    Latitude            27.93
    Longitude           96.16
    Temperature (F)      46.7
    Humidity (%)           73
    Cloudiness (%)          0
    Wind Speed (mph)     1.97
    Name: 478, dtype: object tezu in
    Processing Record 478of Set 1 | tezu
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tezu&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tezu&units=imperial
    479 City Name           udachnyy
    Country Code              ru
    Latitude               66.42
    Longitude              112.4
    Temperature (F)        -1.99
    Humidity (%)              79
    Cloudiness (%)            56
    Wind Speed (mph)        6.11
    Name: 479, dtype: object udachnyy ru
    Processing Record 479of Set 1 | udachnyy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=udachnyy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=udachnyy&units=imperial
    480 City Name           tarnogskiy gorodok
    Country Code                        ru
    Latitude                          60.5
    Longitude                        43.58
    Temperature (F)                  13.67
    Humidity (%)                        85
    Cloudiness (%)                      76
    Wind Speed (mph)                 17.63
    Name: 480, dtype: object tarnogskiy gorodok ru
    Processing Record 480of Set 1 | tarnogskiy gorodok
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tarnogskiy gorodok&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tarnogskiy%20gorodok&units=imperial
    481 City Name           lakhimpur
    Country Code               in
    Latitude                27.24
    Longitude                94.1
    Temperature (F)         53.45
    Humidity (%)               93
    Cloudiness (%)              0
    Wind Speed (mph)         4.65
    Name: 481, dtype: object lakhimpur in
    Processing Record 481of Set 1 | lakhimpur
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lakhimpur&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lakhimpur&units=imperial
    482 City Name           goderich
    Country Code              sl
    Latitude               43.74
    Longitude             -81.71
    Temperature (F)        29.87
    Humidity (%)              66
    Cloudiness (%)             0
    Wind Speed (mph)       10.13
    Name: 482, dtype: object goderich sl
    Processing Record 482of Set 1 | goderich
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=goderich&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=goderich&units=imperial
    485 City Name            fasa
    Country Code           ir
    Latitude            28.94
    Longitude           53.65
    Temperature (F)      48.2
    Humidity (%)           42
    Cloudiness (%)          0
    Wind Speed (mph)     6.93
    Name: 485, dtype: object fasa ir
    Processing Record 485of Set 1 | fasa
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fasa&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fasa&units=imperial
    486 City Name           sarankhola
    Country Code                bd
    Latitude                 22.31
    Longitude                89.79
    Temperature (F)          67.13
    Humidity (%)                93
    Cloudiness (%)               0
    Wind Speed (mph)          2.53
    Name: 486, dtype: object sarankhola bd
    Processing Record 486of Set 1 | sarankhola
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sarankhola&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sarankhola&units=imperial
    487 City Name           berdigestyakh
    Country Code                   ru
    Latitude                     62.1
    Longitude                   126.7
    Temperature (F)               -10
    Humidity (%)                   52
    Cloudiness (%)                  0
    Wind Speed (mph)             5.66
    Name: 487, dtype: object berdigestyakh ru
    Processing Record 487of Set 1 | berdigestyakh
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=berdigestyakh&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=berdigestyakh&units=imperial
    488 City Name           valle hermoso
    Country Code                   mx
    Latitude                    25.67
    Longitude                  -97.82
    Temperature (F)             84.22
    Humidity (%)                   58
    Cloudiness (%)                  1
    Wind Speed (mph)            10.29
    Name: 488, dtype: object valle hermoso mx
    Processing Record 488of Set 1 | valle hermoso
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=valle hermoso&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=valle%20hermoso&units=imperial
    489 City Name           rapid valley
    Country Code                  us
    Latitude                   44.06
    Longitude                -103.15
    Temperature (F)            28.87
    Humidity (%)                 100
    Cloudiness (%)                90
    Wind Speed (mph)           14.99
    Name: 489, dtype: object rapid valley us
    Processing Record 489of Set 1 | rapid valley
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rapid valley&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rapid%20valley&units=imperial
    490 City Name           mvuma
    Country Code           zw
    Latitude           -19.28
    Longitude           30.53
    Temperature (F)     60.83
    Humidity (%)           92
    Cloudiness (%)         44
    Wind Speed (mph)     7.11
    Name: 490, dtype: object mvuma zw
    Processing Record 490of Set 1 | mvuma
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mvuma&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mvuma&units=imperial
    491 City Name           camocim
    Country Code             br
    Latitude               -2.9
    Longitude            -40.84
    Temperature (F)       77.12
    Humidity (%)             87
    Cloudiness (%)           20
    Wind Speed (mph)       7.23
    Name: 491, dtype: object camocim br
    Processing Record 491of Set 1 | camocim
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=camocim&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=camocim&units=imperial
    492 City Name           rabo de peixe
    Country Code                   pt
    Latitude                     37.8
    Longitude                  -25.58
    Temperature (F)              60.8
    Humidity (%)                  100
    Cloudiness (%)                 75
    Wind Speed (mph)            10.29
    Name: 492, dtype: object rabo de peixe pt
    Processing Record 492of Set 1 | rabo de peixe
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rabo de peixe&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rabo%20de%20peixe&units=imperial
    493 City Name           port macquarie
    Country Code                    au
    Latitude                    -31.43
    Longitude                   152.91
    Temperature (F)               82.4
    Humidity (%)                    54
    Cloudiness (%)                   0
    Wind Speed (mph)              5.82
    Name: 493, dtype: object port macquarie au
    Processing Record 493of Set 1 | port macquarie
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port macquarie&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port%20macquarie&units=imperial
    494 City Name           shimoda
    Country Code             jp
    Latitude               34.7
    Longitude            138.93
    Temperature (F)       59.52
    Humidity (%)             67
    Cloudiness (%)           75
    Wind Speed (mph)      18.34
    Name: 494, dtype: object shimoda jp
    Processing Record 494of Set 1 | shimoda
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=shimoda&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=shimoda&units=imperial
    495 City Name           winnemucca
    Country Code                us
    Latitude                 40.97
    Longitude              -117.74
    Temperature (F)           37.4
    Humidity (%)                59
    Cloudiness (%)               1
    Wind Speed (mph)          9.17
    Name: 495, dtype: object winnemucca us
    Processing Record 495of Set 1 | winnemucca
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=winnemucca&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=winnemucca&units=imperial
    497 City Name           brokopondo
    Country Code                sr
    Latitude                  5.06
    Longitude               -54.98
    Temperature (F)           78.8
    Humidity (%)                74
    Cloudiness (%)              40
    Wind Speed (mph)          5.82
    Name: 497, dtype: object brokopondo sr
    Processing Record 497of Set 1 | brokopondo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=brokopondo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=brokopondo&units=imperial
    500 City Name             kaeo
    Country Code            nz
    Latitude             -35.1
    Longitude           173.78
    Temperature (F)      70.91
    Humidity (%)            78
    Cloudiness (%)          92
    Wind Speed (mph)     15.73
    Name: 500, dtype: object kaeo nz
    Processing Record 500of Set 1 | kaeo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kaeo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kaeo&units=imperial
    501 City Name           tabas
    Country Code           ir
    Latitude             33.6
    Longitude           56.92
    Temperature (F)     44.45
    Humidity (%)          100
    Cloudiness (%)        100
    Wind Speed (mph)     4.43
    Name: 501, dtype: object tabas ir
    Processing Record 501of Set 1 | tabas
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tabas&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tabas&units=imperial
    502 City Name            alofi
    Country Code            nu
    Latitude            -19.06
    Longitude          -169.92
    Temperature (F)         86
    Humidity (%)            70
    Cloudiness (%)          88
    Wind Speed (mph)      5.82
    Name: 502, dtype: object alofi nu
    Processing Record 502of Set 1 | alofi
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=alofi&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=alofi&units=imperial
    504 City Name           causapscal
    Country Code                ca
    Latitude                 48.35
    Longitude               -67.23
    Temperature (F)          13.04
    Humidity (%)                69
    Cloudiness (%)              64
    Wind Speed (mph)         10.47
    Name: 504, dtype: object causapscal ca
    Processing Record 504of Set 1 | causapscal
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=causapscal&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=causapscal&units=imperial
    505 City Name           kristianstad
    Country Code                  se
    Latitude                   56.03
    Longitude                  14.16
    Temperature (F)             17.6
    Humidity (%)                  92
    Cloudiness (%)                 0
    Wind Speed (mph)            3.87
    Name: 505, dtype: object kristianstad se
    Processing Record 505of Set 1 | kristianstad
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kristianstad&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kristianstad&units=imperial
    506 City Name           naryan-mar
    Country Code                ru
    Latitude                 67.67
    Longitude                53.09
    Temperature (F)         -16.12
    Humidity (%)                47
    Cloudiness (%)              24
    Wind Speed (mph)          4.21
    Name: 506, dtype: object naryan-mar ru
    Processing Record 506of Set 1 | naryan-mar
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=naryan-mar&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=naryan-mar&units=imperial
    507 City Name           agadez
    Country Code            ne
    Latitude             16.97
    Longitude             7.99
    Temperature (F)      70.28
    Humidity (%)            20
    Cloudiness (%)           8
    Wind Speed (mph)     10.92
    Name: 507, dtype: object agadez ne
    Processing Record 507of Set 1 | agadez
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=agadez&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=agadez&units=imperial
    508 City Name           warangal
    Country Code              in
    Latitude               17.99
    Longitude               79.6
    Temperature (F)        67.85
    Humidity (%)              72
    Cloudiness (%)            32
    Wind Speed (mph)        4.21
    Name: 508, dtype: object warangal in
    Processing Record 508of Set 1 | warangal
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=warangal&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=warangal&units=imperial
    510 City Name           fairbanks
    Country Code               us
    Latitude                64.84
    Longitude             -147.72
    Temperature (F)         30.27
    Humidity (%)               79
    Cloudiness (%)             90
    Wind Speed (mph)         4.88
    Name: 510, dtype: object fairbanks us
    Processing Record 510of Set 1 | fairbanks
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fairbanks&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fairbanks&units=imperial
    511 City Name           rawson
    Country Code            ar
    Latitude             -43.3
    Longitude           -65.11
    Temperature (F)      68.21
    Humidity (%)            26
    Cloudiness (%)           0
    Wind Speed (mph)     19.75
    Name: 511, dtype: object rawson ar
    Processing Record 511of Set 1 | rawson
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rawson&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rawson&units=imperial
    512 City Name           namatanai
    Country Code               pg
    Latitude                -3.66
    Longitude              152.44
    Temperature (F)         82.25
    Humidity (%)              100
    Cloudiness (%)             56
    Wind Speed (mph)        14.94
    Name: 512, dtype: object namatanai pg
    Processing Record 512of Set 1 | namatanai
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=namatanai&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=namatanai&units=imperial
    513 City Name           deputatskiy
    Country Code                 ru
    Latitude                   69.3
    Longitude                 139.9
    Temperature (F)          -26.74
    Humidity (%)                 39
    Cloudiness (%)                8
    Wind Speed (mph)           2.86
    Name: 513, dtype: object deputatskiy ru
    Processing Record 513of Set 1 | deputatskiy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=deputatskiy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=deputatskiy&units=imperial
    514 City Name           nivala
    Country Code            fi
    Latitude             63.93
    Longitude            24.96
    Temperature (F)      24.38
    Humidity (%)            79
    Cloudiness (%)          36
    Wind Speed (mph)      8.01
    Name: 514, dtype: object nivala fi
    Processing Record 514of Set 1 | nivala
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nivala&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nivala&units=imperial
    515 City Name           ovalle
    Country Code            cl
    Latitude             -30.6
    Longitude            -71.2
    Temperature (F)      59.75
    Humidity (%)            86
    Cloudiness (%)           0
    Wind Speed (mph)       3.2
    Name: 515, dtype: object ovalle cl
    Processing Record 515of Set 1 | ovalle
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ovalle&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ovalle&units=imperial
    516 City Name           ekhabi
    Country Code            ru
    Latitude             53.51
    Longitude           142.97
    Temperature (F)       4.13
    Humidity (%)            89
    Cloudiness (%)           0
    Wind Speed (mph)      15.5
    Name: 516, dtype: object ekhabi ru
    Processing Record 516of Set 1 | ekhabi
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ekhabi&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ekhabi&units=imperial
    517 City Name           mporokoso
    Country Code               zm
    Latitude                -9.37
    Longitude               30.12
    Temperature (F)         61.55
    Humidity (%)               97
    Cloudiness (%)             80
    Wind Speed (mph)         1.41
    Name: 517, dtype: object mporokoso zm
    Processing Record 517of Set 1 | mporokoso
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mporokoso&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mporokoso&units=imperial
    519 City Name           biltine
    Country Code             td
    Latitude              14.53
    Longitude             20.93
    Temperature (F)       66.14
    Humidity (%)             20
    Cloudiness (%)            0
    Wind Speed (mph)       8.23
    Name: 519, dtype: object biltine td
    Processing Record 519of Set 1 | biltine
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=biltine&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=biltine&units=imperial
    520 City Name           guaruja
    Country Code             br
    Latitude             -23.99
    Longitude            -46.26
    Temperature (F)       74.84
    Humidity (%)             83
    Cloudiness (%)           20
    Wind Speed (mph)        4.7
    Name: 520, dtype: object guaruja br
    Processing Record 520of Set 1 | guaruja
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=guaruja&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=guaruja&units=imperial
    521 City Name           binzhou
    Country Code             cn
    Latitude              37.38
    Longitude            117.96
    Temperature (F)       35.63
    Humidity (%)             89
    Cloudiness (%)           32
    Wind Speed (mph)       4.32
    Name: 521, dtype: object binzhou cn
    Processing Record 521of Set 1 | binzhou
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=binzhou&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=binzhou&units=imperial
    523 City Name           northam
    Country Code             au
    Latitude              51.04
    Longitude             -4.21
    Temperature (F)        30.2
    Humidity (%)             80
    Cloudiness (%)           80
    Wind Speed (mph)       20.8
    Name: 523, dtype: object northam au
    Processing Record 523of Set 1 | northam
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=northam&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=northam&units=imperial
    524 City Name           isiro
    Country Code           cd
    Latitude             2.77
    Longitude           27.62
    Temperature (F)     71.27
    Humidity (%)           90
    Cloudiness (%)         80
    Wind Speed (mph)     2.75
    Name: 524, dtype: object isiro cd
    Processing Record 524of Set 1 | isiro
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=isiro&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=isiro&units=imperial
    525 City Name           lufilufi
    Country Code              ws
    Latitude              -13.87
    Longitude             -171.6
    Temperature (F)         91.4
    Humidity (%)              66
    Cloudiness (%)            75
    Wind Speed (mph)       14.99
    Name: 525, dtype: object lufilufi ws
    Processing Record 525of Set 1 | lufilufi
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lufilufi&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lufilufi&units=imperial
    526 City Name           walvis bay
    Country Code                na
    Latitude                -22.95
    Longitude                14.51
    Temperature (F)             59
    Humidity (%)                87
    Cloudiness (%)              64
    Wind Speed (mph)          1.12
    Name: 526, dtype: object walvis bay na
    Processing Record 526of Set 1 | walvis bay
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=walvis bay&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=walvis%20bay&units=imperial
    527 City Name           cockburn town
    Country Code                   bs
    Latitude                    21.46
    Longitude                  -71.14
    Temperature (F)             74.78
    Humidity (%)                  100
    Cloudiness (%)                 32
    Wind Speed (mph)             6.89
    Name: 527, dtype: object cockburn town bs
    Processing Record 527of Set 1 | cockburn town
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cockburn town&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cockburn%20town&units=imperial
    528 City Name           pisco
    Country Code           pe
    Latitude           -13.71
    Longitude           -76.2
    Temperature (F)      69.8
    Humidity (%)           83
    Cloudiness (%)          0
    Wind Speed (mph)    19.46
    Name: 528, dtype: object pisco pe
    Processing Record 528of Set 1 | pisco
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pisco&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pisco&units=imperial
    529 City Name           polunochnoye
    Country Code                  ru
    Latitude                   60.87
    Longitude                  60.43
    Temperature (F)             3.05
    Humidity (%)                  64
    Cloudiness (%)                56
    Wind Speed (mph)           11.81
    Name: 529, dtype: object polunochnoye ru
    Processing Record 529of Set 1 | polunochnoye
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=polunochnoye&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=polunochnoye&units=imperial
    530 City Name           ambanja
    Country Code             mg
    Latitude             -13.68
    Longitude             48.45
    Temperature (F)       69.65
    Humidity (%)            100
    Cloudiness (%)           88
    Wind Speed (mph)        2.3
    Name: 530, dtype: object ambanja mg
    Processing Record 530of Set 1 | ambanja
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ambanja&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ambanja&units=imperial
    531 City Name           gazojak
    Country Code             tm
    Latitude              41.19
    Longitude              61.4
    Temperature (F)       39.23
    Humidity (%)             96
    Cloudiness (%)            0
    Wind Speed (mph)       4.76
    Name: 531, dtype: object gazojak tm
    Processing Record 531of Set 1 | gazojak
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gazojak&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gazojak&units=imperial
    532 City Name           pacific grove
    Country Code                   us
    Latitude                    36.62
    Longitude                 -121.92
    Temperature (F)             54.99
    Humidity (%)                   66
    Cloudiness (%)                  1
    Wind Speed (mph)              4.7
    Name: 532, dtype: object pacific grove us
    Processing Record 532of Set 1 | pacific grove
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pacific grove&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pacific%20grove&units=imperial
    533 City Name            teya
    Country Code           ru
    Latitude            21.05
    Longitude          -89.07
    Temperature (F)      96.8
    Humidity (%)           28
    Cloudiness (%)         75
    Wind Speed (mph)     9.17
    Name: 533, dtype: object teya ru
    Processing Record 533of Set 1 | teya
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=teya&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=teya&units=imperial
    534 City Name           rocky mount
    Country Code                 us
    Latitude                  35.94
    Longitude                 -77.8
    Temperature (F)           45.37
    Humidity (%)                 76
    Cloudiness (%)                1
    Wind Speed (mph)           2.75
    Name: 534, dtype: object rocky mount us
    Processing Record 534of Set 1 | rocky mount
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rocky mount&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rocky%20mount&units=imperial
    535 City Name           podor
    Country Code           sn
    Latitude            16.65
    Longitude          -14.96
    Temperature (F)      80.6
    Humidity (%)           19
    Cloudiness (%)          0
    Wind Speed (mph)     2.24
    Name: 535, dtype: object podor sn
    Processing Record 535of Set 1 | podor
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=podor&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=podor&units=imperial
    536 City Name           lagos
    Country Code           ng
    Latitude             6.46
    Longitude            3.39
    Temperature (F)      82.4
    Humidity (%)           88
    Cloudiness (%)         20
    Wind Speed (mph)     2.24
    Name: 536, dtype: object lagos ng
    Processing Record 536of Set 1 | lagos
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lagos&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lagos&units=imperial
    537 City Name           sorong
    Country Code            id
    Latitude             -0.86
    Longitude           131.25
    Temperature (F)      80.63
    Humidity (%)           100
    Cloudiness (%)          56
    Wind Speed (mph)      9.69
    Name: 537, dtype: object sorong id
    Processing Record 537of Set 1 | sorong
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sorong&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sorong&units=imperial
    538 City Name            tupik
    Country Code            ru
    Latitude             54.43
    Longitude           119.94
    Temperature (F)        4.4
    Humidity (%)            57
    Cloudiness (%)           0
    Wind Speed (mph)      2.53
    Name: 538, dtype: object tupik ru
    Processing Record 538of Set 1 | tupik
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tupik&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tupik&units=imperial
    540 City Name            izumo
    Country Code            jp
    Latitude             35.37
    Longitude           132.75
    Temperature (F)       53.6
    Humidity (%)            93
    Cloudiness (%)          75
    Wind Speed (mph)     13.87
    Name: 540, dtype: object izumo jp
    Processing Record 540of Set 1 | izumo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=izumo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=izumo&units=imperial
    541 City Name           azanka
    Country Code            ru
    Latitude             58.04
    Longitude            64.79
    Temperature (F)      32.93
    Humidity (%)            94
    Cloudiness (%)          88
    Wind Speed (mph)      5.66
    Name: 541, dtype: object azanka ru
    Processing Record 541of Set 1 | azanka
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=azanka&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=azanka&units=imperial
    542 City Name           letlhakane
    Country Code                bw
    Latitude                -21.42
    Longitude                25.59
    Temperature (F)          64.88
    Humidity (%)                75
    Cloudiness (%)               0
    Wind Speed (mph)         11.03
    Name: 542, dtype: object letlhakane bw
    Processing Record 542of Set 1 | letlhakane
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=letlhakane&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=letlhakane&units=imperial
    544 City Name           mont-dore
    Country Code               nc
    Latitude               -22.23
    Longitude              166.53
    Temperature (F)          84.2
    Humidity (%)               51
    Cloudiness (%)             20
    Wind Speed (mph)        11.41
    Name: 544, dtype: object mont-dore nc
    Processing Record 544of Set 1 | mont-dore
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mont-dore&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mont-dore&units=imperial
    545 City Name           cangzhou
    Country Code              cn
    Latitude                38.3
    Longitude             116.85
    Temperature (F)        38.51
    Humidity (%)              86
    Cloudiness (%)            32
    Wind Speed (mph)           7
    Name: 545, dtype: object cangzhou cn
    Processing Record 545of Set 1 | cangzhou
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cangzhou&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cangzhou&units=imperial
    546 City Name           dryden
    Country Code            ca
    Latitude             49.79
    Longitude           -92.84
    Temperature (F)       30.2
    Humidity (%)            54
    Cloudiness (%)          90
    Wind Speed (mph)      8.05
    Name: 546, dtype: object dryden ca
    Processing Record 546of Set 1 | dryden
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dryden&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dryden&units=imperial
    547 City Name           manta
    Country Code           ec
    Latitude             45.1
    Longitude            24.1
    Temperature (F)     35.54
    Humidity (%)           99
    Cloudiness (%)         92
    Wind Speed (mph)     4.43
    Name: 547, dtype: object manta ec
    Processing Record 547of Set 1 | manta
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=manta&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=manta&units=imperial
    548 City Name           atherton
    Country Code              au
    Latitude               53.52
    Longitude              -2.49
    Temperature (F)        31.95
    Humidity (%)              78
    Cloudiness (%)            90
    Wind Speed (mph)        9.17
    Name: 548, dtype: object atherton au
    Processing Record 548of Set 1 | atherton
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=atherton&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=atherton&units=imperial
    549 City Name           sumenep
    Country Code             id
    Latitude              -7.02
    Longitude            113.87
    Temperature (F)       82.25
    Humidity (%)            100
    Cloudiness (%)           44
    Wind Speed (mph)       4.43
    Name: 549, dtype: object sumenep id
    Processing Record 549of Set 1 | sumenep
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sumenep&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sumenep&units=imperial
    550 City Name           high level
    Country Code                ca
    Latitude                 58.52
    Longitude              -117.13
    Temperature (F)           33.8
    Humidity (%)                47
    Cloudiness (%)              40
    Wind Speed (mph)         10.29
    Name: 550, dtype: object high level ca
    Processing Record 550of Set 1 | high level
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=high level&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=high%20level&units=imperial
    551 City Name             ati
    Country Code           td
    Latitude            13.21
    Longitude           18.34
    Temperature (F)     66.68
    Humidity (%)           40
    Cloudiness (%)          0
    Wind Speed (mph)     2.86
    Name: 551, dtype: object ati td
    Processing Record 551of Set 1 | ati
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ati&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ati&units=imperial
    552 City Name           tilichiki
    Country Code               ru
    Latitude                60.47
    Longitude               166.1
    Temperature (F)          4.94
    Humidity (%)               84
    Cloudiness (%)             32
    Wind Speed (mph)         6.44
    Name: 552, dtype: object tilichiki ru
    Processing Record 552of Set 1 | tilichiki
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tilichiki&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tilichiki&units=imperial
    553 City Name           klaksvik
    Country Code              fo
    Latitude               62.23
    Longitude              -6.59
    Temperature (F)         39.2
    Humidity (%)              92
    Cloudiness (%)            80
    Wind Speed (mph)       11.41
    Name: 553, dtype: object klaksvik fo
    Processing Record 553of Set 1 | klaksvik
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=klaksvik&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=klaksvik&units=imperial
    554 City Name           freeport
    Country Code              us
    Latitude               26.54
    Longitude              -78.7
    Temperature (F)           77
    Humidity (%)              44
    Cloudiness (%)             0
    Wind Speed (mph)         4.7
    Name: 554, dtype: object freeport us
    Processing Record 554of Set 1 | freeport
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=freeport&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=freeport&units=imperial
    555 City Name           batemans bay
    Country Code                  au
    Latitude                  -35.71
    Longitude                 150.18
    Temperature (F)            71.81
    Humidity (%)                  66
    Cloudiness (%)                 0
    Wind Speed (mph)            2.19
    Name: 555, dtype: object batemans bay au
    Processing Record 555of Set 1 | batemans bay
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=batemans bay&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=batemans%20bay&units=imperial
    556 City Name           haverhill
    Country Code               us
    Latitude                52.08
    Longitude                0.44
    Temperature (F)         32.23
    Humidity (%)               74
    Cloudiness (%)             40
    Wind Speed (mph)         8.05
    Name: 556, dtype: object haverhill us
    Processing Record 556of Set 1 | haverhill
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=haverhill&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=haverhill&units=imperial
    557 City Name            esso
    Country Code           ru
    Latitude            55.93
    Longitude           158.7
    Temperature (F)       7.1
    Humidity (%)           81
    Cloudiness (%)         76
    Wind Speed (mph)     2.64
    Name: 557, dtype: object esso ru
    Processing Record 557of Set 1 | esso
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=esso&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=esso&units=imperial
    558 City Name           diamantino
    Country Code                br
    Latitude                 -14.4
    Longitude               -56.44
    Temperature (F)          80.27
    Humidity (%)                76
    Cloudiness (%)              36
    Wind Speed (mph)          3.65
    Name: 558, dtype: object diamantino br
    Processing Record 558of Set 1 | diamantino
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=diamantino&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=diamantino&units=imperial
    559 City Name           ambon
    Country Code           id
    Latitude            47.55
    Longitude           -2.56
    Temperature (F)        32
    Humidity (%)           74
    Cloudiness (%)          0
    Wind Speed (mph)     1.12
    Name: 559, dtype: object ambon id
    Processing Record 559of Set 1 | ambon
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ambon&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ambon&units=imperial
    560 City Name            namie
    Country Code            jp
    Latitude             37.52
    Longitude           140.86
    Temperature (F)      53.76
    Humidity (%)            57
    Cloudiness (%)          75
    Wind Speed (mph)     13.87
    Name: 560, dtype: object namie jp
    Processing Record 560of Set 1 | namie
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=namie&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=namie&units=imperial
    561 City Name           pangkalanbuun
    Country Code                   id
    Latitude                    -2.68
    Longitude                  111.62
    Temperature (F)             76.67
    Humidity (%)                   95
    Cloudiness (%)                 36
    Wind Speed (mph)             4.21
    Name: 561, dtype: object pangkalanbuun id
    Processing Record 561of Set 1 | pangkalanbuun
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pangkalanbuun&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pangkalanbuun&units=imperial
    562 City Name             eyl
    Country Code           so
    Latitude             7.98
    Longitude           49.82
    Temperature (F)     75.86
    Humidity (%)          100
    Cloudiness (%)         68
    Wind Speed (mph)    11.36
    Name: 562, dtype: object eyl so
    Processing Record 562of Set 1 | eyl
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=eyl&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=eyl&units=imperial
    563 City Name           sisimiut
    Country Code              gl
    Latitude               66.94
    Longitude             -53.67
    Temperature (F)         7.28
    Humidity (%)              74
    Cloudiness (%)            36
    Wind Speed (mph)        2.19
    Name: 563, dtype: object sisimiut gl
    Processing Record 563of Set 1 | sisimiut
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sisimiut&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sisimiut&units=imperial
    564 City Name           sinegorye
    Country Code               ru
    Latitude                62.09
    Longitude              150.53
    Temperature (F)        -14.41
    Humidity (%)               78
    Cloudiness (%)             64
    Wind Speed (mph)         2.64
    Name: 564, dtype: object sinegorye ru
    Processing Record 564of Set 1 | sinegorye
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sinegorye&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sinegorye&units=imperial
    567 City Name            bowen
    Country Code            au
    Latitude            -20.01
    Longitude           148.25
    Temperature (F)       84.2
    Humidity (%)            62
    Cloudiness (%)          40
    Wind Speed (mph)     16.11
    Name: 567, dtype: object bowen au
    Processing Record 567of Set 1 | bowen
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bowen&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bowen&units=imperial
    568 City Name           smithers
    Country Code              ca
    Latitude               54.78
    Longitude            -127.17
    Temperature (F)           50
    Humidity (%)              43
    Cloudiness (%)            75
    Wind Speed (mph)        5.82
    Name: 568, dtype: object smithers ca
    Processing Record 568of Set 1 | smithers
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=smithers&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=smithers&units=imperial
    569 City Name           taoudenni
    Country Code               ml
    Latitude                22.68
    Longitude               -3.98
    Temperature (F)          55.7
    Humidity (%)               24
    Cloudiness (%)              0
    Wind Speed (mph)         2.75
    Name: 569, dtype: object taoudenni ml
    Processing Record 569of Set 1 | taoudenni
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=taoudenni&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=taoudenni&units=imperial
    570 City Name           dingle
    Country Code            ie
    Latitude                11
    Longitude           122.67
    Temperature (F)      79.46
    Humidity (%)            83
    Cloudiness (%)          36
    Wind Speed (mph)     10.47
    Name: 570, dtype: object dingle ie
    Processing Record 570of Set 1 | dingle
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dingle&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dingle&units=imperial
    571 City Name           itarema
    Country Code             br
    Latitude              -2.92
    Longitude            -39.92
    Temperature (F)        77.3
    Humidity (%)             88
    Cloudiness (%)           12
    Wind Speed (mph)       6.78
    Name: 571, dtype: object itarema br
    Processing Record 571of Set 1 | itarema
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=itarema&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=itarema&units=imperial
    572 City Name           whitehorse
    Country Code                ca
    Latitude                 60.72
    Longitude              -135.06
    Temperature (F)           44.6
    Humidity (%)                39
    Cloudiness (%)              75
    Wind Speed (mph)          2.24
    Name: 572, dtype: object whitehorse ca
    Processing Record 572of Set 1 | whitehorse
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=whitehorse&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=whitehorse&units=imperial
    573 City Name             puro
    Country Code            ph
    Latitude             13.13
    Longitude           123.76
    Temperature (F)      75.05
    Humidity (%)           100
    Cloudiness (%)          88
    Wind Speed (mph)      3.42
    Name: 573, dtype: object puro ph
    Processing Record 573of Set 1 | puro
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=puro&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=puro&units=imperial
    574 City Name           narsaq
    Country Code            gl
    Latitude             60.91
    Longitude           -46.05
    Temperature (F)       44.6
    Humidity (%)            39
    Cloudiness (%)          80
    Wind Speed (mph)     43.62
    Name: 574, dtype: object narsaq gl
    Processing Record 574of Set 1 | narsaq
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=narsaq&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=narsaq&units=imperial
    575 City Name           normandin
    Country Code               ca
    Latitude                48.84
    Longitude              -72.53
    Temperature (F)          12.2
    Humidity (%)               47
    Cloudiness (%)             75
    Wind Speed (mph)        12.75
    Name: 575, dtype: object normandin ca
    Processing Record 575of Set 1 | normandin
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=normandin&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=normandin&units=imperial
    576 City Name           ancud
    Country Code           cl
    Latitude           -41.87
    Longitude          -73.83
    Temperature (F)     54.53
    Humidity (%)           91
    Cloudiness (%)         92
    Wind Speed (mph)     5.44
    Name: 576, dtype: object ancud cl
    Processing Record 576of Set 1 | ancud
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ancud&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ancud&units=imperial
    577 City Name           kingsport
    Country Code               us
    Latitude                36.55
    Longitude              -82.56
    Temperature (F)         53.26
    Humidity (%)               81
    Cloudiness (%)              1
    Wind Speed (mph)          2.3
    Name: 577, dtype: object kingsport us
    Processing Record 577of Set 1 | kingsport
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kingsport&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kingsport&units=imperial
    578 City Name           morozovsk
    Country Code               ru
    Latitude                48.35
    Longitude               41.83
    Temperature (F)            17
    Humidity (%)               90
    Cloudiness (%)             92
    Wind Speed (mph)        16.73
    Name: 578, dtype: object morozovsk ru
    Processing Record 578of Set 1 | morozovsk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=morozovsk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=morozovsk&units=imperial
    579 City Name           baherden
    Country Code              tm
    Latitude               38.44
    Longitude              57.43
    Temperature (F)         43.1
    Humidity (%)              95
    Cloudiness (%)             8
    Wind Speed (mph)        3.31
    Name: 579, dtype: object baherden tm
    Processing Record 579of Set 1 | baherden
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=baherden&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=baherden&units=imperial
    580 City Name           jaisalmer
    Country Code               in
    Latitude                26.91
    Longitude               70.91
    Temperature (F)          64.7
    Humidity (%)               46
    Cloudiness (%)             24
    Wind Speed (mph)         3.42
    Name: 580, dtype: object jaisalmer in
    Processing Record 580of Set 1 | jaisalmer
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=jaisalmer&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=jaisalmer&units=imperial
    581 City Name           sadri
    Country Code           in
    Latitude            25.19
    Longitude           73.45
    Temperature (F)     65.87
    Humidity (%)           46
    Cloudiness (%)         76
    Wind Speed (mph)     2.42
    Name: 581, dtype: object sadri in
    Processing Record 581of Set 1 | sadri
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sadri&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sadri&units=imperial
    582 City Name           blagoyevo
    Country Code               ru
    Latitude                63.37
    Longitude               47.92
    Temperature (F)          1.79
    Humidity (%)               77
    Cloudiness (%)             76
    Wind Speed (mph)         6.78
    Name: 582, dtype: object blagoyevo ru
    Processing Record 582of Set 1 | blagoyevo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=blagoyevo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=blagoyevo&units=imperial
    583 City Name           sai buri
    Country Code              th
    Latitude                 6.7
    Longitude             101.62
    Temperature (F)         75.2
    Humidity (%)              94
    Cloudiness (%)            20
    Wind Speed (mph)        3.36
    Name: 583, dtype: object sai buri th
    Processing Record 583of Set 1 | sai buri
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sai buri&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sai%20buri&units=imperial
    585 City Name           navirai
    Country Code             br
    Latitude             -23.07
    Longitude            -54.19
    Temperature (F)       79.01
    Humidity (%)             88
    Cloudiness (%)           36
    Wind Speed (mph)       1.86
    Name: 585, dtype: object navirai br
    Processing Record 585of Set 1 | navirai
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=navirai&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=navirai&units=imperial
    586 City Name           safranbolu
    Country Code                tr
    Latitude                 41.25
    Longitude                32.69
    Temperature (F)          52.28
    Humidity (%)                99
    Cloudiness (%)              80
    Wind Speed (mph)          1.63
    Name: 586, dtype: object safranbolu tr
    Processing Record 586of Set 1 | safranbolu
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=safranbolu&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=safranbolu&units=imperial
    587 City Name           lorengau
    Country Code              pg
    Latitude               -2.02
    Longitude             147.27
    Temperature (F)        83.33
    Humidity (%)              97
    Cloudiness (%)            68
    Wind Speed (mph)        9.69
    Name: 587, dtype: object lorengau pg
    Processing Record 587of Set 1 | lorengau
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lorengau&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lorengau&units=imperial
    589 City Name           ust-ordynskiy
    Country Code                   ru
    Latitude                     52.8
    Longitude                  104.75
    Temperature (F)              19.4
    Humidity (%)                   85
    Cloudiness (%)                  0
    Wind Speed (mph)             4.47
    Name: 589, dtype: object ust-ordynskiy ru
    Processing Record 589of Set 1 | ust-ordynskiy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ust-ordynskiy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ust-ordynskiy&units=imperial
    590 City Name           springbok
    Country Code               za
    Latitude               -29.67
    Longitude               17.88
    Temperature (F)         45.98
    Humidity (%)               67
    Cloudiness (%)              0
    Wind Speed (mph)         2.75
    Name: 590, dtype: object springbok za
    Processing Record 590of Set 1 | springbok
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=springbok&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=springbok&units=imperial
    591 City Name           sabang
    Country Code            id
    Latitude             13.72
    Longitude           123.58
    Temperature (F)      79.46
    Humidity (%)            95
    Cloudiness (%)          48
    Wind Speed (mph)      8.68
    Name: 591, dtype: object sabang id
    Processing Record 591of Set 1 | sabang
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sabang&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sabang&units=imperial
    592 City Name           trapani
    Country Code             it
    Latitude              38.02
    Longitude             12.54
    Temperature (F)        53.6
    Humidity (%)             81
    Cloudiness (%)           40
    Wind Speed (mph)      12.75
    Name: 592, dtype: object trapani it
    Processing Record 592of Set 1 | trapani
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=trapani&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=trapani&units=imperial
    593 City Name           barra
    Country Code           br
    Latitude           -11.09
    Longitude          -43.14
    Temperature (F)     74.78
    Humidity (%)           80
    Cloudiness (%)         20
    Wind Speed (mph)     3.65
    Name: 593, dtype: object barra br
    Processing Record 593of Set 1 | barra
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=barra&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=barra&units=imperial
    594 City Name           salinas
    Country Code             ec
    Latitude              36.67
    Longitude           -121.66
    Temperature (F)       54.95
    Humidity (%)             54
    Cloudiness (%)            1
    Wind Speed (mph)      11.41
    Name: 594, dtype: object salinas ec
    Processing Record 594of Set 1 | salinas
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=salinas&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=salinas&units=imperial
    595 City Name           bryan
    Country Code           us
    Latitude            30.67
    Longitude          -96.37
    Temperature (F)      65.8
    Humidity (%)          100
    Cloudiness (%)         90
    Wind Speed (mph)     8.05
    Name: 595, dtype: object bryan us
    Processing Record 595of Set 1 | bryan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bryan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bryan&units=imperial
    596 City Name           kamaishi
    Country Code              jp
    Latitude               39.28
    Longitude             141.86
    Temperature (F)         53.6
    Humidity (%)              50
    Cloudiness (%)            40
    Wind Speed (mph)        8.05
    Name: 596, dtype: object kamaishi jp
    Processing Record 596of Set 1 | kamaishi
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kamaishi&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kamaishi&units=imperial
    598 City Name           makakilo city
    Country Code                   us
    Latitude                    21.35
    Longitude                 -158.09
    Temperature (F)             78.33
    Humidity (%)                   69
    Cloudiness (%)                 90
    Wind Speed (mph)            13.82
    Name: 598, dtype: object makakilo city us
    Processing Record 598of Set 1 | makakilo city
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=makakilo city&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=makakilo%20city&units=imperial
    599 City Name           fort nelson
    Country Code                 ca
    Latitude                  58.81
    Longitude               -122.69
    Temperature (F)            44.6
    Humidity (%)                 39
    Cloudiness (%)               75
    Wind Speed (mph)           6.93
    Name: 599, dtype: object fort nelson ca
    Processing Record 599of Set 1 | fort nelson
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fort nelson&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fort%20nelson&units=imperial
    600 City Name           barra do garcas
    Country Code                     br
    Latitude                     -15.89
    Longitude                    -52.26
    Temperature (F)               76.13
    Humidity (%)                     89
    Cloudiness (%)                   12
    Wind Speed (mph)               2.64
    Name: 600, dtype: object barra do garcas br
    Processing Record 600of Set 1 | barra do garcas
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=barra do garcas&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=barra%20do%20garcas&units=imperial
    602 City Name           iwanai
    Country Code            jp
    Latitude             42.97
    Longitude           140.51
    Temperature (F)      33.74
    Humidity (%)           100
    Cloudiness (%)          80
    Wind Speed (mph)     21.65
    Name: 602, dtype: object iwanai jp
    Processing Record 602of Set 1 | iwanai
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=iwanai&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=iwanai&units=imperial
    603 City Name           siocon
    Country Code            ph
    Latitude             11.03
    Longitude           124.04
    Temperature (F)      81.26
    Humidity (%)            98
    Cloudiness (%)          64
    Wind Speed (mph)      6.89
    Name: 603, dtype: object siocon ph
    Processing Record 603of Set 1 | siocon
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=siocon&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=siocon&units=imperial
    604 City Name            bagh
    Country Code           in
    Latitude            33.98
    Longitude           73.78
    Temperature (F)     43.01
    Humidity (%)           55
    Cloudiness (%)         80
    Wind Speed (mph)     1.97
    Name: 604, dtype: object bagh in
    Processing Record 604of Set 1 | bagh
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bagh&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bagh&units=imperial
    605 City Name           chapais
    Country Code             ca
    Latitude              49.78
    Longitude            -74.86
    Temperature (F)         3.2
    Humidity (%)             71
    Cloudiness (%)           90
    Wind Speed (mph)      11.41
    Name: 605, dtype: object chapais ca
    Processing Record 605of Set 1 | chapais
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=chapais&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=chapais&units=imperial
    606 City Name           sao gotardo
    Country Code                 br
    Latitude                 -19.31
    Longitude                -46.05
    Temperature (F)           72.44
    Humidity (%)                 84
    Cloudiness (%)               88
    Wind Speed (mph)           3.53
    Name: 606, dtype: object sao gotardo br
    Processing Record 606of Set 1 | sao gotardo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sao gotardo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sao%20gotardo&units=imperial
    607 City Name           brownsville
    Country Code                 us
    Latitude                  25.91
    Longitude                -97.49
    Temperature (F)            82.4
    Humidity (%)                 74
    Cloudiness (%)                1
    Wind Speed (mph)          13.87
    Name: 607, dtype: object brownsville us
    Processing Record 607of Set 1 | brownsville
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=brownsville&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=brownsville&units=imperial
    608 City Name           callaway
    Country Code              us
    Latitude               46.98
    Longitude             -95.91
    Temperature (F)        35.26
    Humidity (%)              96
    Cloudiness (%)            90
    Wind Speed (mph)        5.82
    Name: 608, dtype: object callaway us
    Processing Record 608of Set 1 | callaway
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=callaway&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=callaway&units=imperial
    609 City Name           gushikawa
    Country Code               jp
    Latitude                26.35
    Longitude              127.87
    Temperature (F)          71.6
    Humidity (%)               78
    Cloudiness (%)              1
    Wind Speed (mph)         9.17
    Name: 609, dtype: object gushikawa jp
    Processing Record 609of Set 1 | gushikawa
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gushikawa&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gushikawa&units=imperial
    610 City Name           santa isabel do rio negro
    Country Code                               br
    Latitude                                -0.41
    Longitude                              -65.02
    Temperature (F)                         84.86
    Humidity (%)                               65
    Cloudiness (%)                             32
    Wind Speed (mph)                         6.67
    Name: 610, dtype: object santa isabel do rio negro br
    Processing Record 610of Set 1 | santa isabel do rio negro
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=santa isabel do rio negro&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=santa%20isabel%20do%20rio%20negro&units=imperial
    611 City Name           neryungri
    Country Code               ru
    Latitude                56.66
    Longitude              124.71
    Temperature (F)          0.08
    Humidity (%)               49
    Cloudiness (%)             36
    Wind Speed (mph)        12.03
    Name: 611, dtype: object neryungri ru
    Processing Record 611of Set 1 | neryungri
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=neryungri&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=neryungri&units=imperial
    612 City Name           morris
    Country Code            ca
    Latitude             41.36
    Longitude           -88.42
    Temperature (F)      48.79
    Humidity (%)            49
    Cloudiness (%)           1
    Wind Speed (mph)      2.98
    Name: 612, dtype: object morris ca
    Processing Record 612of Set 1 | morris
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=morris&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=morris&units=imperial
    613 City Name           manapparai
    Country Code                in
    Latitude                 10.61
    Longitude                78.42
    Temperature (F)           75.2
    Humidity (%)                88
    Cloudiness (%)              20
    Wind Speed (mph)          3.36
    Name: 613, dtype: object manapparai in
    Processing Record 613of Set 1 | manapparai
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=manapparai&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=manapparai&units=imperial
    614 City Name           cantapoy
    Country Code              ph
    Latitude                9.49
    Longitude             125.44
    Temperature (F)        79.73
    Humidity (%)             100
    Cloudiness (%)            48
    Wind Speed (mph)         7.9
    Name: 614, dtype: object cantapoy ph
    Processing Record 614of Set 1 | cantapoy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cantapoy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cantapoy&units=imperial
    615 City Name           zelenoborskiy
    Country Code                   ru
    Latitude                    66.84
    Longitude                   32.36
    Temperature (F)             16.73
    Humidity (%)                   64
    Cloudiness (%)                  0
    Wind Speed (mph)             7.67
    Name: 615, dtype: object zelenoborskiy ru
    Processing Record 615of Set 1 | zelenoborskiy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=zelenoborskiy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=zelenoborskiy&units=imperial
    617 City Name           shelburne
    Country Code               ca
    Latitude                44.08
    Longitude               -80.2
    Temperature (F)         32.68
    Humidity (%)               86
    Cloudiness (%)              1
    Wind Speed (mph)        11.41
    Name: 617, dtype: object shelburne ca
    Processing Record 617of Set 1 | shelburne
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=shelburne&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=shelburne&units=imperial
    618 City Name           emerald
    Country Code             au
    Latitude             -23.53
    Longitude            148.16
    Temperature (F)       83.78
    Humidity (%)             68
    Cloudiness (%)            0
    Wind Speed (mph)       4.65
    Name: 618, dtype: object emerald au
    Processing Record 618of Set 1 | emerald
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=emerald&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=emerald&units=imperial
    619 City Name           cesvaine
    Country Code              lv
    Latitude               56.97
    Longitude              26.31
    Temperature (F)         23.3
    Humidity (%)              80
    Cloudiness (%)            12
    Wind Speed (mph)       10.13
    Name: 619, dtype: object cesvaine lv
    Processing Record 619of Set 1 | cesvaine
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cesvaine&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cesvaine&units=imperial
    620 City Name           mogocha
    Country Code             ru
    Latitude              53.73
    Longitude            119.76
    Temperature (F)        4.85
    Humidity (%)             56
    Cloudiness (%)            0
    Wind Speed (mph)       2.42
    Name: 620, dtype: object mogocha ru
    Processing Record 620of Set 1 | mogocha
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mogocha&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mogocha&units=imperial
    621 City Name             ugu
    Country Code           in
    Latitude             6.55
    Longitude            6.44
    Temperature (F)     81.35
    Humidity (%)           73
    Cloudiness (%)          8
    Wind Speed (mph)    11.25
    Name: 621, dtype: object ugu in
    Processing Record 621of Set 1 | ugu
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ugu&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ugu&units=imperial
    622 City Name            praya
    Country Code            id
    Latitude             -8.71
    Longitude           116.27
    Temperature (F)       80.6
    Humidity (%)            83
    Cloudiness (%)          20
    Wind Speed (mph)      2.24
    Name: 622, dtype: object praya id
    Processing Record 622of Set 1 | praya
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=praya&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=praya&units=imperial
    623 City Name           ventspils
    Country Code               lv
    Latitude                57.39
    Longitude               21.56
    Temperature (F)          33.8
    Humidity (%)               80
    Cloudiness (%)              0
    Wind Speed (mph)        12.75
    Name: 623, dtype: object ventspils lv
    Processing Record 623of Set 1 | ventspils
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ventspils&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ventspils&units=imperial
    624 City Name           port said
    Country Code               eg
    Latitude                25.34
    Longitude               30.55
    Temperature (F)          66.5
    Humidity (%)               16
    Cloudiness (%)              0
    Wind Speed (mph)         6.78
    Name: 624, dtype: object port said eg
    Processing Record 624of Set 1 | port said
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port said&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=port%20said&units=imperial
    625 City Name           jacksonville
    Country Code                  us
    Latitude                   30.33
    Longitude                 -81.66
    Temperature (F)            74.16
    Humidity (%)                  42
    Cloudiness (%)                75
    Wind Speed (mph)             4.7
    Name: 625, dtype: object jacksonville us
    Processing Record 625of Set 1 | jacksonville
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=jacksonville&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=jacksonville&units=imperial
    626 City Name           pacifica
    Country Code              us
    Latitude               37.61
    Longitude            -122.49
    Temperature (F)        55.56
    Humidity (%)              62
    Cloudiness (%)            90
    Wind Speed (mph)        6.93
    Name: 626, dtype: object pacifica us
    Processing Record 626of Set 1 | pacifica
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pacifica&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pacifica&units=imperial
    627 City Name           reshetnikovo
    Country Code                  ru
    Latitude                   56.45
    Longitude                  36.57
    Temperature (F)            18.62
    Humidity (%)                  83
    Cloudiness (%)                44
    Wind Speed (mph)           14.16
    Name: 627, dtype: object reshetnikovo ru
    Processing Record 627of Set 1 | reshetnikovo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=reshetnikovo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=reshetnikovo&units=imperial
    628 City Name           huainan
    Country Code             cn
    Latitude              32.59
    Longitude            117.01
    Temperature (F)       46.25
    Humidity (%)            100
    Cloudiness (%)           92
    Wind Speed (mph)      10.02
    Name: 628, dtype: object huainan cn
    Processing Record 628of Set 1 | huainan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=huainan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=huainan&units=imperial
    629 City Name           knysna
    Country Code            za
    Latitude            -34.04
    Longitude            23.05
    Temperature (F)       64.4
    Humidity (%)            88
    Cloudiness (%)          68
    Wind Speed (mph)      9.17
    Name: 629, dtype: object knysna za
    Processing Record 629of Set 1 | knysna
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=knysna&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=knysna&units=imperial
    630 City Name           uporovo
    Country Code             ru
    Latitude              56.31
    Longitude             66.27
    Temperature (F)       34.73
    Humidity (%)             97
    Cloudiness (%)           88
    Wind Speed (mph)      21.88
    Name: 630, dtype: object uporovo ru
    Processing Record 630of Set 1 | uporovo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=uporovo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=uporovo&units=imperial
    631 City Name           sayyan
    Country Code            ye
    Latitude             15.17
    Longitude            44.32
    Temperature (F)       46.7
    Humidity (%)            44
    Cloudiness (%)           0
    Wind Speed (mph)      3.09
    Name: 631, dtype: object sayyan ye
    Processing Record 631of Set 1 | sayyan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sayyan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sayyan&units=imperial
    632 City Name           bonavista
    Country Code               ca
    Latitude                48.65
    Longitude              -53.11
    Temperature (F)         32.84
    Humidity (%)               88
    Cloudiness (%)             92
    Wind Speed (mph)        30.82
    Name: 632, dtype: object bonavista ca
    Processing Record 632of Set 1 | bonavista
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bonavista&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bonavista&units=imperial
    633 City Name           mozelos
    Country Code             pt
    Latitude                 41
    Longitude             -8.56
    Temperature (F)        51.8
    Humidity (%)             81
    Cloudiness (%)           40
    Wind Speed (mph)       6.93
    Name: 633, dtype: object mozelos pt
    Processing Record 633of Set 1 | mozelos
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mozelos&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mozelos&units=imperial
    634 City Name           provideniya
    Country Code                 ru
    Latitude                  64.42
    Longitude               -173.23
    Temperature (F)           28.88
    Humidity (%)                100
    Cloudiness (%)               24
    Wind Speed (mph)          18.19
    Name: 634, dtype: object provideniya ru
    Processing Record 634of Set 1 | provideniya
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=provideniya&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=provideniya&units=imperial
    635 City Name           bubaque
    Country Code             gw
    Latitude              11.28
    Longitude            -15.83
    Temperature (F)          77
    Humidity (%)             83
    Cloudiness (%)            8
    Wind Speed (mph)       8.05
    Name: 635, dtype: object bubaque gw
    Processing Record 635of Set 1 | bubaque
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bubaque&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bubaque&units=imperial
    637 City Name            daru
    Country Code           pg
    Latitude             7.99
    Longitude          -10.85
    Temperature (F)     77.03
    Humidity (%)           81
    Cloudiness (%)         12
    Wind Speed (mph)     6.78
    Name: 637, dtype: object daru pg
    Processing Record 637of Set 1 | daru
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=daru&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=daru&units=imperial
    638 City Name            hami
    Country Code           cn
    Latitude            42.84
    Longitude           93.51
    Temperature (F)     36.71
    Humidity (%)           90
    Cloudiness (%)         88
    Wind Speed (mph)     3.76
    Name: 638, dtype: object hami cn
    Processing Record 638of Set 1 | hami
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hami&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hami&units=imperial
    639 City Name           kungurtug
    Country Code               ru
    Latitude                 50.6
    Longitude               97.53
    Temperature (F)          -0.1
    Humidity (%)               61
    Cloudiness (%)              0
    Wind Speed (mph)          3.2
    Name: 639, dtype: object kungurtug ru
    Processing Record 639of Set 1 | kungurtug
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kungurtug&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kungurtug&units=imperial
    641 City Name           sao joao da ponte
    Country Code                       br
    Latitude                       -15.93
    Longitude                      -44.01
    Temperature (F)                 70.01
    Humidity (%)                       92
    Cloudiness (%)                     68
    Wind Speed (mph)                 5.99
    Name: 641, dtype: object sao joao da ponte br
    Processing Record 641of Set 1 | sao joao da ponte
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sao joao da ponte&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sao%20joao%20da%20ponte&units=imperial
    642 City Name           juneau
    Country Code            us
    Latitude              58.3
    Longitude          -134.42
    Temperature (F)      41.23
    Humidity (%)            86
    Cloudiness (%)          90
    Wind Speed (mph)      5.82
    Name: 642, dtype: object juneau us
    Processing Record 642of Set 1 | juneau
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=juneau&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=juneau&units=imperial
    643 City Name           ossora
    Country Code            ru
    Latitude             59.24
    Longitude           163.07
    Temperature (F)       6.29
    Humidity (%)            95
    Cloudiness (%)          20
    Wind Speed (mph)     14.16
    Name: 643, dtype: object ossora ru
    Processing Record 643of Set 1 | ossora
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ossora&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ossora&units=imperial
    644 City Name           magdagachi
    Country Code                ru
    Latitude                 53.45
    Longitude                125.8
    Temperature (F)           9.35
    Humidity (%)                41
    Cloudiness (%)               0
    Wind Speed (mph)          5.66
    Name: 644, dtype: object magdagachi ru
    Processing Record 644of Set 1 | magdagachi
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=magdagachi&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=magdagachi&units=imperial
    645 City Name           labuhan
    Country Code             id
    Latitude              -2.54
    Longitude            115.51
    Temperature (F)       75.05
    Humidity (%)             94
    Cloudiness (%)           76
    Wind Speed (mph)       1.97
    Name: 645, dtype: object labuhan id
    Processing Record 645of Set 1 | labuhan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=labuhan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=labuhan&units=imperial
    646 City Name            selma
    Country Code            us
    Latitude             36.57
    Longitude          -119.61
    Temperature (F)       57.2
    Humidity (%)            50
    Cloudiness (%)           1
    Wind Speed (mph)      9.17
    Name: 646, dtype: object selma us
    Processing Record 646of Set 1 | selma
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=selma&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=selma&units=imperial
    647 City Name           temir
    Country Code           kz
    Latitude            49.14
    Longitude           57.13
    Temperature (F)     34.28
    Humidity (%)           99
    Cloudiness (%)         92
    Wind Speed (mph)    15.84
    Name: 647, dtype: object temir kz
    Processing Record 647of Set 1 | temir
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=temir&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=temir&units=imperial
    648 City Name           urengoy
    Country Code             ru
    Latitude              65.96
    Longitude             78.37
    Temperature (F)       -0.73
    Humidity (%)             67
    Cloudiness (%)           80
    Wind Speed (mph)       6.33
    Name: 648, dtype: object urengoy ru
    Processing Record 648of Set 1 | urengoy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=urengoy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=urengoy&units=imperial
    649 City Name           kilindoni
    Country Code               tz
    Latitude                -7.91
    Longitude               39.67
    Temperature (F)         82.97
    Humidity (%)              100
    Cloudiness (%)             48
    Wind Speed (mph)         7.34
    Name: 649, dtype: object kilindoni tz
    Processing Record 649of Set 1 | kilindoni
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kilindoni&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kilindoni&units=imperial
    650 City Name           okahandja
    Country Code               na
    Latitude               -21.98
    Longitude               16.91
    Temperature (F)          66.2
    Humidity (%)               68
    Cloudiness (%)              0
    Wind Speed (mph)          4.7
    Name: 650, dtype: object okahandja na
    Processing Record 650of Set 1 | okahandja
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=okahandja&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=okahandja&units=imperial
    651 City Name           tagusao
    Country Code             ph
    Latitude               9.19
    Longitude            117.81
    Temperature (F)       79.91
    Humidity (%)             94
    Cloudiness (%)           64
    Wind Speed (mph)       5.21
    Name: 651, dtype: object tagusao ph
    Processing Record 651of Set 1 | tagusao
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tagusao&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tagusao&units=imperial
    652 City Name           liverpool
    Country Code               ca
    Latitude                53.41
    Longitude               -2.98
    Temperature (F)         32.81
    Humidity (%)               69
    Cloudiness (%)             75
    Wind Speed (mph)        16.11
    Name: 652, dtype: object liverpool ca
    Processing Record 652of Set 1 | liverpool
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=liverpool&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=liverpool&units=imperial
    653 City Name           san jeronimo
    Country Code                  mx
    Latitude                  -13.65
    Longitude                 -73.37
    Temperature (F)            46.79
    Humidity (%)                  95
    Cloudiness (%)                80
    Wind Speed (mph)            1.97
    Name: 653, dtype: object san jeronimo mx
    Processing Record 653of Set 1 | san jeronimo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=san jeronimo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=san%20jeronimo&units=imperial
    654 City Name            suez
    Country Code           eg
    Latitude            29.97
    Longitude           32.54
    Temperature (F)     51.92
    Humidity (%)           76
    Cloudiness (%)          0
    Wind Speed (mph)     4.76
    Name: 654, dtype: object suez eg
    Processing Record 654of Set 1 | suez
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=suez&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=suez&units=imperial
    655 City Name           thinadhoo
    Country Code               mv
    Latitude                 0.53
    Longitude               72.93
    Temperature (F)          85.4
    Humidity (%)               97
    Cloudiness (%)             92
    Wind Speed (mph)         7.67
    Name: 655, dtype: object thinadhoo mv
    Processing Record 655of Set 1 | thinadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=thinadhoo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=thinadhoo&units=imperial
    657 City Name           guanambi
    Country Code              br
    Latitude              -14.22
    Longitude             -42.78
    Temperature (F)        70.91
    Humidity (%)              79
    Cloudiness (%)             0
    Wind Speed (mph)        6.11
    Name: 657, dtype: object guanambi br
    Processing Record 657of Set 1 | guanambi
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=guanambi&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=guanambi&units=imperial
    658 City Name           joshimath
    Country Code               in
    Latitude                30.57
    Longitude               79.57
    Temperature (F)         -2.35
    Humidity (%)               86
    Cloudiness (%)             12
    Wind Speed (mph)         1.41
    Name: 658, dtype: object joshimath in
    Processing Record 658of Set 1 | joshimath
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=joshimath&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=joshimath&units=imperial
    659 City Name           muroto
    Country Code            jp
    Latitude             33.37
    Longitude           134.14
    Temperature (F)       57.2
    Humidity (%)            93
    Cloudiness (%)          75
    Wind Speed (mph)      5.82
    Name: 659, dtype: object muroto jp
    Processing Record 659of Set 1 | muroto
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=muroto&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=muroto&units=imperial
    660 City Name           cacoal
    Country Code            br
    Latitude            -11.43
    Longitude           -61.44
    Temperature (F)       77.3
    Humidity (%)            90
    Cloudiness (%)          56
    Wind Speed (mph)      1.97
    Name: 660, dtype: object cacoal br
    Processing Record 660of Set 1 | cacoal
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cacoal&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cacoal&units=imperial
    661 City Name           paramirim
    Country Code               br
    Latitude               -13.44
    Longitude              -42.24
    Temperature (F)         68.66
    Humidity (%)               88
    Cloudiness (%)             44
    Wind Speed (mph)         3.42
    Name: 661, dtype: object paramirim br
    Processing Record 661of Set 1 | paramirim
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=paramirim&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=paramirim&units=imperial
    662 City Name           takoradi
    Country Code              gh
    Latitude                4.89
    Longitude              -1.75
    Temperature (F)        80.63
    Humidity (%)             100
    Cloudiness (%)            12
    Wind Speed (mph)        8.68
    Name: 662, dtype: object takoradi gh
    Processing Record 662of Set 1 | takoradi
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=takoradi&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=takoradi&units=imperial
    664 City Name           srednekolymsk
    Country Code                   ru
    Latitude                    67.46
    Longitude                  153.71
    Temperature (F)            -16.03
    Humidity (%)                   48
    Cloudiness (%)                 48
    Wind Speed (mph)             6.44
    Name: 664, dtype: object srednekolymsk ru
    Processing Record 664of Set 1 | srednekolymsk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=srednekolymsk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=srednekolymsk&units=imperial
    665 City Name           hamilton
    Country Code              bm
    Latitude                32.3
    Longitude             -64.78
    Temperature (F)         64.4
    Humidity (%)              77
    Cloudiness (%)            75
    Wind Speed (mph)       18.34
    Name: 665, dtype: object hamilton bm
    Processing Record 665of Set 1 | hamilton
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hamilton&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hamilton&units=imperial
    666 City Name           yabrud
    Country Code            sy
    Latitude             33.97
    Longitude            36.66
    Temperature (F)       48.2
    Humidity (%)            45
    Cloudiness (%)           0
    Wind Speed (mph)      6.93
    Name: 666, dtype: object yabrud sy
    Processing Record 666of Set 1 | yabrud
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yabrud&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yabrud&units=imperial
    668 City Name             bam
    Country Code           ir
    Latitude            29.11
    Longitude           58.36
    Temperature (F)     45.17
    Humidity (%)           83
    Cloudiness (%)         76
    Wind Speed (mph)     1.41
    Name: 668, dtype: object bam ir
    Processing Record 668of Set 1 | bam
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bam&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bam&units=imperial
    669 City Name           abbeville
    Country Code               us
    Latitude                50.11
    Longitude                1.83
    Temperature (F)         31.06
    Humidity (%)               77
    Cloudiness (%)             90
    Wind Speed (mph)         9.17
    Name: 669, dtype: object abbeville us
    Processing Record 669of Set 1 | abbeville
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=abbeville&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=abbeville&units=imperial
    670 City Name           lodeynoye pole
    Country Code                    ru
    Latitude                     60.73
    Longitude                    33.55
    Temperature (F)              29.33
    Humidity (%)                    91
    Cloudiness (%)                  92
    Wind Speed (mph)             15.39
    Name: 670, dtype: object lodeynoye pole ru
    Processing Record 670of Set 1 | lodeynoye pole
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lodeynoye pole&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lodeynoye%20pole&units=imperial
    671 City Name           tazovskiy
    Country Code               ru
    Latitude                67.47
    Longitude                78.7
    Temperature (F)          -7.3
    Humidity (%)               61
    Cloudiness (%)             12
    Wind Speed (mph)        10.02
    Name: 671, dtype: object tazovskiy ru
    Processing Record 671of Set 1 | tazovskiy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tazovskiy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tazovskiy&units=imperial
    672 City Name           pimentel
    Country Code              pe
    Latitude                -3.7
    Longitude              -45.5
    Temperature (F)        79.01
    Humidity (%)              82
    Cloudiness (%)            68
    Wind Speed (mph)        4.43
    Name: 672, dtype: object pimentel pe
    Processing Record 672of Set 1 | pimentel
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pimentel&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pimentel&units=imperial
    673 City Name             mon
    Country Code           in
    Latitude            26.73
    Longitude           95.03
    Temperature (F)     51.83
    Humidity (%)           92
    Cloudiness (%)         12
    Wind Speed (mph)     2.75
    Name: 673, dtype: object mon in
    Processing Record 673of Set 1 | mon
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mon&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mon&units=imperial
    674 City Name           nakasongola
    Country Code                 ug
    Latitude                   1.32
    Longitude                 32.46
    Temperature (F)           65.33
    Humidity (%)                100
    Cloudiness (%)               88
    Wind Speed (mph)           3.09
    Name: 674, dtype: object nakasongola ug
    Processing Record 674of Set 1 | nakasongola
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nakasongola&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nakasongola&units=imperial
    675 City Name            adre
    Country Code           td
    Latitude            13.47
    Longitude            22.2
    Temperature (F)     59.93
    Humidity (%)           31
    Cloudiness (%)          0
    Wind Speed (mph)     5.21
    Name: 675, dtype: object adre td
    Processing Record 675of Set 1 | adre
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=adre&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=adre&units=imperial
    676 City Name           taltal
    Country Code            cl
    Latitude            -25.41
    Longitude           -70.49
    Temperature (F)      58.13
    Humidity (%)            87
    Cloudiness (%)           0
    Wind Speed (mph)      2.42
    Name: 676, dtype: object taltal cl
    Processing Record 676of Set 1 | taltal
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=taltal&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=taltal&units=imperial
    677 City Name           oranjestad
    Country Code                aw
    Latitude                 12.52
    Longitude               -70.03
    Temperature (F)           78.8
    Humidity (%)                83
    Cloudiness (%)              20
    Wind Speed (mph)         17.22
    Name: 677, dtype: object oranjestad aw
    Processing Record 677of Set 1 | oranjestad
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=oranjestad&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=oranjestad&units=imperial
    678 City Name           wasilla
    Country Code             us
    Latitude              61.58
    Longitude           -149.44
    Temperature (F)       38.61
    Humidity (%)             80
    Cloudiness (%)           90
    Wind Speed (mph)       3.36
    Name: 678, dtype: object wasilla us
    Processing Record 678of Set 1 | wasilla
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=wasilla&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=wasilla&units=imperial
    679 City Name           mandalgovi
    Country Code                mn
    Latitude                 45.76
    Longitude               106.27
    Temperature (F)          28.88
    Humidity (%)                81
    Cloudiness (%)              92
    Wind Speed (mph)          2.98
    Name: 679, dtype: object mandalgovi mn
    Processing Record 679of Set 1 | mandalgovi
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mandalgovi&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mandalgovi&units=imperial
    680 City Name           griffith
    Country Code              au
    Latitude              -34.29
    Longitude             146.06
    Temperature (F)        70.82
    Humidity (%)              35
    Cloudiness (%)             0
    Wind Speed (mph)        4.43
    Name: 680, dtype: object griffith au
    Processing Record 680of Set 1 | griffith
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=griffith&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=griffith&units=imperial
    681 City Name             hof
    Country Code           no
    Latitude            50.32
    Longitude           11.92
    Temperature (F)        23
    Humidity (%)           62
    Cloudiness (%)         90
    Wind Speed (mph)     9.17
    Name: 681, dtype: object hof no
    Processing Record 681of Set 1 | hof
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hof&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hof&units=imperial
    682 City Name           bereznik
    Country Code              ru
    Latitude               62.86
    Longitude              42.71
    Temperature (F)        23.93
    Humidity (%)              84
    Cloudiness (%)            92
    Wind Speed (mph)       12.26
    Name: 682, dtype: object bereznik ru
    Processing Record 682of Set 1 | bereznik
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bereznik&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bereznik&units=imperial
    683 City Name           surgana
    Country Code             in
    Latitude              20.56
    Longitude             73.64
    Temperature (F)       69.83
    Humidity (%)             59
    Cloudiness (%)           24
    Wind Speed (mph)       3.42
    Name: 683, dtype: object surgana in
    Processing Record 683of Set 1 | surgana
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=surgana&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=surgana&units=imperial
    684 City Name           djambala
    Country Code              cg
    Latitude               -2.55
    Longitude              14.76
    Temperature (F)         67.4
    Humidity (%)              99
    Cloudiness (%)            92
    Wind Speed (mph)        6.78
    Name: 684, dtype: object djambala cg
    Processing Record 684of Set 1 | djambala
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=djambala&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=djambala&units=imperial
    686 City Name            nome
    Country Code           us
    Latitude            30.04
    Longitude          -94.42
    Temperature (F)     74.73
    Humidity (%)           73
    Cloudiness (%)          1
    Wind Speed (mph)      4.7
    Name: 686, dtype: object nome us
    Processing Record 686of Set 1 | nome
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nome&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nome&units=imperial
    687 City Name           bahia de caraquez
    Country Code                       ec
    Latitude                         -0.6
    Longitude                      -80.42
    Temperature (F)                  80.6
    Humidity (%)                       74
    Cloudiness (%)                     20
    Wind Speed (mph)                 3.36
    Name: 687, dtype: object bahia de caraquez ec
    Processing Record 687of Set 1 | bahia de caraquez
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bahia de caraquez&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bahia%20de%20caraquez&units=imperial
    689 City Name           huanta
    Country Code            pe
    Latitude            -12.94
    Longitude           -74.25
    Temperature (F)         68
    Humidity (%)            52
    Cloudiness (%)          75
    Wind Speed (mph)      5.82
    Name: 689, dtype: object huanta pe
    Processing Record 689of Set 1 | huanta
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=huanta&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=huanta&units=imperial
    691 City Name           gamba
    Country Code           ga
    Latitude            28.28
    Longitude           88.52
    Temperature (F)     -6.58
    Humidity (%)           57
    Cloudiness (%)          0
    Wind Speed (mph)     2.98
    Name: 691, dtype: object gamba ga
    Processing Record 691of Set 1 | gamba
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gamba&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gamba&units=imperial
    692 City Name           bondo
    Country Code           cd
    Latitude             3.81
    Longitude           23.69
    Temperature (F)     69.29
    Humidity (%)          100
    Cloudiness (%)         76
    Wind Speed (mph)     2.53
    Name: 692, dtype: object bondo cd
    Processing Record 692of Set 1 | bondo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bondo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bondo&units=imperial
    693 City Name           jasper
    Country Code            ca
    Latitude             33.83
    Longitude           -87.28
    Temperature (F)      62.69
    Humidity (%)           100
    Cloudiness (%)         100
    Wind Speed (mph)      3.98
    Name: 693, dtype: object jasper ca
    Processing Record 693of Set 1 | jasper
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=jasper&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=jasper&units=imperial
    694 City Name           snasa
    Country Code           no
    Latitude            64.25
    Longitude           12.38
    Temperature (F)     29.06
    Humidity (%)           88
    Cloudiness (%)         88
    Wind Speed (mph)    10.69
    Name: 694, dtype: object snasa no
    Processing Record 694of Set 1 | snasa
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=snasa&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=snasa&units=imperial
    695 City Name           dmitriyevka
    Country Code                 ru
    Latitude                  43.08
    Longitude                    41
    Temperature (F)           47.33
    Humidity (%)                 79
    Cloudiness (%)               92
    Wind Speed (mph)           1.19
    Name: 695, dtype: object dmitriyevka ru
    Processing Record 695of Set 1 | dmitriyevka
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dmitriyevka&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dmitriyevka&units=imperial
    696 City Name           priargunsk
    Country Code                ru
    Latitude                 50.37
    Longitude               119.09
    Temperature (F)          15.11
    Humidity (%)                52
    Cloudiness (%)              24
    Wind Speed (mph)          3.42
    Name: 696, dtype: object priargunsk ru
    Processing Record 696of Set 1 | priargunsk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=priargunsk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=priargunsk&units=imperial
    697 City Name           ojinaga
    Country Code             mx
    Latitude              29.56
    Longitude           -104.41
    Temperature (F)       79.46
    Humidity (%)             10
    Cloudiness (%)            0
    Wind Speed (mph)       14.5
    Name: 697, dtype: object ojinaga mx
    Processing Record 697of Set 1 | ojinaga
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ojinaga&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ojinaga&units=imperial
    699 City Name           conselheiro lafaiete
    Country Code                          br
    Latitude                          -20.66
    Longitude                         -43.79
    Temperature (F)                     69.8
    Humidity (%)                          83
    Cloudiness (%)                         0
    Wind Speed (mph)                    1.12
    Name: 699, dtype: object conselheiro lafaiete br
    Processing Record 699of Set 1 | conselheiro lafaiete
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=conselheiro lafaiete&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=conselheiro%20lafaiete&units=imperial
    700 City Name           kulhudhuffushi
    Country Code                    mv
    Latitude                      6.62
    Longitude                    73.07
    Temperature (F)              82.61
    Humidity (%)                   100
    Cloudiness (%)                  56
    Wind Speed (mph)              6.67
    Name: 700, dtype: object kulhudhuffushi mv
    Processing Record 700of Set 1 | kulhudhuffushi
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kulhudhuffushi&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kulhudhuffushi&units=imperial
    701 City Name           pedernales
    Country Code                do
    Latitude                  0.07
    Longitude               -80.05
    Temperature (F)           77.3
    Humidity (%)                77
    Cloudiness (%)              24
    Wind Speed (mph)          4.54
    Name: 701, dtype: object pedernales do
    Processing Record 701of Set 1 | pedernales
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pedernales&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pedernales&units=imperial
    702 City Name           meadow lake
    Country Code                 ca
    Latitude                  54.13
    Longitude               -108.44
    Temperature (F)            24.8
    Humidity (%)                 68
    Cloudiness (%)               90
    Wind Speed (mph)           5.82
    Name: 702, dtype: object meadow lake ca
    Processing Record 702of Set 1 | meadow lake
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=meadow lake&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=meadow%20lake&units=imperial
    703 City Name           korbach
    Country Code             de
    Latitude              51.28
    Longitude              8.87
    Temperature (F)        24.8
    Humidity (%)             45
    Cloudiness (%)            0
    Wind Speed (mph)      14.99
    Name: 703, dtype: object korbach de
    Processing Record 703of Set 1 | korbach
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=korbach&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=korbach&units=imperial
    705 City Name             mao
    Country Code           td
    Latitude            19.55
    Longitude          -71.08
    Temperature (F)        77
    Humidity (%)           78
    Cloudiness (%)         40
    Wind Speed (mph)     3.36
    Name: 705, dtype: object mao td
    Processing Record 705of Set 1 | mao
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mao&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mao&units=imperial
    706 City Name            auki
    Country Code           sb
    Latitude            12.18
    Longitude            6.51
    Temperature (F)      66.5
    Humidity (%)           45
    Cloudiness (%)          0
    Wind Speed (mph)     2.64
    Name: 706, dtype: object auki sb
    Processing Record 706of Set 1 | auki
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=auki&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=auki&units=imperial
    707 City Name           sao joao da barra
    Country Code                       br
    Latitude                       -21.64
    Longitude                      -41.05
    Temperature (F)                    77
    Humidity (%)                       88
    Cloudiness (%)                      0
    Wind Speed (mph)                  4.7
    Name: 707, dtype: object sao joao da barra br
    Processing Record 707of Set 1 | sao joao da barra
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sao joao da barra&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sao%20joao%20da%20barra&units=imperial
    709 City Name           lukuledi
    Country Code              tz
    Latitude              -10.57
    Longitude               38.8
    Temperature (F)        68.03
    Humidity (%)              93
    Cloudiness (%)             0
    Wind Speed (mph)        3.31
    Name: 709, dtype: object lukuledi tz
    Processing Record 709of Set 1 | lukuledi
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lukuledi&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lukuledi&units=imperial
    710 City Name           kirkenaer
    Country Code               no
    Latitude                60.46
    Longitude               12.06
    Temperature (F)          9.71
    Humidity (%)               60
    Cloudiness (%)             36
    Wind Speed (mph)         4.21
    Name: 710, dtype: object kirkenaer no
    Processing Record 710of Set 1 | kirkenaer
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kirkenaer&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kirkenaer&units=imperial
    711 City Name           bitung
    Country Code            id
    Latitude              1.44
    Longitude           125.19
    Temperature (F)       84.2
    Humidity (%)            74
    Cloudiness (%)          40
    Wind Speed (mph)      2.24
    Name: 711, dtype: object bitung id
    Processing Record 711of Set 1 | bitung
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bitung&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bitung&units=imperial
    712 City Name           neiafu
    Country Code            to
    Latitude            -18.65
    Longitude          -173.98
    Temperature (F)       84.2
    Humidity (%)            70
    Cloudiness (%)          75
    Wind Speed (mph)      6.93
    Name: 712, dtype: object neiafu to
    Processing Record 712of Set 1 | neiafu
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=neiafu&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=neiafu&units=imperial
    713 City Name           ust-kut
    Country Code             ru
    Latitude              56.78
    Longitude            105.75
    Temperature (F)        9.26
    Humidity (%)             67
    Cloudiness (%)            0
    Wind Speed (mph)        2.3
    Name: 713, dtype: object ust-kut ru
    Processing Record 713of Set 1 | ust-kut
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ust-kut&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ust-kut&units=imperial
    714 City Name           houston
    Country Code             ca
    Latitude              29.76
    Longitude            -95.37
    Temperature (F)       77.74
    Humidity (%)             69
    Cloudiness (%)           75
    Wind Speed (mph)       9.17
    Name: 714, dtype: object houston ca
    Processing Record 714of Set 1 | houston
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=houston&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=houston&units=imperial
    715 City Name           partizansk
    Country Code                ru
    Latitude                 43.13
    Longitude               133.13
    Temperature (F)          20.24
    Humidity (%)                50
    Cloudiness (%)               0
    Wind Speed (mph)           8.9
    Name: 715, dtype: object partizansk ru
    Processing Record 715of Set 1 | partizansk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=partizansk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=partizansk&units=imperial
    716 City Name           sinzheim
    Country Code              de
    Latitude               48.76
    Longitude               8.17
    Temperature (F)         30.2
    Humidity (%)              80
    Cloudiness (%)            90
    Wind Speed (mph)       14.99
    Name: 716, dtype: object sinzheim de
    Processing Record 716of Set 1 | sinzheim
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sinzheim&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sinzheim&units=imperial
    717 City Name            arman
    Country Code            ru
    Latitude              59.7
    Longitude           150.17
    Temperature (F)          5
    Humidity (%)            42
    Cloudiness (%)          36
    Wind Speed (mph)     11.18
    Name: 717, dtype: object arman ru
    Processing Record 717of Set 1 | arman
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=arman&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=arman&units=imperial
    718 City Name           haines junction
    Country Code                     ca
    Latitude                      60.75
    Longitude                   -137.51
    Temperature (F)               31.85
    Humidity (%)                     88
    Cloudiness (%)                    8
    Wind Speed (mph)               2.75
    Name: 718, dtype: object haines junction ca
    Processing Record 718of Set 1 | haines junction
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=haines junction&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=haines%20junction&units=imperial
    719 City Name           bandar-e anzali
    Country Code                     ir
    Latitude                      37.47
    Longitude                     49.46
    Temperature (F)                42.8
    Humidity (%)                     93
    Cloudiness (%)                    0
    Wind Speed (mph)                2.3
    Name: 719, dtype: object bandar-e anzali ir
    Processing Record 719of Set 1 | bandar-e anzali
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bandar-e anzali&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bandar-e%20anzali&units=imperial
    720 City Name           yenagoa
    Country Code             ng
    Latitude               4.92
    Longitude              6.26
    Temperature (F)        76.4
    Humidity (%)             95
    Cloudiness (%)           12
    Wind Speed (mph)       4.99
    Name: 720, dtype: object yenagoa ng
    Processing Record 720of Set 1 | yenagoa
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yenagoa&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yenagoa&units=imperial
    721 City Name           yablochnyy
    Country Code                ru
    Latitude                 47.17
    Longitude               142.07
    Temperature (F)           28.4
    Humidity (%)                58
    Cloudiness (%)              40
    Wind Speed (mph)         11.18
    Name: 721, dtype: object yablochnyy ru
    Processing Record 721of Set 1 | yablochnyy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yablochnyy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yablochnyy&units=imperial
    722 City Name            soro
    Country Code           in
    Latitude            11.71
    Longitude            7.41
    Temperature (F)     67.22
    Humidity (%)           47
    Cloudiness (%)          0
    Wind Speed (mph)     3.98
    Name: 722, dtype: object soro in
    Processing Record 722of Set 1 | soro
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=soro&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=soro&units=imperial
    723 City Name            ayan
    Country Code           ru
    Latitude            40.67
    Longitude            33.6
    Temperature (F)     48.05
    Humidity (%)           60
    Cloudiness (%)         80
    Wind Speed (mph)        7
    Name: 723, dtype: object ayan ru
    Processing Record 723of Set 1 | ayan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ayan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ayan&units=imperial
    724 City Name           alamor
    Country Code            ec
    Latitude             -4.02
    Longitude           -80.02
    Temperature (F)       80.6
    Humidity (%)            74
    Cloudiness (%)          40
    Wind Speed (mph)      3.36
    Name: 724, dtype: object alamor ec
    Processing Record 724of Set 1 | alamor
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=alamor&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=alamor&units=imperial
    725 City Name           lauria
    Country Code            it
    Latitude             40.05
    Longitude            15.84
    Temperature (F)      44.63
    Humidity (%)           100
    Cloudiness (%)          88
    Wind Speed (mph)      2.75
    Name: 725, dtype: object lauria it
    Processing Record 725of Set 1 | lauria
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lauria&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lauria&units=imperial
    727 City Name             poya
    Country Code            nc
    Latitude            -21.35
    Longitude           165.16
    Temperature (F)      77.03
    Humidity (%)            99
    Cloudiness (%)           0
    Wind Speed (mph)     13.04
    Name: 727, dtype: object poya nc
    Processing Record 727of Set 1 | poya
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=poya&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=poya&units=imperial
    728 City Name           shira
    Country Code           ru
    Latitude            54.49
    Longitude           89.96
    Temperature (F)       8.9
    Humidity (%)           61
    Cloudiness (%)          8
    Wind Speed (mph)     1.97
    Name: 728, dtype: object shira ru
    Processing Record 728of Set 1 | shira
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=shira&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=shira&units=imperial
    729 City Name           stodolishche
    Country Code                  ru
    Latitude                   54.18
    Longitude                  32.65
    Temperature (F)             3.68
    Humidity (%)                  72
    Cloudiness (%)                 8
    Wind Speed (mph)               7
    Name: 729, dtype: object stodolishche ru
    Processing Record 729of Set 1 | stodolishche
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=stodolishche&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=stodolishche&units=imperial
    730 City Name           panacan
    Country Code             ph
    Latitude               7.15
    Longitude            125.66
    Temperature (F)        78.8
    Humidity (%)             78
    Cloudiness (%)           75
    Wind Speed (mph)       9.17
    Name: 730, dtype: object panacan ph
    Processing Record 730of Set 1 | panacan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=panacan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=panacan&units=imperial
    731 City Name           portadown
    Country Code               gb
    Latitude                54.42
    Longitude               -6.44
    Temperature (F)          33.8
    Humidity (%)               59
    Cloudiness (%)             75
    Wind Speed (mph)        18.34
    Name: 731, dtype: object portadown gb
    Processing Record 731of Set 1 | portadown
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=portadown&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=portadown&units=imperial
    732 City Name           beloha
    Country Code            mg
    Latitude            -25.17
    Longitude            45.06
    Temperature (F)      74.51
    Humidity (%)            85
    Cloudiness (%)          12
    Wind Speed (mph)     13.82
    Name: 732, dtype: object beloha mg
    Processing Record 732of Set 1 | beloha
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=beloha&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=beloha&units=imperial
    733 City Name           fereydun kenar
    Country Code                    ir
    Latitude                     36.69
    Longitude                    52.52
    Temperature (F)                 41
    Humidity (%)                    93
    Cloudiness (%)                   0
    Wind Speed (mph)              3.42
    Name: 733, dtype: object fereydun kenar ir
    Processing Record 733of Set 1 | fereydun kenar
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fereydun kenar&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fereydun%20kenar&units=imperial
    734 City Name           vila franca do campo
    Country Code                          pt
    Latitude                           37.72
    Longitude                         -25.43
    Temperature (F)                     60.8
    Humidity (%)                         100
    Cloudiness (%)                        75
    Wind Speed (mph)                   10.29
    Name: 734, dtype: object vila franca do campo pt
    Processing Record 734of Set 1 | vila franca do campo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vila franca do campo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=vila%20franca%20do%20campo&units=imperial
    736 City Name           nampula
    Country Code             mz
    Latitude             -15.12
    Longitude             39.26
    Temperature (F)        75.2
    Humidity (%)             78
    Cloudiness (%)           20
    Wind Speed (mph)       5.21
    Name: 736, dtype: object nampula mz
    Processing Record 736of Set 1 | nampula
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nampula&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nampula&units=imperial
    737 City Name           dakar
    Country Code           sn
    Latitude            14.69
    Longitude          -17.45
    Temperature (F)        68
    Humidity (%)           72
    Cloudiness (%)          0
    Wind Speed (mph)     9.17
    Name: 737, dtype: object dakar sn
    Processing Record 737of Set 1 | dakar
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dakar&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dakar&units=imperial
    738 City Name           antanifotsy
    Country Code                 mg
    Latitude                 -19.66
    Longitude                 47.32
    Temperature (F)           58.94
    Humidity (%)                 97
    Cloudiness (%)               88
    Wind Speed (mph)           7.67
    Name: 738, dtype: object antanifotsy mg
    Processing Record 738of Set 1 | antanifotsy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=antanifotsy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=antanifotsy&units=imperial
    739 City Name           san andres
    Country Code                co
    Latitude                 13.32
    Longitude               122.68
    Temperature (F)          79.46
    Humidity (%)               100
    Cloudiness (%)              48
    Wind Speed (mph)         13.04
    Name: 739, dtype: object san andres co
    Processing Record 739of Set 1 | san andres
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=san andres&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=san%20andres&units=imperial
    740 City Name           praia
    Country Code           cv
    Latitude           -20.25
    Longitude          -43.81
    Temperature (F)     74.98
    Humidity (%)           65
    Cloudiness (%)         20
    Wind Speed (mph)     3.36
    Name: 740, dtype: object praia cv
    Processing Record 740of Set 1 | praia
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=praia&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=praia&units=imperial
    741 City Name           orastioara de sus
    Country Code                       ro
    Latitude                        45.73
    Longitude                       23.17
    Temperature (F)                 34.55
    Humidity (%)                       99
    Cloudiness (%)                     64
    Wind Speed (mph)                 1.07
    Name: 741, dtype: object orastioara de sus ro
    Processing Record 741of Set 1 | orastioara de sus
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=orastioara de sus&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=orastioara%20de%20sus&units=imperial
    742 City Name           bilma
    Country Code           ne
    Latitude            18.69
    Longitude           12.92
    Temperature (F)     63.08
    Humidity (%)           25
    Cloudiness (%)          0
    Wind Speed (mph)     6.11
    Name: 742, dtype: object bilma ne
    Processing Record 742of Set 1 | bilma
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bilma&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bilma&units=imperial
    743 City Name           hobyo
    Country Code           so
    Latitude             5.35
    Longitude           48.53
    Temperature (F)     80.81
    Humidity (%)           78
    Cloudiness (%)         88
    Wind Speed (mph)    14.72
    Name: 743, dtype: object hobyo so
    Processing Record 743of Set 1 | hobyo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hobyo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hobyo&units=imperial
    744 City Name           tosya
    Country Code           tr
    Latitude            41.02
    Longitude           34.04
    Temperature (F)     47.33
    Humidity (%)           68
    Cloudiness (%)         44
    Wind Speed (mph)     6.67
    Name: 744, dtype: object tosya tr
    Processing Record 744of Set 1 | tosya
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tosya&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tosya&units=imperial
    745 City Name           bilibino
    Country Code              ru
    Latitude               68.06
    Longitude             166.44
    Temperature (F)        -3.88
    Humidity (%)              62
    Cloudiness (%)            68
    Wind Speed (mph)        5.88
    Name: 745, dtype: object bilibino ru
    Processing Record 745of Set 1 | bilibino
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bilibino&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bilibino&units=imperial
    746 City Name           corralillo
    Country Code                cu
    Latitude                 22.98
    Longitude               -80.59
    Temperature (F)          79.46
    Humidity (%)                70
    Cloudiness (%)              48
    Wind Speed (mph)          5.21
    Name: 746, dtype: object corralillo cu
    Processing Record 746of Set 1 | corralillo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=corralillo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=corralillo&units=imperial
    747 City Name           manikling
    Country Code               ph
    Latitude                 6.87
    Longitude              126.09
    Temperature (F)          78.8
    Humidity (%)               78
    Cloudiness (%)             75
    Wind Speed (mph)         9.17
    Name: 747, dtype: object manikling ph
    Processing Record 747of Set 1 | manikling
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=manikling&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=manikling&units=imperial
    748 City Name            ajra
    Country Code           in
    Latitude            16.12
    Longitude           74.21
    Temperature (F)     64.07
    Humidity (%)           95
    Cloudiness (%)         20
    Wind Speed (mph)     3.76
    Name: 748, dtype: object ajra in
    Processing Record 748of Set 1 | ajra
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ajra&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ajra&units=imperial
    749 City Name           rorvik
    Country Code            no
    Latitude             64.86
    Longitude            11.24
    Temperature (F)      35.36
    Humidity (%)           100
    Cloudiness (%)          92
    Wind Speed (mph)     22.32
    Name: 749, dtype: object rorvik no
    Processing Record 749of Set 1 | rorvik
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rorvik&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rorvik&units=imperial
    750 City Name           barcelos
    Country Code              br
    Latitude               41.53
    Longitude              -8.62
    Temperature (F)         51.8
    Humidity (%)              76
    Cloudiness (%)            20
    Wind Speed (mph)         4.7
    Name: 750, dtype: object barcelos br
    Processing Record 750of Set 1 | barcelos
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=barcelos&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=barcelos&units=imperial
    751 City Name           teguldet
    Country Code              ru
    Latitude               57.31
    Longitude              88.17
    Temperature (F)        15.02
    Humidity (%)              87
    Cloudiness (%)             0
    Wind Speed (mph)        4.65
    Name: 751, dtype: object teguldet ru
    Processing Record 751of Set 1 | teguldet
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=teguldet&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=teguldet&units=imperial
    752 City Name           cochrane
    Country Code              ca
    Latitude              -47.25
    Longitude             -72.57
    Temperature (F)        47.69
    Humidity (%)              54
    Cloudiness (%)             0
    Wind Speed (mph)        7.56
    Name: 752, dtype: object cochrane ca
    Processing Record 752of Set 1 | cochrane
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cochrane&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=cochrane&units=imperial
    753 City Name           tutoia
    Country Code            br
    Latitude             -2.76
    Longitude           -42.27
    Temperature (F)         80
    Humidity (%)            80
    Cloudiness (%)          36
    Wind Speed (mph)      8.34
    Name: 753, dtype: object tutoia br
    Processing Record 753of Set 1 | tutoia
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tutoia&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tutoia&units=imperial
    754 City Name           kununurra
    Country Code               au
    Latitude               -15.77
    Longitude              128.74
    Temperature (F)          80.6
    Humidity (%)               94
    Cloudiness (%)             75
    Wind Speed (mph)          4.7
    Name: 754, dtype: object kununurra au
    Processing Record 754of Set 1 | kununurra
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kununurra&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kununurra&units=imperial
    755 City Name           svobodnyy
    Country Code               ru
    Latitude                53.85
    Longitude               40.53
    Temperature (F)         -5.95
    Humidity (%)               57
    Cloudiness (%)              0
    Wind Speed (mph)         7.23
    Name: 755, dtype: object svobodnyy ru
    Processing Record 755of Set 1 | svobodnyy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=svobodnyy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=svobodnyy&units=imperial
    757 City Name           reitz
    Country Code           za
    Latitude            -27.8
    Longitude           28.43
    Temperature (F)     53.81
    Humidity (%)           98
    Cloudiness (%)         88
    Wind Speed (mph)     2.98
    Name: 757, dtype: object reitz za
    Processing Record 757of Set 1 | reitz
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=reitz&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=reitz&units=imperial
    758 City Name           norton shores
    Country Code                   us
    Latitude                    43.17
    Longitude                  -86.26
    Temperature (F)             42.04
    Humidity (%)                   64
    Cloudiness (%)                 75
    Wind Speed (mph)              4.7
    Name: 758, dtype: object norton shores us
    Processing Record 758of Set 1 | norton shores
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=norton shores&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=norton%20shores&units=imperial
    760 City Name           nhamunda
    Country Code              br
    Latitude               -2.19
    Longitude             -56.71
    Temperature (F)        76.04
    Humidity (%)              96
    Cloudiness (%)            24
    Wind Speed (mph)        1.19
    Name: 760, dtype: object nhamunda br
    Processing Record 760of Set 1 | nhamunda
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nhamunda&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nhamunda&units=imperial
    761 City Name           scalea
    Country Code            it
    Latitude             39.82
    Longitude            15.79
    Temperature (F)      52.82
    Humidity (%)           100
    Cloudiness (%)          92
    Wind Speed (mph)      8.79
    Name: 761, dtype: object scalea it
    Processing Record 761of Set 1 | scalea
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=scalea&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=scalea&units=imperial
    762 City Name           taywarah
    Country Code              af
    Latitude               33.35
    Longitude              64.42
    Temperature (F)        36.71
    Humidity (%)              97
    Cloudiness (%)            80
    Wind Speed (mph)        2.19
    Name: 762, dtype: object taywarah af
    Processing Record 762of Set 1 | taywarah
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=taywarah&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=taywarah&units=imperial
    763 City Name           chisinau
    Country Code              md
    Latitude               47.01
    Longitude              28.86
    Temperature (F)         17.6
    Humidity (%)              85
    Cloudiness (%)            90
    Wind Speed (mph)        6.93
    Name: 763, dtype: object chisinau md
    Processing Record 763of Set 1 | chisinau
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=chisinau&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=chisinau&units=imperial
    764 City Name           kidal
    Country Code           ml
    Latitude            18.44
    Longitude            1.41
    Temperature (F)     67.67
    Humidity (%)           19
    Cloudiness (%)         12
    Wind Speed (mph)      9.8
    Name: 764, dtype: object kidal ml
    Processing Record 764of Set 1 | kidal
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kidal&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kidal&units=imperial
    765 City Name           hervey bay
    Country Code                au
    Latitude                 -25.3
    Longitude               152.85
    Temperature (F)           78.2
    Humidity (%)                98
    Cloudiness (%)              92
    Wind Speed (mph)          6.89
    Name: 765, dtype: object hervey bay au
    Processing Record 765of Set 1 | hervey bay
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hervey bay&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hervey%20bay&units=imperial
    766 City Name           ocampo
    Country Code            mx
    Latitude             13.56
    Longitude           123.37
    Temperature (F)      79.46
    Humidity (%)            95
    Cloudiness (%)          48
    Wind Speed (mph)      8.68
    Name: 766, dtype: object ocampo mx
    Processing Record 766of Set 1 | ocampo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ocampo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ocampo&units=imperial
    767 City Name           semey
    Country Code           kz
    Latitude            50.41
    Longitude           80.25
    Temperature (F)     13.58
    Humidity (%)           68
    Cloudiness (%)          0
    Wind Speed (mph)     6.67
    Name: 767, dtype: object semey kz
    Processing Record 767of Set 1 | semey
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=semey&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=semey&units=imperial
    768 City Name           aguai
    Country Code           br
    Latitude             46.3
    Longitude            11.4
    Temperature (F)        23
    Humidity (%)           92
    Cloudiness (%)         90
    Wind Speed (mph)    17.22
    Name: 768, dtype: object aguai br
    Processing Record 768of Set 1 | aguai
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=aguai&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=aguai&units=imperial
    769 City Name           ayagoz
    Country Code            kz
    Latitude             47.96
    Longitude            80.43
    Temperature (F)      12.77
    Humidity (%)            67
    Cloudiness (%)          68
    Wind Speed (mph)      2.98
    Name: 769, dtype: object ayagoz kz
    Processing Record 769of Set 1 | ayagoz
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ayagoz&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ayagoz&units=imperial
    771 City Name           adrar
    Country Code           dz
    Latitude            27.87
    Longitude           -0.29
    Temperature (F)      62.6
    Humidity (%)           13
    Cloudiness (%)          0
    Wind Speed (mph)     6.93
    Name: 771, dtype: object adrar dz
    Processing Record 771of Set 1 | adrar
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=adrar&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=adrar&units=imperial
    772 City Name           puerto escondido
    Country Code                      mx
    Latitude                       15.86
    Longitude                     -97.07
    Temperature (F)                 82.4
    Humidity (%)                      74
    Cloudiness (%)                    40
    Wind Speed (mph)                5.82
    Name: 772, dtype: object puerto escondido mx
    Processing Record 772of Set 1 | puerto escondido
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=puerto escondido&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=puerto%20escondido&units=imperial
    773 City Name           kanigoro
    Country Code              id
    Latitude               -8.13
    Longitude             112.22
    Temperature (F)        80.18
    Humidity (%)              99
    Cloudiness (%)             0
    Wind Speed (mph)        4.43
    Name: 773, dtype: object kanigoro id
    Processing Record 773of Set 1 | kanigoro
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kanigoro&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kanigoro&units=imperial
    774 City Name           hailar
    Country Code            cn
    Latitude              49.2
    Longitude            119.7
    Temperature (F)       5.84
    Humidity (%)            54
    Cloudiness (%)           0
    Wind Speed (mph)       3.2
    Name: 774, dtype: object hailar cn
    Processing Record 774of Set 1 | hailar
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hailar&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=hailar&units=imperial
    775 City Name            brae
    Country Code           gb
    Latitude             60.4
    Longitude           -1.35
    Temperature (F)     32.76
    Humidity (%)           80
    Cloudiness (%)         68
    Wind Speed (mph)      4.7
    Name: 775, dtype: object brae gb
    Processing Record 775of Set 1 | brae
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=brae&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=brae&units=imperial
    776 City Name           wanning
    Country Code             cn
    Latitude              48.64
    Longitude             13.53
    Temperature (F)        24.8
    Humidity (%)             62
    Cloudiness (%)           90
    Wind Speed (mph)      12.75
    Name: 776, dtype: object wanning cn
    Processing Record 776of Set 1 | wanning
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=wanning&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=wanning&units=imperial
    777 City Name           djibo
    Country Code           bf
    Latitude             14.1
    Longitude           -1.63
    Temperature (F)     72.08
    Humidity (%)           66
    Cloudiness (%)          0
    Wind Speed (mph)     2.42
    Name: 777, dtype: object djibo bf
    Processing Record 777of Set 1 | djibo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=djibo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=djibo&units=imperial
    778 City Name           bhadrachalam
    Country Code                  in
    Latitude                   17.67
    Longitude                  80.89
    Temperature (F)            65.96
    Humidity (%)                  88
    Cloudiness (%)                20
    Wind Speed (mph)            2.75
    Name: 778, dtype: object bhadrachalam in
    Processing Record 778of Set 1 | bhadrachalam
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bhadrachalam&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bhadrachalam&units=imperial
    779 City Name           contamana
    Country Code               pe
    Latitude                -7.35
    Longitude              -75.01
    Temperature (F)          82.7
    Humidity (%)               78
    Cloudiness (%)             44
    Wind Speed (mph)         1.19
    Name: 779, dtype: object contamana pe
    Processing Record 779of Set 1 | contamana
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=contamana&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=contamana&units=imperial
    780 City Name           jasdan
    Country Code            in
    Latitude             22.04
    Longitude             71.2
    Temperature (F)       71.6
    Humidity (%)            60
    Cloudiness (%)          20
    Wind Speed (mph)      6.93
    Name: 780, dtype: object jasdan in
    Processing Record 780of Set 1 | jasdan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=jasdan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=jasdan&units=imperial
    782 City Name           tsaratanana
    Country Code                 mg
    Latitude                  -16.8
    Longitude                 47.65
    Temperature (F)           64.88
    Humidity (%)                 95
    Cloudiness (%)               24
    Wind Speed (mph)           2.75
    Name: 782, dtype: object tsaratanana mg
    Processing Record 782of Set 1 | tsaratanana
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tsaratanana&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tsaratanana&units=imperial
    783 City Name           george town
    Country Code                 ky
    Latitude                   5.42
    Longitude                100.33
    Temperature (F)           78.84
    Humidity (%)                 94
    Cloudiness (%)               75
    Wind Speed (mph)           3.36
    Name: 783, dtype: object george town ky
    Processing Record 783of Set 1 | george town
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=george town&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=george%20town&units=imperial
    784 City Name           benguela
    Country Code              ao
    Latitude              -12.58
    Longitude               13.4
    Temperature (F)        75.05
    Humidity (%)             100
    Cloudiness (%)            92
    Wind Speed (mph)        2.75
    Name: 784, dtype: object benguela ao
    Processing Record 784of Set 1 | benguela
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=benguela&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=benguela&units=imperial
    785 City Name           boyolangu
    Country Code               id
    Latitude                -8.09
    Longitude               111.9
    Temperature (F)         72.44
    Humidity (%)               97
    Cloudiness (%)             20
    Wind Speed (mph)         4.65
    Name: 785, dtype: object boyolangu id
    Processing Record 785of Set 1 | boyolangu
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=boyolangu&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=boyolangu&units=imperial
    787 City Name            ukiah
    Country Code            us
    Latitude             39.15
    Longitude          -123.21
    Temperature (F)       55.4
    Humidity (%)            47
    Cloudiness (%)           1
    Wind Speed (mph)       4.7
    Name: 787, dtype: object ukiah us
    Processing Record 787of Set 1 | ukiah
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ukiah&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ukiah&units=imperial
    788 City Name           canala
    Country Code            nc
    Latitude            -21.52
    Longitude           165.96
    Temperature (F)       84.2
    Humidity (%)            51
    Cloudiness (%)          20
    Wind Speed (mph)     11.41
    Name: 788, dtype: object canala nc
    Processing Record 788of Set 1 | canala
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=canala&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=canala&units=imperial
    789 City Name           inhambane
    Country Code               mz
    Latitude               -23.87
    Longitude               35.38
    Temperature (F)         74.33
    Humidity (%)              100
    Cloudiness (%)              0
    Wind Speed (mph)         9.69
    Name: 789, dtype: object inhambane mz
    Processing Record 789of Set 1 | inhambane
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=inhambane&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=inhambane&units=imperial
    790 City Name           rocha
    Country Code           uy
    Latitude           -34.48
    Longitude          -54.34
    Temperature (F)     61.82
    Humidity (%)           90
    Cloudiness (%)         24
    Wind Speed (mph)    21.54
    Name: 790, dtype: object rocha uy
    Processing Record 790of Set 1 | rocha
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rocha&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=rocha&units=imperial
    791 City Name           bagepalli
    Country Code               in
    Latitude                13.78
    Longitude                77.8
    Temperature (F)          64.4
    Humidity (%)              100
    Cloudiness (%)             20
    Wind Speed (mph)         2.24
    Name: 791, dtype: object bagepalli in
    Processing Record 791of Set 1 | bagepalli
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bagepalli&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=bagepalli&units=imperial
    793 City Name           sobolevo
    Country Code              ru
    Latitude               54.43
    Longitude               31.9
    Temperature (F)         8.09
    Humidity (%)              73
    Cloudiness (%)             0
    Wind Speed (mph)       11.81
    Name: 793, dtype: object sobolevo ru
    Processing Record 793of Set 1 | sobolevo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sobolevo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sobolevo&units=imperial
    794 City Name           young
    Country Code           au
    Latitude            -32.7
    Longitude          -57.63
    Temperature (F)     61.28
    Humidity (%)           84
    Cloudiness (%)         48
    Wind Speed (mph)    15.39
    Name: 794, dtype: object young au
    Processing Record 794of Set 1 | young
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=young&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=young&units=imperial
    795 City Name           honiara
    Country Code             sb
    Latitude              -9.43
    Longitude            159.96
    Temperature (F)          86
    Humidity (%)             74
    Cloudiness (%)           40
    Wind Speed (mph)      13.87
    Name: 795, dtype: object honiara sb
    Processing Record 795of Set 1 | honiara
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=honiara&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=honiara&units=imperial
    796 City Name           saiha
    Country Code           in
    Latitude            22.49
    Longitude           92.98
    Temperature (F)     57.23
    Humidity (%)           91
    Cloudiness (%)          0
    Wind Speed (mph)     1.97
    Name: 796, dtype: object saiha in
    Processing Record 796of Set 1 | saiha
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saiha&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saiha&units=imperial
    797 City Name           wamba
    Country Code           cd
    Latitude             2.14
    Longitude           27.99
    Temperature (F)      67.4
    Humidity (%)           98
    Cloudiness (%)         92
    Wind Speed (mph)     2.64
    Name: 797, dtype: object wamba cd
    Processing Record 797of Set 1 | wamba
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=wamba&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=wamba&units=imperial
    798 City Name           ulaanbaatar
    Country Code                 mn
    Latitude                  47.92
    Longitude                106.92
    Temperature (F)            26.6
    Humidity (%)                 73
    Cloudiness (%)               20
    Wind Speed (mph)           2.24
    Name: 798, dtype: object ulaanbaatar mn
    Processing Record 798of Set 1 | ulaanbaatar
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ulaanbaatar&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ulaanbaatar&units=imperial
    799 City Name           grindavik
    Country Code               is
    Latitude                63.84
    Longitude              -22.43
    Temperature (F)          42.8
    Humidity (%)               93
    Cloudiness (%)             75
    Wind Speed (mph)        13.87
    Name: 799, dtype: object grindavik is
    Processing Record 799of Set 1 | grindavik
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=grindavik&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=grindavik&units=imperial
    800 City Name           porto tolle
    Country Code                 it
    Latitude                  44.95
    Longitude                 12.32
    Temperature (F)            35.6
    Humidity (%)                 93
    Cloudiness (%)               75
    Wind Speed (mph)           26.4
    Name: 800, dtype: object porto tolle it
    Processing Record 800of Set 1 | porto tolle
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=porto tolle&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=porto%20tolle&units=imperial
    801 City Name           phan thiet
    Country Code                vn
    Latitude                 10.93
    Longitude                108.1
    Temperature (F)          77.21
    Humidity (%)                97
    Cloudiness (%)              20
    Wind Speed (mph)          8.68
    Name: 801, dtype: object phan thiet vn
    Processing Record 801of Set 1 | phan thiet
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=phan thiet&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=phan%20thiet&units=imperial
    802 City Name           coahuayana
    Country Code                mx
    Latitude                 18.62
    Longitude              -100.35
    Temperature (F)           84.5
    Humidity (%)                16
    Cloudiness (%)              80
    Wind Speed (mph)          2.19
    Name: 802, dtype: object coahuayana mx
    Processing Record 802of Set 1 | coahuayana
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=coahuayana&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=coahuayana&units=imperial
    803 City Name           leshukonskoye
    Country Code                   ru
    Latitude                     64.9
    Longitude                   45.76
    Temperature (F)              9.35
    Humidity (%)                   82
    Cloudiness (%)                 88
    Wind Speed (mph)             7.23
    Name: 803, dtype: object leshukonskoye ru
    Processing Record 803of Set 1 | leshukonskoye
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=leshukonskoye&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=leshukonskoye&units=imperial
    804 City Name             fare
    Country Code            pf
    Latitude             -16.7
    Longitude          -151.02
    Temperature (F)      82.34
    Humidity (%)            99
    Cloudiness (%)          80
    Wind Speed (mph)     15.84
    Name: 804, dtype: object fare pf
    Processing Record 804of Set 1 | fare
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fare&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fare&units=imperial
    805 City Name           sao gabriel
    Country Code                 br
    Latitude                 -30.34
    Longitude                -54.32
    Temperature (F)           76.76
    Humidity (%)                 88
    Cloudiness (%)               68
    Wind Speed (mph)           13.6
    Name: 805, dtype: object sao gabriel br
    Processing Record 805of Set 1 | sao gabriel
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sao gabriel&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sao%20gabriel&units=imperial
    807 City Name            poum
    Country Code           nc
    Latitude            41.28
    Longitude           20.71
    Temperature (F)     37.61
    Humidity (%)           73
    Cloudiness (%)         68
    Wind Speed (mph)     1.74
    Name: 807, dtype: object poum nc
    Processing Record 807of Set 1 | poum
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=poum&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=poum&units=imperial
    808 City Name           samarkand
    Country Code               uz
    Latitude                39.65
    Longitude               66.98
    Temperature (F)            50
    Humidity (%)               71
    Cloudiness (%)             48
    Wind Speed (mph)          3.2
    Name: 808, dtype: object samarkand uz
    Processing Record 808of Set 1 | samarkand
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=samarkand&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=samarkand&units=imperial
    809 City Name             tual
    Country Code            id
    Latitude             -5.67
    Longitude           132.75
    Temperature (F)       84.5
    Humidity (%)            98
    Cloudiness (%)           0
    Wind Speed (mph)      6.78
    Name: 809, dtype: object tual id
    Processing Record 809of Set 1 | tual
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tual&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tual&units=imperial
    811 City Name           santa rosalia
    Country Code                   mx
    Latitude                    38.07
    Longitude                   13.27
    Temperature (F)              53.6
    Humidity (%)                   76
    Cloudiness (%)                 20
    Wind Speed (mph)             9.17
    Name: 811, dtype: object santa rosalia mx
    Processing Record 811of Set 1 | santa rosalia
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=santa rosalia&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=santa%20rosalia&units=imperial
    812 City Name           viedma
    Country Code            ar
    Latitude            -40.81
    Longitude           -62.99
    Temperature (F)      58.76
    Humidity (%)            46
    Cloudiness (%)           0
    Wind Speed (mph)      8.12
    Name: 812, dtype: object viedma ar
    Processing Record 812of Set 1 | viedma
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=viedma&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=viedma&units=imperial
    813 City Name           umm kaddadah
    Country Code                  sd
    Latitude                    13.6
    Longitude                  26.69
    Temperature (F)            58.13
    Humidity (%)                  25
    Cloudiness (%)                 0
    Wind Speed (mph)            4.43
    Name: 813, dtype: object umm kaddadah sd
    Processing Record 813of Set 1 | umm kaddadah
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=umm kaddadah&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=umm%20kaddadah&units=imperial
    814 City Name            luau
    Country Code           ao
    Latitude            -10.7
    Longitude           22.23
    Temperature (F)     65.15
    Humidity (%)           97
    Cloudiness (%)         64
    Wind Speed (mph)     2.42
    Name: 814, dtype: object luau ao
    Processing Record 814of Set 1 | luau
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=luau&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=luau&units=imperial
    815 City Name           petrovskiy
    Country Code                ru
    Latitude                 50.75
    Longitude                41.98
    Temperature (F)           3.95
    Humidity (%)                81
    Cloudiness (%)              56
    Wind Speed (mph)             7
    Name: 815, dtype: object petrovskiy ru
    Processing Record 815of Set 1 | petrovskiy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=petrovskiy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=petrovskiy&units=imperial
    817 City Name           haverfordwest
    Country Code                   gb
    Latitude                     51.8
    Longitude                   -4.97
    Temperature (F)                32
    Humidity (%)                  100
    Cloudiness (%)                 88
    Wind Speed (mph)            14.99
    Name: 817, dtype: object haverfordwest gb
    Processing Record 817of Set 1 | haverfordwest
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=haverfordwest&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=haverfordwest&units=imperial
    818 City Name           siddapur
    Country Code              in
    Latitude               13.66
    Longitude              74.91
    Temperature (F)        70.55
    Humidity (%)              97
    Cloudiness (%)            56
    Wind Speed (mph)         2.3
    Name: 818, dtype: object siddapur in
    Processing Record 818of Set 1 | siddapur
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=siddapur&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=siddapur&units=imperial
    819 City Name           sao jose da coroa grande
    Country Code                              br
    Latitude                                -8.9
    Longitude                             -35.15
    Temperature (F)                         75.5
    Humidity (%)                              95
    Cloudiness (%)                            44
    Wind Speed (mph)                        7.23
    Name: 819, dtype: object sao jose da coroa grande br
    Processing Record 819of Set 1 | sao jose da coroa grande
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sao jose da coroa grande&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sao%20jose%20da%20coroa%20grande&units=imperial
    820 City Name           channel-port aux basques
    Country Code                              ca
    Latitude                               47.57
    Longitude                             -59.14
    Temperature (F)                        27.98
    Humidity (%)                             100
    Cloudiness (%)                            32
    Wind Speed (mph)                       33.62
    Name: 820, dtype: object channel-port aux basques ca
    Processing Record 820of Set 1 | channel-port aux basques
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=channel-port aux basques&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=channel-port%20aux%20basques&units=imperial
    821 City Name           diffa
    Country Code           ne
    Latitude            13.32
    Longitude           12.61
    Temperature (F)     65.33
    Humidity (%)           34
    Cloudiness (%)          0
    Wind Speed (mph)     4.54
    Name: 821, dtype: object diffa ne
    Processing Record 821of Set 1 | diffa
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=diffa&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=diffa&units=imperial
    822 City Name            gasa
    Country Code           bt
    Latitude            27.91
    Longitude           89.73
    Temperature (F)    -24.49
    Humidity (%)           20
    Cloudiness (%)          0
    Wind Speed (mph)     2.08
    Name: 822, dtype: object gasa bt
    Processing Record 822of Set 1 | gasa
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gasa&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gasa&units=imperial
    823 City Name           qostanay
    Country Code              kz
    Latitude               53.17
    Longitude              63.58
    Temperature (F)         33.8
    Humidity (%)             100
    Cloudiness (%)            90
    Wind Speed (mph)       26.84
    Name: 823, dtype: object qostanay kz
    Processing Record 823of Set 1 | qostanay
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=qostanay&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=qostanay&units=imperial
    824 City Name           nioro
    Country Code           ml
    Latitude            13.79
    Longitude          -15.05
    Temperature (F)     76.76
    Humidity (%)           34
    Cloudiness (%)          0
    Wind Speed (mph)     9.69
    Name: 824, dtype: object nioro ml
    Processing Record 824of Set 1 | nioro
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nioro&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=nioro&units=imperial
    825 City Name           ambilobe
    Country Code              mg
    Latitude              -13.19
    Longitude              49.05
    Temperature (F)        72.98
    Humidity (%)              98
    Cloudiness (%)            92
    Wind Speed (mph)        5.88
    Name: 825, dtype: object ambilobe mg
    Processing Record 825of Set 1 | ambilobe
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ambilobe&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ambilobe&units=imperial
    826 City Name           lachute
    Country Code             ca
    Latitude              45.66
    Longitude            -74.34
    Temperature (F)       18.43
    Humidity (%)             28
    Cloudiness (%)            5
    Wind Speed (mph)       5.82
    Name: 826, dtype: object lachute ca
    Processing Record 826of Set 1 | lachute
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lachute&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lachute&units=imperial
    827 City Name           fernie
    Country Code            ca
    Latitude              49.5
    Longitude          -115.06
    Temperature (F)      36.64
    Humidity (%)            64
    Cloudiness (%)          90
    Wind Speed (mph)     10.29
    Name: 827, dtype: object fernie ca
    Processing Record 827of Set 1 | fernie
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fernie&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=fernie&units=imperial
    828 City Name             olga
    Country Code            ru
    Latitude             34.11
    Longitude          -118.17
    Temperature (F)      60.03
    Humidity (%)            51
    Cloudiness (%)           1
    Wind Speed (mph)      6.93
    Name: 828, dtype: object olga ru
    Processing Record 828of Set 1 | olga
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=olga&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=olga&units=imperial
    829 City Name           troitsko-pechorsk
    Country Code                       ru
    Latitude                        62.71
    Longitude                       56.19
    Temperature (F)                -12.43
    Humidity (%)                       77
    Cloudiness (%)                      0
    Wind Speed (mph)                 2.98
    Name: 829, dtype: object troitsko-pechorsk ru
    Processing Record 829of Set 1 | troitsko-pechorsk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=troitsko-pechorsk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=troitsko-pechorsk&units=imperial
    830 City Name           redlands
    Country Code              us
    Latitude               34.06
    Longitude            -117.19
    Temperature (F)         62.2
    Humidity (%)              31
    Cloudiness (%)            75
    Wind Speed (mph)        8.05
    Name: 830, dtype: object redlands us
    Processing Record 830of Set 1 | redlands
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=redlands&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=redlands&units=imperial
    831 City Name           igarka
    Country Code            ru
    Latitude             67.47
    Longitude            86.57
    Temperature (F)      -1.09
    Humidity (%)            68
    Cloudiness (%)          68
    Wind Speed (mph)      7.78
    Name: 831, dtype: object igarka ru
    Processing Record 831of Set 1 | igarka
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=igarka&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=igarka&units=imperial
    832 City Name           san javier
    Country Code                bo
    Latitude                 37.81
    Longitude                -0.83
    Temperature (F)           48.2
    Humidity (%)                61
    Cloudiness (%)               0
    Wind Speed (mph)          3.36
    Name: 832, dtype: object san javier bo
    Processing Record 832of Set 1 | san javier
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=san javier&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=san%20javier&units=imperial
    833 City Name           calama
    Country Code            cl
    Latitude            -22.46
    Longitude           -68.93
    Temperature (F)       62.6
    Humidity (%)            36
    Cloudiness (%)           0
    Wind Speed (mph)     19.46
    Name: 833, dtype: object calama cl
    Processing Record 833of Set 1 | calama
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=calama&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=calama&units=imperial
    834 City Name           gboko
    Country Code           ng
    Latitude             7.32
    Longitude               9
    Temperature (F)     83.06
    Humidity (%)           62
    Cloudiness (%)         36
    Wind Speed (mph)    12.03
    Name: 834, dtype: object gboko ng
    Processing Record 834of Set 1 | gboko
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gboko&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gboko&units=imperial
    835 City Name           shache
    Country Code            cn
    Latitude             38.42
    Longitude            77.24
    Temperature (F)      33.56
    Humidity (%)            74
    Cloudiness (%)           0
    Wind Speed (mph)       3.2
    Name: 835, dtype: object shache cn
    Processing Record 835of Set 1 | shache
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=shache&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=shache&units=imperial
    836 City Name           kargasok
    Country Code              ru
    Latitude               59.06
    Longitude              80.87
    Temperature (F)        30.23
    Humidity (%)              83
    Cloudiness (%)            32
    Wind Speed (mph)         8.9
    Name: 836, dtype: object kargasok ru
    Processing Record 836of Set 1 | kargasok
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kargasok&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kargasok&units=imperial
    837 City Name           banda aceh
    Country Code                id
    Latitude                  5.56
    Longitude                95.32
    Temperature (F)          73.07
    Humidity (%)                99
    Cloudiness (%)               8
    Wind Speed (mph)          1.97
    Name: 837, dtype: object banda aceh id
    Processing Record 837of Set 1 | banda aceh
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=banda aceh&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=banda%20aceh&units=imperial
    838 City Name           denpasar
    Country Code              id
    Latitude               -8.65
    Longitude             115.22
    Temperature (F)        74.33
    Humidity (%)             100
    Cloudiness (%)            24
    Wind Speed (mph)        1.07
    Name: 838, dtype: object denpasar id
    Processing Record 838of Set 1 | denpasar
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=denpasar&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=denpasar&units=imperial
    839 City Name           flin flon
    Country Code               ca
    Latitude                54.77
    Longitude             -101.88
    Temperature (F)          15.8
    Humidity (%)               36
    Cloudiness (%)             90
    Wind Speed (mph)         8.05
    Name: 839, dtype: object flin flon ca
    Processing Record 839of Set 1 | flin flon
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=flin flon&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=flin%20flon&units=imperial
    840 City Name           yinchuan
    Country Code              cn
    Latitude               38.48
    Longitude             106.21
    Temperature (F)        36.17
    Humidity (%)              83
    Cloudiness (%)            36
    Wind Speed (mph)         3.2
    Name: 840, dtype: object yinchuan cn
    Processing Record 840of Set 1 | yinchuan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yinchuan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=yinchuan&units=imperial
    841 City Name           papetoai
    Country Code              pf
    Latitude               -17.5
    Longitude            -149.87
    Temperature (F)         82.4
    Humidity (%)              65
    Cloudiness (%)            75
    Wind Speed (mph)        5.82
    Name: 841, dtype: object papetoai pf
    Processing Record 841of Set 1 | papetoai
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=papetoai&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=papetoai&units=imperial
    842 City Name           omsukchan
    Country Code               ru
    Latitude                62.53
    Longitude               155.8
    Temperature (F)        -11.08
    Humidity (%)               67
    Cloudiness (%)             48
    Wind Speed (mph)         5.66
    Name: 842, dtype: object omsukchan ru
    Processing Record 842of Set 1 | omsukchan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=omsukchan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=omsukchan&units=imperial
    843 City Name           trairi
    Country Code            br
    Latitude             -3.28
    Longitude           -39.27
    Temperature (F)      83.51
    Humidity (%)            99
    Cloudiness (%)          24
    Wind Speed (mph)     16.06
    Name: 843, dtype: object trairi br
    Processing Record 843of Set 1 | trairi
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=trairi&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=trairi&units=imperial
    844 City Name           grand-lahou
    Country Code                 ci
    Latitude                   5.24
    Longitude                    -5
    Temperature (F)           77.93
    Humidity (%)                 88
    Cloudiness (%)               32
    Wind Speed (mph)           5.99
    Name: 844, dtype: object grand-lahou ci
    Processing Record 844of Set 1 | grand-lahou
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=grand-lahou&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=grand-lahou&units=imperial
    845 City Name           copiapo
    Country Code             cl
    Latitude             -27.37
    Longitude            -70.33
    Temperature (F)          59
    Humidity (%)             82
    Cloudiness (%)           75
    Wind Speed (mph)       5.82
    Name: 845, dtype: object copiapo cl
    Processing Record 845of Set 1 | copiapo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=copiapo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=copiapo&units=imperial
    846 City Name           tashara
    Country Code             ru
    Latitude              55.52
    Longitude             83.51
    Temperature (F)       12.68
    Humidity (%)             85
    Cloudiness (%)           20
    Wind Speed (mph)          7
    Name: 846, dtype: object tashara ru
    Processing Record 846of Set 1 | tashara
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tashara&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tashara&units=imperial
    847 City Name             uri
    Country Code           in
    Latitude            40.64
    Longitude            8.49
    Temperature (F)        50
    Humidity (%)           81
    Cloudiness (%)         40
    Wind Speed (mph)     8.05
    Name: 847, dtype: object uri in
    Processing Record 847of Set 1 | uri
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=uri&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=uri&units=imperial
    848 City Name           karczew
    Country Code             pl
    Latitude              52.08
    Longitude             21.25
    Temperature (F)          23
    Humidity (%)             53
    Cloudiness (%)            0
    Wind Speed (mph)      11.41
    Name: 848, dtype: object karczew pl
    Processing Record 848of Set 1 | karczew
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=karczew&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=karczew&units=imperial
    849 City Name           dafeng
    Country Code            cn
    Latitude              33.2
    Longitude           120.46
    Temperature (F)      45.26
    Humidity (%)           100
    Cloudiness (%)          92
    Wind Speed (mph)     14.94
    Name: 849, dtype: object dafeng cn
    Processing Record 849of Set 1 | dafeng
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dafeng&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=dafeng&units=imperial
    851 City Name           sangar
    Country Code            ru
    Latitude             63.92
    Longitude           127.47
    Temperature (F)     -19.09
    Humidity (%)            55
    Cloudiness (%)           0
    Wind Speed (mph)      3.65
    Name: 851, dtype: object sangar ru
    Processing Record 851of Set 1 | sangar
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sangar&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sangar&units=imperial
    853 City Name           gusau
    Country Code           ng
    Latitude            12.17
    Longitude            6.66
    Temperature (F)      66.5
    Humidity (%)           45
    Cloudiness (%)          0
    Wind Speed (mph)     2.64
    Name: 853, dtype: object gusau ng
    Processing Record 853of Set 1 | gusau
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gusau&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gusau&units=imperial
    854 City Name           turayf
    Country Code            sa
    Latitude             31.68
    Longitude            38.65
    Temperature (F)       55.4
    Humidity (%)            35
    Cloudiness (%)           0
    Wind Speed (mph)     13.87
    Name: 854, dtype: object turayf sa
    Processing Record 854of Set 1 | turayf
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=turayf&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=turayf&units=imperial
    855 City Name             zeya
    Country Code            ru
    Latitude             53.74
    Longitude           127.27
    Temperature (F)       5.75
    Humidity (%)            47
    Cloudiness (%)           0
    Wind Speed (mph)      7.56
    Name: 855, dtype: object zeya ru
    Processing Record 855of Set 1 | zeya
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=zeya&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=zeya&units=imperial
    856 City Name           madang
    Country Code            pg
    Latitude             -5.21
    Longitude           145.81
    Temperature (F)      79.19
    Humidity (%)            98
    Cloudiness (%)          20
    Wind Speed (mph)      3.87
    Name: 856, dtype: object madang pg
    Processing Record 856of Set 1 | madang
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=madang&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=madang&units=imperial
    858 City Name           talcahuano
    Country Code                cl
    Latitude                -36.72
    Longitude               -73.12
    Temperature (F)           60.8
    Humidity (%)                77
    Cloudiness (%)               0
    Wind Speed (mph)         12.75
    Name: 858, dtype: object talcahuano cl
    Processing Record 858of Set 1 | talcahuano
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=talcahuano&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=talcahuano&units=imperial
    859 City Name           porto seguro
    Country Code                  br
    Latitude                  -16.44
    Longitude                 -39.06
    Temperature (F)             78.8
    Humidity (%)                  88
    Cloudiness (%)                40
    Wind Speed (mph)            5.82
    Name: 859, dtype: object porto seguro br
    Processing Record 859of Set 1 | porto seguro
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=porto seguro&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=porto%20seguro&units=imperial
    860 City Name           gazimurskiy zavod
    Country Code                       ru
    Latitude                        51.55
    Longitude                      118.33
    Temperature (F)                  12.5
    Humidity (%)                       56
    Cloudiness (%)                     64
    Wind Speed (mph)                 2.64
    Name: 860, dtype: object gazimurskiy zavod ru
    Processing Record 860of Set 1 | gazimurskiy zavod
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gazimurskiy zavod&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=gazimurskiy%20zavod&units=imperial
    861 City Name           wattegama
    Country Code               lk
    Latitude                 7.35
    Longitude               80.68
    Temperature (F)         68.48
    Humidity (%)               95
    Cloudiness (%)             36
    Wind Speed (mph)         2.42
    Name: 861, dtype: object wattegama lk
    Processing Record 861of Set 1 | wattegama
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=wattegama&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=wattegama&units=imperial
    862 City Name           kudahuvadhoo
    Country Code                  mv
    Latitude                    2.67
    Longitude                  72.89
    Temperature (F)            84.23
    Humidity (%)                 100
    Cloudiness (%)                 0
    Wind Speed (mph)            8.12
    Name: 862, dtype: object kudahuvadhoo mv
    Processing Record 862of Set 1 | kudahuvadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kudahuvadhoo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kudahuvadhoo&units=imperial
    863 City Name            doha
    Country Code           qa
    Latitude            25.29
    Longitude           51.53
    Temperature (F)        68
    Humidity (%)           52
    Cloudiness (%)          0
    Wind Speed (mph)      4.7
    Name: 863, dtype: object doha qa
    Processing Record 863of Set 1 | doha
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=doha&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=doha&units=imperial
    864 City Name           desesti
    Country Code             ro
    Latitude              47.77
    Longitude             23.85
    Temperature (F)       19.97
    Humidity (%)             81
    Cloudiness (%)           92
    Wind Speed (mph)       2.75
    Name: 864, dtype: object desesti ro
    Processing Record 864of Set 1 | desesti
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=desesti&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=desesti&units=imperial
    865 City Name           tessalit
    Country Code              ml
    Latitude                20.2
    Longitude               1.01
    Temperature (F)        60.47
    Humidity (%)              21
    Cloudiness (%)            12
    Wind Speed (mph)        8.34
    Name: 865, dtype: object tessalit ml
    Processing Record 865of Set 1 | tessalit
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tessalit&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=tessalit&units=imperial
    866 City Name           aasiaat
    Country Code             gl
    Latitude              68.71
    Longitude            -52.87
    Temperature (F)       20.33
    Humidity (%)             78
    Cloudiness (%)            0
    Wind Speed (mph)       3.76
    Name: 866, dtype: object aasiaat gl
    Processing Record 866of Set 1 | aasiaat
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=aasiaat&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=aasiaat&units=imperial
    867 City Name           villa san giovanni
    Country Code                        it
    Latitude                         45.52
    Longitude                         9.23
    Temperature (F)                   42.8
    Humidity (%)                       100
    Cloudiness (%)                      75
    Wind Speed (mph)                   4.7
    Name: 867, dtype: object villa san giovanni it
    Processing Record 867of Set 1 | villa san giovanni
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=villa san giovanni&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=villa%20san%20giovanni&units=imperial
    868 City Name           buenos aires
    Country Code                  cr
    Latitude                  -34.61
    Longitude                 -58.44
    Temperature (F)            63.81
    Humidity (%)                  52
    Cloudiness (%)                 0
    Wind Speed (mph)            6.93
    Name: 868, dtype: object buenos aires cr
    Processing Record 868of Set 1 | buenos aires
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=buenos aires&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=buenos%20aires&units=imperial
    869 City Name             ures
    Country Code            mx
    Latitude             30.73
    Longitude          -112.95
    Temperature (F)      67.31
    Humidity (%)            40
    Cloudiness (%)           0
    Wind Speed (mph)     11.92
    Name: 869, dtype: object ures mx
    Processing Record 869of Set 1 | ures
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ures&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=ures&units=imperial
    870 City Name           guantanamo
    Country Code                cu
    Latitude                -16.14
    Longitude               -62.05
    Temperature (F)          83.51
    Humidity (%)                59
    Cloudiness (%)              44
    Wind Speed (mph)          6.33
    Name: 870, dtype: object guantanamo cu
    Processing Record 870of Set 1 | guantanamo
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=guantanamo&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=guantanamo&units=imperial
    871 City Name           saint-jerome
    Country Code                  ca
    Latitude                   45.78
    Longitude                    -74
    Temperature (F)            18.16
    Humidity (%)                  28
    Cloudiness (%)                 5
    Wind Speed (mph)            5.82
    Name: 871, dtype: object saint-jerome ca
    Processing Record 871of Set 1 | saint-jerome
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saint-jerome&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=saint-jerome&units=imperial
    872 City Name           lingao
    Country Code            cn
    Latitude             19.91
    Longitude           109.69
    Temperature (F)      70.28
    Humidity (%)            90
    Cloudiness (%)          12
    Wind Speed (mph)      3.76
    Name: 872, dtype: object lingao cn
    Processing Record 872of Set 1 | lingao
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lingao&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=lingao&units=imperial
    873 City Name           conceicao do araguaia
    Country Code                           br
    Latitude                            -8.26
    Longitude                          -49.26
    Temperature (F)                     78.92
    Humidity (%)                           89
    Cloudiness (%)                         36
    Wind Speed (mph)                     2.19
    Name: 873, dtype: object conceicao do araguaia br
    Processing Record 873of Set 1 | conceicao do araguaia
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=conceicao do araguaia&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=conceicao%20do%20araguaia&units=imperial
    874 City Name           straumen
    Country Code              no
    Latitude               63.87
    Longitude               11.3
    Temperature (F)         35.6
    Humidity (%)              93
    Cloudiness (%)            90
    Wind Speed (mph)       24.16
    Name: 874, dtype: object straumen no
    Processing Record 874of Set 1 | straumen
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=straumen&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=straumen&units=imperial
    875 City Name           mecca
    Country Code           sa
    Latitude            21.43
    Longitude           39.83
    Temperature (F)      80.6
    Humidity (%)           57
    Cloudiness (%)          0
    Wind Speed (mph)     2.75
    Name: 875, dtype: object mecca sa
    Processing Record 875of Set 1 | mecca
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mecca&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mecca&units=imperial
    876 City Name           mataram
    Country Code             id
    Latitude              -5.32
    Longitude            105.06
    Temperature (F)       74.51
    Humidity (%)             94
    Cloudiness (%)           48
    Wind Speed (mph)       2.75
    Name: 876, dtype: object mataram id
    Processing Record 876of Set 1 | mataram
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mataram&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mataram&units=imperial
    877 City Name           el tigre
    Country Code              ve
    Latitude                8.89
    Longitude             -64.25
    Temperature (F)        85.49
    Humidity (%)              39
    Cloudiness (%)            20
    Wind Speed (mph)        14.5
    Name: 877, dtype: object el tigre ve
    Processing Record 877of Set 1 | el tigre
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=el tigre&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=el%20tigre&units=imperial
    878 City Name           arkhangelsk
    Country Code                 ru
    Latitude                  64.54
    Longitude                 40.54
    Temperature (F)            24.8
    Humidity (%)                100
    Cloudiness (%)               90
    Wind Speed (mph)           6.71
    Name: 878, dtype: object arkhangelsk ru
    Processing Record 878of Set 1 | arkhangelsk
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=arkhangelsk&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=arkhangelsk&units=imperial
    879 City Name           pandan
    Country Code            ph
    Latitude             11.72
    Longitude           122.09
    Temperature (F)      80.99
    Humidity (%)           100
    Cloudiness (%)          68
    Wind Speed (mph)      17.4
    Name: 879, dtype: object pandan ph
    Processing Record 879of Set 1 | pandan
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pandan&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pandan&units=imperial
    880 City Name           chara
    Country Code           ru
    Latitude            39.42
    Longitude           22.43
    Temperature (F)        59
    Humidity (%)           38
    Cloudiness (%)         20
    Wind Speed (mph)     6.93
    Name: 880, dtype: object chara ru
    Processing Record 880of Set 1 | chara
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=chara&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=chara&units=imperial
    881 City Name           devonport
    Country Code               au
    Latitude               -41.18
    Longitude              146.36
    Temperature (F)         58.22
    Humidity (%)               97
    Cloudiness (%)             92
    Wind Speed (mph)        18.86
    Name: 881, dtype: object devonport au
    Processing Record 881of Set 1 | devonport
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=devonport&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=devonport&units=imperial
    882 City Name           pervomayskoye
    Country Code                   ru
    Latitude                    57.07
    Longitude                   86.23
    Temperature (F)               7.1
    Humidity (%)                   70
    Cloudiness (%)                  0
    Wind Speed (mph)             4.43
    Name: 882, dtype: object pervomayskoye ru
    Processing Record 882of Set 1 | pervomayskoye
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pervomayskoye&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=pervomayskoye&units=imperial
    883 City Name           waynesboro
    Country Code                us
    Latitude                 35.32
    Longitude               -87.76
    Temperature (F)          57.27
    Humidity (%)                67
    Cloudiness (%)              90
    Wind Speed (mph)           3.2
    Name: 883, dtype: object waynesboro us
    Processing Record 883of Set 1 | waynesboro
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=waynesboro&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=waynesboro&units=imperial
    884 City Name           iquitos
    Country Code             pe
    Latitude              -3.75
    Longitude            -73.25
    Temperature (F)        80.6
    Humidity (%)             83
    Cloudiness (%)           40
    Wind Speed (mph)       5.82
    Name: 884, dtype: object iquitos pe
    Processing Record 884of Set 1 | iquitos
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=iquitos&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=iquitos&units=imperial
    885 City Name            kiama
    Country Code            au
    Latitude            -34.67
    Longitude           150.86
    Temperature (F)         77
    Humidity (%)            27
    Cloudiness (%)           0
    Wind Speed (mph)     13.87
    Name: 885, dtype: object kiama au
    Processing Record 885of Set 1 | kiama
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kiama&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=kiama&units=imperial
    886 City Name           poltava
    Country Code             ua
    Latitude              49.59
    Longitude             34.55
    Temperature (F)       13.49
    Humidity (%)             87
    Cloudiness (%)           88
    Wind Speed (mph)      16.96
    Name: 886, dtype: object poltava ua
    Processing Record 886of Set 1 | poltava
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=poltava&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=poltava&units=imperial
    887 City Name           henties bay
    Country Code                 na
    Latitude                 -22.12
    Longitude                 14.28
    Temperature (F)           60.83
    Humidity (%)                100
    Cloudiness (%)               76
    Wind Speed (mph)           3.76
    Name: 887, dtype: object henties bay na
    Processing Record 887of Set 1 | henties bay
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=henties bay&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=henties%20bay&units=imperial
    888 City Name           sovetskiy
    Country Code               ru
    Latitude                56.76
    Longitude               48.47
    Temperature (F)         -4.87
    Humidity (%)               67
    Cloudiness (%)              0
    Wind Speed (mph)        10.25
    Name: 888, dtype: object sovetskiy ru
    Processing Record 888of Set 1 | sovetskiy
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sovetskiy&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=sovetskiy&units=imperial
    889 City Name           andra
    Country Code           ru
    Latitude            62.52
    Longitude           65.89
    Temperature (F)      8.54
    Humidity (%)           86
    Cloudiness (%)         76
    Wind Speed (mph)      8.9
    Name: 889, dtype: object andra ru
    Processing Record 889of Set 1 | andra
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=andra&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=andra&units=imperial
    890 City Name           magaria
    Country Code             ne
    Latitude                 13
    Longitude              8.91
    Temperature (F)       67.04
    Humidity (%)             53
    Cloudiness (%)            0
    Wind Speed (mph)       3.09
    Name: 890, dtype: object magaria ne
    Processing Record 890of Set 1 | magaria
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=magaria&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=magaria&units=imperial
    891 City Name           mitsamiouli
    Country Code                 km
    Latitude                 -11.38
    Longitude                 43.28
    Temperature (F)            78.8
    Humidity (%)                 88
    Cloudiness (%)               20
    Wind Speed (mph)            4.7
    Name: 891, dtype: object mitsamiouli km
    Processing Record 891of Set 1 | mitsamiouli
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mitsamiouli&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=mitsamiouli&units=imperial
    892 City Name           abu samrah
    Country Code                qa
    Latitude                  35.3
    Longitude                37.18
    Temperature (F)          54.89
    Humidity (%)                46
    Cloudiness (%)               0
    Wind Speed (mph)          5.66
    Name: 892, dtype: object abu samrah qa
    Processing Record 892of Set 1 | abu samrah
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=abu samrah&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=abu%20samrah&units=imperial
    894 City Name           luganville
    Country Code                vu
    Latitude                -15.51
    Longitude               167.18
    Temperature (F)          83.06
    Humidity (%)                99
    Cloudiness (%)              80
    Wind Speed (mph)          10.8
    Name: 894, dtype: object luganville vu
    Processing Record 894of Set 1 | luganville
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=luganville&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=luganville&units=imperial
    895 City Name           prince george
    Country Code                   ca
    Latitude                    53.92
    Longitude                 -122.75
    Temperature (F)              44.6
    Humidity (%)                   45
    Cloudiness (%)                  1
    Wind Speed (mph)             5.82
    Name: 895, dtype: object prince george ca
    Processing Record 895of Set 1 | prince george
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=prince george&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=prince%20george&units=imperial
    896 City Name           coos bay
    Country Code              us
    Latitude               43.37
    Longitude            -124.22
    Temperature (F)         48.2
    Humidity (%)              66
    Cloudiness (%)            75
    Wind Speed (mph)        3.36
    Name: 896, dtype: object coos bay us
    Processing Record 896of Set 1 | coos bay
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=coos bay&units=imperial
    http://api.openweathermap.org/data/2.5/weather?appid=224f7b3f56aba61567ec7c77e410d506&q=coos%20bay&units=imperial
    ---------------------------------
    Data Retrieval Complete
    ---------------------------------
    


```python
cities_df.head()
#pandas iloc 
# for index, row in cities_df.iterrows():
#     row["City Name"] = cities_df.iloc[index,0].city_name
#     row["Country Code"] = cities_df.iloc[index,0].country_code
#     #iloc[index,0]
# print(cities_df)
# Drop duplicate cities.
# cities_df.drop_duplicates(['City Name', 'Country Code'], inplace=True)
# cities_df.reset_index(inplace=True)

# # # Delete unnecessary columns
# del cities_df[0]
# del cities_df['index']

# cities_df.head()
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
      <th>City Name</th>
      <th>Country Code</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature (F)</th>
      <th>Humidity (%)</th>
      <th>Cloudiness (%)</th>
      <th>Wind Speed (mph)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>punta arenas</td>
      <td>cl</td>
      <td>-53.16</td>
      <td>-70.91</td>
      <td>48.2</td>
      <td></td>
      <td>75</td>
      <td>29.97</td>
    </tr>
    <tr>
      <th>1</th>
      <td>isangel</td>
      <td>vu</td>
      <td>-19.55</td>
      <td>169.27</td>
      <td>80.36</td>
      <td></td>
      <td>68</td>
      <td>10.18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>biak</td>
      <td>id</td>
      <td>-0.91</td>
      <td>122.88</td>
      <td>74.78</td>
      <td></td>
      <td>80</td>
      <td>2.57</td>
    </tr>
    <tr>
      <th>3</th>
      <td>kapaa</td>
      <td>us</td>
      <td>22.08</td>
      <td>-159.32</td>
      <td>71.6</td>
      <td></td>
      <td>90</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ushuaia</td>
      <td>ar</td>
      <td>-54.81</td>
      <td>-68.31</td>
      <td>39.2</td>
      <td></td>
      <td>75</td>
      <td>19.46</td>
    </tr>
  </tbody>
</table>
</div>




```python
pwd
```




    'C:\\Users\\nisha\\Desktop\\Week7'




```python
#Confirm that cities_df has at least 500 unique values by exporting it to Excel.
path_d = 'C:\\Users\\nisha\\Desktop\\test'
cities_df.to_csv(os.path.join(path_d, 'weatherpy.csv'))
#df.to_dense().to_csv("submission.csv", index = False, sep=',', encoding='utf-8')
```


```python
#Delete blank values from dataframe. Use dropna.
#cities_df.dropna(inplace=True)
#subset=['Latitude'],
cities_df
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
      <th>City Name</th>
      <th>Country Code</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature (F)</th>
      <th>Humidity (%)</th>
      <th>Cloudiness (%)</th>
      <th>Wind Speed (mph)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>punta arenas</td>
      <td>cl</td>
      <td>-53.16</td>
      <td>-70.91</td>
      <td>48.2</td>
      <td></td>
      <td>75</td>
      <td>29.97</td>
    </tr>
    <tr>
      <th>1</th>
      <td>isangel</td>
      <td>vu</td>
      <td>-19.55</td>
      <td>169.27</td>
      <td>80.36</td>
      <td></td>
      <td>68</td>
      <td>10.18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>biak</td>
      <td>id</td>
      <td>-0.91</td>
      <td>122.88</td>
      <td>74.78</td>
      <td></td>
      <td>80</td>
      <td>2.57</td>
    </tr>
    <tr>
      <th>3</th>
      <td>kapaa</td>
      <td>us</td>
      <td>22.08</td>
      <td>-159.32</td>
      <td>71.6</td>
      <td></td>
      <td>90</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ushuaia</td>
      <td>ar</td>
      <td>-54.81</td>
      <td>-68.31</td>
      <td>39.2</td>
      <td></td>
      <td>75</td>
      <td>19.46</td>
    </tr>
    <tr>
      <th>5</th>
      <td>neulengbach</td>
      <td>at</td>
      <td>48.2</td>
      <td>15.91</td>
      <td>27.55</td>
      <td></td>
      <td>90</td>
      <td>10.29</td>
    </tr>
    <tr>
      <th>6</th>
      <td>mataura</td>
      <td>pf</td>
      <td>-46.19</td>
      <td>168.86</td>
      <td>66.05</td>
      <td></td>
      <td>100</td>
      <td>17</td>
    </tr>
    <tr>
      <th>7</th>
      <td>chokurdakh</td>
      <td>ru</td>
      <td>70.62</td>
      <td>147.9</td>
      <td>-36.29</td>
      <td></td>
      <td>8</td>
      <td>3.13</td>
    </tr>
    <tr>
      <th>8</th>
      <td>ngukurr</td>
      <td>au</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>9</th>
      <td>clyde river</td>
      <td>ca</td>
      <td>70.47</td>
      <td>-68.59</td>
      <td>-13.01</td>
      <td></td>
      <td>40</td>
      <td>16.11</td>
    </tr>
    <tr>
      <th>10</th>
      <td>hilo</td>
      <td>us</td>
      <td>19.71</td>
      <td>-155.08</td>
      <td>75.2</td>
      <td></td>
      <td>75</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>11</th>
      <td>albany</td>
      <td>au</td>
      <td>42.65</td>
      <td>-73.75</td>
      <td>27.03</td>
      <td></td>
      <td>20</td>
      <td>16.11</td>
    </tr>
    <tr>
      <th>12</th>
      <td>hithadhoo</td>
      <td>mv</td>
      <td>-0.6</td>
      <td>73.08</td>
      <td>85.22</td>
      <td></td>
      <td>88</td>
      <td>8.61</td>
    </tr>
    <tr>
      <th>13</th>
      <td>halifax</td>
      <td>ca</td>
      <td>44.65</td>
      <td>-63.58</td>
      <td>26.6</td>
      <td></td>
      <td>20</td>
      <td>16.11</td>
    </tr>
    <tr>
      <th>14</th>
      <td>carballo</td>
      <td>es</td>
      <td>43.21</td>
      <td>-8.69</td>
      <td>46.35</td>
      <td></td>
      <td>40</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>15</th>
      <td>illoqqortoormiut</td>
      <td>gl</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>16</th>
      <td>tsihombe</td>
      <td>mg</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>17</th>
      <td>vaini</td>
      <td>to</td>
      <td>15.34</td>
      <td>74.49</td>
      <td>66.59</td>
      <td></td>
      <td>8</td>
      <td>2.13</td>
    </tr>
    <tr>
      <th>18</th>
      <td>margate</td>
      <td>za</td>
      <td>-43.03</td>
      <td>147.26</td>
      <td>64.4</td>
      <td></td>
      <td>40</td>
      <td>27.51</td>
    </tr>
    <tr>
      <th>19</th>
      <td>atuona</td>
      <td>pf</td>
      <td>-9.8</td>
      <td>-139.03</td>
      <td>82.34</td>
      <td></td>
      <td>56</td>
      <td>19.24</td>
    </tr>
    <tr>
      <th>20</th>
      <td>fort smith</td>
      <td>us</td>
      <td>35.39</td>
      <td>-94.42</td>
      <td>66.67</td>
      <td></td>
      <td>1</td>
      <td>6.93</td>
    </tr>
    <tr>
      <th>21</th>
      <td>taolanaro</td>
      <td>mg</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>22</th>
      <td>dzerzhinsk</td>
      <td>ru</td>
      <td>56.24</td>
      <td>43.46</td>
      <td>10.4</td>
      <td></td>
      <td>0</td>
      <td>4.47</td>
    </tr>
    <tr>
      <th>23</th>
      <td>belushya guba</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>24</th>
      <td>rikitea</td>
      <td>pf</td>
      <td>-23.12</td>
      <td>-134.97</td>
      <td>80</td>
      <td></td>
      <td>56</td>
      <td>14.32</td>
    </tr>
    <tr>
      <th>25</th>
      <td>bathsheba</td>
      <td>bb</td>
      <td>13.22</td>
      <td>-59.52</td>
      <td>82.4</td>
      <td></td>
      <td>20</td>
      <td>13.87</td>
    </tr>
    <tr>
      <th>26</th>
      <td>longyearbyen</td>
      <td>sj</td>
      <td>78.22</td>
      <td>15.63</td>
      <td>14</td>
      <td></td>
      <td>40</td>
      <td>26.4</td>
    </tr>
    <tr>
      <th>27</th>
      <td>souillac</td>
      <td>mu</td>
      <td>45.6</td>
      <td>-0.6</td>
      <td>43.61</td>
      <td></td>
      <td>0</td>
      <td>9.17</td>
    </tr>
    <tr>
      <th>28</th>
      <td>princeton</td>
      <td>ca</td>
      <td>41.37</td>
      <td>-89.46</td>
      <td>50.92</td>
      <td></td>
      <td>1</td>
      <td>4.81</td>
    </tr>
    <tr>
      <th>29</th>
      <td>lompoc</td>
      <td>us</td>
      <td>34.64</td>
      <td>-120.46</td>
      <td>57.24</td>
      <td></td>
      <td>1</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>868</th>
      <td>buenos aires</td>
      <td>cr</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>869</th>
      <td>ures</td>
      <td>mx</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>870</th>
      <td>guantanamo</td>
      <td>cu</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>871</th>
      <td>saint-jerome</td>
      <td>ca</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>872</th>
      <td>lingao</td>
      <td>cn</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>873</th>
      <td>conceicao do araguaia</td>
      <td>br</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>874</th>
      <td>straumen</td>
      <td>no</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>875</th>
      <td>mecca</td>
      <td>sa</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>876</th>
      <td>mataram</td>
      <td>id</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>877</th>
      <td>el tigre</td>
      <td>ve</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>878</th>
      <td>arkhangelsk</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>879</th>
      <td>pandan</td>
      <td>ph</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>880</th>
      <td>chara</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>881</th>
      <td>devonport</td>
      <td>au</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>882</th>
      <td>pervomayskoye</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>883</th>
      <td>waynesboro</td>
      <td>us</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>884</th>
      <td>iquitos</td>
      <td>pe</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>885</th>
      <td>kiama</td>
      <td>au</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>886</th>
      <td>poltava</td>
      <td>ua</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>887</th>
      <td>henties bay</td>
      <td>na</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>888</th>
      <td>sovetskiy</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>889</th>
      <td>andra</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>890</th>
      <td>magaria</td>
      <td>ne</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>891</th>
      <td>mitsamiouli</td>
      <td>km</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>892</th>
      <td>abu samrah</td>
      <td>qa</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>893</th>
      <td>maghama</td>
      <td>mr</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>894</th>
      <td>luganville</td>
      <td>vu</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>895</th>
      <td>prince george</td>
      <td>ca</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>896</th>
      <td>coos bay</td>
      <td>us</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>897</th>
      <td>port-de-paix</td>
      <td>ht</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
<p>898 rows × 8 columns</p>
</div>




```python
# Your objective is to build a series of scatter plots to showcase the following relationships:

# Temperature (F) vs. Latitude
# Humidity (%) vs. Latitude
# Cloudiness (%) vs. Latitude
# Wind Speed (mph) vs. Latitude
```

<strong>Latitude versus Temperature Plot</strong>


```python
cities_df = cities_df[pd.notnull(cities_df['Latitude'])]
cities_df
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
      <th>City Name</th>
      <th>Country Code</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature (F)</th>
      <th>Humidity (%)</th>
      <th>Cloudiness (%)</th>
      <th>Wind Speed (mph)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>punta arenas</td>
      <td>cl</td>
      <td>-53.16</td>
      <td>-70.91</td>
      <td>48.2</td>
      <td></td>
      <td>75</td>
      <td>29.97</td>
    </tr>
    <tr>
      <th>1</th>
      <td>isangel</td>
      <td>vu</td>
      <td>-19.55</td>
      <td>169.27</td>
      <td>80.36</td>
      <td></td>
      <td>68</td>
      <td>10.18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>biak</td>
      <td>id</td>
      <td>-0.91</td>
      <td>122.88</td>
      <td>74.78</td>
      <td></td>
      <td>80</td>
      <td>2.57</td>
    </tr>
    <tr>
      <th>3</th>
      <td>kapaa</td>
      <td>us</td>
      <td>22.08</td>
      <td>-159.32</td>
      <td>71.6</td>
      <td></td>
      <td>90</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ushuaia</td>
      <td>ar</td>
      <td>-54.81</td>
      <td>-68.31</td>
      <td>39.2</td>
      <td></td>
      <td>75</td>
      <td>19.46</td>
    </tr>
    <tr>
      <th>5</th>
      <td>neulengbach</td>
      <td>at</td>
      <td>48.2</td>
      <td>15.91</td>
      <td>27.55</td>
      <td></td>
      <td>90</td>
      <td>10.29</td>
    </tr>
    <tr>
      <th>6</th>
      <td>mataura</td>
      <td>pf</td>
      <td>-46.19</td>
      <td>168.86</td>
      <td>66.05</td>
      <td></td>
      <td>100</td>
      <td>17</td>
    </tr>
    <tr>
      <th>7</th>
      <td>chokurdakh</td>
      <td>ru</td>
      <td>70.62</td>
      <td>147.9</td>
      <td>-36.29</td>
      <td></td>
      <td>8</td>
      <td>3.13</td>
    </tr>
    <tr>
      <th>8</th>
      <td>ngukurr</td>
      <td>au</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>9</th>
      <td>clyde river</td>
      <td>ca</td>
      <td>70.47</td>
      <td>-68.59</td>
      <td>-13.01</td>
      <td></td>
      <td>40</td>
      <td>16.11</td>
    </tr>
    <tr>
      <th>10</th>
      <td>hilo</td>
      <td>us</td>
      <td>19.71</td>
      <td>-155.08</td>
      <td>75.2</td>
      <td></td>
      <td>75</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>11</th>
      <td>albany</td>
      <td>au</td>
      <td>42.65</td>
      <td>-73.75</td>
      <td>27.03</td>
      <td></td>
      <td>20</td>
      <td>16.11</td>
    </tr>
    <tr>
      <th>12</th>
      <td>hithadhoo</td>
      <td>mv</td>
      <td>-0.6</td>
      <td>73.08</td>
      <td>85.22</td>
      <td></td>
      <td>88</td>
      <td>8.61</td>
    </tr>
    <tr>
      <th>13</th>
      <td>halifax</td>
      <td>ca</td>
      <td>44.65</td>
      <td>-63.58</td>
      <td>26.6</td>
      <td></td>
      <td>20</td>
      <td>16.11</td>
    </tr>
    <tr>
      <th>14</th>
      <td>carballo</td>
      <td>es</td>
      <td>43.21</td>
      <td>-8.69</td>
      <td>46.35</td>
      <td></td>
      <td>40</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>15</th>
      <td>illoqqortoormiut</td>
      <td>gl</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>16</th>
      <td>tsihombe</td>
      <td>mg</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>17</th>
      <td>vaini</td>
      <td>to</td>
      <td>15.34</td>
      <td>74.49</td>
      <td>66.59</td>
      <td></td>
      <td>8</td>
      <td>2.13</td>
    </tr>
    <tr>
      <th>18</th>
      <td>margate</td>
      <td>za</td>
      <td>-43.03</td>
      <td>147.26</td>
      <td>64.4</td>
      <td></td>
      <td>40</td>
      <td>27.51</td>
    </tr>
    <tr>
      <th>19</th>
      <td>atuona</td>
      <td>pf</td>
      <td>-9.8</td>
      <td>-139.03</td>
      <td>82.34</td>
      <td></td>
      <td>56</td>
      <td>19.24</td>
    </tr>
    <tr>
      <th>20</th>
      <td>fort smith</td>
      <td>us</td>
      <td>35.39</td>
      <td>-94.42</td>
      <td>66.67</td>
      <td></td>
      <td>1</td>
      <td>6.93</td>
    </tr>
    <tr>
      <th>21</th>
      <td>taolanaro</td>
      <td>mg</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>22</th>
      <td>dzerzhinsk</td>
      <td>ru</td>
      <td>56.24</td>
      <td>43.46</td>
      <td>10.4</td>
      <td></td>
      <td>0</td>
      <td>4.47</td>
    </tr>
    <tr>
      <th>23</th>
      <td>belushya guba</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>24</th>
      <td>rikitea</td>
      <td>pf</td>
      <td>-23.12</td>
      <td>-134.97</td>
      <td>80</td>
      <td></td>
      <td>56</td>
      <td>14.32</td>
    </tr>
    <tr>
      <th>25</th>
      <td>bathsheba</td>
      <td>bb</td>
      <td>13.22</td>
      <td>-59.52</td>
      <td>82.4</td>
      <td></td>
      <td>20</td>
      <td>13.87</td>
    </tr>
    <tr>
      <th>26</th>
      <td>longyearbyen</td>
      <td>sj</td>
      <td>78.22</td>
      <td>15.63</td>
      <td>14</td>
      <td></td>
      <td>40</td>
      <td>26.4</td>
    </tr>
    <tr>
      <th>27</th>
      <td>souillac</td>
      <td>mu</td>
      <td>45.6</td>
      <td>-0.6</td>
      <td>43.61</td>
      <td></td>
      <td>0</td>
      <td>9.17</td>
    </tr>
    <tr>
      <th>28</th>
      <td>princeton</td>
      <td>ca</td>
      <td>41.37</td>
      <td>-89.46</td>
      <td>50.92</td>
      <td></td>
      <td>1</td>
      <td>4.81</td>
    </tr>
    <tr>
      <th>29</th>
      <td>lompoc</td>
      <td>us</td>
      <td>34.64</td>
      <td>-120.46</td>
      <td>57.24</td>
      <td></td>
      <td>1</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>868</th>
      <td>buenos aires</td>
      <td>cr</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>869</th>
      <td>ures</td>
      <td>mx</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>870</th>
      <td>guantanamo</td>
      <td>cu</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>871</th>
      <td>saint-jerome</td>
      <td>ca</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>872</th>
      <td>lingao</td>
      <td>cn</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>873</th>
      <td>conceicao do araguaia</td>
      <td>br</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>874</th>
      <td>straumen</td>
      <td>no</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>875</th>
      <td>mecca</td>
      <td>sa</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>876</th>
      <td>mataram</td>
      <td>id</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>877</th>
      <td>el tigre</td>
      <td>ve</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>878</th>
      <td>arkhangelsk</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>879</th>
      <td>pandan</td>
      <td>ph</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>880</th>
      <td>chara</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>881</th>
      <td>devonport</td>
      <td>au</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>882</th>
      <td>pervomayskoye</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>883</th>
      <td>waynesboro</td>
      <td>us</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>884</th>
      <td>iquitos</td>
      <td>pe</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>885</th>
      <td>kiama</td>
      <td>au</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>886</th>
      <td>poltava</td>
      <td>ua</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>887</th>
      <td>henties bay</td>
      <td>na</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>888</th>
      <td>sovetskiy</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>889</th>
      <td>andra</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>890</th>
      <td>magaria</td>
      <td>ne</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>891</th>
      <td>mitsamiouli</td>
      <td>km</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>892</th>
      <td>abu samrah</td>
      <td>qa</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>893</th>
      <td>maghama</td>
      <td>mr</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>894</th>
      <td>luganville</td>
      <td>vu</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>895</th>
      <td>prince george</td>
      <td>ca</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>896</th>
      <td>coos bay</td>
      <td>us</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>897</th>
      <td>port-de-paix</td>
      <td>ht</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
<p>898 rows × 8 columns</p>
</div>




```python
# Changing strings to floats
columns = ['Latitude', 'Temperature (F)', 'Humidity (%)', 'Cloudiness (%)', 'Wind Speed (mph)']
for column in columns:
    cities_df[column] = pd.to_numeric(cities_df[column], errors='coerce')
    
# Dropping NaN values
cities_df.dropna(inplace=True)

cities_df.head()
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
      <th>City Name</th>
      <th>Country Code</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature (F)</th>
      <th>Humidity (%)</th>
      <th>Cloudiness (%)</th>
      <th>Wind Speed (mph)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>punta arenas</td>
      <td>cl</td>
      <td>-53.16</td>
      <td>-70.91</td>
      <td>46.40</td>
      <td>100.0</td>
      <td>75.0</td>
      <td>26.40</td>
    </tr>
    <tr>
      <th>1</th>
      <td>isangel</td>
      <td>vu</td>
      <td>-19.55</td>
      <td>169.27</td>
      <td>80.36</td>
      <td>100.0</td>
      <td>68.0</td>
      <td>10.18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>biak</td>
      <td>id</td>
      <td>-0.91</td>
      <td>122.88</td>
      <td>74.78</td>
      <td>100.0</td>
      <td>80.0</td>
      <td>2.57</td>
    </tr>
    <tr>
      <th>3</th>
      <td>kapaa</td>
      <td>us</td>
      <td>22.08</td>
      <td>-159.32</td>
      <td>71.60</td>
      <td>94.0</td>
      <td>90.0</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ushuaia</td>
      <td>ar</td>
      <td>-54.81</td>
      <td>-68.31</td>
      <td>41.00</td>
      <td>80.0</td>
      <td>75.0</td>
      <td>20.80</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Confirm that cities_df has at least 500 unique values by exporting it to Excel.
path_d = 'C:\\Users\\nisha\\Desktop\\test'
cities_df.to_csv(os.path.join(path_d, 'weatherpy2.csv'))
```

<a id='another_cell'></a>


```python
# plt.scatter(cities_df["Latitude"],
#            cities_df["Temperature (F)"],
#            edgecolor = "black", c= 'navy', linewidths = 1, marker = "o",
#            alpha = 0.8)
plt.scatter(cities_df["Latitude"],
            cities_df["Temperature (F)"])
plt.title("City Latitude vs. Max Temperature")
plt.ylabel("Temperature (F)")
plt.axvline(0, color = 'black', alpha = .25, label = 'Equator')
plt.text(1,30,'Equator',rotation=90)
plt.ylim(-50,120)
plt.xlabel("Latitude")
plt.xlim(-60,90)
plt.grid(True)
plt.savefig("WeatherPyGraphs/LatitudeVsTemperature.png")
plt.show()
```


![png](output_26_0.png)


<a id='hum_cell'></a>


```python
# Humidity (%) vs. Latitude
# lshum = cities_df["Humidity (%)"].tolist()
# plt.scatter(cities_df["Latitude"],
#            cities_df["Humidity (%)"],
#            edgecolor = "black", c= 'navy', linewidths = 1, marker = "o",
#            alpha = 0.8)
plt.scatter(cities_df["Latitude"],
            cities_df["Humidity (%)"])
plt.title("Latitude vs. Humidity Plot")
plt.ylabel("Humidity (%)")
plt.axvline(0, color = 'black', alpha = .25, label = 'Equator')
plt.text(1,30,'Equator',rotation=90)
plt.ylim(-50,120)
plt.xlabel("Latitude")
plt.xlim(-60,90)
plt.grid(True)
plt.savefig("WeatherPyGraphs/LatitudeVsHumidity.png")
plt.show()
```


![png](output_28_0.png)


<a id='cloud_cell'></a>


```python
# Cloudiness (%) vs. Latitude
# Wind Speed (mph) vs. Latitude
# plt.scatter(cities_df["Latitude"],
#            cities_df["Cloudiness (%)"],
#            edgecolor = "black", c= 'navy', linewidths = 1, marker = "o",
#            alpha = 0.8)
plt.scatter(cities_df["Latitude"],
            cities_df["Cloudiness (%)"])
plt.title("Latitude vs. Cloudiness Plot")
plt.ylabel("Cloudiness (%)")
plt.ylim(-50,120)
plt.axvline(0, color = 'black', alpha = .25, label = 'Equator')
plt.text(1,30,'Equator',rotation=90)
plt.xlabel("Latitude")
plt.xlim(-60,90)
plt.grid(True)
plt.savefig("WeatherPyGraphs/LatitudeVsCloudiness.png")
plt.show()
```


![png](output_30_0.png)


<a id='wind_cell'></a>


```python
#Latitude vs Wind Speed
plt.scatter(cities_df["Latitude"],
            cities_df["Wind Speed (mph)"])
plt.title("Latitude vs. Windspeed Plot")
plt.ylabel("Wind Speed (mph)")
plt.axvline(0, color = 'black', alpha = .25, label = 'Equator')
plt.text(1,30,'Equator',rotation=90)
plt.ylim(-5,40)
plt.xlabel("Latitude")
plt.xlim(-60,90)
plt.grid(True)
plt.savefig("WeatherPyGraphs/LatitudeVsWindSpeed.png")
plt.show()
```


![png](output_32_0.png)


[Return to Top](#top_cell)
