# Proximity Heatmap (A×B)

Generates a proximity map highlighting areas simultaneously close to two lithology groups:

* Group A (ultramafic / serpentinized): ultramafic, serpentin\*, dunite, harzburgite, peridotite, komatiite, ophiolite
* Group B (granodiorite): granodior\*
  Outputs: GeoTIFF (1 band, float32, 0–1, GIS-ready) and PNG preview.

## Prerequisites

* Python ≥ 3.10
* Dependencies: geopandas, rasterio, shapely, numpy, matplotlib
* Tip: if installation fails (GDAL/GEOS/PROJ issues), use Conda (conda-forge channel).

#To Configure (at the top of the script)

* INPUT: path to BedrockP.gpkg
* OUT_TIF / OUT_PNG: output paths
* RES_M: cell size in meters (e.g., 250)
* DECAY_M: influence distance where proximity drops to 0 (e.g., 10,000)
* CRS_EPSG: projected EPSG in meters (e.g., 26910)

## How to Run

* In Jupyter: run the cells (config + functions), then call the main function(`make\_heatmap`) to produce the GeoTIFF and PNG.
* As a script: execute the Python file after adjusting paths and parameters.
* Quick check: run the self-test to confirm the scoring logic.

## Method (summary)

* Regex-based tagging of polygons into A and B (text columns).
* Separate geometric unions for groups A and B.
* Regular grid over the extent (resolution = RES_M).
* For each cell center: vector distances dA and dB (Shapely).
* Linear decay: f(d) = max(0, 1 − d / DECAY_M).
* Final score: fA × fB (high only if close to both).
* Export: 0–1 GeoTIFF + PNG + summary (metadata).

## Key Parameters

* RES_M: detail vs. time/memory tradeoff.
* DECAY_M: controls the width of the proximity “halo.”
* PATTERN_A / PATTERN_B: adapt to your lithology labels.
* TEXT_COLS_\*: columns scanned for tagging.
* CRS_EPSG: must be projected in meters (script reprojects if needed).

## Interpretation

* Values near 1: priority areas, close to both groups.
* Values near 0: far from at least one group.

## Troubleshooting

* Empty/unreadable input file: check the path and that polygons exist.
* No A/B matches: loosen regex patterns or add more columns to scan.
* Odd distances / empty grid: use a projected CRS in meters and a RES_M consistent with your area extent.

## Production Note

For very large areas or repeated runs, prefer a raster + Euclidean Distance Transform (EDT) workflow: faster and more robust, while keeping the same logic (linear decay + product).

