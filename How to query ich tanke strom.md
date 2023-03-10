# How to query the ich-tanke-strom-Feature-API

## General remarks

* [Common Query Language](https://docs.geoserver.org/latest/en/user/tutorials/cql/cql_tutorial.html) and [Filter-Functions](https://docs.geoserver.org/latest/en/user/filter/function_reference.html#filter-function-reference)
* [Filter Function Reference](https://docs.geoserver.org/latest/en/user/filter/function_reference.html#filter-function-reference) and [ECQL Reference](https://docs.geoserver.org/stable/en/user/filter/ecql_reference.html#filter-ecql-reference)
* Multiple filters can be combined with `OR` or `AND`
* Arithmetic expressions calculated with `+, -, *, /` can be used on the left and/or right side of an operator
* `%` as wildcard for string comparisons (note: must be encoded correctly as `%25` for queries in URL)
* Nested attributes can be used with ".", e.g. ChargingFacilities.Power > 22

| Data type    | Operators | Examples |
| --------------- | --------- |--------- |
| string | =, like, ilike, in, is null, not like, not ilike (% als Wildcard) | city = ‘Zürich', city ilike ‘%ich’, city in (’Bern', 'Zürich') |
| number |  =, <>, >, <, >=, <= |  |
| boolean | = true, = false | IsOpen24Hours = true |


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
