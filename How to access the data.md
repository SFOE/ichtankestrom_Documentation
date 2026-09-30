# Access the data
> **Note**: The information on this page applies from November 2026. Until then, please use the [current data access](https://github.com/SFOE/ichtankestrom_Documentation/blob/main/ich-tanke-strom-old/Access%20Download%20the%20data%20old).


ich-tanke-strom.ch aggregates the data of charging point operators in real-time. The data is available as:

* GeoJSON files
* JSON (via API)

**Terms of use**: Open use. Must provide the source. Use for commercial purposes requires permission of the data owner.

* You may use this dataset for non-commercial purposes.
* You may use this dataset for commercial purposes, but you must seek prior permission from the data owner.
* You must provide the source (author, title and link to the dataset).

See [opendata.swiss](https://opendata.swiss/dataset/ladestationen-fuer-elektroautos/) for more information.

## GeoJSON files

GeoJSON is an open standard for geographic data. The map on [www.ich-tanke-strom.ch](https://www.ich-tanke-strom.ch) is based on these GeoJSON files. The files contain information on the charging points, aggregated into locations, as well as HTML code for the display on the map.

The files are updated every 30 to 60 seconds. There is one file per language (German, French, Italian and English). You can also access the GeoJSON files directly:

* German: https://data.geo.admin.ch/ch.bfe.ladestellen-elektromobilitaet/data/ch.bfe.ladestellen-elektromobilitaet_de.json
* French: https://data.geo.admin.ch/ch.bfe.ladestellen-elektromobilitaet/data/ch.bfe.ladestellen-elektromobilitaet_fr.json
* Italian: https://data.geo.admin.ch/ch.bfe.ladestellen-elektromobilitaet/data/ch.bfe.ladestellen-elektromobilitaet_it.json
* English: https://data.geo.admin.ch/ch.bfe.ladestellen-elektromobilitaet/data/ch.bfe.ladestellen-elektromobilitaet_en.json

## ich-tanke-strom.ch API

The ich-tanke-strom.ch API is a REST API that provides the charging point and tariff data of various CPOs, aggregated by ich-tanke-strom.ch, read-only to external data consumers. The data is returned as JSON based on OCPI. Three endpoints are available:

* **Locations API**: charging locations with their charging points (EVSEs) and connectors
* **Tariffs API**: tariffs of the CPOs
* **EVSE Status API**: current availability of the charging points

See [How to query the ich-tanke-strom.ch API](https://github.com/SFOE/ichtankestrom_Documentation/blob/main/How%20to%20query%20ich%20tanke%20strom.md) for details.

## Data model

* API: [OCPI 2.3.0](https://evroaming.org/wp-content/uploads/2025/02/OCPI-2.3.0.pdf) & [OCPI 2.2.1](https://evroaming.org/wp-content/uploads/2024/11/OCPI-2.2.1-d2.pdf)
* GeoJSON: [GeoJSON.org](https://geojson.org/)
