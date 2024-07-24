
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
### Step 3: Creating Composites - Filter to Get Images from Different Look Angles
We separate the images into ascending and descending orbits.
```ruby
var vhAscending = vh.filter(ee.Filter.eq('orbitProperties_pass', 'ASCENDING'));
var vhDescending = vh.filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING'));
```
Step 4: Create Composite Images
We create composite images by selecting the maximum VH value for each pixel from the ascending and descending pass images.


var vhMaxA = vhAscending.select('VH').max();
var vhMaxD = vhDescending.select('VH').max();
Visualizing Results
Step 5: Map Composite over Venice, Italy
We set the map center to Venice, Italy, and add the composite layers to the map.

javascript
Copy code
Map.setCenter(12.3358, 45.4375, 11);
Map.addLayer(vhMaxA, {min: -15, max: 0}, 'VH ASC max', 0);
Map.addLayer(vhMaxD, {min: -15, max: 0}, 'VH DES max');
Explanation of Ascending and Descending Orbits
Ascending Pass
Orbit Direction: South to North
Local Time: Afternoon or evening
Viewing Geometry: Provides a specific angle different from descending pass, useful for capturing unique surface features.
Descending Pass
Orbit Direction: North to South
Local Time: Morning
Viewing Geometry: Offers a complementary angle to the ascending pass, revealing additional surface details.
Key Differences
Lighting Conditions: Different times of the day (afternoon for ascending, morning for descending) affect surface reflections.
Look Angle and Geometry: Different incidence angles reveal different surface features.
Complementary Information: Combining both passes provides a comprehensive understanding of surface features.
Temporal Resolution: Increases data frequency for monitoring changes over time.
Conclusion
By analyzing both ascending and descending pass images, we gain a more detailed and accurate understanding of the Earth's surface. This tutorial provides a foundation for working with Sentinel-1 VH polarization data in Google Earth Engine.

For further customization and analysis, explore the GEE documentation and modify the script to suit your needs.
