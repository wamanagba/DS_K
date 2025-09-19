README — Proximity Heatmap Generator (A × B)

This notebook builds a **proximity heatmap** that highlights areas simultaneously close to two lithology groups:
- **Group A**: ultramafic/serpentinite units (e.g., dunite, peridotite, komatiite)
- **Group B**: granodiorite units

The score in each grid cell is:
\[
\text{score} \;=\; \max\!\left(0,\, 1 - \frac{d_A}{D}\right)\; \times\; \max\!\left(0,\, 1 - \frac{d_B}{D}\right)
\]
where \(d_A\) and \(d_B\) are distances (meters) to Groups A and B, and \(D\) is the **decay distance** (meters).  
The output is a **GeoTIFF** (GIS-ready raster) and a **PNG** preview.

---

## 1) Requirements

Install once (e.g., in a terminal or a notebook cell with `!`):

```bash
pip install geopandas rasterio shapely matplotlib numpy


## 2) Files & Configuration

Edit the **CONFIG** block near the top of the notebook:

- `INPUT`: path to the **GeoPackage** with bedrock polygons and lithology text columns.  
- `OUT_TIF`, `OUT_PNG`: output paths for the raster and image.  
- `PATTERN_A`, `PATTERN_B`: regex patterns to tag Groups A and B from attributes.  
- `RES_M`: grid cell size (meters). Smaller → finer detail (heavier compute).  
- `DECAY_M`: distance (m) where proximity decays to 0; larger → broader halos.  
- `CRS_EPSG`: projected CRS EPSG code (meters).  
- `TEXT_COLS_ALL`, `TEXT_COLS_B`: columns scanned for regex matches.  
- `NODATA_VAL`: value written for nodata cells in the GeoTIFF.
