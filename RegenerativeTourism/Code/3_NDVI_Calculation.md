## 🧰 Import Libraries


```python
# Import libraries for raster processing and array manipulation

import numpy as np
import rasterio
```

## 📁 Define Input and Output Paths


```python
# Path to the clipped mosaic created in the previous step
fn_clip = "Path to the clipped mosaic (e.g., clipped_corridor.tif)"

# Path to save the NDVI output
fn_ndvi = "Path to save the NDVI raster (e.g., ndvi_corridor.tif)"
```

## 🧮 Define the NDVI Calculation Function


```python

# This function calculates NDVI using a PlanetScope mosaic:
#    - Reads Red (Band 3) and NIR (Band 4) from the input raster
#    - Applies a validity mask to exclude NoData or zero-value pixels
#    - Uses the NDVI formula: (NIR - Red) / (NIR + Red)
#    - Adds a small constant (+1e-10) to prevent division by zero
#    - Saves the result as a single-band raster with updated metadata

def calcular_ndvi(fn_clip, fn_ndvi):
    """Calcular NDVI a partir del mosaico recortado."""
    
    # 📥 Open the input raster
    with rasterio.open(fn_clip) as src:
        # Read NIR (Band 4) and Red (Band 3)
        nir_band = src.read(4)
        red_band = src.read(3)
        
        # Define the NoData value
        nodata_value = src.nodata or -9999
        
        # 🧼 Create a validity mask to exclude invalid or zero-value pixels
        mask = (
            (nir_band != nodata_value) &
            (red_band != nodata_value) &
            (nir_band > 0) &
            (red_band > 0)
        )
        
        # 🌿 Calculate NDVI with safety offset to avoid division by zero
        ndvi = np.where(
            mask,
            (nir_band - red_band) / (nir_band + red_band + 1e-10),
            nodata_value
        )
        
        # 🧾 Update metadata for the NDVI raster
        meta = src.meta.copy()
        meta.update({
            "count": 1,
            "dtype": "float32",
            "nodata": nodata_value
        })
        
        # 💾 Save the NDVI raster
        with rasterio.open(fn_ndvi, "w", **meta) as dst:
            dst.write(ndvi, 1)
    
    return fn_ndvi
    
```

## 🚀 4. Execute the function


```python

# Run the NDVI calculation
ndvi_output = calcular_ndvi(fn_clip, fn_ndvi)
print(f"✅ NDVI raster generated and saved to: {ndvi_output}")

```
