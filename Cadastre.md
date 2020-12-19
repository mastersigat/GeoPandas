```python
#Import librairies

import pandas as pd
import geopandas as gpd
import geopy
import descartes
import matplotlib.pyplot as plt
```


```python
#Importer un dataset en local

filename = "C:/Users/Xo/Desktop/Cadastre/cadastre-35051-parcelles.json"
file = open(filename)
Parcelles= gpd.read_file(file, encoding='utf-8')

filename = "C:/Users/Xo/Desktop/Cadastre/cadastre-35051-batiments.json"
file = open(filename)
Batiments= gpd.read_file(file, encoding='utf-8')

filename = "C:/Users/Xo/Desktop/Cadastre/cadastre-35051-sections.json"
file = open(filename)
Sections= gpd.read_file(file, encoding='utf-8')


```


```python
#Afficher la table  des parcelles

Parcelles.head()
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
      <th>id</th>
      <th>commune</th>
      <th>prefixe</th>
      <th>section</th>
      <th>numero</th>
      <th>contenance</th>
      <th>arpente</th>
      <th>created</th>
      <th>updated</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>35051000AA0564</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>564</td>
      <td>21375</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((-1.61265 48.12611, -1.61252 48.12616...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>35051000AA0562</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>562</td>
      <td>42655</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((-1.60718 48.12526, -1.60721 48.12533...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>35051000AA0563</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>563</td>
      <td>6378</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((-1.60389 48.12371, -1.60428 48.12370...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35051000AA0566</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>566</td>
      <td>959</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((-1.61078 48.12329, -1.61086 48.12335...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35051000AA0192</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>192</td>
      <td>6752</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((-1.60716 48.12685, -1.60714 48.12705...</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Créer un ID unique pour chaque parcelle

Parcelles["ID_Parcelletest"] = Parcelles["prefixe"] + "-" + Parcelles["numero"]
Parcelles = Parcelles.rename(columns={"id": 'ID_Parcelle'})

Parcelles.head()
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
      <th>ID_Parcelle</th>
      <th>commune</th>
      <th>prefixe</th>
      <th>section</th>
      <th>numero</th>
      <th>contenance</th>
      <th>arpente</th>
      <th>created</th>
      <th>updated</th>
      <th>geometry</th>
      <th>ID_Parcelletest</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>35051000AA0564</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>564</td>
      <td>21375</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((-1.61265 48.12611, -1.61252 48.12616...</td>
      <td>000-564</td>
    </tr>
    <tr>
      <th>1</th>
      <td>35051000AA0562</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>562</td>
      <td>42655</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((-1.60718 48.12526, -1.60721 48.12533...</td>
      <td>000-562</td>
    </tr>
    <tr>
      <th>2</th>
      <td>35051000AA0563</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>563</td>
      <td>6378</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((-1.60389 48.12371, -1.60428 48.12370...</td>
      <td>000-563</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35051000AA0566</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>566</td>
      <td>959</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((-1.61078 48.12329, -1.61086 48.12335...</td>
      <td>000-566</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35051000AA0192</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>192</td>
      <td>6752</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((-1.60716 48.12685, -1.60714 48.12705...</td>
      <td>000-192</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Afficher la table des batiments

Batiments.head()
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
      <th>type</th>
      <th>nom</th>
      <th>commune</th>
      <th>created</th>
      <th>updated</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>02</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((-1.61137 48.12450, -1.61136 48...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((-1.61126 48.12495, -1.61133 48...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((-1.60973 48.12449, -1.60980 48...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((-1.61103 48.12360, -1.61101 48...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((-1.61097 48.12380, -1.61106 48...</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Créer un ID unique pour chaque batiment

Batiments["ID_Bati"] = Batiments.index
Batiments.head()
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
      <th>type</th>
      <th>nom</th>
      <th>commune</th>
      <th>created</th>
      <th>updated</th>
      <th>geometry</th>
      <th>ID_Bati</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>02</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((-1.61137 48.12450, -1.61136 48...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((-1.61126 48.12495, -1.61133 48...</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((-1.60973 48.12449, -1.60980 48...</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((-1.61103 48.12360, -1.61101 48...</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((-1.61097 48.12380, -1.61106 48...</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Afficher la table des sections cadastrales

Sections.head(5)
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
      <th>id</th>
      <th>commune</th>
      <th>prefixe</th>
      <th>code</th>
      <th>created</th>
      <th>updated</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>35051000AA</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>2004-06-18</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((-1.60596 48.12865, -1.60601 48...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>35051000AB</td>
      <td>35051</td>
      <td>000</td>
      <td>AB</td>
      <td>2004-05-27</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((-1.60762 48.12349, -1.60812 48...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>35051000AC</td>
      <td>35051</td>
      <td>000</td>
      <td>AC</td>
      <td>2004-05-27</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((-1.60259 48.12062, -1.60287 48...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35051000AD</td>
      <td>35051</td>
      <td>000</td>
      <td>AD</td>
      <td>2004-05-27</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((-1.59779 48.12371, -1.59820 48...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35051000AE</td>
      <td>35051</td>
      <td>000</td>
      <td>AE</td>
      <td>2004-05-27</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((-1.59676 48.12855, -1.59683 48...</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Faire une carte des section cadastrales

Sections.plot(figsize=(15,15),column='code', legend=True)
```




    <AxesSubplot:>




    
![png](output_7_1.png)
    



```python
#Faire une carte des parcelles cadastrales avec une catégorisation par section

Parcelles.plot(figsize=(500,15),column='section', legend=True)
```




    <AxesSubplot:>




    
![png](output_8_1.png)
    



```python
# Vérifier le SCR des couches

Batiments.crs
Parcelles.crs
```




    <Geographic 2D CRS: EPSG:4326>
    Name: WGS 84
    Axis Info [ellipsoidal]:
    - Lat[north]: Geodetic latitude (degree)
    - Lon[east]: Geodetic longitude (degree)
    Area of Use:
    - name: World
    - bounds: (-180.0, -90.0, 180.0, 90.0)
    Datum: World Geodetic System 1984
    - Ellipsoid: WGS 84
    - Prime Meridian: Greenwich




```python
#Reprojeter les sections cadastrales

Sections2154 = Sections.to_crs("EPSG:2154")
Sections2154.crs
```




    <Projected CRS: EPSG:2154>
    Name: RGF93 / Lambert-93
    Axis Info [cartesian]:
    - X[east]: Easting (metre)
    - Y[north]: Northing (metre)
    Area of Use:
    - name: France
    - bounds: (-9.86, 41.15, 10.38, 51.56)
    Coordinate Operation:
    - name: Lambert-93
    - method: Lambert Conic Conformal (2SP)
    Datum: Reseau Geodesique Francais 1993
    - Ellipsoid: GRS 1980
    - Prime Meridian: Greenwich




```python
# Ajouter une colonne surface aux sections cadastrales

Sections2154["Surface_Section"] = Sections2154['geometry'].area
Sections2154.head(10) 
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
      <th>id</th>
      <th>commune</th>
      <th>prefixe</th>
      <th>code</th>
      <th>created</th>
      <th>updated</th>
      <th>geometry</th>
      <th>Surface_Section</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>35051000AA</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>2004-06-18</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((357517.470 6790911.435, 357514...</td>
      <td>369063.408197</td>
    </tr>
    <tr>
      <th>1</th>
      <td>35051000AB</td>
      <td>35051</td>
      <td>000</td>
      <td>AB</td>
      <td>2004-05-27</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((357360.791 6790345.960, 357323...</td>
      <td>214289.844646</td>
    </tr>
    <tr>
      <th>2</th>
      <td>35051000AC</td>
      <td>35051</td>
      <td>000</td>
      <td>AC</td>
      <td>2004-05-27</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((357715.587 6790006.212, 357698...</td>
      <td>142514.452758</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35051000AD</td>
      <td>35051</td>
      <td>000</td>
      <td>AD</td>
      <td>2004-05-27</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((358092.442 6790328.433, 358058...</td>
      <td>191803.352173</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35051000AE</td>
      <td>35051</td>
      <td>000</td>
      <td>AE</td>
      <td>2004-05-27</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((358200.027 6790860.405, 358194...</td>
      <td>228557.377366</td>
    </tr>
    <tr>
      <th>5</th>
      <td>35051000AH</td>
      <td>35051</td>
      <td>000</td>
      <td>AH</td>
      <td>2004-05-27</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((358539.808 6790744.566, 358534...</td>
      <td>289118.451241</td>
    </tr>
    <tr>
      <th>6</th>
      <td>35051000AI</td>
      <td>35051</td>
      <td>000</td>
      <td>AI</td>
      <td>2004-05-27</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((358497.269 6790194.431, 358496...</td>
      <td>320672.307780</td>
    </tr>
    <tr>
      <th>7</th>
      <td>35051000AK</td>
      <td>35051</td>
      <td>000</td>
      <td>AK</td>
      <td>2004-05-27</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((355517.130 6789821.877, 355516...</td>
      <td>187119.027078</td>
    </tr>
    <tr>
      <th>8</th>
      <td>35051000AL</td>
      <td>35051</td>
      <td>000</td>
      <td>AL</td>
      <td>2004-05-27</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((355676.370 6790202.111, 355672...</td>
      <td>132812.366440</td>
    </tr>
    <tr>
      <th>9</th>
      <td>35051000AM</td>
      <td>35051</td>
      <td>000</td>
      <td>AM</td>
      <td>2004-05-27</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((356225.098 6790411.637, 356209...</td>
      <td>316638.399777</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Reprojeter les parcelles cadastrales et ajouter une colonne surface

Parcelles2154 = Parcelles.to_crs("EPSG:2154")
Parcelles2154["Surface_Parcelle"] = Parcelles2154['geometry'].area
Parcelles2154.head(10) 
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
      <th>ID_Parcelle</th>
      <th>commune</th>
      <th>prefixe</th>
      <th>section</th>
      <th>numero</th>
      <th>contenance</th>
      <th>arpente</th>
      <th>created</th>
      <th>updated</th>
      <th>geometry</th>
      <th>ID_Parcelletest</th>
      <th>Surface_Parcelle</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>35051000AA0564</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>564</td>
      <td>21375</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357004.270 6790658.715, 357014.531 6...</td>
      <td>000-564</td>
      <td>23922.911984</td>
    </tr>
    <tr>
      <th>1</th>
      <td>35051000AA0562</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>562</td>
      <td>42655</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357405.271 6790540.522, 357403.462 6...</td>
      <td>000-562</td>
      <td>42587.307820</td>
    </tr>
    <tr>
      <th>2</th>
      <td>35051000AA0563</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>563</td>
      <td>6378</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357639.229 6790355.015, 357609.990 6...</td>
      <td>000-563</td>
      <td>5647.509707</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35051000AA0566</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>566</td>
      <td>959</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357124.631 6790338.265, 357119.119 6...</td>
      <td>000-566</td>
      <td>1613.160670</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35051000AA0192</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>192</td>
      <td>6752</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357417.028 6790716.924, 357419.271 6...</td>
      <td>000-192</td>
      <td>6609.099724</td>
    </tr>
    <tr>
      <th>5</th>
      <td>35051000AA0565</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>565</td>
      <td>2881</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357004.270 6790658.715, 357004.548 6...</td>
      <td>000-565</td>
      <td>3063.325236</td>
    </tr>
    <tr>
      <th>6</th>
      <td>35051000AA0553</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>553</td>
      <td>1290</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357028.302 6790578.606, 357028.060 6...</td>
      <td>000-553</td>
      <td>1287.809973</td>
    </tr>
    <tr>
      <th>7</th>
      <td>35051000AA0059</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>59</td>
      <td>619</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357024.044 6790482.974, 357043.639 6...</td>
      <td>000-59</td>
      <td>621.336887</td>
    </tr>
    <tr>
      <th>8</th>
      <td>35051000AA0089</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>89</td>
      <td>698</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357112.439 6790467.687, 357090.570 6...</td>
      <td>000-89</td>
      <td>701.243504</td>
    </tr>
    <tr>
      <th>9</th>
      <td>35051000AA0088</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>88</td>
      <td>715</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357112.439 6790467.687, 357118.292 6...</td>
      <td>000-88</td>
      <td>706.718863</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Reprojeter les batiments et ajouter une colonne surface

Batiments2154 = Batiments.to_crs("EPSG:2154")
Batiments2154["Surface_Bati"] = Batiments2154['geometry'].area
Batiments2154.head(10) 
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
      <th>type</th>
      <th>nom</th>
      <th>commune</th>
      <th>created</th>
      <th>updated</th>
      <th>geometry</th>
      <th>ID_Bati</th>
      <th>Surface_Bati</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>02</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((357089.170 6790474.569, 357089...</td>
      <td>0</td>
      <td>10.835598</td>
    </tr>
    <tr>
      <th>1</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((357099.887 6790523.903, 357094...</td>
      <td>1</td>
      <td>156.415285</td>
    </tr>
    <tr>
      <th>2</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((357210.803 6790466.891, 357205...</td>
      <td>2</td>
      <td>100.495147</td>
    </tr>
    <tr>
      <th>3</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((357108.530 6790373.682, 357109...</td>
      <td>3</td>
      <td>132.496058</td>
    </tr>
    <tr>
      <th>4</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((357113.809 6790395.515, 357106...</td>
      <td>4</td>
      <td>91.291023</td>
    </tr>
    <tr>
      <th>5</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((357127.911 6790406.480, 357125...</td>
      <td>5</td>
      <td>111.440761</td>
    </tr>
    <tr>
      <th>6</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((357132.822 6790410.178, 357139...</td>
      <td>6</td>
      <td>139.104492</td>
    </tr>
    <tr>
      <th>7</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((357159.227 6790415.492, 357165...</td>
      <td>7</td>
      <td>84.995102</td>
    </tr>
    <tr>
      <th>8</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((357149.649 6790430.779, 357152...</td>
      <td>8</td>
      <td>98.878468</td>
    </tr>
    <tr>
      <th>9</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>MULTIPOLYGON (((357136.659 6790442.793, 357140...</td>
      <td>9</td>
      <td>111.671953</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Transformer les batiments (polygones) en points (centroides)

BatimentsCentro = Batiments2154.copy()
BatimentsCentro.geometry = BatimentsCentro['geometry'].centroid
BatimentsCentro.crs =Batiments2154.crs
BatimentsCentro.plot(figsize=(300,10), markersize=0.5, legend=True)
BatimentsCentro.head()
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
      <th>type</th>
      <th>nom</th>
      <th>commune</th>
      <th>created</th>
      <th>updated</th>
      <th>geometry</th>
      <th>ID_Bati</th>
      <th>Surface_Bati</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>02</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>POINT (357090.104 6790476.401)</td>
      <td>0</td>
      <td>10.835598</td>
    </tr>
    <tr>
      <th>1</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>POINT (357091.551 6790520.886)</td>
      <td>1</td>
      <td>156.415285</td>
    </tr>
    <tr>
      <th>2</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>POINT (357211.654 6790458.961)</td>
      <td>2</td>
      <td>100.495147</td>
    </tr>
    <tr>
      <th>3</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>POINT (357115.256 6790368.125)</td>
      <td>3</td>
      <td>132.496058</td>
    </tr>
    <tr>
      <th>4</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>POINT (357113.730 6790388.793)</td>
      <td>4</td>
      <td>91.291023</td>
    </tr>
  </tbody>
</table>
</div>




    
![png](output_14_1.png)
    



```python
# Encrichir les batiments des informations de la couche des Sections Cadastrales (jointure sptaiale)

BatimentsEtape1 = gpd.sjoin(BatimentsCentro, Sections2154)
BatimentsEtape1.head(5)
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
      <th>type</th>
      <th>nom</th>
      <th>commune_left</th>
      <th>created_left</th>
      <th>updated_left</th>
      <th>geometry</th>
      <th>ID_Bati</th>
      <th>Surface_Bati</th>
      <th>index_right</th>
      <th>id</th>
      <th>commune_right</th>
      <th>prefixe</th>
      <th>code</th>
      <th>created_right</th>
      <th>updated_right</th>
      <th>Surface_Section</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>02</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>POINT (357090.104 6790476.401)</td>
      <td>0</td>
      <td>10.835598</td>
      <td>0</td>
      <td>35051000AA</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>2004-06-18</td>
      <td>2017-09-14</td>
      <td>369063.408197</td>
    </tr>
    <tr>
      <th>1</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>POINT (357091.551 6790520.886)</td>
      <td>1</td>
      <td>156.415285</td>
      <td>0</td>
      <td>35051000AA</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>2004-06-18</td>
      <td>2017-09-14</td>
      <td>369063.408197</td>
    </tr>
    <tr>
      <th>2</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>POINT (357211.654 6790458.961)</td>
      <td>2</td>
      <td>100.495147</td>
      <td>0</td>
      <td>35051000AA</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>2004-06-18</td>
      <td>2017-09-14</td>
      <td>369063.408197</td>
    </tr>
    <tr>
      <th>3</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>POINT (357115.256 6790368.125)</td>
      <td>3</td>
      <td>132.496058</td>
      <td>0</td>
      <td>35051000AA</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>2004-06-18</td>
      <td>2017-09-14</td>
      <td>369063.408197</td>
    </tr>
    <tr>
      <th>4</th>
      <td>01</td>
      <td>None</td>
      <td>35051</td>
      <td>2004-06-18</td>
      <td>2018-06-12</td>
      <td>POINT (357113.730 6790388.793)</td>
      <td>4</td>
      <td>91.291023</td>
      <td>0</td>
      <td>35051000AA</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>2004-06-18</td>
      <td>2017-09-14</td>
      <td>369063.408197</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Réorganiser la table

BatimentsEtape2 = BatimentsEtape1[["ID_Bati", "type", "code", "Surface_Bati", "Surface_Section", "geometry"]]
BatimentsEtape2.head()
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
      <th>ID_Bati</th>
      <th>type</th>
      <th>code</th>
      <th>Surface_Bati</th>
      <th>Surface_Section</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>02</td>
      <td>AA</td>
      <td>10.835598</td>
      <td>369063.408197</td>
      <td>POINT (357090.104 6790476.401)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>01</td>
      <td>AA</td>
      <td>156.415285</td>
      <td>369063.408197</td>
      <td>POINT (357091.551 6790520.886)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>01</td>
      <td>AA</td>
      <td>100.495147</td>
      <td>369063.408197</td>
      <td>POINT (357211.654 6790458.961)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>01</td>
      <td>AA</td>
      <td>132.496058</td>
      <td>369063.408197</td>
      <td>POINT (357115.256 6790368.125)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>01</td>
      <td>AA</td>
      <td>91.291023</td>
      <td>369063.408197</td>
      <td>POINT (357113.730 6790388.793)</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Encrichir les batiments des informations de la couche des parcelles Cadastrales (jointure sptaiale)

BatimentsEtape3 = gpd.sjoin(BatimentsEtape2, Parcelles2154)
BatimentsEtape3.head(5)
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
      <th>ID_Bati</th>
      <th>type</th>
      <th>code</th>
      <th>Surface_Bati</th>
      <th>Surface_Section</th>
      <th>geometry</th>
      <th>index_right</th>
      <th>ID_Parcelle</th>
      <th>commune</th>
      <th>prefixe</th>
      <th>section</th>
      <th>numero</th>
      <th>contenance</th>
      <th>arpente</th>
      <th>created</th>
      <th>updated</th>
      <th>ID_Parcelletest</th>
      <th>Surface_Parcelle</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>02</td>
      <td>AA</td>
      <td>10.835598</td>
      <td>369063.408197</td>
      <td>POINT (357090.104 6790476.401)</td>
      <td>8</td>
      <td>35051000AA0089</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>89</td>
      <td>698</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>000-89</td>
      <td>701.243504</td>
    </tr>
    <tr>
      <th>15</th>
      <td>15</td>
      <td>01</td>
      <td>AA</td>
      <td>159.092542</td>
      <td>369063.408197</td>
      <td>POINT (357094.986 6790473.368)</td>
      <td>8</td>
      <td>35051000AA0089</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>89</td>
      <td>698</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>000-89</td>
      <td>701.243504</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>01</td>
      <td>AA</td>
      <td>156.415285</td>
      <td>369063.408197</td>
      <td>POINT (357091.551 6790520.886)</td>
      <td>32</td>
      <td>35051000AA0067</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>67</td>
      <td>576</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>000-67</td>
      <td>583.170712</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>01</td>
      <td>AA</td>
      <td>100.495147</td>
      <td>369063.408197</td>
      <td>POINT (357211.654 6790458.961)</td>
      <td>54</td>
      <td>35051000AA0127</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>127</td>
      <td>455</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>000-127</td>
      <td>456.044515</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>01</td>
      <td>AA</td>
      <td>132.496058</td>
      <td>369063.408197</td>
      <td>POINT (357115.256 6790368.125)</td>
      <td>23</td>
      <td>35051000AA0104</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>104</td>
      <td>650</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>000-104</td>
      <td>654.934381</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Réorganiser la table

BatimentsFinal = BatimentsEtape3[["ID_Bati", "type", "code", "Surface_Bati", "Surface_Section", "ID_Parcelle", "Surface_Parcelle", "geometry"]]
BatimentsFinal.head()
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
      <th>ID_Bati</th>
      <th>type</th>
      <th>code</th>
      <th>Surface_Bati</th>
      <th>Surface_Section</th>
      <th>ID_Parcelle</th>
      <th>Surface_Parcelle</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>02</td>
      <td>AA</td>
      <td>10.835598</td>
      <td>369063.408197</td>
      <td>35051000AA0089</td>
      <td>701.243504</td>
      <td>POINT (357090.104 6790476.401)</td>
    </tr>
    <tr>
      <th>15</th>
      <td>15</td>
      <td>01</td>
      <td>AA</td>
      <td>159.092542</td>
      <td>369063.408197</td>
      <td>35051000AA0089</td>
      <td>701.243504</td>
      <td>POINT (357094.986 6790473.368)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>01</td>
      <td>AA</td>
      <td>156.415285</td>
      <td>369063.408197</td>
      <td>35051000AA0067</td>
      <td>583.170712</td>
      <td>POINT (357091.551 6790520.886)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>01</td>
      <td>AA</td>
      <td>100.495147</td>
      <td>369063.408197</td>
      <td>35051000AA0127</td>
      <td>456.044515</td>
      <td>POINT (357211.654 6790458.961)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>01</td>
      <td>AA</td>
      <td>132.496058</td>
      <td>369063.408197</td>
      <td>35051000AA0104</td>
      <td>654.934381</td>
      <td>POINT (357115.256 6790368.125)</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Faire une carte des batiments catégorisés par code de sections

BatimentsFinal.plot(figsize=(15,15), column='code', markersize=0.5, legend=True)
```




    <AxesSubplot:>




    
![png](output_19_1.png)
    



```python
# Calculer le  nombre de batiments par sections cadastrale

NbBatiSection = BatimentsFinal[["code", "ID_Bati"]].groupby("code").size()
NbBatiSection = pd.DataFrame(NbBatiSection)
NbBatiSection = NbBatiSection.rename(columns={0: 'NbBatis'})
NbBatiSection.head(10)
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
      <th>NbBatis</th>
    </tr>
    <tr>
      <th>code</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>AA</th>
      <td>473</td>
    </tr>
    <tr>
      <th>AB</th>
      <td>263</td>
    </tr>
    <tr>
      <th>AC</th>
      <td>104</td>
    </tr>
    <tr>
      <th>AD</th>
      <td>162</td>
    </tr>
    <tr>
      <th>AE</th>
      <td>268</td>
    </tr>
    <tr>
      <th>AH</th>
      <td>203</td>
    </tr>
    <tr>
      <th>AI</th>
      <td>193</td>
    </tr>
    <tr>
      <th>AK</th>
      <td>326</td>
    </tr>
    <tr>
      <th>AL</th>
      <td>132</td>
    </tr>
    <tr>
      <th>AM</th>
      <td>52</td>
    </tr>
  </tbody>
</table>
</div>




```python
total = NbBatiSection['NbBatis'].sum()
print(total)
```

    8863
    


```python
# Jointure attributaire pour repasser sur la couche des sections cadastrales

Sections2154 = Sections2154.merge(NbBatiSection, on='code')
Sections2154.head()
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
      <th>id</th>
      <th>commune</th>
      <th>prefixe</th>
      <th>code</th>
      <th>created</th>
      <th>updated</th>
      <th>geometry</th>
      <th>Surface_Section</th>
      <th>NbBatis</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>35051000AA</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>2004-06-18</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((357517.470 6790911.435, 357514...</td>
      <td>369063.408197</td>
      <td>473</td>
    </tr>
    <tr>
      <th>1</th>
      <td>35051000AB</td>
      <td>35051</td>
      <td>000</td>
      <td>AB</td>
      <td>2004-05-27</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((357360.791 6790345.960, 357323...</td>
      <td>214289.844646</td>
      <td>263</td>
    </tr>
    <tr>
      <th>2</th>
      <td>35051000AC</td>
      <td>35051</td>
      <td>000</td>
      <td>AC</td>
      <td>2004-05-27</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((357715.587 6790006.212, 357698...</td>
      <td>142514.452758</td>
      <td>104</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35051000AD</td>
      <td>35051</td>
      <td>000</td>
      <td>AD</td>
      <td>2004-05-27</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((358092.442 6790328.433, 358058...</td>
      <td>191803.352173</td>
      <td>162</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35051000AE</td>
      <td>35051</td>
      <td>000</td>
      <td>AE</td>
      <td>2004-05-27</td>
      <td>2017-09-14</td>
      <td>MULTIPOLYGON (((358200.027 6790860.405, 358194...</td>
      <td>228557.377366</td>
      <td>268</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Faire une carte pour représenter le nombre de batiments pas section cadastrale

Sections2154centro = Sections2154.copy()
Sections2154centro.geometry = Sections2154centro['geometry'].centroid


fig, ax = plt.subplots(figsize=(10,10))
Sections2154.plot(ax=ax, color="lightgray", edgecolor="grey", linewidth=0.4)
Sections2154centro.plot(ax=ax,color="#ee0db1", markersize="NbBatis",alpha=0.7, categorical=False, legend=True)
ax.axis("off")
plt.axis("equal")
plt.show()
```


    
![png](output_23_0.png)
    



```python
# Calculer le Nombre de batiments par parcelle cadastrale

NbBatiParcelle  = BatimentsFinal[["ID_Parcelle", "ID_Bati"]].groupby("ID_Parcelle").size()
NbBatiParcelle =pd.DataFrame(NbBatiParcelle)
NbBatiParcelle = NbBatiParcelle.rename(columns={0: 'NbBatis'})
NbBatiParcelle.head(20)
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
      <th>NbBatis</th>
    </tr>
    <tr>
      <th>ID_Parcelle</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>35051000AA0003</th>
      <td>1</td>
    </tr>
    <tr>
      <th>35051000AA0007</th>
      <td>2</td>
    </tr>
    <tr>
      <th>35051000AA0008</th>
      <td>1</td>
    </tr>
    <tr>
      <th>35051000AA0009</th>
      <td>1</td>
    </tr>
    <tr>
      <th>35051000AA0010</th>
      <td>2</td>
    </tr>
    <tr>
      <th>35051000AA0011</th>
      <td>2</td>
    </tr>
    <tr>
      <th>35051000AA0012</th>
      <td>1</td>
    </tr>
    <tr>
      <th>35051000AA0013</th>
      <td>2</td>
    </tr>
    <tr>
      <th>35051000AA0014</th>
      <td>1</td>
    </tr>
    <tr>
      <th>35051000AA0015</th>
      <td>1</td>
    </tr>
    <tr>
      <th>35051000AA0016</th>
      <td>1</td>
    </tr>
    <tr>
      <th>35051000AA0017</th>
      <td>2</td>
    </tr>
    <tr>
      <th>35051000AA0018</th>
      <td>1</td>
    </tr>
    <tr>
      <th>35051000AA0019</th>
      <td>1</td>
    </tr>
    <tr>
      <th>35051000AA0020</th>
      <td>1</td>
    </tr>
    <tr>
      <th>35051000AA0021</th>
      <td>1</td>
    </tr>
    <tr>
      <th>35051000AA0022</th>
      <td>1</td>
    </tr>
    <tr>
      <th>35051000AA0023</th>
      <td>2</td>
    </tr>
    <tr>
      <th>35051000AA0024</th>
      <td>1</td>
    </tr>
    <tr>
      <th>35051000AA0025</th>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
total = NbBatiParcelle['NbBatis'].sum()
print(total)
```

    8863
    


```python
# Jointure attributaire pour repasser sur la couche des parcelles  cadastrales

ParcellesOK = Parcelles2154.merge(NbBatiParcelle, on='ID_Parcelle')
ParcellesOK.head()

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
      <th>ID_Parcelle</th>
      <th>commune</th>
      <th>prefixe</th>
      <th>section</th>
      <th>numero</th>
      <th>contenance</th>
      <th>arpente</th>
      <th>created</th>
      <th>updated</th>
      <th>geometry</th>
      <th>ID_Parcelletest</th>
      <th>Surface_Parcelle</th>
      <th>NbBatis</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>35051000AA0562</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>562</td>
      <td>42655</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357405.271 6790540.522, 357403.462 6...</td>
      <td>000-562</td>
      <td>42587.307820</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>35051000AA0192</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>192</td>
      <td>6752</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357417.028 6790716.924, 357419.271 6...</td>
      <td>000-192</td>
      <td>6609.099724</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>35051000AA0553</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>553</td>
      <td>1290</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357028.302 6790578.606, 357028.060 6...</td>
      <td>000-553</td>
      <td>1287.809973</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35051000AA0059</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>59</td>
      <td>619</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357024.044 6790482.974, 357043.639 6...</td>
      <td>000-59</td>
      <td>621.336887</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35051000AA0089</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>89</td>
      <td>698</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357112.439 6790467.687, 357090.570 6...</td>
      <td>000-89</td>
      <td>701.243504</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Faire une carte pour représenter le nombre de batiments par parcelle cadastrale

Parcellescentro = ParcellesOK.copy()
Parcellescentro.geometry = Parcellescentro['geometry'].centroid


fig, ax = plt.subplots(figsize=(16,16))
Parcelles2154.plot(ax=ax, color="lightgray", edgecolor="grey", linewidth=0.1)
Parcellescentro.plot(ax=ax,color="#07424A", markersize="NbBatis",alpha=0.7, categorical=False, legend=True)
ax.axis("off")
plt.axis("equal")
plt.show()
```


    
![png](output_27_0.png)
    



```python
# Calculer la proportion des surfaces bati par parcelle cadastrale

recap = BatimentsFinal.groupby('ID_Parcelle').agg({'Surface_Bati':'sum','Surface_Parcelle':'max'})
recap["propbatiparcelle"] = recap["Surface_Bati"] / recap["Surface_Parcelle"] *100
recap.head()

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
      <th>Surface_Bati</th>
      <th>Surface_Parcelle</th>
      <th>propbatiparcelle</th>
    </tr>
    <tr>
      <th>ID_Parcelle</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>35051000AA0003</th>
      <td>6.689129</td>
      <td>126.838334</td>
      <td>5.273744</td>
    </tr>
    <tr>
      <th>35051000AA0007</th>
      <td>97.564337</td>
      <td>702.924188</td>
      <td>13.879781</td>
    </tr>
    <tr>
      <th>35051000AA0008</th>
      <td>135.133297</td>
      <td>639.498175</td>
      <td>21.131147</td>
    </tr>
    <tr>
      <th>35051000AA0009</th>
      <td>102.497395</td>
      <td>632.568063</td>
      <td>16.203378</td>
    </tr>
    <tr>
      <th>35051000AA0010</th>
      <td>99.817772</td>
      <td>796.936781</td>
      <td>12.525181</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Faire la jointure attributaire avec la couche des parcelles cadastrales

ParcellesOK = Parcelles2154.merge(recap, on='ID_Parcelle')

fig, ax = plt.subplots(figsize=(16,16))
Parcelles2154.plot(ax=ax, color="lightgray", edgecolor="black", linewidth=0.1)
ParcellesOK.plot(ax=ax, column='propbatiparcelle', cmap='Spectral_r',scheme='quantiles', legend=True)
ax.axis("off")
plt.axis("equal")
plt.show()



ParcellesOK.head()

```


    
![png](output_29_0.png)
    





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
      <th>ID_Parcelle</th>
      <th>commune</th>
      <th>prefixe</th>
      <th>section</th>
      <th>numero</th>
      <th>contenance</th>
      <th>arpente</th>
      <th>created</th>
      <th>updated</th>
      <th>geometry</th>
      <th>ID_Parcelletest</th>
      <th>Surface_Parcelle_x</th>
      <th>Surface_Bati</th>
      <th>Surface_Parcelle_y</th>
      <th>propbatiparcelle</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>35051000AA0562</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>562</td>
      <td>42655</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357405.271 6790540.522, 357403.462 6...</td>
      <td>000-562</td>
      <td>42587.307820</td>
      <td>8.866531</td>
      <td>42587.307820</td>
      <td>0.020820</td>
    </tr>
    <tr>
      <th>1</th>
      <td>35051000AA0192</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>192</td>
      <td>6752</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357417.028 6790716.924, 357419.271 6...</td>
      <td>000-192</td>
      <td>6609.099724</td>
      <td>7.467778</td>
      <td>6609.099724</td>
      <td>0.112992</td>
    </tr>
    <tr>
      <th>2</th>
      <td>35051000AA0553</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>553</td>
      <td>1290</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357028.302 6790578.606, 357028.060 6...</td>
      <td>000-553</td>
      <td>1287.809973</td>
      <td>434.213558</td>
      <td>1287.809973</td>
      <td>33.717207</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35051000AA0059</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>59</td>
      <td>619</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357024.044 6790482.974, 357043.639 6...</td>
      <td>000-59</td>
      <td>621.336887</td>
      <td>125.324059</td>
      <td>621.336887</td>
      <td>20.170066</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35051000AA0089</td>
      <td>35051</td>
      <td>000</td>
      <td>AA</td>
      <td>89</td>
      <td>698</td>
      <td>False</td>
      <td>2004-06-18</td>
      <td>2019-11-21</td>
      <td>POLYGON ((357112.439 6790467.687, 357090.570 6...</td>
      <td>000-89</td>
      <td>701.243504</td>
      <td>169.928140</td>
      <td>701.243504</td>
      <td>24.232401</td>
    </tr>
  </tbody>
</table>
</div>



#Ecrire un shapefile

ParcellesOK.to_file("C:/Users/Xo/Desktop/Cadastre/test1.shp")
