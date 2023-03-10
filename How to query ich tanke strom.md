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

There are two functionalities of the API:
* **Identify**: Used to retrieve charging stations at / near a specified location. Additionally, it can be searched / filtered by attributes of the charging stations. 
* **Find**: Used to search / filter for attributes of the charging stations. The exact location of the charging stations is not taken into account.

## Identify examples (discover features at a specific location)

### Example 1

[Stations within a distance of 300 m from coordinate 48.0, 12.0](http://ich-tanke-strom-int.switzerlandnorth.cloudapp.azure.com:8080/geoserver/ich-tanke-strom/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=ich-tanke-strom%3Aevse&maxFeatures=50&outputFormat=application%2Fjson&cql_filter=DWithin(geometry,POINT(12%2048),300,meters))

```
http://ich-tanke-strom-int.switzerlandnorth.cloudapp.azure.com:8080/geoserver/ich-tanke-strom/ows?
service=WFS&
version=1.0.0&
request=GetFeature&
typeName=ich-tanke-strom%3Aevse&
maxFeatures=50&
outputFormat=application%2Fjson&
cql_filter=DWithin(geometry,POINT(12%2048),300,meters)
```

Additionally, [IsOpen24Hours = true](http://ich-tanke-strom-int.switzerlandnorth.cloudapp.azure.com:8080/geoserver/ich-tanke-strom/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=ich-tanke-strom%3Aevse&maxFeatures=50&outputFormat=application%2Fjson&cql_filter=DWithin(geometry,POINT(12%2048),300,meters)AND%20IsOpen24Hours=true)

```
&cql_filter = DWithin(geometry,POINT(12 48),300,meters) AND IsOpen24Hours=true
```
Additionally, [Authentication with NFC](http://ich-tanke-strom-int.switzerlandnorth.cloudapp.azure.com:8080/geoserver/ich-tanke-strom/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=ich-tanke-strom%3Aevse&maxFeatures=50&outputFormat=application%2Fjson&cql_filter=DWithin(geometry,POINT(12%2048),300,meters)%20AND%20IsOpen24Hours=true%20AND%20AuthenticationModes%20ILIKE%20%27%25nfc%25%27)

```
&cql_filter = DWithin(geometry,POINT(12 48),300,meters) AND IsOpen24Hours=true AND AuthenticationModes ilike '%nfc%'
```

### Example 2

[Identify all the features intersecting an bounding box around the village Puidoux](http://ich-tanke-strom-int.switzerlandnorth.cloudapp.azure.com:8080/geoserver/ich-tanke-strom/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=ich-tanke-strom%3Aevse&maxFeatures=50&outputFormat=application%2Fjson&cql_filter=bbox(geometry,11,48,12,59))

```
&cql_filter = bbox(geometry,11,48,12,49)
```

Additionally, [Plug like Type 2](http://ich-tanke-strom-int.switzerlandnorth.cloudapp.azure.com:8080/geoserver/ich-tanke-strom/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=ich-tanke-strom%3Aevse&maxFeatures=50&outputFormat=application%2Fjson&cql_filter=bbox(geometry,11,48,12,59)%20AND%20Plugs%20ILIKE%20%27%25Type%202%25%27)

```
&cql_filter = bbox(geometry,11,48,12,49) AND Plugs ilike '%Type 2%'
```

### Example 3

[Identify all the features intersecting an polygon](http://ich-tanke-strom-int.switzerlandnorth.cloudapp.azure.com:8080/geoserver/ich-tanke-strom/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=ich-tanke-strom%3Aevse&maxFeatures=50&outputFormat=application%2Fjson&cql_filter=Intersects(geometry,POLYGON((11%2045,%2012%2045,%2012%2050,%2011%2050,%2011%2045))))

```
&cql_filter = Intersects(geometry,POLYGON((11 45, 12 45, 12 50, 11 50, 11 45)))
```

Additionally, [Plug like Type 2](http://ich-tanke-strom-int.switzerlandnorth.cloudapp.azure.com:8080/geoserver/ich-tanke-strom/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=ich-tanke-strom%3Aevse&maxFeatures=50&outputFormat=application%2Fjson&cql_filter=Intersects(geometry,POLYGON((11%2045,%2012%2045,%2012%2050,%2011%2050,%2011%2045)))%20AND%20Plugs%20ILIKE%20%27%25Type%202%25%27)

```
&cql_filter = Intersects(geometry,POLYGON((11 45, 12 45, 12 50, 11 50, 11 45))) AND Plugs ilike '%Type 2%'
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
