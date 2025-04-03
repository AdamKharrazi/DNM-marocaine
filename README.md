# DNM-marocaine
💧 Mapping Water for Sustainable Agriculture in Morocco.  This project entitled "La DNM marocaine" aimed at mapping the Maximum Net Dose (DNM) across the entire Moroccan territory.🌱 This project aims to provide a flexible technical tool for agronomists to better plan irrigation and make the most of our water resources.

Introduction
This project aims to map the Maximum Net Dose (DNM) across the Moroccan territory. The DNM is a key parameter in planning and optimizing localized irrigation, as it helps determine the maximum amount of water that can be applied without causing losses or water stress for crops. This mapping is based on soil texture data at different depths and validated scientific models. The application, developed using ArcGIS and RStudio, is designed to be flexible and adaptable to any crop type by simply adjusting a few parameters: the management coefficient (f), rooting depth, and the percentage of wetted soil.

How the Application Works
1. Download RStudio

2. Download the folder “La DNM marocaine”

3. Extract the folder

4. Open the app.R file

5. Select and run the code

6. Choose the coefficient f:

  The f values can be determined as follows (UNDP, 2020):

    f = 0.3: deep-rooted crops with low added value

    f = 0.2: medium-rooted crops with average value

    f = 0.1: shallow-rooted crops with high added value

7. Choose the rooting depth

8. Choose the percentage of wetted soil, using the estimation guide by J. Keller and D. Karmelli (UNDP, 2020), and based on your spacing between drip lines and emitters, soil texture, and emitter flow rate.

Project Development Steps

1) Data Collection
Extract TIF files containing soil texture data at four depth intervals:
0–5 cm, 5–15 cm, 15–30 cm, and 30–60 cm.
These files, created by SoilGrids, are based on a study by Hengl et al. (2015) conducted as part of the Africa Soil Information Service (AfSIS) project from 2008 to 2014. Over 28,000 sampling sites were analyzed using the Random Forest machine learning algorithm to generate spatial predictions of key soil properties for agricultural management, including texture.

  Source 1:
  These files provide clay and sand percentages for the four depths.

  Source 2:
  These files provide data based on a 12-class USDA texture scale (Moreno-Maroto et al., 2022):

2) Data Cleaning and Preparation
Check raster metadata in RStudio:

  Resolution: 250 m × 250 m
  Coordinate system: LAEA

Reproject the rasters to WGS84 in RStudio:

The original projection is Lambert Azimuthal Equal Area (LAEA), which must be converted to WGS84 for proper display in Leaflet (R Shiny).

Clean NODATA values and remove outliers in RStudio.

3) Data Processing
Import rasters into ArcGIS

Convert the 4 USDA rasters from Source 1 into percentage values for clay and sand:

Create a lookup table matching each USDA class to corresponding clay/sand percentages

Use the Reclassify by Table tool

Output:

8 rasters for clay percentages at 4 depths

8 rasters for sand percentages at 4 depths

Compute the weighted average for each property across the 4 depths using the Raster Calculator:

For clay percentage:

scss
Copier
Modifier
(clay_0005 * 0.084) + (clay_0515 * 0.166) + (clay_1530 * 0.25) + (clay_3060 * 0.50)  
For sand percentage:

scss
Copier
Modifier
(sand_0005 * 0.084) + (sand_0515 * 0.166) + (sand_1530 * 0.25) + (sand_3060 * 0.50)  
Verify compatibility between the two data sources and compare results

Compute the average of the two sources if appropriate

Calculate Field Capacity (HCC) and Permanent Wilting Point (HPFP) using the Saxton model

4) Creation of the Interactive DNM Map
Limit the map to Moroccan borders using a shapefile

Develop the application using R Shiny and Leaflet

References
Hengl, T., Heuvelink, G. B., Kempen, B., Leenaars, J. G., Walsh, M. G., Shepherd, K. D., ... & Tondoh, J. E. (2015). Mapping soil properties of Africa at 250 m resolution: Random forests significantly improve current predictions. PLoS ONE, 10(6), e0125814.

Moreno-Maroto, J. M., & Alonso-Azcarate, J. (2022). Evaluation of the USDA soil texture triangle through Atterberg limits and an alternative classification system. Applied Clay Science, 229, 106689.

UNDP – United Nations Development Programme. (2020). Fertigation Manual – Fertilizer irrigation in irrigated vegetable farming. French Version, 133.
