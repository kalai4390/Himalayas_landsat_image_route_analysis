# Analyzing the change in Himalayan terrain using classification and finding optimal route to move cattle towards grazing lands


## Objective : The objective of this analysis is two fold. 

First, I analyze the change in terrain of a particular region in the Himalayan range over a period of seven years
Second, I find an optimal route from a source location to destination (both locations identified through geoprocessing). People in the hills around this region migrate their cattle on a yearly basis, crossing the Himalayas for large grazelands. This objective tries to find an optimal route for them by combining various parameters.
Datasets : I have downloaded the datasets required for this analysis from United States Geological Survey (USGS). The images from Landsat 8 and the corresponding Digital Elevation Model for the area was downloaded.
       Landsat : 2020/12/03  Path: 145  Row: 039
                 2013/05/22  Path: 145  Row: 039

 ## Analysis: The DEM and Landsat rasters were imported into ArcMap. 
The different raster bands were combined using the Bands tool available in ArcToolbox. Then, the bands values for RGB were reassigned effective visualization of the raster images. This process was repeated for both the raster datasets (2020 and 2013). 
Clip raster tool was used to align both the DEM and landsat rasters. 

## Digitizing Rasters: 

The next step was to compare the changes in the terrain over the period of seven years. The raster obtained during 2020, was digitized for this purpose. I used four classes for my analysis. Image classification tool was used for this purpose. A total of 80 samples digitized, assigning 20 samples for each class (snow, forest, bare hills, grazing land). The samples were combined to form four classes. The corresponding signature file was exported.
Maximum Likelihood Classification was used to classify the raster data. Isocluster Unsupervised Classification was also tried. Better results were obtained using the supervised method for four classes. The signature file was also used to classify the raster image of 2013 dataset. 
Observations: Tabulate area tool was used to compare the area occupied by each class and the percentage between 2020 and 2013. The forest cover area is high in 2020 when compared to 2013. This could be attributed to the time of measurement of these rasters, since seasonal changes greatly affect the greenery present in the terrain. Snowcover is also seen to occupy a larger area over the northern side of the Himalayas. 

## Objective 2: To find suitable areas for grazing cattle, and plan an optimal route from the source to destination. 

# Part 1 Analysis: Two toolboxes were designed to accomplish this task using ModelBuilder. 

The Area Analysis Toolbox has the suitableareas model. It accepts the Digital Elevation Model and the raster obtained from the classification operation. Suitable areas for grazing cattle is determined using this model. The areas must not be steep, must not be covered in snow or barren. 

 
Slope tool is used to calculate the slope of the DEM. Then Reclassify tool is used to reclassify the slope values into a common scale for comparison with the classification raster. Since the classification raster has four classes, the output of slope tool is reclassified into four classes with 4 being the area with least slope. The classification raster is also reclassified to represent the grazing land with the highest value 4. Weighted overlay tool is used to assign 60 percent weightage to slope and 40 percent to classification raster. The output is then passed as input to a conditional tool. The output of this operation is fed as input to majority filter tool with number of neighbors’ parameter set to 8. The final result is visualized as filtered optimal areas. 	

Raster to polygon tool was used to convert these areas to polygons. This was fed as input to Select layer by attribute tool to select only values greater than 9809371 square metres. Out of 4638 polygons, seven satisfied the criteria for a huge graze land. The source and destination were extracted from this layer using the copy features tool.

## Part 2 Analysis: 

Once the source and destination areas are identified, an optimal path needs to be found to traverse. The OptimalRoute toolbox was created to satisfy this requirement. It takes the suitable areas, source and destination as input parameters and outputs an optimal route. 
 

Cost distance tool takes source and suitable areas as input and outputs cost distance and a backlink raster which along with the destination feature is used as input for the cost path tool. It outputs a path for each cell or zone depending on the user settings. This project finds a single route from source to destination and also multiples routes from source to destination using each cell parameter. Finally, raster to polygon tool is used to visualize the route. 
