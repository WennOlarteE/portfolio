## 🧰 Import Libraries


```python
# Import core libraries for raster processing and file handling

import os
from osgeo import gdal
import rasterio
import matplotlib.pyplot as plt
```

## 📁 Define Input and Output Paths


```python
# Path to the temporary mosaic created in the previous step
temp_path = "Path to the temporary mosaic (e.g., temp_mosaic.tif)"

# Path to the shapefile defining the study area (tourism corridor)
fn_poly = "Path to the shapefile of the tourism corridor"

# Path to save the clipped raster output
fn_clip = "Path to save the clipped raster (e.g., clipped_corridor.tif)"
```

## ✂️ Define the Clipping Function


```python

# Clip the temporary mosaic using a shapefile of the study area (tourism corridor).
    
#    Processing steps:
#    - Use GDAL Warp to apply exact clipping based on the vector geometry
#    - Enable multithreading for better performance
#    - Set NoData value to -9999 to exclude masked areas from future analyses
#    - Output a Float32 raster optimized for large-scale spatial operations


def cortar_mosaico(temp_path, fn_poly, fn_clip):
    
    # 🛠️ Define warp options for clipping
    options = gdal.WarpOptions(
        format='GTiff',
        cutlineDSName=fn_poly,
        cropToCutline=True,
        dstNodata=-9999,
        multithread=True,
        outputType=gdal.GDT_Float32
    )
    
    # ✂️ Perform the clipping operation
    result = gdal.Warp(fn_clip, temp_path, options=options)
    
    # 🚨 Raise an error if the process fails
    if result is None:
        raise RuntimeError("Clipping process failed.")
    
    return fn_clip
    
```

## 🚀 4. Execute the function


```python
# Run the clipping function using the defined paths
mosaico_recortado = cortar_mosaico(temp_path, fn_poly, fn_clip)
print(f"✅ Raster clipped and saved to: {mosaico_recortado}")
```

## 🖼️ Visualize the Clipped Raster


```python
# Open the clipped raster
with rasterio.open(mosaico_recortado) as src:
    clipped_image = src.read(1)  # Read the first band
    profile = src.profile

# 🖼️ Plot the raster using matplotlib
plt.figure(figsize=(10, 8))
plt.imshow(clipped_image, cmap='viridis')
plt.colorbar(label='Pixel Value')
plt.title("Clipped Mosaic of the Tourism Corridor", fontsize=14)
plt.xlabel("Column Index")
plt.ylabel("Row Index")
plt.grid(False)
plt.show()
```
