# ISCCP HXG Dataset

## Overview

The ISCCP (International Satellite Cloud Climatology Project) HXG dataset is a collection of satellite-based cloud observations and climate data, including data from multiple geostationary and polar-orbiting satellites. The dataset was designed to provide high-resolution information about cloud cover and related atmospheric variables, such as cloud cover, cloud-top temperature, cloud-top pressure, and various radiative fluxes. These variables are organized by geographic location and time. The dataset covers the period from July 1983 to June 2017, and more data from later years is being currently processed to extend its coverage. The dataset can be used to analyze cloud patterns, study climate change, assess the impact of clouds on Earth’s radiation budget, and improve weather and climate models.

The first step in the analysis was to identify groups of pixels as convective systems and link these systems into convective families based on their temporal and spatial properties. We used the TOBAC (Tracking and Object-Based Analysis of Clouds) Python package for this step. The package employs an object-based approach, treating clouds as distinct objects with specified attributes, such as size, shape and motion (see Section 2 for more details). Given that deep convection was the primary focus of this project, brightness temperature threshold of <245 K was established for convective system identification. The processing output provides basic convective system parameters such as geographic and temporal position, as well as relative position within convective families.

The second and final step in the analysis focused on expanding TOBAC output and creating statistical summary for convective systems and convective families. Pixel level values of brightness temperature and optical thickness were retrieved from the HXG data for every convective system. Next, aggregation was performed to determine minimum, maximum, average and standard deviation of these parameters within each convective system and each convective family. 

## TOBAC parameters

Processing input data using TOBAC consists of three main steps:
	1) Feature (convective system) detection
	2) Segmentation
	3) Linking 

The following parameters were used for feature detection:
	target ='minimum'
	threshold = [245,220] 
	n_min_threshold = 2  
	position_threshold = 'weighted_diff'
	sigma_threshold = 1.5
	n_erosion_threshold = 2 
The following parameters were used for segmentation:
	target ='minimum'
	threshold = 245
The following parameters were used for feature linking:
	method_linking = 'predict' 
	v_max = 30 
	adaptive_stop = 2
	adaptive_step = 0.95 
 	stubs = 2                                 
	subnetwork_size = 20 
	time_cell_min = 5*60

More information about the package and its parameters can be found here: 
https://tobac.readthedocs.io
https://github.com/tobac-project/tobac

## File Format and Parameter Description
Each file in the database is a netCDF file that contains one year of data*. Every line (row) of the file contains following variables** for each convective system:
	1. frame - id number of a timeframe a convective system belongs to (Each frame represents a 3 hour period. The variable is required to calculate eccentricity, orientation, central latitude, central longitude, axis major length and axis minor length of a convective system).
	2. feature - a unique id of a convective system.
	3. datetime - date and time at which a convective system existed (YYYY-MM-DD HH:MM:SS).
	4. latitude - latitude of a central pixel within a convective system (-60 to +60 degrees. Pixel’s location depends on the position_threshold parameter used during feature identification).
	5. longitude - longitude of a central pixel within a convective system (0 to 360 degrees east. Pixel’s location depends on the position_threshold parameter used during feature identification).
	6. cell - a unique id number of a cell (convective family) (contains two or more convective systems).
	7. time_cell = time at which a cell existed (# days HH:MM:SS)
	8. year (YYYY)
	9. month (MM)
	10. day (DD)
	11. time (HH:MM:SS)
	12. pixel_count - total number of pixels within a convective system.
	13. pixels_below_220 - number of pixels below 220K.
	14. pixels_below_200 - number of pixels below 200K.
	15. convective_fraction - ratio of pixels_below_220 to pixel_count (%).
	16. minTB_feature - minimum brightness temperature of a convective system (K).
	17. minTB_cell - minimum brightness temperature of a convective family (K).
	18. avgTB_feature - average brightness temperature of a convective system (K).
	19. avgTB_cell - average brightness temperature of a convective family (K).
	20. maxTB_feature - maximum brightness temperature of a convective system (K).
	21. maxTB_cell - maximum brightness temperature of a convective family (K).
	22. 10th - 10th percentile of brightness temperature values of a convective system.
	23. 25th - 25th percentile of brightness temperature values of a convective system.
	24. 50th - 50th percentile of brightness temperature values of a convective system.
	25. 75th - 75th percentile of brightness temperature values of a convective system.
	26. 90th - 90th percentile of brightness temperature values of a convective system.
	27. 95th - 95th percentile of brightness temperature values of a convective system.
	28. 99th - 99th percentile of brightness temperature values of a convective system.
	29. std_dev_tb - standard deviation of brightness temperature values within a convective system (K).
	30. radius - radius of a convective system (km).
	31. max_radius_cell - radius of the largest convective system within convective family (km).
	32. total_hours - number of hours since a convective family first occurred (unique for every convective system within convective family).
	33. lifetime_hours - maximum duration of a convective family.
	34. lifetime_num_cs - duration of a convective family represented as a total number of convective systems it consisted of.
	35. avg_optical_thickness -  average optical thickness of a convective system.
 	36. max_optical_thickness - maximum optical thickness of a convective system.
	37. squared_corr - squared correlation coefficient of latitudes and longitudes within a convective system.
	38. wind_speed (m/s)
	39. wind_dir - wind direction (degrees with respect to north).
	40. wind_dir_letter - cardinal wind direction.
	41. min_lat - minimum possible latitude in a convective system.
	42. max_lat - maximum possible latitude in a convective system.
	43. min_lon - minimum possible longitude in a convective system.
	44. max_lon - maximum possible longitude in a convective system.
	45. land_water_mask - land or water flag determined by the location of a majority of pixels with a convective system.
	46. percent_overlap - percent overlap between this and last convective system within the same convective family.
	47. percent_non_overlap - percent non-overlap between this and last convective system with the same convective family.
	48. cs_gradient - gradient of convective system temperatures (K/1000km).
	49. central_latitude - north-south position of the ellipse’s center on Earth.
	50. central_longitude - east-west position of the ellipse’s center on Earth.
	51. semi_major - half of the longest diameter of the ellipse.
	52. semi_minor - half of the shortest diameter of the ellipse.
	53. inclination - tilt of the ellipse’s plane relative to the Earth’s equatorial plane.
	54. eccentricity - measure of how much the ellipse deviates from a perfect circle.

* Missing values in the database are represented by NaN (Not-a-Number)
** Detection and tracking of convective clusters within convective systems is not supported by TOBAC at the time, therefore several statistical parameters describing these clusters are absent from the database. 

## References
Machado, L.A.T., and W.B. Rossow, 1993: Structural characteristics and radiative properties of tropical cloud clusters. Mon. Wea. Rev, 121, 3234-3260.
Machado, L.A.T., W.B. Rossow, R.L. Guedes, and A.W. Walker, 1998: Life cycle variations of mesoscale convective systems over the Americas. Mon. Wea. Rev., 126, 1630-1654.
Schiffer, R.A., and W.B. Rossow, 1983: The International Satellite Cloud Climatology Project (ISCCP): The first project of the World Climate Research Programme. Bull. Amer. Meteor. Soc., 64, 779-784.

## Contacts for Information
Yuliya Selevich (yuliya.selevich@gmail.com)
Zhengzhao Johnny Luo (z.johnny.luo@gmail.com)
Hanii Takahashi (hanii.takahashi@jpl.nasa.gov)

## Data Size Information

The database comprises annual data stored in both CSV and netCDF formats, 
with each CSV file averaging 1.3 GB in size and each netCDF file averaging 1.1 GB in size.
