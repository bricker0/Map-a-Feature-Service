# Seattle-Feature-Service
Here I show how to call Seattle Open Data Esri Feature Services using Leaflet. 
This example is based on this <a href= "https://esri.github.io/esri-leaflet/examples/feature-layer-popups.html">Esri Custom popup tutorial.</a> 

Seattle offers many stylized feature services that you may incorporate use. I use <a href= "https://esri.github.io/esri-leaflet/examples/feature-layer-popups.html">Tree data found here </a>, but you can browse other <a href= "https://gisrevprxy.seattle.gov/arcgis/rest/services/ext/WM_CityGISLayers/MapServerl">Seattle data here </a>. 

I first provide you basic with can see we have the basic HTML requirements, the title of the page is Calling Features with Leaflet, the map is stylized to fill the whole screen, the center of the map is set to Seattle and zoom level is appropriately framing Seattle.

Start with the following: 

```
<!doctype html>
<html lang="en">
<head>  
  <meta charset="utf-8">
  <title>Calling Features with Leaflet</title>  
  <meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' />

  <!-- Load Leaflet from CDN-->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@0.7.7/dist/leaflet.css" />
  <script src="https://unpkg.com/leaflet@0.7.7/dist/leaflet.js"></script>

  <!-- Load Esri Leaflet from CDN -->
  <script src="https://cdn.jsdelivr.net/leaflet.esri/1.0.4/esri-leaflet.js"></script>

  <style>
    html,
    body,
    #map {
      height: 100%;
      width: 100%;
      margin: 0;
      padding: 0;
    }
  </style>
</head>
<body>    
    <div id="map"></div>

    <script>
        var map = L.map('map', {
          center: [47.6062, -122.3321],
           zoom: 12
        });

        var esriStreets = L.esri.basemapLayer('Streets').addTo(map);

            
    </script>    
</body>
</html>
```

