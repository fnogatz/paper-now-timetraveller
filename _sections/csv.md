---
title: CSV as Data Source
order: 5
---

Another simple data format to store traces is CSV. This way it is possible to specify each position at a given time by a single line in a file.

We implemented a general importer for CSV files as part of our [zeitpunkt](#source-code-zeitpunkt) module. It creates GeoJSON `LineString` documents with time components as specified in the [previous section](#geojson-linestrings-with-time-components). Although with [mapbox/csv2geojson](https://github.com/mapbox/csv2geojson) there already existed a similar tool, we had to ensure numeric values for the time component <small>(see [#31](https://github.com/mapbox/csv2geojson/issues/31))</small> and to handle the traces of multiple vehicles in a single file, distinguished only by an additional `id` column.

Our importer can be used programmatically as well as command line tool. It supports multiple options, for example to convert the coordinates from one coordinate system to another. Being able to import the network simulation traces created by [Veins](http://veins.car2x.org/) (UTM coordinates) as well as the open data of all [Taxi Trajectories of Beijing](http://research.microsoft.com/apps/pubs/?id=152883) (WGS84, different column order and delimiter), we can emphasize the general applicability of the import tool.
