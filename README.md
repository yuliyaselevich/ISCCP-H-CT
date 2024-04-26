# ISCCP HXG Dataset

## Overview

The ISCCP (International Satellite Cloud Climatology Project) HXG dataset is a collection of satellite-based cloud observations and climate data, including data from multiple geostationary and polar-orbiting satellites. The dataset provides high-resolution information about cloud cover and related atmospheric variables, such as cloud-top temperature, cloud-top pressure, and various radiative fluxes, organized by geographic location and time. Covering the period from July 1983 to June 2017, the dataset is continuously updated to extend its coverage, facilitating analyses of cloud patterns, climate change, Earth's radiation budget, and weather and climate models improvement.

## Analysis Steps

The analysis involves two main steps:
1. Identification and grouping of convective systems using the TOBAC (Tracking and Object-Based Analysis of Clouds) Python package.
2. Statistical summary generation for convective systems and families based on TOBAC output.

## TOBAC Parameters

TOBAC parameters used in the analysis:
- Feature detection:
  - Target: 'minimum'
  - Threshold: [245, 220]
  - Minimum threshold count: 2
  - Position threshold: 'weighted_diff'
  - Sigma threshold: 1.5
  - Erosion threshold: 2
- Segmentation:
  - Target: 'minimum'
  - Threshold: 245
- Feature linking:
  - Method: 'predict'
  - Maximum velocity: 30
  - Adaptive stop: 2
  - Adaptive step: 0.95
  - Stubs: 2
  - Subnetwork size: 20
  - Minimum time cell: 5 hours

For more information on TOBAC and its parameters, visit [TOBAC Documentation](https://tobac.readthedocs.io) and [TOBAC GitHub](https://github.com/tobac-project/tobac).

## File Format and Parameters

Each file in the database is a netCDF file containing one year of data. Variables stored for each convective system include frame ID, feature ID, datetime, latitude, longitude, cell ID, time cell, and various statistical parameters describing the convective system and family.

## References

- Machado, L.A.T., and W.B. Rossow, 1993: Structural characteristics and radiative properties of tropical cloud clusters.
- Machado, L.A.T., et al., 1998: Life cycle variations of mesoscale convective systems over the Americas.
- Schiffer, R.A., and W.B. Rossow, 1983: The International Satellite Cloud Climatology Project (ISCCP).

## Contacts for Information

- Yuliya Selevich (yuliya.selevich@gmail.com)
- Zhengzhao Johnny Luo (z.johnny.luo@gmail.com)
- Hanii Takahashi (hanii.takahashi@jpl.nasa.gov)

## Data Size Information

The database comprises annual data stored in both CSV and netCDF formats:
- Each CSV file averages 1.3 GB in size.
- Each netCDF file averages 1.1 GB in size.
