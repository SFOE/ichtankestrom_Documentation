# How to query the DIEMO-API

## Available services

All services of [api3.geo.admin.ch](http://api3.geo.admin.ch/services/sdiservices.html) are available. To query the National Data Infrastructure For Electromobility, two services are of special interest:
* [Identify](http://api3.geo.admin.ch/services/sdiservices.html#identify-features): This service can be used to discover charging points at a specific location. Additionally, attributes can be searched. Use the layerDefs-functionality to query specific attributes.
* [Find](http://api3.geo.admin.ch/services/sdiservices.html#find): This service is used to search the attributes of charging points. The specific location of features is not taken into account. Use the layerDefs-functionality to query specific attributes.

## General remarks
* There is a limit of 200 results per query
* [List of available queryable attributes](https://api3.geo.admin.ch/rest/services/all/MapServer/ch.bfe.ladestellen-elektromobilitaet?lang=de)
* The layerDefs-functionality does not support **OR** and string search is not case sensistive
* layerDefs is adapted from [ESRI](https://developers.arcgis.com/rest/services-reference/identify-map-service-.htm)


| Data type    | Operators | Examples |
| --------------- | --------- |--------- |
| varchar | =, +=, like, ilike, not like, not ilike, is null, is not null | toto ='3455 Kloten', toto ilike '%SH%', toto is null, toto ilike 'SH%' |
| number |  =, <, >, >=, <=, != | tutu >= 2.4, tutu < 5 |
| boolean | is (true\|false), is not (true\|false) | tata is not false |


## Identify examples (discover features at a specific location)

### Example 1

[Stations within a distance of 300 m from coordinate 2'600'000 / 1'200'000](https://api3.geo.admin.ch/rest/services/all/MapServer/identify?geometry=2600000,1200000&mapExtent=0,0,100,100&imageDisplay=100,100,100&geometryFormat=geojson&geometryType=esriGeometryPoint&lang=fr&layers=all:ch.bfe.ladestellen-elektromobilitaet&returnGeometry=true&tolerance=300&sr=2056)

```
https://api3.geo.admin.ch/rest/services/all/MapServer/identify?
geometry=2600000,1200000&
mapExtent=0,0,100,100&
imageDisplay=100,100,100&
geometryFormat=geojson&
geometryType=esriGeometryPoint&
lang=de&
layers=all:ch.bfe.ladestellen-elektromobilitaet&
returnGeometry=true&
tolerance=300&
sr=2056
```

Additionally, [IsOpen24Hours is true](https://api3.geo.admin.ch/rest/services/all/MapServer/identify?geometry=2600000,1200000&mapExtent=0,0,100,100&imageDisplay=100,100,100&geometryFormat=geojson&geometryType=esriGeometryPoint&lang=fr&layers=all:ch.bfe.ladestellen-elektromobilitaet&returnGeometry=true&tolerance=300&sr=2056&layerDefs={%22ch.bfe.ladestellen-elektromobilitaet%22:%22IsOpen24Hours%20is%20true%22})

```
&layerDefs={"ch.bfe.ladestellen-elektromobilitaet": "IsOpen24Hours is true"}
```
Additionally, [Authentication with NFC](https://api3.geo.admin.ch/rest/services/all/MapServer/identify?geometry=2600000,1200000&mapExtent=0,0,100,100&imageDisplay=100,100,100&geometryFormat=geojson&geometryType=esriGeometryPoint&lang=fr&layers=all:ch.bfe.ladestellen-elektromobilitaet&returnGeometry=true&tolerance=300&sr=2056&layerDefs={%22ch.bfe.ladestellen-elektromobilitaet%22:%20%22IsOpen24Hours%20is%20true%22,%20%22ch.bfe.ladestellen-elektromobilitaet%22:%22QueryAuthenticationModes%20ilike%20%27%nfc%%27%22})

```
&layerDefs={
"ch.bfe.ladestellen-elektromobilitaet": "IsOpen24Hours is true", 
"ch.bfe.ladestellen-elektromobilitaet": "QueryAuthenticationModes ilike '%nfc%'"}
```

Additionally, [Longitude > 7.43842](https://api3.geo.admin.ch/rest/services/all/MapServer/identify?geometry=2600000,1200000&mapExtent=0,0,100,100&imageDisplay=100,100,100&geometryFormat=geojson&geometryType=esriGeometryPoint&lang=fr&layers=all:ch.bfe.ladestellen-elektromobilitaet&returnGeometry=true&tolerance=300&sr=2056&layerDefs={%22ch.bfe.ladestellen-elektromobilitaet%22:%20%22IsOpen24Hours%20is%20true%22,%20%22ch.bfe.ladestellen-elektromobilitaet%22:%22QueryAuthenticationModes%20ilike%20%27%nfc%%27%22,%20%22ch.bfe.ladestellen-elektromobilitaet%22:%22Longitude%20%3E%207.43842%22})

```
&layerDefs={
"ch.bfe.ladestellen-elektromobilitaet": "IsOpen24Hours is true", 
"ch.bfe.ladestellen-elektromobilitaet": "QueryAuthenticationModes ilike '%nfc%'", 
"ch.bfe.ladestellen-elektromobilitaet": "Longitude > 7.43842"}
```

### Example 2

[Identify all the features intersecting an bounding box around the village Puidoux](https://api3.geo.admin.ch/rest/services/api/MapServer/identify?geometryType=esriGeometryEnvelope&geometry=2547800,1148679,2549444,1150013&imageDisplay=3600,2400,96&mapExtent=2480000,170000,2840000,1310000&tolerance=0&layers=all:ch.bfe.ladestellen-elektromobilitaet&sr=2056)

```
https://api3.geo.admin.ch/rest/services/api/MapServer/identify?
geometryType=esriGeometryEnvelope&
geometry=2547800,1148679,2549444,1150013&
imageDisplay=3600,2400,96&
mapExtent=2480000,170000,2840000,1310000&
tolerance=0&
layers=all:ch.bfe.ladestellen-elektromobilitaet&
sr=2056
```

Additionally, [Plug like Type 2](
https://api3.geo.admin.ch/rest/services/api/MapServer/identify?geometryType=esriGeometryEnvelope&geometry=2547800,1148679,2549444,1150013&imageDisplay=3600,2400,96&mapExtent=2480000,170000,2840000,1310000&tolerance=0&layers=all:ch.bfe.ladestellen-elektromobilitaet&sr=2056&layerDefs={%22ch.bfe.ladestellen-elektromobilitaet%22:%20%22QueryPlugs%20ilike%20%27%Type%202%%27%22})

```
&layerDefs={"ch.bfe.ladestellen-elektromobilitaet": "QueryPlugs ilike '%Type 2%'"}
```

### Example 3

[Identify all the features intersecting an polygon around Chur](https://api3.geo.admin.ch/rest/services/api/MapServer/identify?geometryType=esriGeometryPolygon&geometry={%22rings%22%20:%20[[%20[2758610,1196685],%20[2765510,1188085],%20[2750210,1188135],%20[2758610,1196685]]]}&imageDisplay=3600,2400,96&mapExtent=2480000,170000,2840000,1310000&tolerance=0&layers=all:ch.bfe.ladestellen-elektromobilitaet&sr=2056)

```
https://api3.geo.admin.ch/rest/services/api/MapServer/identify?
geometryType=esriGeometryPolygon&
geometry={"rings" : [[ [2758610,1196685], [2765510,1188085], [2750210,1188135], [2758610,1196685]]]}&
imageDisplay=3600,2400,96&
mapExtent=2480000,170000,2840000,1310000&
tolerance=0&
layers=all:ch.bfe.ladestellen-elektromobilitaet&
sr=2056
```

Additionally, [Plug like Type 2](https://api3.geo.admin.ch/rest/services/api/MapServer/identify?geometryType=esriGeometryPolygon&geometry={%22rings%22%20:%20[[%20[2758610,1196685],%20[2765510,1188085],%20[2750210,1188135],%20[2758610,1196685]]]}&imageDisplay=3600,2400,96&mapExtent=2480000,170000,2840000,1310000&tolerance=0&layers=all:ch.bfe.ladestellen-elektromobilitaet&sr=2056&layerDefs={%22ch.bfe.ladestellen-elektromobilitaet%22:%20%22QueryPlugs%20ilike%20'%Type%202%'%22})

```
&layerDefs={"ch.bfe.ladestellen-elektromobilitaet": "QueryPlugs ilike '%Type 2%'"}
```


## Find examples (search the attributes of features)

### Example 1

[Search for “ich” in the field “City” (infix match)](https://api3.geo.admin.ch/rest/services/all/MapServer/find?layer=ch.bfe.ladestellen-elektromobilitaet&searchText=ich&searchField=City&returnGeometry=false)

```
https://api3.geo.admin.ch/rest/services/all/MapServer/find?
layer=ch.bfe.ladestellen-elektromobilitaet&
searchText=ich&
searchField=City&
returnGeometry=false
```

Additionally, [IsOpen24Hours is true](https://api3.geo.admin.ch/rest/services/all/MapServer/find?layer=ch.bfe.ladestellen-elektromobilitaet&searchText=ich&searchField=City&returnGeometry=false&layerDefs={%22ch.bfe.ladestellen-elektromobilitaet%22:%20%22IsOpen24Hours%20is%20true%22})

```
&layerDefs={"ch.bfe.ladestellen-elektromobilitaet": "IsOpen24Hours is true"}
```
