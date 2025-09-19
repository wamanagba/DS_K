## Cobalt Heatmap Generator

This repository contains a Python script that generates a **cobalt prospectivity heatmap** from geological bedrock polygons.  
The method is based on **distance to specific rock types** and produces both a GIS-ready raster (`.tif`) and a visualization image (`.png`).

---

## Problem & Approach

The challenge is to identify areas that are **potentially favorable for cobalt** by analyzing geological formations.  
We assume that cobalt is most likely to be found where **ultramafic/serpentinite rocks (Group A)** occur in proximity to **granodiorite rocks (Group B)**.

The solution uses a **linear decay proximity model**:
- Each raster cell receives a **score between 0 and 1**.  
- Score = `max(0, 1 - d1/D) * max(0, 1 - d2/D)`  
  - `d1`, `d2`: distances to nearest Group A and Group B rocks.  
  - `D`: influence distance (`DECAY_M`).  
- The score is **high only where both groups are close**.  
- Larger `DECAY_M` values → more regional influence.  
- Smaller values → more local hotspots.

---

## Input Data

- **Bedrock polygons** (`.gpkg`) with lithology descriptions.  
- Optional: a **land/ocean mask layer** to restrict analysis to land.  

The script scans several text columns (`rock_class`, `rock_type`, `unit_desc`, etc.) for keywords defined by regex patterns.

---

