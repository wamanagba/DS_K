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
