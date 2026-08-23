# KNP LAI GEE

Research project studying **Leaf Area Index (LAI) estimation** in Khadimnagar National Park (KNP), Sylhet, Bangladesh.

## Overview

The project combines field-measured LAI with satellite remote sensing data to train and validate an LAI estimation model for the park's forest canopy.

- **Field measurements**: LAI was measured in situ using a plant canopy analyzer.
- **Satellite data**: Optical/remote sensing imagery is retrieved and processed via [Google Earth Engine](https://earthengine.google.com/) (GEE).
- **Goal**: Use the satellite-derived predictors together with field LAI to train and validate an LAI estimation model.

## Data Processing

### Field data (ArcGIS Pro)

Raw point-level LAI measurements collected in the field were processed in ArcGIS Pro, where they were spatially clustered into **40 plot-level mean LAI values**. These plot locations correspond to the `SitesLocation` feature collection used in the GEE workflow.

### Satellite / GEE data (this repository)

The GEE and Python side of the analysis is being developed in this repository. Current work (see [lai_gee_workflow.ipynb](lai_gee_workflow.ipynb)) includes:

- Authenticating and initializing the Earth Engine Python API (`earthengine-api`, `geemap`)
- Loading the KNP boundary (`projects/osmgee/assets/KNP`)
- Loading the 40 plot-level site locations (`projects/osmgee/assets/SitesLocation`)

Planned/ongoing steps include extracting satellite predictors at each plot, and training and validating an LAI estimation model against the field-derived plot means.

## Repository Contents

- [lai_gee_workflow.ipynb](lai_gee_workflow.ipynb) — Main notebook for the Google Earth Engine / Python workflow.

## Requirements

- Python environment with `earthengine-api` and `geemap`
- A Google Earth Engine account with access to the `osmgee` GEE project
