# 🌿 Monitoring Ecological Change in Regenerative Tourism Sites

<br>

A story of pixels, plants, and purpose — built from satellite data and powered by the idea that tourism can help nature thrive.
<br>

### 📅 Date
<ol>
January 2025
</ol>
   
### 🏢 Organization
<ol>
DAI, USAID's Destination Nature Activity
</ol>

### 🎯 Objectives

<ol>
<p>🔢 Quantify the number of hectares showing ecological improvement as a result of regenerative tourism activities.<br>🔦 Strengthen evidence-based decision-making for adaptive management of nature-based tourism projects.<br>🚀 Identify strategies to enhance ecosystem recovery in areas showing signs of vegetation stress.</p>
</ol>

### 💻 How it was done (Methodological Workflow)
<br>
<ol>
   
<details>
<summary>1. Context Review & Data Gathering</summary>
<br>
<ol>
   - Defined relevant timeframes for baseline, intervention, and post-intervention analysis.<br>
   - Collected and normalized monthly PlanetScope imagery for each tourism corridor.<br>
</ol>
</details>

   
<details>
<summary>2. Mosaic generation</summary>
<br>
<ol>
   - Union of the unit downloaded images to create seamless image coverage with precise spatial alignment. <br>

<br>
<br>

![Alt text here](https://github.com/WennOlarteE/portfolio/blob/main/RegenerativeTourism/MosaicGen.drawio.png)



</ol>


</details>



<details>
<summary>3. Study Area Delimitation</summary>
<br>
<ol>
   - Used vector shapefiles to clip mosaics to the extent of each corridor.<br>
   - Masked out urban zones and water bodies to focus on vegetated areas.  
</ol>
</details>


<details>
<summary>4. NDVI Calculation</summary>
<br>
<ol>
- Computed monthly NDVI to assess vegetation health:<br>
<ol>
   - High NDVI → Dense, healthy vegetation<br>
   - Low NDVI → Bare soil or water
</ol>
</ol>
</details>


<details>
<summary>5. Exploratory Statistics & Visualization</summary>
<br>
<ol>
- Extracted metrics per pixel and corridor level:<br>
<ol>
   - Max, Min, Mean, Median, and 90th Percentile<br>
</ol>
- Created:<br>
<ol>
   - Thematic NDVI maps<br>
   - Histograms to explore data distribution<br>
   - Monthly time series graphs<br>
</ol>
</ol>
</details>



<details>
<summary>6. Annual Processing & Spatial Modeling</summary>
<br>
<ol>
- Generated yearly NDVI composites and standardized resolutions.<br>
<ol>
   - Applied Spatial Autoregressive (SAR) Models to:<br>
   <ol>
      - Quantify change over time<br>
      - Detect spatial trends and hotspots<br>
      - Identify significant improvement or degradation clusters<br>
   </ol>
   - Produced:<br>
   <ol>
      - Annual NDVI change maps<br>
      - Comparative boxplots<br>
      - Spatial autocorrelation visuals<br>
   </ol>
</ol>
</ol>
</details>
     
</ol>




### ⚙️ Tools & Technologies

<ol>
- Languages: Python (GeoPandas, rasterio, NumPy, matplotlib) <br>
- Geospatial Tools: ArcGIS Pro, QGIS, Google Earth Engine <br>
- Visualization: matplotlib, seaborn <br>
- Version Control: Git & GitHub <br>
- Jupyter Notebook <br>
</ol>


### 📊 Outputs

<ol>
- Reproducible code and methodology in Jupyter Notebook <br>
- Vegetation trend graphs per corridor <br>
- Spatial regression maps supporting decision-making <br>
</ol>

### 🔍 Key Takeaways

<ol>
- Integrating NDVI with SAR modeling provided a rich and credible view of ecological dynamics. <br>
- Spatial insights helped ground adaptive management in real, localized evidence. <br>
- The entire workflow is scalable and adaptable to other contexts involving nature-based interventions. <br>
</ol>

```mermaid
graph LR
    A[Tiles PlanetScope] --> B[Mosaico]
    B --> C[Recorte]
    C --> D[NDVI]
    D --> E[Estadísticas]
    D --> F[Visualización]
```

```mermaid
flowchart LR
    subgraph A[📥 Input]
        A1["Monthly normalized PlanetScope tiles (.tif)"]
    end

    subgraph B[⚙️ Processing]
        B1["List .tif files"]
        B2["Open with rasterio"]
        B3["Merge using merge()"]
        B4["Update metadata"]
        B5["Save temp_mosaic.tif"]
    end

    subgraph C[📤 Output]
        C1["Temporary GeoTIFF mosaic"]
        C2["Ensures spatial continuity"]
    end

    A1 --> B1 --> B2 --> B3 --> B4 --> B5 --> C1 --> C2
```
