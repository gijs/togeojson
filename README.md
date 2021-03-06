# Convert things to GeoJSON.

[![Build Status](https://travis-ci.org/tmcw/togeojson.png)](https://travis-ci.org/tmcw/togeojson)

This converts [KML](https://developers.google.com/kml/documentation/) into
[GeoJSON](http://www.geojson.org/), in a browser or with [nodejs](http://nodejs.org/).

It is

* Dependency-free
* Tiny
* Written in vanilla javascript that's jshint-friendly
* Tested

It is not

* Concerned about ugly extensions to KML
* Concerned with having an 'internal format' of its own

## Using it as a console utility

Install it into your path with `npm install -g togeojson`.

```
~> togeojson file.kml > file.geojson
```

## Using it as a nodejs library

Install it into your project with `npm install --save togeojson`.

```javascript
// using togeojson in nodejs

var tj = require('togeojson'),
    fs = require('fs'),
    // node doesn't have xml parsing or a dom. use jsdom
    jsdom = require('jsdom').jsdom;

var kml = jsdom(fs.readFileSync('foo.kml', 'utf8'));

var converted = tj.kml(kml);

var converted_with_styles = tj.kml(kml, { styles: true });
```

## Using it as a browser library

Download it into your project like

    wget https://raw.github.com/tmcw/togeojson/gh-pages/togeojson.js

```html
<script src='jquery.js'></script>
<script src='togeojson.js'></script>
<script>
$.ajax('test/data/linestring.kml').done(function(xml) {
    console.log(toGeoJSON.kml(xml));
});
</script>
```

toGeoJSON doesn't include AJAX - you can use [jQuery](http://jquery.com/),
[reqwest](https://github.com/ded/reqwest), [d3](http://d3js.org/), or anything
else that can request an XML document.

## KML

Supported:

* Point
* Polygon
* LineString
* name & description
* ExtendedData
* SimpleData
* MultiGeometry -> GeometryCollection
* Styles with hashing

Not supported yet:

* Various silly Google extensions (will never be supported)
* NetworkLinks
* GroundOverlays

## FAQ

### What is hashing?

KML's style system isn't semantic: a typical document made through official tools
(read Google) has hundreds of identical styles. So, togeojson does its best to
make this into something usable, by taking a quick hash of each style and exposing
`styleUrl` and `styleHash` to users. This lets you work backwards from the awful
representation and build your own styles or derive data based on the classes
chosen.

Implied here is that this does not try to represent all data contained in KML
styles.
