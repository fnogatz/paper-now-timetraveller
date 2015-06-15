---
title: Polyline Simplification
order: 6
---

In order to simplify the polyline of a given GeoJSON linestring, the [zeitpunkt](#source-code-zeitpunkt) module provides multiple transformation tools, which are demonstrated in the following code snippet:

    // remove invalid points
    geojson = zeitpunkt.clean({
      lat: [30,40] // only points with latitude between 30 and 40
    }, geojson)

    // simplify the given linestring
    geojson = zeitpunkt.simplify({
      tolerance: 0.01,
      highQuality: true
    }, geojson)

    // split at 1000 points
    geojson = zeitpunkt.split({
      points: 1000
    }, geojson)

The `geojson.clean()` method removes points outside of a given longitude, latitude or time range. This way it can be ensured that the remaining points are only of a given bounding box. The method is also useful to remove points whose coordinates are not valid at all, for example if the latitude is greater than 90.

With the help of `zeitpunkt.simplify()` it is possible to reduce the number of points in a polyline while retaining its shape. The underlying [simplify.js](https://github.com/mourner/simplify-js) library would support 3D polyline simplification, i.e. it would be possible to take the time into account too. However the 2D simplification is distinctly faster and sufficient for the tested use cases.

`zeitpunkt.split()` simply splits a given GeoJSON `LineString` at a given number of points. This is necessary for continuous data streams like the [Taxi Trajectories of Beijing](http://research.microsoft.com/apps/pubs/?id=152883).
