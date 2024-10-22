# GEOBODIES - AAVS TerrainLab
Workshop Web Spectral Indexes - July 2024 (Venice)

:globe_with_meridians:
WEB

<img width="1529" alt="Screenshot 2024-07-26 at 12 05 22 AM" src="https://github.com/user-attachments/assets/0bb9f9f2-1520-4df4-a448-47786240741b">

## Tutorial: Upload and Style Your GeoJSON and GeoTIFF Layers on a Mapbox Map

### Step 1: Setup and Basic Map Configuration

Start with the provided code, which initializes a Mapbox map and adds a countries outline layer:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>Geobodies_WebMap_Test</title>
    <meta name="viewport" content="initial-scale=1,maximum-scale=1,user-scalable=no" />
    <script src="https://api.mapbox.com/mapbox-gl-js/v2.9.2/mapbox-gl.js"></script>
    <link href="https://api.mapbox.com/mapbox-gl-js/v2.9.2/mapbox-gl.css" rel="stylesheet" />
    <style>
        body { margin: 0; padding: 0; }
        #map { position: absolute; top: 0; bottom: 0; width: 100%; }
    </style>
</head>
<body>

<div id="map"></div>
<script>
    // Set your Mapbox access token
    mapboxgl.accessToken = 'YOUR_MAPBOX_ACCESS_TOKEN'; // Replace with your Mapbox access token

    // Initialize the map
    var map = new mapboxgl.Map({
        container: 'map',
        style: {
            "version": 8,
            "sources": {},
            "layers": []
        },
        center: [12.292051, 45.372576], // Venice Lagoon coordinates
        zoom: 10
    });

    // Add countries outline layer
    map.on('load', function () {
        map.addSource('countries', {
            'type': 'vector',
            'url': 'mapbox://mapbox.country-boundaries-v1'
        });

        map.addLayer({
            'id': 'countries-outline',
            'type': 'line',
            'source': 'countries',
            'source-layer': 'country_boundaries',
            'paint': {
                'line-color': '#000000',
                'line-width': 0.5
            }
        });

        // Add zoom and rotation controls to the map
        map.addControl(new mapboxgl.NavigationControl());

        // Add custom tileset layer for bathymetry
        map.addSource('bathymetry', {
            'type': 'vector',
            'url': 'mapbox://YOUR_TILESET_ID' // Replace with your actual tileset ID
        });

        map.addLayer({
            'id': 'bathymetry-outline',
            'type': 'line',
            'source': 'bathymetry',
            'source-layer': 'YOUR_SOURCE_LAYER_NAME', // This should be the name of the source layer in your tileset
            'paint': {
                'line-color': 'blue',
                'line-width': 0.1
            }
        });
    });
</script>
</body>
</html>
```

### Adding Your Own GeoJSON and GeoTIFF Layers
### Example 1: Adding a Point Shapefile Layer
- Step 2: Convert and Upload Your Point Shapefile as GeoJSON
1. Convert your point shapefile to GeoJSON format.
1. Upload your GeoJSON file to Mapbox Studio:
1. Go to Mapbox Studio.
1. Click on "Tilesets" and then "New tileset".
1. Upload your GeoJSON file and publish it.
Note the tileset ID provided after the upload is complete.

- Step 3: Add the GeoJSON Layer to Your Map
1. Replace YOUR_TILESET_ID with the tileset ID of your uploaded GeoJSON file.

```javascript
<script>
    map.on('load', function () {
        // Add a GeoJSON point layer
        map.addSource('points', {
            'type': 'vector',
            'url': 'mapbox://YOUR_TILESET_ID' // Replace with the tileset ID of your GeoJSON file
        });

        map.addLayer({
            'id': 'points-layer',
            'type': 'circle',
            'source': 'points',
            'source-layer': 'YOUR_SOURCE_LAYER_NAME', // This should be the name of the source layer in your tileset
            'paint': {
                'circle-radius': 5,
                'circle-color': '#ff0000'
            }
        });
    });
</script>
```
### Example 2: Adding a Polygon or Line Shapefile Layer
- Step 4: Convert and Upload Your Polygon or Line Shapefile as GeoJSON
1. Convert your polygon or line shapefile to GeoJSON format.
1. Upload your GeoJSON file to Mapbox Studio:
1. Go to Mapbox Studio.
1. Click on "Tilesets" and then "New tileset".
1. Upload your GeoJSON file and publish it.
1. Note the tileset ID provided after the upload is complete.
- Step 5: Add the GeoJSON Layer to Your Map
1. Replace YOUR_TILESET_ID with the tileset ID of your uploaded GeoJSON file.

```javascript
<script>
    map.on('load', function () {
        // Add a GeoJSON polygon layer
        map.addSource('polygons', {
            'type': 'vector',
            'url': 'mapbox://YOUR_TILESET_ID' // Replace with the tileset ID of your GeoJSON file
        });

        // Add fill layer for the polygons
        map.addLayer({
            'id': 'polygons-fill-layer',
            'type': 'fill',
            'source': 'polygons',
            'source-layer': 'YOUR_SOURCE_LAYER_NAME', // This should be the name of the source layer in your tileset
            'paint': {
                'fill-color': '#00ff00',
                'fill-opacity': 0.5
            }
        });

        // Add line layer for the polygon outlines
        map.addLayer({
            'id': 'polygons-line-layer',
            'type': 'line',
            'source': 'polygons',
            'source-layer': 'YOUR_SOURCE_LAYER_NAME', // This should be the name of the source layer in your tileset
            'paint': {
                'line-color': '#000000', // Stroke color
                'line-width': 2 // Stroke width
            }
        });
    });
</script>
```
### Example 3: Adding a GeoTIFF Layer
- Step 6: Convert and Upload Your GeoTIFF File
1. Convert your GeoTIFF file to a format that Mapbox GL JS can use (e.g., using gdal2tiles.py).
1. Upload your GeoTIFF file to Mapbox Studio:
1. Go to Mapbox Studio.
1. Click on "Tilesets" and then "New tileset".
1. Upload your GeoTIFF file and publish it.
1. Note the tileset ID provided after the upload is complete.
- Step 7: Add the GeoTIFF Layer to Your Map
1.  Replace YOUR_TILESET_ID with the tileset ID of your uploaded GeoTIFF file.

```javascript
<script>
    map.on('load', function () {
        // Add a GeoTIFF layer
        map.addSource('geotiff', {
            'type': 'raster',
            'url': 'mapbox://YOUR_TILESET_ID' // Replace with the tileset ID of your GeoTIFF file
        });

        map.addLayer({
            'id': 'geotiff-layer',
            'type': 'raster',
            'source': 'geotiff',
            'paint': {
                'raster-opacity': 0.8
            }
        });
    });
</script>
```
- Step 9: Make the Tileset Public - (!!! very important so these layers will be added to the main Web Map)
1. In Mapbox Studio, go to your tilesets.
1. Find the tileset you just uploaded and click on it.
1. Click on "Share" or "Share & use".
1. Toggle the switch to make the tileset public.
1. Note the tileset ID provided after the upload is complete.

:globe_with_meridians:
SPECTRAL INDEXES

## 1. NDVI (Normalized Difference Vegetation Index) - Sentinel2 

The Normalized Difference Vegetation Index (NDVI) is a numerical indicator that uses the visible and near-infrared bands of the electromagnetic spectrum to analyze vegetation health. The NDVI is calculated using the following formula:

$$
\text{NDVI} = \frac{\text{NIR} - \text{Red}}{\text{NIR} + \text{Red}}
$$

<img width="1529" alt="Screenshot 2024-07-25 at 9 39 52 PM" src="https://github.com/user-attachments/assets/821004f5-0725-4876-ba8b-19135a999932">

> GO TO gee code editor [AA_NDVI](https://code.earthengine.google.com/1221aad06de10e9d9abba5128bac9be9)

### Step 1: Define the NDVI Color Palette
We start by defining the color palette to visualize the NDVI values. This palette ranges from white (low NDVI) to dark green (high NDVI).
```javascript
var ndvi_palette = [
  'FFFFFF', 'CE7E45', 'DF923D', 'F1B555', 'FCD163', 
  '99B718', '74A901', '66A000', '529400', '3E8601', 
  '207401', '056201', '004C00'
].join(',');
```

### Step 2: Define Your Geometry
Replace the placeholder with your specific geometry coordinates.
```javascript
var geometry = geometry;
```
### Step 3: Center the Map on the Geometry
Center the map view on the defined geometry with a zoom level of 12.
```javascript
Map.centerObject(geometry, 12);
```
### Step 4: Function to Add NDVI Band to Sentinel-2 Images
Define a function to calculate NDVI and add it as a band to each Sentinel-2 image.
```javascript
function addnd(input) {
  // Sentinel-2 NDVI calculation (B8 - NIR, B4 - Red)
  var nd = input.normalizedDifference(['B8', 'B4']).rename('ndvi');
  return input.addBands(nd);
}
```

### Step 5: Set Date Range for Sentinel-2
Define the start and end dates for filtering the Sentinel-2 image collection.
```javascript
var S2_startDate = '2020-01-01'; // Define your start date
var S2_endDate = '2020-12-31'; // Define your end date
```

### Step 6: Create Sentinel-2 Image Collection and Apply Filters
Create the image collection, filter it by bounds, date range, and cloud cover percentage, and apply the NDVI calculation function.
```javascript
var S2_collection = ee.ImageCollection("COPERNICUS/S2")
  .filterBounds(geometry) // Filter collection by geometry
  .filterDate(S2_startDate, S2_endDate) // Filter collection by date range
  .filterMetadata('CLOUDY_PIXEL_PERCENTAGE', 'less_than', 10) // Filter images with less than 10% cloud cover
  .map(addnd); // Apply the addnd function to each image in the collection
```

### Step 7: Print the Image Collection to the Console for Inspection
Output the filtered image collection to the console for inspection.
```javascript
print(S2_collection);
```

### Step 8: Create a Mosaic (Median Composite) of the Collection
Create a median composite of the filtered collection and clip it to the geometry.
```javascript
var S2_mosaic = S2_collection.median().clip(geometry); // Create a median composite and clip to the geometry

```

### Step 9: Display the Mosaic in Natural Color
Display the natural color composite of the mosaic on the map.
```javascript
var S2_display = {bands: ['B4', 'B3', 'B2'], min: 0, max: 3000};
Map.addLayer(S2_mosaic, S2_display, "Sentinel-2"); // Add the natural color mosaic to the map
```

### Step 10: Select the NDVI Band and Create a Mosaic
Select the NDVI band, create a median composite, and clip it to the geometry.
```javascript
var ndvi_S2 = S2_collection.select('ndvi').median().clip(geometry); // Create a median composite of the NDVI band and clip to the geometry
```

### Step 11: Display the NDVI Mosaic
Display the NDVI mosaic on the map with the specified color palette.
```javascript
Map.addLayer(ndvi_S2, {min: -0.1, max: 1, palette: ndvi_palette}, 'NDVI S2', false); // Add the NDVI mosaic to the map
```

### Step 12: Export the NDVI Mosaic as a GeoTIFF to Google Drive
Export the NDVI mosaic as a GeoTIFF file to Google Drive.
```javascript
Export.image.toDrive({
  image: ndvi_S2, // The image to export
  description: 'NDVI_Sentinel2', // Description of the export task
  scale: 10, // Sentinel-2 resolution
  region: geometry, // The region to export
  fileFormat: 'GeoTIFF', // Export format
  formatOptions: {
    cloudOptimized: true
  }
});
```
## FULL CODE
```javascript
// Define the NDVI color palette
var ndvi_palette = [
  'FFFFFF', 'CE7E45', 'DF923D', 'F1B555', 'FCD163', 
  '99B718', '74A901', '66A000', '529400', '3E8601', 
  '207401', '056201', '004C00'
].join(',');

// Define your geometry (replace with your coordinates)
var geometry = geometry;

// Center the map on the geometry
Map.centerObject(geometry, 12);

// Function to add NDVI band to Sentinel-2 images
function addnd(input) {
  // Sentinel-2 NDVI calculation (B8 - NIR, B4 - Red)
  var nd = input.normalizedDifference(['B8', 'B4']).rename('ndvi');
  return input.addBands(nd);
}

// Set date range for Sentinel-2
var S2_startDate = '2020-01-01'; // Define your start date
var S2_endDate = '2020-12-31'; // Define your end date

// Create Sentinel-2 image collection and apply filters
var S2_collection = ee.ImageCollection("COPERNICUS/S2")
  .filterBounds(geometry) // Filter collection by geometry
  .filterDate(S2_startDate, S2_endDate) // Filter collection by date range
  .filterMetadata('CLOUDY_PIXEL_PERCENTAGE', 'less_than', 10) // Filter images with less than 10% cloud cover
  .map(addnd); // Apply the addnd function to each image in the collection

// Print the image collection to the console for inspection
print(S2_collection);

// Create a mosaic (median composite) of the collection
var S2_mosaic = S2_collection.median().clip(geometry); // Create a median composite and clip to the geometry

// Display the mosaic in natural color
var S2_display = {bands: ['B4', 'B3', 'B2'], min: 0, max: 3000};
Map.addLayer(S2_mosaic, S2_display, "Sentinel-2"); // Add the natural color mosaic to the map

// Select the NDVI band and create a mosaic
var ndvi_S2 = S2_collection.select('ndvi').median().clip(geometry); // Create a median composite of the NDVI band and clip to the geometry

// Display the NDVI mosaic
Map.addLayer(ndvi_S2, {min: -0.1, max: 1, palette: ndvi_palette}, 'NDVI S2', false); // Add the NDVI mosaic to the map

// Export the NDVI mosaic as a GeoTIFF to Google Drive
Export.image.toDrive({
  image: ndvi_S2, // The image to export
  description: 'NDVI_Sentinel2', // Description of the export task
  scale: 10, // Sentinel-2 resolution
  region: geometry, // The region to export
  fileFormat: 'GeoTIFF', // Export format
  formatOptions: {
    cloudOptimized: true
  }
}); // Export the NDVI mosaic to Google Drive

```



# 2. NDWI (Normalized Difference Water Index) - Sentinel2 

The Normalized Difference Water Index (NDWI) is a numerical indicator that uses the green and near-infrared (NIR) bands of the electromagnetic spectrum to delineate open water features and enhance the presence of water bodies in remotely sensed digital imagery. It is particularly useful for identifying water bodies and monitoring changes in water content in vegetation and soil. The NDWI is calculated using the following formula:

$$
\text{NDWI} = \frac{\text{Green} - \text{NIR}}{\text{Green} + \text{NIR}}
$$

<img width="1529" alt="Screenshot 2024-07-25 at 9 41 19 PM" src="https://github.com/user-attachments/assets/e7b63b69-a6d2-4cda-8072-f0c4bbe8a6b5">

> GO TO gee code editor [AA_NDWI](https://code.earthengine.google.com/91b53de819f56a0c041d31f152dce276)

## FULL CODE
```javascript
// Define the area of interest
var areaOfInterest = geometry; 

// Print the coordinates of the area of interest
print("Coordinates of the area of interest:", areaOfInterest.coordinates());

// Zoom to the area of interest and add it to the map
Map.centerObject(areaOfInterest);
Map.addLayer(areaOfInterest);

// Define the date range for the image collection
var startDate = '2022-04-11';
var endDate = '2022-12-31';

// Get all the Landsat 8 images taken over the area of interest within the date range
var landsatImageCollection = ee.ImageCollection('LANDSAT/LC08/C02/T1_L2')
                  .filterBounds(areaOfInterest)
                  .filterDate(startDate, endDate)
                  .sort('system:time_start');

// Print the number of images in the collection
print("Number of images = ", landsatImageCollection.size());

// Check the first few images and their metadata
var imageList = landsatImageCollection.toList(10);
for (var i = 0; i < 10; i++) {
  var image = ee.Image(imageList.get(i));
  print("Image", i, image);
}

// Function to create an NDWI image with a specific visualization
function createNdwiImage(image) {
  var ndwi = image.normalizedDifference(['SR_B3', 'SR_B5']).rename('NDWI');
  var ndwiVis = ndwi.visualize({min: -0.4, max: 0.2, palette: ['black', 'black', 'black', 'pink', 'blue']});
  return ndwiVis.set({
    'system:time_start': image.get('system:time_start')
  });
}

// Map the function over the collection and select the NDWI visualization
var ndwiCollection = landsatImageCollection.map(createNdwiImage);

// Define video parameters
var videoArgs = {
  region: areaOfInterest,
  framesPerSecond: 10,
  crs: 'EPSG:3857',
  min: -0.4,
  max: 0.2,
  palette: ['black', 'black', 'black', 'pink', 'blue']
};

// Print the URL to download the video
print(ui.Thumbnail(ndwiCollection, videoArgs));

// Export the NDWI time-lapse video to Google Drive
Export.video.toDrive({
  collection: ndwiCollection,
  description: 'NDWI_Time_Lapse',
  fileNamePrefix: 'NDWI_Time_Lapse',
  framesPerSecond: 10,
  region: areaOfInterest,
  dimensions: 720,
  crs: 'EPSG:3857'
});

// Visualize the first image if available
if (landsatImageCollection.size().getInfo() > 0) {
  // Get the first image with the least cloud cover
  var landsatImage = landsatImageCollection.sort('CLOUD_COVER')
      .first()
      .clip(areaOfInterest);

  // Print the date of the Landsat image
  print("Landsat image taken at = ", landsatImage.date());

  // Visualize the image using RGB bands
  Map.addLayer(landsatImage, {min: 500.0, max: 12000.0, gamma: 0.5, bands: ['SR_B4', 'SR_B3', 'SR_B2']}, 'Landsat 8');

  // Calculate and visualize NDWI
  var ndwi = landsatImage.normalizedDifference(['SR_B3', 'SR_B5']).rename('NDWI');
  // Enhanced NDWI visualization
  Map.addLayer(ndwi, {min: -0.4, max: 0.2, palette: ['black', 'black', 'black', 'pink', 'blue']}, 'NDWI');

  // Create NDWI mask for water (thresholded NDWI)
  var ndwiThreshold = ndwi.gte(0.0);
  var ndwiMask = ndwiThreshold.updateMask(ndwiThreshold);
  Map.addLayer(ndwiThreshold, {palette:['black','white']}, 'NDWI Binary Mask');
  
  // Export the NDWI image as a GeoTIFF to Google Drive
  Export.image.toDrive({
    image: ndwi, // The NDWI image to export
    description: 'NDWI_Image', // Description of the export task
    dimensions: 720, // Resolution of the export
    region: areaOfInterest, // The region to export
    fileFormat: 'GeoTIFF', // Export format
    formatOptions: {
      cloudOptimized: true // Cloud optimization for GeoTIFF
    }
  });
  
  // Export the NDWI mask as a GeoTIFF to Google Drive
  Export.image.toDrive({
    image: ndwiMask, // The NDWI mask to export
    description: 'NDWI_Mask', // Description of the export task
    dimensions: 720, // Resolution of the export
    region: areaOfInterest, // The region to export
    fileFormat: 'GeoTIFF', // Export format
    formatOptions: {
      cloudOptimized: true // Cloud optimization for GeoTIFF
    }
  });
}
```



# 3. NDCI (Normalized Difference Chlorophyll) - Sentinel2 
The Normalized Difference Chlorophyll Index (NDCI) is a numerical indicator used to estimate the concentration of chlorophyll in water bodies. It leverages the reflectance properties of water at specific wavelengths to provide an estimate of chlorophyll concentration, which is an important parameter for monitoring water quality and the health of aquatic ecosystems. The NDCI is calculated using the following formula:

$$
\text{NDCI} = \frac{\text{Red Edge} - \text{Red}}{\text{Red Edge} + \text{Red}}
$$

In this formula:
- **Red Edge** refers to a wavelength band around 705 nm.
- **Red** refers to a wavelength band around 665 nm.

The NDCI value ranges from -1 to 1, where higher values indicate higher chlorophyll concentrations.
<img width="1529" alt="Screenshot 2024-07-25 at 9 40 38 PM" src="https://github.com/user-attachments/assets/cad79d7a-3219-4df1-b2ca-520fae57ebb9">

> GO TO gee code editor [AA_NDCI](https://code.earthengine.google.com/107ac215e048de968bc85b5972aaf704)
## FULL CODE
```javascript
// Define the area of interest
var geometry = geometry;
          
// Load the Sentinel-2 SR Harmonized ImageCollection
var s2 = ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED');

// Define the start and end dates for the image collection
var startDate = ee.Date.fromYMD(2022, 8, 15);
var endDate = startDate.advance(10, 'day');

// Filter the image collection by date and region of interest (geometry)
var filtered = s2
  .filter(ee.Filter.date(startDate, endDate))
  .filter(ee.Filter.bounds(geometry));

// Select the first image from the filtered collection
var image = filtered.first(); 

// Calculate the Normalized Difference Chlorophyll Index (NDCI)
// NDCI aims to predict the chlorophyll content in turbid productive waters
// using 'RED EDGE' (B5) and 'RED' (B4) bands
var ndci = image.normalizedDifference(['B5', 'B4']).rename(['ndci']);

// Calculate the Modified Normalized Difference Water Index (MNDWI)
// MNDWI uses 'GREEN' (B3) and 'SWIR1' (B11) bands to highlight water bodies
var mndwi = image.normalizedDifference(['B3', 'B11']).rename(['mndwi']); 

// Select all pixels with high NDCI and high MNDWI to identify algal blooms
// Thresholds may need adjustment based on the specific region
var algae = ndci.gt(0.1).and(mndwi.gt(0.5));

// Define visualization parameters for different layers
var rgbVis = {min: 500, max: 2500, bands: ['B4', 'B3', 'B2']};
var ndwiVis = {min:0, max:0.5, palette: ['white', 'blue']};
var ndciVis = {min:0, max:0.5, palette: ['white', 'red']};
var algaeVis = {min:0, max:1, palette: ['white', '#31a354']};

// Center the map on the specified geometry at a zoom level of 12
Map.centerObject(geometry, 12);

// Add the RGB image layer to the map
Map.addLayer(image.clip(geometry), rgbVis, 'Image');

// Add the MNDWI layer to the map (initially hidden)
Map.addLayer(mndwi.clip(geometry), ndwiVis, 'MNDWI', false);

// Add the NDCI layer to the map (initially hidden)
Map.addLayer(ndci.clip(geometry), ndciVis, 'NDCI', false);

// Add the algal bloom detection layer to the map
Map.addLayer(algae.clip(geometry).selfMask(), algaeVis, 'Algal Bloom');

// Export the algal bloom detection as a GeoTIFF to Google Drive
Export.image.toDrive({
  image: algae, // The image to export
  description: 'Algal_Bloom_Detection', // Description of the export task
  region: geometry, // The region to export
  dimensions:720, 
  crs: 'EPSG:3857', // Coordinate reference system
  fileFormat: 'GeoTIFF', // Export format
  formatOptions: {
    cloudOptimized: true // Cloud optimization for GeoTIFF
  }
});
```

# 4.TSS (Total Suspended Solids) - Sentinel2

Total Suspended Solids (TSS) refer to the solid particles suspended in water, which can include a wide variety of material such as silt, decaying plant and animal matter, industrial wastes, and sewage. High concentrations of suspended solids can affect water quality and aquatic life.

The TSS is calculated using the following empirical formula:

$$
\text{TSS} = \frac{R_{\text{Red}} \times 961}{1 - \frac{R_{\text{Red}}}{0.1728}} + 29
$$

<img width="1529" alt="Screenshot 2024-07-25 at 9 43 00 PM" src="https://github.com/user-attachments/assets/ca705a6f-0d83-4538-b954-9d7e718bc6c1">

> GO TO gee code editor [AA_TSS](https://code.earthengine.google.com/107ac215e048de968bc85b5972aaf704)
## FULL CODE
```javascript
// Define the area of interest
var geometry = geometry;

// Center the map on the specified geometry at a zoom level of 14
Map.centerObject(geometry, 14);

// Print the coordinates of the geometry
print("Coordinates of the geometry:", geometry.coordinates());

// Function to calculate Total Suspended Solids (TSS) from a Sentinel-2 image
function calculateTSS(image) {
  // Calculate the numerator for TSS
  var num = image.select('B4').multiply(961);
  
  // Calculate the denominator for TSS
  var dem = ee.Image(1).subtract(image.select('B4').divide(0.1728));
  
  // Calculate the TSS image
  var tssimg = num.divide(dem).add(29);
  
  // Create a mask using the normalized difference between Band 3 (Green) and Band 8 (NIR)
  var mask = image.normalizedDifference(['B3', 'B8']);
  
  // Return the TSS image with the mask applied, clipped to the geometry, and with the original image timestamp
  return tssimg.updateMask(mask.gte(0.1)).clip(geometry)
      .set('system:time_start', image.get('system:time_start'));
}

// Function to create a cloud-masked mosaic from Sentinel-2 images within a date range
function createMosaic(date1, date2) {
  // Filter the Sentinel-2 image collection by date range and region of interest
  var collection = ee.ImageCollection('COPERNICUS/S2')
    .filterDate(date1, date2)
    .filterBounds(geometry);
  
  // Function to mask clouds using the Sentinel-2 QA60 band
  function maskClouds(image) {
    var qa = image.select('QA60');
    
    // Bits 10 and 11 are clouds and cirrus, respectively
    var cloudBitMask = 1 << 10;
    var cirrusBitMask = 1 << 11;
    
    // Both cloud and cirrus bits should be set to zero (clear condition)
    var mask = qa.bitwiseAnd(cloudBitMask).eq(0).and(
      qa.bitwiseAnd(cirrusBitMask).eq(0)
    );
    
    // Mask the image, scale it to reflectance values, and select all bands
    return image.updateMask(mask).divide(10000).select("B.*")
      .copyProperties(image, ["system:time_start"]);
  }
  
  // Apply the cloud masking function and calculate the median composite
  var composite = collection.map(maskClouds).median();
  
  // Calculate TSS for the composite image
  var tssImage = calculateTSS(composite);
  
  // Add the TSS image to the map with the specified color palette and value range
  Map.addLayer(tssImage, {min: 0, max: 350, palette: ['#4a4e4d','#0e9aa7','#3da4ab','#f6cd61','#fe8a71','#ee4035','#f37736']}, "TSS Calculation");
  
  // Create an NDVI mask using the normalized difference between Band 3 (Green) and Band 8 (NIR)
  var mask = composite.normalizedDifference(['B3', 'B8']);
  
  // Add the NDVI mask to the map
  Map.addLayer(mask, {}, "NDVI Mask");

  // Export the TSS mosaic as a GeoTIFF to Google Drive
  Export.image.toDrive({
    image: tssImage, // The image to export
    description: 'TSS_Mosaic', // Description of the export task
    region: geometry, // The region to export
    dimensions: 720, // Dimensions of the export
    crs: 'EPSG:3857', // Coordinate reference system
    fileFormat: 'GeoTIFF', // Export format
    formatOptions: {
      cloudOptimized: true // Cloud optimization for GeoTIFF
    }
  });
}

// Function to create a 3-band RGB visualization for the video export
function createTSSVisualization(image) {
  // Create a visualization of the TSS image with the specified color palette and value range
  var tssVis = image.visualize({
    min: 0,
    max: 350,
    palette: ['#4a4e4d','#0e9aa7','#3da4ab','#f6cd61','#fe8a71','#ee4035','#f37736']
  });
  
  // Return the visualization with the original image timestamp
  return tssVis.set('system:time_start', image.get('system:time_start'));
}

// Function to export a video and print the number of images
function exportVideoAndCountImages(startDate, endDate) {
  // Filter the Sentinel-2 image collection by date range and region of interest
  var collection = ee.ImageCollection('COPERNICUS/S2')
    .filterDate(startDate, endDate)
    .filterBounds(geometry);
  
  // Get the number of images in the collection
  var count = collection.size().getInfo();
  print('Number of images in the date range:', count);
  
  // Function to mask clouds and calculate TSS for each image
  function maskCloudsAndCalculateTSS(image) {
    var qa = image.select('QA60');
    var cloudBitMask = 1 << 10;
    var cirrusBitMask = 1 << 11;
    var mask = qa.bitwiseAnd(cloudBitMask).eq(0).and(
      qa.bitwiseAnd(cirrusBitMask).eq(0)
    );
    var cloudMasked = image.updateMask(mask).divide(10000);
    return calculateTSS(cloudMasked);
  }
  
  // Map the cloud masking and TSS calculation function over the collection
  var tssCollection = collection.map(maskCloudsAndCalculateTSS);
  
  // Create a 3-band RGB visualization for each TSS image
  var tssVisCollection = tssCollection.map(createTSSVisualization);
  
  // Define video export parameters
  var videoParams = {
    region: geometry,
    framesPerSecond: 2,
    crs: 'EPSG:3857',
    min: 0,
    max: 255, // Adjusted to match the scaled range
  };
  
  // Export the TSS video to Google Drive
  Export.video.toDrive({
    collection: tssVisCollection,
    dimensions: 720,
    description: 'TSS_Video',
    folder: 'EarthEngineExports',
    fileNamePrefix: 'TSS_Video',
    framesPerSecond: 2,
    region: geometry,
  });
}

// Create a mosaic for a specific date range and export it as a GeoTIFF
createMosaic('2019-10-31', '2020-01-21');

// Export a video and print the number of images in a longer date range
exportVideoAndCountImages('2019-01-01', '2019-02-01');
```

# 5. SHIPPING LANES - Sentinel1 - SAR

<img width="1529" alt="Screenshot 2024-07-25 at 9 41 52 PM" src="https://github.com/user-attachments/assets/50d4dc88-4b5a-435d-97ea-aa3b46ae55bd">



> GO TO gee code editor [AA_SAR](https://code.earthengine.google.com/2d9864dbfaf32a8c8b77e07611274635)
## FULL CODE
```javascript
// Load the Sentinel-1 ImageCollection
var sentinel1 = ee.ImageCollection('COPERNICUS/S1_GRD');

// Filter by metadata properties to get VH polarization and IW mode
var vh = sentinel1
  .filter(ee.Filter.listContains('transmitterReceiverPolarisation', 'VH'))
  .filter(ee.Filter.eq('instrumentMode', 'IW'))
  .filterDate("2015-01-01","2019-02-01");

// Further filter to get images from different look angles (ascending and descending)
var vhAscending = vh.filter(ee.Filter.eq('orbitProperties_pass', 'ASCENDING'));
var vhDescending = vh.filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING'));

// Create composites by taking the maximum value of VH polarization for ascending and descending passes
var vhMaxA = vhAscending.select('VH').max();
var vhMaxD = vhDescending.select('VH').max();

// Center the map on a specific location (latitude, longitude) and zoom level
Map.setCenter(12.3358, 45.4375, 11);

// Add the ascending pass composite to the map with specified visualization parameters
Map.addLayer(vhMaxA, {min: -15, max: 0}, 'VH ASC max', 0);

// Add the descending pass composite to the map with specified visualization parameters
Map.addLayer(vhMaxD, {min: -15, max: 0}, 'VH DES max');

// Export the ascending pass composite as a GeoTIFF to Google Drive
Export.image.toDrive({
  image: vhMaxA, // The image to export
  description: 'VH_ASC_max', // Description of the export task
  region: Map.getBounds(true), // The region to export
  dimensions: 720,
  crs: 'EPSG:3857', // Coordinate reference system
  fileFormat: 'GeoTIFF', // Export format
  formatOptions: {
    cloudOptimized: true // Cloud optimization for GeoTIFF
  }
});

// Export the descending pass composite as a GeoTIFF to Google Drive
Export.image.toDrive({
  image: vhMaxD, // The image to export
  description: 'VH_DES_max', // Description of the export task
  region: Map.getBounds(true), // The region to export
  dimensions: 720,
  crs: 'EPSG:3857', // Coordinate reference system
  fileFormat: 'GeoTIFF', // Export format
  formatOptions: {
    cloudOptimized: true // Cloud optimization for GeoTIFF
  }
});
```

## Explanation of Ascending and Descending Orbits

> Ascending passes occur when the satellite moves from south to north, typically in the afternoon or evening.
> Descending passes happen when the satellite moves from north to south, usually in the morning.
>



> These orbits have key differences. The different times of day—afternoon for ascending and morning for descending—affect surface reflections. Additionally, the varying incidence angles of each pass reveal different surface features.
>
> In this tutorial, we use these differences to detect ships. Bright spots in the radar images indicate strong signals returned from large ships, while darker areas represent the sea surface. By creating a maximum value composite from these images, we can effectively highlight sea lanes and track ship movements. This method also helps distinguish real ships from artifacts or "ghost ships" caused by interferences and SAR ambiguities.
>



## Are.na 
### Archiving and sharing videos to be hosted on the main Map

<img width="1529" alt="Screenshot 2024-07-26 at 12 25 36 AM" src="https://github.com/user-attachments/assets/9c913fcb-d569-466b-a2c8-41427aa0b871">

> GO to [Are.na Channel](https://www.are.na/lucia-rebolino/geobodies-aa-workshop)

Every video or timelapse exported from Google Earth Engine (GEE) will be saved as a Block, with the coordinates previously printed in the console added as comments. This will enable geolocation on the map.

Feel free to test different areas—smaller or larger, with multiple palettes and varying time frames. The idea is to populate the lagoon and the area surrounding Venice with as many video indexes as possible.

Regarding the GeoJSON, it is important that all the tilesets uploaded to Mapbox are shared as public so that they can be included on the main map. Please copy and paste the line of code as an example with all the layers you want to add and be visible on the platform. Experiment with shapes and colors, and add your group number (e.g., G2) to the title. In the description, write two or more lines describing your layer. These descriptions will be visible to users toggling your layer on the map in the future.

:earth_americas:
:purple_heart:
