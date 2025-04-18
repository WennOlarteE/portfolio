## 🌿 Monitoring Ecological Change in Regenerative Tourism Sites
A story of pixels, plants, and purpose — built from satellite data and powered by the idea that tourism can help nature thrive.

📅 Date
January 2025

🏢 Organization
DAI, USAID's Destination Nature Activity

🎯 Objectives
Quantify the number of hectares showing ecological improvement as a result of regenerative tourism activities.

Strengthen evidence-based decision-making for adaptive management of nature-based tourism projects.

Identify strategies to enhance ecosystem recovery in areas showing signs of vegetation stress.

🛤️ Methodological Approach
1. Context Review & Data Gathering
Defined relevant timeframes for baseline, intervention, and post-intervention analysis.

Collected and normalized monthly PlanetScope imagery for each tourism corridor.

Mosaicked tiles to create seamless image coverage with precise spatial alignment.

2. Study Area Delimitation
Used vector shapefiles to clip mosaics to the extent of each corridor.

Masked out urban zones and water bodies to focus on vegetated areas.

3. NDVI Calculation
Computed monthly NDVI to assess vegetation health:

High NDVI → Dense, healthy vegetation

Low NDVI → Bare soil or water

4. Exploratory Statistics & Visualization
Extracted metrics per pixel and corridor level:

Max, Min, Mean, Median, and 90th Percentile

Created:

Thematic NDVI maps

Histograms to explore data distribution

Monthly time series graphs

5. Annual Processing & Spatial Modeling
Generated yearly NDVI composites and standardized resolutions.

Applied Spatial Autoregressive (SAR) Models to:

Quantify change over time

Detect spatial trends and hotspots

Identify significant improvement or degradation clusters

Produced:

Annual NDVI change maps

Comparative boxplots

Spatial autocorrelation visuals

⚙️ Tools & Technologies
Python (GeoPandas, rasterio, NumPy, matplotlib) · QGIS · Jupyter Notebook

📊 Outputs
Reproducible code and methodology in Jupyter Notebook

Vegetation trend graphs per corridor

Spatial regression maps supporting decision-making

🔍 Key Takeaways
Integrating NDVI with SAR modeling provided a rich and credible view of ecological dynamics.

Spatial insights helped ground adaptive management in real, localized evidence.

The entire workflow is scalable and adaptable to other contexts involving nature-based interventions.

