---
title: GTFS as Data Source
order: 4
---

A very common dataset with entities of changing positions over time is the schedule of a public transit agency. The buses, trams and ferrys have discrete times on bus stops and stations. And for most agencies the real traces in between are known.

As an interoperable data format to specify transportation schedules GTFS has been established. Its [specification](https://developers.google.com/transit/gtfs/) states:

> The General Transit Feed Specification (GTFS) defines a common format for public transportation schedules and associated geographic information. GTFS "feeds" allow public transit agencies to publish their transit data and developers to write applications that consume that data in an interoperable way.

Today, [more than 900 transit agencies worldwide](http://www.gtfs-data-exchange.com/agencies) have published their transportation schedules in this format. Among them: the [Stadtwerke Ulm](http://www.swu.de/privatkunden/swu-nahverkehr/gtfs-daten.html) from Ulm, Germany. Like any other GTFS dataset it is only a collection of CSV files of a specified format, that means that a GTFS dataset in general contains a list of files of comma-separated values. Below the beginning of the `stop_times.txt` is presented as an example:

	trip_id,arrival_time,departure_time,stop_id,stop_sequence
	87001R0-0535,05:35:00,05:35:00,900107011,1
	87001R0-0535,05:36:00,05:36:00,900107212,2
	87001R0-0535,05:37:00,05:37:00,900107312,3
	87001R0-0535,05:38:00,05:38:00,900107412,4

The GTFS format makes use of unique identifiers to refer to entities of different CSV files. To simplify the use of the provided GTFS files we created [transportation](#source-code-transportation), a node.js module that parses the given files and returns a semantic representation.

### Extrapolation of Traces

With [transportation](#source-code-transportation) it is also possible to combine the trace of a vehicle, specified in the file `shape.txt` of the GTFS data, with the given stop times of `stop_times.txt`. By searching for the nearest position of the trace from a stop we can extrapolate the time of every position in the vehicle's trace. The following example shows a part of the positions array (displayed as table with the help of [tconsole](#source-code-tconsole)). The first line shows a regular point (note the time of `05:35:00`), whereas the time components of the following points have been extrapolated based on their position.

	> konsole(t.agencies.SWU.routes[87001]
	           .trips['87001R0-0535'].positions)
	┌───────────────┬───────────────┬──────────────┐
	│ Lon           │ Lat           │ Time         │
	├───────────────┼───────────────┼──────────────┤
	│ 10.0304153141 │ 48.4343540454 │ 05:35:00     │
	├───────────────┼───────────────┼──────────────┤
	│ 10.0304244062 │ 48.4343511306 │ 05:35:00.088 │
	├───────────────┼───────────────┼──────────────┤
	│ 10.0299151142 │ 48.4345077263 │ 05:35:05.020 │
	├───────────────┼───────────────┼──────────────┤
	│ 10.0295294398 │ 48.4346278873 │ 05:35:08.763 │
	└───────────────┴───────────────┴──────────────┘

The traces can either be used programmatically or exported as GeoJSON LineStrings with time components as specified in the [previous section](#geojson-linestrings-with-time-components).

