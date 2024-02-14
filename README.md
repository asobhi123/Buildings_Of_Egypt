# Buildings_Of_Egypt
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>PMTiles MapLibre Example</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@3.3.1/dist/maplibre-gl.css" crossorigin="anonymous">
  <style>
    body {
      margin: 0;
    }
    #map {
      height: 100vh;
    }
  </style>
</head>
<body>
  <div id="map"></div>
  <script src="https://unpkg.com/maplibre-gl@3.3.1/dist/maplibre-gl.js" crossorigin="anonymous"></script>
  <script src="https://unpkg.com/pmtiles@3.0.3/dist/pmtiles.js"></script>
  <script>
    // add the PMTiles plugin to the maplibregl global.
    let protocol = new pmtiles.Protocol();
    maplibregl.addProtocol("pmtiles", protocol.tile);

    let PMTILES_URL = "https://link.storjshare.io/raw/jxygasl25kyq6errbenzyudwgdwa/building%2Fegypt_buildings_vt.pmtiles";

    const p = new pmtiles.PMTiles(PMTILES_URL);
    protocol.add(p);

    p.getHeader().then(h => {
      const map = new maplibregl.Map({
        container: 'map',
        zoom: 8,
        center: [31.17, 30.07],
        style: {
          version: 8,
          sources: {
            "osm": {
              type: "raster",
              tiles: ["https://a.tile.openstreetmap.org/{z}/{x}/{y}.png"],
              tileSize: 256,
            },
            "example_source": {
              type: "vector",
              url: "pmtiles://" + PMTILES_URL,
              attribution: '© <a href="https://openstreetmap.org">OpenStreetMap</a>'
            }
          },
          layers: [
            {
              "id": "osm",
              "type": "raster",
              "source": "osm",
            },
            {
              "id": "buildings",
              "source": "example_source",
              "source-layer": "buildings",
              "type": "fill",
              "paint": {
                "fill-color": "steelblue"
              }
            },
          ]
        }
      });
    });
  </script>
</body>
</html>
