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

Before diving into the analytical workflow, the project began with a context review and the careful selection of satellite imagery.

This step laid the groundwork for spatial and temporal consistency throughout the monitoring process. Three distinct timeframes were defined to guide the analysis:
<ol>
  • ⏳ Baseline: captures pre-intervention conditions<br>
  • 🔨 Intervention: during implementation of regenerative tourism strategies<br>
  • 🌱 Post-intervention: to assess outcomes over time<br>
</ol>

<details>
<summary>1. Context Review & Data Gathering</summary>
<br>
<ol>

The monitoring process began with a spatial and temporal framing of the study. Three timeframes were defined to capture changes in vegetation linked to regenerative tourism efforts:
<ol>
  • ⏳ Baseline (pre-intervention)  
  • 🔨 Implementation (during intervention)  
  • 🌱 Post-intervention (outcome assessment)
</ol>

To support this, monthly PlanetScope imagery was gathered for each tourism corridor. Only scenes with **less than 10% cloud coverage across the entire corridor extent** were considered valid. This threshold helped minimize noise in future NDVI calculations and ensured data consistency.

A structured validation process was implemented to transparently track usable and missing data:
<ol>
   • ✅ Each month-corridor combination was reviewed.  
   • 🔗 A checklist was produced with image links, cloud flags (when <10% cloud presence might still affect NDVI), and notes justifying temporal gaps.
</ol>

📝 Note:  
This transparent verification ensured high-quality imagery inputs while documenting limitations openly—a key practice in responsible environmental monitoring.

<br>🔄 Process Diagram

```mermaid
flowchart LR
    subgraph A[📥 Input]
        A1["• Monthly PlanetScope catalog<br>• Regenerative tourism corridor shapefiles"]
    end

    subgraph B[⚙️ Processing]
        B1["• Define analysis periods<br>• Filter imagery by <10% cloud coverage per corridor<br>• Validate and tag data availability"]
    end

    subgraph C[📋 Output]
        C1["• Curated list of valid images<br>• Checklist with links, cloud flags, and gap justifications"]
    end

    A1 --> B1 --> C1
```
<br>
📦 Monthly PlanetScope imagery was gathered for each tourism corridor, following two key quality criteria:<br>
<ol>
   • ☁️ Cloud Coverage: imagery with more than 10% cloud coverage over the full corridor extent was excluded to avoid noise in NDVI and temporal trends.<br>
   • ✅ Visual Validation: for each corridor and month, a checklist was produced with the following outputs:<br>
   <ul>
      - A link to visually inspect each tile over the corridor<br>  
      - An "R" tag marking scenes with <10% cloud presence that might still affect NDVI calculations<br>  
      - Notes indicating months with no usable data, justifying their exclusion from temporal analysis<br>
   </ul>
</ol>
<br>
📝 Note:  
This visual and quantitative vetting process helped ensure that temporal gaps were transparently documented, and that remaining imagery provided a consistent and reliable input for monitoring vegetation change.

<br>🔄 Process Diagram

```mermaid
flowchart LR
    subgraph A[📥 Input]
        A1["• Monthly PlanetScope catalog<br>• Regenerative tourism corridor shapefiles"]
    end

    subgraph B[⚙️ Processing]
        B1["• Define analysis periods<br>• Filter imagery by cloud coverage over corridor extent<br>• Perform visual validation and tagging"]
    end

    subgraph C[📋 Output]
        C1["• Folder with individual PlanetScope tiles (GeoTIFFs)<br>• Curated list of valid monthly images per corridor<br>• Checklist with links, cloud tags, and missing data notes"]
    end

    A1 --> B1 --> C1
```
</ol>
</details>

   
<details>
<summary>2. Mosaic generation</summary>
<br>
<ol>
To prepare the satellite imagery for analysis, the individual PlanetScope tiles downloaded for each tourism corridor were first merged into a single raster mosaic. This preprocessing step ensures all tiles are spatially aligned and simplifies subsequent workflows by reducing the number of input files.
<br><br>
This step has two main objectives:<br><br>
<ol>
   • 🧩 Integration: unify fragmented tiles into a seamless mosaic for the full corridor extent.<br>
   • 🛠️ Preprocessing: generate a base raster for further spatial analysis and clipping.
</ol>
<br>
📝 Note:  
The merge process defaults to pixel-wise averaging in areas where tiles overlap. This helps minimize radiometric discrepancies and smooths transitions between adjacent scenes, especially in zones with partial cloud cover.

<br>🔄 Process Diagram

```mermaid
flowchart LR
    subgraph A[📥 Input]
        A1["• Folder with individual PlanetScope tiles (GeoTIFFs)"]
    end

    subgraph B[⚙️ Processing]
        B1["• Uses rasterio to open all .tif files<br>• Merges them with rasterio.merge (defaults to average on overlap)<br>• Updates metadata (dimensions, transform, band count)<br>• Saves result as a temporary GeoTIFF mosaic"]
    end

    subgraph C[📤 Analysis]
        C1["• Creates a single raster aligned with corridor extent<br>• Reduces input complexity for future operations"]
    end

    subgraph D[📤 Output]
        D1["Temporary mosaic (GeoTIFF) covering the full area of interest"]
    end

    A1 --> B1 --> C1 --> D1
```
📷 Below is an example of the resulting mosaic raster for one of the regenerative tourism corridors:<br><br>
![alt text](https://github.com/WennOlarteE/portfolio/blob/main/RegenerativeTourism/Mosaico.png)<br>

💻 Want to explore the code behind this step? Check out the Jupyter Notebook:
[🔗 View the mosaic generation code](https://github.com/WennOlarteE/portfolio/blob/main/RegenerativeTourism/MosaicGeneration.md).

</ol>

</details>



<details>
<summary>3. Study Area Delimitation</summary>
<br>
<ol>

To ensure that all subsequent analyses focus solely on the relevant geographic extent, the temporary mosaic created in the previous step was clipped using the shapefile corresponding to the regenerative tourism corridor.

This step serves two main purposes:
<ol>
   • 🎯 Spatial focus: removing irrelevant surroundings and keeping only the core study area.

   •⚡ Performance optimization: reducing processing load for large-scale modeling.
</ol>
📝 Note:
Setting a NoData value (-9999) ensures that masked-out areas are excluded from subsequent analyses. This avoids distortions in calculations such as NDVI, where undefined pixels could otherwise bias results or trigger processing errors.
<br>
<br>🔄 Process Diagram

```mermaid

flowchart LR
    subgraph A[📥 Input]
        A1["• Temporary mosaic generated in the previous step (GeoTIFF format)<br>• Shapefile delimiting the tourism corridor"]
    end

    subgraph B[⚙️ Processing]
        B1["• Uses GDAL Warp to clip the mosaic with the shapefile<br>• Applies exact masking (cropToCutline=True) to restrict output strictly to the corridor<br>• Sets NoData value to -9999 for excluded areas<br>• Enables multithreaded processing for efficiency<br>• Maintains Float32 data type for optimized performance with large datasets<br>"]
    end

    subgraph C[📤 Analysis]
        C1["• Removes irrelevant areas outside the corridor, improving analytical focus<br>•Optimizes computational resources via parallel processing<br>•Preserves metadata integrity and precision for further modeling"]
    end

    subgraph D[📤 Output]
        D1["Clipped GeoTIFF raster containing only the target corridor area"]
    end

    A1 --> B1 --> C1 --> D1
```
📷 Below is a preview of one output tile resulting from the clipping process. This image is provided as an example; the full analysis included multiple mosaics across various corridors.<br><br>
![alt text](https://github.com/WennOlarteE/portfolio/blob/main/RegenerativeTourism/2_ClippedMosaic.png)<br>


💻 The full code used in this section is available at the following link:  
[🔗 Explore the full clipping workflow in this markdown](https://github.com/WennOlarteE/portfolio/blob/main/RegenerativeTourism/AreaDelimitation.md)

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


