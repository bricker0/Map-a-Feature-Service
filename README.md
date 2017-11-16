# Seattle-Feature-Service
Here I show how to call Seattle Open Data Esri Feature Services using Leaflet. 
This example is based on this <a href= "http://esri.github.io/esri-leaflet/tutorials/working-with-feature-layers.html">Esri Custom popup tutorial.</a> and <a href="https://esri.github.io/esri-leaflet/examples/feature-layer-popups.html">this example</a>.

Seattle offers many stylized feature services that you may incorporate use. I use <a href= "https://gisrevprxy.seattle.gov/arcgis/rest/services/ext/WM_CityGISLayers/MapServer/33">Tree data found here </a>, but you can browse other <a href= "https://gisrevprxy.seattle.gov/arcgis/rest/services/ext/WM_CityGISLayers/MapServer">Seattle data here</a>. Before you commit to a dataset you can browse what is contains when you click ArcGIS Online MapViewer on the data page. 

##Let's get started
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
         var map = L.map('map').setView([47.6062, -122.3321], 12)
        });

        var esriStreets = L.esri.basemapLayer('Topographic').addTo(map);

            
    </script>    
</body>
</html>
``

This will provide us with a Topographic base map that fills the browser frame.

Now let's add some data to feature! You have looked around what Seattle has to offer. As I mentioned, I am using the heritage tree data, so under the map variable and before I close the script tag I will name the variable seattleHeriageTrees to call the feature layer and add to the map. I add tags to remind me what the code does. 

``
 //set the tree variable to call the server to then display or add to the map
        var seattleHeritageTrees = L.esri.featureLayer({url: 'https://gisrevprxy.seattle.gov/arcgis/rest/services/ext/WM_CityGISLayers/MapServer/33'}).addTo(map);
        
 ```

If we open the code now, you will see the default pin markers and nothing else. we want them to tell use something! We can see from the Esri page that the symbology has been set. We need to tell the browser to get that style. To do so we add the following to the head. 

 ```
<!-- Load Esri Leaflet Renderers from CDN -->
<script src="https://cdn.jsdelivr.net/leaflet.esri.renderers/1.0.1/esri-leaflet-renderers.js"></script>
 ```
 Now the blue dots should be replaced with trees! 
 
Next, we need to add the popup window to say something meaningful. We do not want a laundry list of all the attributes that Esri Online provides. We only want to share what the end users need to see. I can see all the attributes available, from these, I decide I only want to share. I decided to simply share the common and scientific names of the tree selected.

I added the following just under the add to map. Again, I added comments to remember what I am showing.
 ```
 //customize the popup, do you no present a laundry list of attributes, think about what the end user needs to know.
        seattleHeritageTrees.bindPopup(function(feature) {
    return L.Util.template('<h3>{DESCRIPT }</h3><hr /><p> Its scientific name is {SCINAME}.', feature.properties);
 ```
Hooray! Now when I click a tree, meaningful information pops up. This is fun so I want to add more information. 

#Add another layer 

I want to see if these trees are close to heritage landmarks. I can see that there is a Landmarks layer. I will make a new variable called Seattle landmarks and add it to the map. I add this next bit just under the add to map. 

 ```
 //set the Seattle Landmarks variable call the server to then display or add to the map
        var seattleLandmarks = L.esri.featureLayer({url: 'https://gisrevprxy.seattle.gov/arcgis/rest/services/ext/WM_CityGISLayers/MapServer/55'}).addTo(map);
 ```
 
 I need popup information for this new layer too. I see what attributes are available and decide to share information about the address and how long this site has been recognized. I add the following under the feature properties
 
  ```
  //popup information for landmarks
 
  seattleLandmarks.bindPopup(function(feature) {
            return L.Util.template('<h3>{NAME}</h3><hr /><p>This landmark is located at {ADDRESS} and has been recognized since {EFF_DATE}.', feature.properties);
                
         });       
          
  ```

The dates look bizarre so I decide to remove that from the label, it is meaningless! 
Now we should see trees and landmarks. Fun! Let's keep going!
          
##Add base map selector

I want to user to be able to change the base maps if they would like. 

Add the following into the style tag. Make sure you place it properly, after the } the close of the #map

```
#basemaps-wrapper {
    position: absolute;
    top: 10px;
    right: 10px;
    z-index: 400;
    background: white;
    padding: 10px;
  }
  #basemaps {
    margin-bottom: 5px;
  }
 ```
 
 Then we need to add the div to place the dropdown within the frame. 
 
  ```
 <div id="basemaps-wrapper" class="leaflet-bar">
  <select name="basemaps" id="basemaps" onChange="changeBasemap(basemaps)">
    <option value="Topographic">Topographic</option>
    <option value="Streets">Streets</option>
    <option value="NationalGeographic">National Geographic</option>
    <option value="Oceans">Oceans</option>
    <option value="Gray">Gray</option>
    <option value="DarkGray">Dark Gray</option>
    <option value="Imagery">Imagery</option>
    <option value="ShadedRelief">Shaded Relief</option>
  </select>
</div>
 ```
Looks great! 

##That is all for now
Here you have learned to call data straight from an Esri server. Awesome! There are so many different ways to call feature layers. <a href="http://esri.github.io/esri-leaflet/tutorials/introduction-to-layer-types.html">See more examples here.</a> Enjoy!
