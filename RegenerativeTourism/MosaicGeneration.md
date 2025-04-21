## 📦 1. Import required libraries


```python
# Import core libraries for raster processing and file handling

import os
import rasterio
from rasterio.merge import merge
```

## 📂 2. Define the input directory


```python
# Path to the folder containing all the normalized and georeferenced tiles to be merged

folder_path = "/path/to/your/tiles"
```

## 🧠 3. Define the function to generate the mosaic


```python
# This function searches for all GeoTIFF tiles in the folder,
# opens them, merges them into a single raster,
# and saves a temporary mosaic file with updated metadata.

def unir_tiles(folder_path):

    # Merge all PlanetScope tiles from a folder into a single temporary mosaic.
 
    
    # 🔍 List all GeoTIFF files in the folder
    raster_files = [os.path.join(folder_path, f) for f in os.listdir(folder_path) if f.endswith('.tif')]
    
    # 📂 Open each raster as a dataset object
    datasets = [rasterio.open(f) for f in raster_files]
    
    # 🔄 Merge all tiles into a single mosaic (mean by default on overlaps)
    mosaic, transform = merge(datasets)
    
    # 🛠️ Copy metadata from the first tile and update with new dimensions & transform
    meta = datasets[0].meta.copy()
    meta.update({
        "driver": "GTiff",
        "height": mosaic.shape[1],
        "width": mosaic.shape[2],
        "transform": transform,
        "count": mosaic.shape[0]
    })
    
    # 💾 Save the result as a temporary GeoTIFF mosaic
    temp_path = "temp_mosaic.tif"
    with rasterio.open(temp_path, "w", **meta) as dest:
        dest.write(mosaic)
    
    # 🧹 Clean up by closing all opened datasets
    for ds in datasets:
        ds.close()
    
    return temp_path

```

## 🚀 4. Execute the function


```python
# Run the mosaic creation process
mosaic_path = unir_tiles(folder_path)
print(f"Mosaic successfully saved at: {mosaic_path}")
```
