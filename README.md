# 🗺️ Cobalt Heatmap Generator

This repository provides a Python script to generate a **geospatial heatmap** that highlights areas with potential cobalt-rich geology.  
The method is based on **proximity scoring**: cells close to both **ultramafic/serpentinite rocks** (Group A) and **granodiorite rocks** (Group B) receive higher scores.

---

## 📌 Overview

- **Input:** Geological bedrock polygons (`.gpkg`).  
- **Process:**  
  1. Extract polygons containing specific lithology keywords (regex-based search).  
  2. Compute distances to **Group A** and **Group B** rock types.  
  3. Apply a **linear decay function** with cutoff distance `DECAY_M`.  
  4. Rasterize results into a grid (`RES_M` resolution).  
- **Output:**  
  - A **GeoTIFF raster** (`.tif`) with scores between `0–1`.  
  - A **PNG heatmap** for visualization.  

The score is **1.0** when a cell touches both rock groups, and decreases linearly to **0.0** at distance `DECAY_M`.

---

## ⚙️ Configuration

Edit the configuration block at the top of the script before running:

```python
INPUT      = "path/to/BedrockP.gpkg"     # Bedrock polygons
OUT_TIF    = "path/to/heatmap_cobalt.tif" # GeoTIFF output
OUT_PNG    = "path/to/heatmap_cobalt.png" # PNG preview

PATTERN_A  = r"ultramafic|serpentin\w*|dunite|harzburgite|peridotite|komatiite|ophiolite"
PATTERN_B  = r"granodior\w*"

RES_M      = 250     # Grid resolution in meters
DECAY_M    = 10000   # Influence radius (meters)
CRS_EPSG   = 26910   # Coordinate reference system (EPSG code)

