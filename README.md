# OSM Liberty Topo [![BSD licensed](https://img.shields.io/badge/license-BSD-blue.svg)](https://github.com/maputnik/osm-liberty/blob/gh-pages/LICENSE.md)

<img align="right" alt="OSM Liberty" src="logo.png" />

A free Mapbox GL basemap style for everyone with complete liberty to use and
self host. OSM Liberty Topo is a fork of OSM Liberty, which is itself a fork of
OSM Bright. OSM Liberty Topo is based on free data sources with a mission for a
clear good looking design for the everyday user. It is based on the vector tile
schema of [OpenMapTiles](https://github.com/openmaptiles/openmaptiles).

**[Preview OSM Liberty Topo with Maputnik](https://raw.githubusercontent.com/nst-guide/osm-liberty-topo/gh-pages/style.json)**


## Overview

The main changes of this fork compared to the original:

- Contours layer, using 40' contours data from USGS, created from [this repository](https://github.com/nst-guide/contours).
- Hillshade layer, using generated tiles conforming to the Mapbox Terrain RGB standard, created from [this repository](https://github.com/nst-guide/hillshade)
- Uses Open Sans fonts instead of Roboto.
- Puts more focus on trails, and adds mountain peaks

#### `style.json` vs `style-png.json`

When creating a [terrain-rgb hillshade](https://github.com/nst-guide/hillshade),
the output tiles can be in either `png` or `webp` format. The `webp` output
format has ~30-40% smaller file sizes, which mean less bandwidth cost and faster
load times for users. However, older browsers and all iOS browsers don't
currently support `webp` images, so I store `png` images as a fallback.

I develop with `style.json` and then copy changes to `style-png.json`, changing
just the terrain rgb source.

#### Fonts

Currently, the only fonts used are:

- Open Sans Regular
- Open Sans Semibold Italic
- Open Sans Italic

The full ranges for each of these three font stacks are in the `fonts/`
directory. Alternatively, pregenerated Open Sans and Roboto font stacks can be
downloaded from
[openmaptiles/fonts](https://github.com/openmaptiles/fonts/releases).

## Usage

You can use this style in your Mapbox GL maps by copying the `style.json` file
and pointing to its URL whenever Mapbox asks for a style URL.

You will, however, need to adjust the `source` URLs in `style.json` to point to
other sources. For the OSM vector tiles, you can either subscribe to [Maptiler
Cloud](https://www.maptiler.com/cloud/) or generate them yourself with the
[OpenMapTiles](https://github.com/openmaptiles/openmaptiles) project.

For the hillshading and contours layers, you'll need to generate them yourself.
Instructions for generating the hillshade are
[here](https://github.com/nst-guide/hillshade), and instructions for the
contours are [here](https://github.com/nst-guide/contours). Currently, these
repositories use data from the US Geological Survey, and thus work only for the
United States.

By default, the fonts and sprites are served from this Github repository.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset=utf-8 />
  <title>OSM Liberty Topo</title>
  <meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' />
  <style>
    body { margin:0; padding:0; }
    #map { position:absolute; top:0; bottom:0; width:100%; }
  </style>
  <script src='https://api.tiles.mapbox.com/mapbox-gl-js/v1.6.0/mapbox-gl.js'></script>
  <link href='https://api.tiles.mapbox.com/mapbox-gl-js/v1.6.0/mapbox-gl.css' rel='stylesheet' />
</head>
<body>
  <div id='map'></div>
  <script>
  var map = new mapboxgl.Map({
      container: 'map',
      style: 'https://raw.githubusercontent.com/nst-guide/osm-liberty-topo/gh-pages/style.json',
      center: [-119.5936, 37.7456],
      zoom: 12,
      hash: true
  });

  map.addControl(new mapboxgl.NavigationControl());
  </script>
</body>
</html>
```

## Data Sources

- [OpenMapTiles](http://openmaptiles.org/) as vector data source
- [Natural Earth Tiles](https://klokantech.github.io/naturalearthtiles/) for low-zoom relief shading
- [Maki](https://www.mapbox.com/maki-icons/) as icon set
- [USGS 1/3rd arc-second DEM files](https://www.usgs.gov/core-science-systems/ngp/3dep/about-3dep-products-services)
- USGS 40' contours

## Map Design

The map design originates from OSM Bright but strives to reach a unobtrusive and clean design for everyday use.
Colored relief shading from Natural Earth make the low zoom levels look good.

<!-- [![OSM Liberty Map demo](demo/zoom.gif)](https://maputnik.github.io/osm-liberty/) -->

## Edit the Style

You can [edit the style directly online in Maputnik](https://maputnik.github.io/editor?style=https://raw.githubusercontent.com/nst-guide/osm-liberty-topo/gh-pages/style.json).

## Icon Design

A [Maki](https://github.com/mapbox/maki) icon set using colors to distinguish between icon categories.

**Color Palette**

Color Name   | Hex Value
-------------|----------
Blue         | `#5d60be`
Light Blue   | `#4898ff`
Orange       | `#d97200`
Red          | `#ba3827`
Brown        | `#725a50`
Green        | `#76a723`

**Modify Icons**

1. Take the `iconset.json` and import it to the [Maki Editor](https://www.mapbox.com/maki-icons/editor/).
2. Apply your changes and download the icons in SVG format and the iconset in JSON format.
3. Optional: Format the JSON with `cat iconset.json | jq -MS '.'` for better legibility.
4. Add the SVG files from the folder [svgs_not_in_iconset](https://github.com/maputnik/osm-liberty/tree/gh-pages/svgs/svgs_not_in_iconset) to the folder `svgs` downloaded from the Maki Editor.
These are the SVGs for road shields, the dot used for city and town layers and the road area pattern which could not be modified using the Maki Editor. To modify these you could use e.g. [Inkscape](https://inkscape.org).
5. Install [spritezero-cli](https://github.com/mapbox/spritezero-cli): `npm install -g @mapbox/spritezero-cli`
6. Generate the low resolution sprite: `spritezero osm-liberty ./svgs/`
7. Generate the high resolution sprite: `spritezero --retina osm-liberty@2x ./svgs/`
