# Lab: QGIS Mutant Team
## Assignment

## Deliverable
`Pull request` to merge a new branch named `assignment` with `master`. Your branch should contain:
1. `screenshot-manhole-density.png`
2. `neighborhood-manhole-density.csv`

### Overview
Recent reports of mutant creatures emerging from Tucson's sewer pipes at night has prompted a local realtor to hire 
you to conduct a Geospatial Analysis to identify the neighborhoods most at risk of encountering these mutant creatures 
while walking around the neighborhood at night. While there have been no negative incidents associated with the creatures, 
some residents have reported feeling uneasy at their presence. On the other hand, some officers from the Tucson Police 
Department have anecdotally reported that crime has actually decreased in locations where they have been found. 

![PBF Comic: Secret Mutant Hero Team](PBF198-Secret_Mutant_Hero_Team.jpg)
You will use the City of Tucson's Open GIS Data to conduct this analysis. The City of Tucson makes their GIS Data publicly available for download but also available for consumption as web services. 

### Objective
Find the neighborhoods with the highest density of man hole covers.

### Source Data
You will use two layers from the [City of Tucson's Open GIS Datasets](http://gisdata.tucsonaz.gov/) to conduct this analysis.
The data are available in d2l (no longer publicly available from COT):
- [Man Holes](https://d2l.arizona.edu/d2l/le/content/1094533/viewContent/11518017/View)
- [Neighborhood Associations](https://d2l.arizona.edu/d2l/le/content/1094533/viewContent/11518018/View)

### Strategy
The overall goal is to identify the density of manhole covers within each subdivision. 

`Density = Count / Area`

Complications: The Man Holes data is a line feature showing the outline of each of the manholes. While it might make sense
to convert them directly to points, that will simply export the polygon boundaries as points and whwn you calculate density, you will get different (and wrong) results if some man holes were digitized were more precision than others (or if they are simply bigger). The metric you want with man hole covers is `Count`. 

I suggest:
1. Derive centroids of the Manholes _Lines_ layer.

Another complication: The Source Data is in EPSG:4326, a lat/long coordinate reference system which uses units `degrees`, making measurement impractical at the city scale. You will need to reproject the source data into a coordinate reference system in which distance is preserved. I suggest UTM Zone 12, NAD83 HARN (aka `EPSG:102206`). 

1. Reproject both input layers to a UTM NAD83_HARN projection.

### Key QGIS Tutorial for reference
While you would and should be tempted to perform a spatial join as in http://www.qgistutorials.com/en/docs/performing_spatial_joins.html, doing so raises a common side effect of using open source software and of using publicly available tutorials. 
This tutorial was written for a previous version of QGIS. The work flow for QGIS 3 is slightly different. Follow the tutorial as it is written but when you get to step 8, which asks you to `Join attributes by location`, you can easily get 
sucked into the vortex of using a look-alike tool that does not do what you want to summarize the intersection features of the join layer. That is, you want to `Sum` all the man hole covers within each `neighborhood association` but the default Menu Item in QGIS (`Vector` -> `Data Management Tools` -> `Join attributes by location`) does not give you the option to summarize the man hole covers. Instead, use the `Processing Toolbox` and search for `join`. Look for the option labeled `Join attributes by location (summary)`
![Join Processing Toolbox Screenshot](join_processing.png)

The projection must be in a metric like UTM in order for density calculation to work. To calculate density (`Density = Count / Area`), open the Attribute Table and open the Field Calculator. Use the middle panel to search for Fields and Functions to help you find the right syntax. The final calculation will look like this:

![Screenshot of field calculator](field_calculator.png)

### Visualization
Calculate the density of the manhole covers by neighborhood and use a `Graduated` symbology to classify the density field into 10 classes. Use any color scheme that you like. When you click `Apply` you should get a map like this:
![Screenshot of density map](sample_screenshot.png)

Save a screenshot of your QGIS workspace in a file named `screenshot-manhole-density.png`.

### Summary File
The realtor is most interested in the five neighborhoods with the densest coverage of man hole covers. Open the attribute table, click on the column that you created for `density` and sort it descending. Select the top five rows by clicking on the header at the left of the row. The selected rows should turn yellow. Right click on the layer in the `Layers` panel and `Export` the layer, choosing the `CSV` format. Name the file `neighborhood-manhole-density.csv`.
