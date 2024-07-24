# 1. SHIPS
## Sentinel-1 VH Polarization Analysis
### Step 1: Load the Sentinel-1 ImageCollection
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

<img width="732" alt="Screenshot 2024-07-24 at 12 33 41 PM" src="https://github.com/user-attachments/assets/e7021140-8896-482f-8daf-f4531adc0fc7">



