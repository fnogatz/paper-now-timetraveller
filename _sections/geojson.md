---
title: "GeoJSON: LineStrings with Time Components"
order: 3
---

To ensure the support of multiple data source formats we have to determine a basic format used by all tools in the chain. This way it is possible to write parsers for various formats and implement basic functionality like the polyline simplification algorithm only once.

A very widely used format to exchange geospatial data is [GeoJSON](http://geojson.org/geojson-spec.html). It is based on JSON, the JavaScript Object Notation. Although GeoJSON is very common, there is currently no (de-facto) standard to store the time components along a `LineString`, `MultiPoint` or `FeatureCollection` of `Point`s.

At the end we defined the following GeoJSON instance, already supported by the tools [LeafletPlayback](#additional-repositories-leafletplayback) and [csv2geojson by mapbox](https://github.com/mapbox/csv2geojson), as the standard format:

	{
	  "type": "Feature",
	  "geometry": {
	    "type": "LineString",  // or: "MultiPoint"
	    "coordinates": [       // array of [lng,lat] coordinates
	      [ 10.0304153141,      48.4343540454     ],
	      [ 10.030424406259081, 48.43435113062047 ],
	      ...
	    ]
	  },
	  "properties": {
	    "time": [              // array of timestamps
	      1433907300000,
	      1433907300088,
	      ...
	    ]
	  }
	}

To support logical times, the timestamps don't have to be UNIX timestamps.

Prior to version 1.0 our [timetraveller](#source-code-timetraveller) module used a GeoJSON `FeatureCollection` of `Point`s as `Feature`s to store the time component directly at the related point. While this seems to be reasonable, it creates a huge amount of GeoJSON meta data. Besides that, MongoDB, the database later used to store the traces, has built-in geospatial queries that support `LineString`s better than `FeatureCollection`s.
