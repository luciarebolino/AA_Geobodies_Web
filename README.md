


# 1. NDVI (Normalized Difference Vegetation Index) - Sentinel-2 

The Normalized Difference Vegetation Index (NDVI) is a numerical indicator that uses the visible and near-infrared bands of the electromagnetic spectrum to analyze vegetation health. The NDVI is calculated using the following formula:

$$
\text{NDVI} = \frac{\text{NIR} - \text{Red}}{\text{NIR} + \text{Red}}
$$

<img width="1529" alt="Screenshot 2024-07-25 at 9 39 52 PM" src="https://github.com/user-attachments/assets/821004f5-0725-4876-ba8b-19135a999932">



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



# 2. NDWI (Normalized Difference Water Index) - Sentinel-2 

The Normalized Difference Water Index (NDWI) is a numerical indicator that uses the green and near-infrared (NIR) bands of the electromagnetic spectrum to delineate open water features and enhance the presence of water bodies in remotely sensed digital imagery. It is particularly useful for identifying water bodies and monitoring changes in water content in vegetation and soil. The NDWI is calculated using the following formula:

$$
\text{NDWI} = \frac{\text{Green} - \text{NIR}}{\text{Green} + \text{NIR}}
$$

<img width="1529" alt="Screenshot 2024-07-25 at 9 41 19 PM" src="https://github.com/user-attachments/assets/e7b63b69-a6d2-4cda-8072-f0c4bbe8a6b5">


# 3. NDCI (Normalized Difference Chlorophyll) - Sentinel-2 
The Normalized Difference Chlorophyll Index (NDCI) is a numerical indicator used to estimate the concentration of chlorophyll in water bodies. It leverages the reflectance properties of water at specific wavelengths to provide an estimate of chlorophyll concentration, which is an important parameter for monitoring water quality and the health of aquatic ecosystems. The NDCI is calculated using the following formula:

$$
\text{NDCI} = \frac{\text{Red Edge} - \text{Red}}{\text{Red Edge} + \text{Red}}
$$

In this formula:
- **Red Edge** refers to a wavelength band around 705 nm.
- **Red** refers to a wavelength band around 665 nm.

The NDCI value ranges from -1 to 1, where higher values indicate higher chlorophyll concentrations.
<img width="1529" alt="Screenshot 2024-07-25 at 9 40 38 PM" src="https://github.com/user-attachments/assets/cad79d7a-3219-4df1-b2ca-520fae57ebb9">

# 4.TSS (Total Suspended Solids) - Sentinel-2

Total Suspended Solids (TSS) refer to the solid particles suspended in water, which can include a wide variety of material such as silt, decaying plant and animal matter, industrial wastes, and sewage. High concentrations of suspended solids can affect water quality and aquatic life.

The TSS is calculated using the following empirical formula:

$$
\text{TSS} = \frac{R_{\text{Red}} \times 961}{1 - \frac{R_{\text{Red}}}{0.1728}} + 29
$$

<img width="1529" alt="Screenshot 2024-07-25 at 9 43 00 PM" src="https://github.com/user-attachments/assets/ca705a6f-0d83-4538-b954-9d7e718bc6c1">




# 5. SHIPPING LANES - Sentinel-1 - SAR

<img width="1529" alt="Screenshot 2024-07-25 at 9 41 52 PM" src="https://github.com/user-attachments/assets/50d4dc88-4b5a-435d-97ea-aa3b46ae55bd">




### Step 1: Sentinel-1 VH Polarization Analysis
The Sentinel-1 GRD (Ground Range Detected) data is loaded from the GEE Data Catalog.
```ruby
var sentinel1 = ee.ImageCollection('COPERNICUS/S1_GRD');
```
### Step 2: Filter by Metadata Properties
We filter the data to include images with VH polarization and those collected in Interferometric Wide Swath (IW) mode within the specified date range.
```ruby
var vh = sentinel1
  .filter(ee.Filter.listContains('transmitterReceiverPolarisation', 'VH'))
  .filter(ee.Filter.eq('instrumentMode', 'IW'))
  .filterDate("2015-01-01","2019-02-01");
```
### Step 3: Filter to Get Images from Different Look Angles
We separate the images into ascending and descending orbits.
```ruby
var vhAscending = vh.filter(ee.Filter.eq('orbitProperties_pass', 'ASCENDING'));
var vhDescending = vh.filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING'));
```
### Step 4: Create Composite Images 
We create composite images by selecting the maximum VH value for each pixel from the ascending and descending pass images.
```ruby
var vhMaxA = vhAscending.select('VH').max();
var vhMaxD = vhDescending.select('VH').max();
```
### Step 5: Visualizing Results
We set the map center to Venice, and add the composite layers to the map.
```ruby
Map.setCenter(12.3358, 45.4375, 11);
Map.addLayer(vhMaxA, {min: -15, max: 0}, 'VH ASC max', 0);
Map.addLayer(vhMaxD, {min: -15, max: 0}, 'VH DES max');
```
## Explanation of Ascending and Descending Orbits

> Ascending passes occur when the satellite moves from south to north, typically in the afternoon or evening.
> Descending passes happen when the satellite moves from north to south, usually in the morning.
>



> These orbits have key differences. The different times of day—afternoon for ascending and morning for descending—affect surface reflections. Additionally, the varying incidence angles of each pass reveal different surface features.
>
> In this tutorial, we use these differences to detect ships. Bright spots in the radar images indicate strong signals returned from large ships, while darker areas represent the sea surface. By creating a maximum value composite from these images, we can effectively highlight sea lanes and track ship movements. This method also helps distinguish real ships from artifacts or "ghost ships" caused by interferences and SAR ambiguities.
>

