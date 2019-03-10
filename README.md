# Lab: Secret Mutant Hero Team Map
## Worth: 40
## Due: 7 days
## Assignment

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
The data are summarized below:
- [Man Holes](http://gisdata.tucsonaz.gov/datasets/60a2bb58e8054bee8562127bfa0d9fc1_9)
- [Neighborhood Associations](http://gisdata.tucsonaz.gov/datasets/828d637891e94d95a2e62cf62ad2f7e0_0)

#### Data Instructions
Use the links above to find the landing page for each of the datasets or find the data using the Search feature on the [City of Tucson GIS Open Data website](http://gisdata.tucsonaz.gov/) using the search terms "Man holes" and "Subdivisions". The following instructions are for Man Holes but can be applied to any of the City of Tucson's Open Data datasets:

1. Open the "Man Holes - Open Data" page and look for a Drop-down for "APIs" -- Click this and select the link listed under "GeoJSON".
2. In QGIS, select "Layer" from the Menu Bar, then "Add Layer" -> "Add Vector Layer". 
3. For "Source Type", select "Protocol: HTTP(S), cloud, etc.)
4. For Protocol, select "GeoJSON" and paste the URI you copied from the City of Tucson's website. 
5. Click "Add" to add to add the data to your QGIS project. 
6. It will have an unreadable name, so right click on the Layer in the Layer Menu and Rename it to "Man Holes".

Repeat for Neighborhood Assocations.

### Strategy
The overall goal is to identify the density of manhole covers within each subdivision. 

`Density = Count / Area`

Complications: The Man Holes data is a line feature showing the outline of each of the manholes. While it might make sense
to convert them directly to points, that will simply export the polygon boundaries as points and whwn you calculate density, you will get different (and wrong) results if some man holes were digitized were more precision than others (or if they are simply bigger). I suggest:
1. Polygonize the Lines to create closed polygon features for the Man Holes
2. Find centroids of the polygons
3. Perform a spatial join as in http://www.qgistutorials.com/en/docs/performing_spatial_joins.html. 

Issue with spatial join: reproject both layers to UTM. 
