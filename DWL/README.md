## Requirements

This section of the project uses both **Python** and **MATLAB**:

- Python is required to download HRRR model data using the Herbie library.
- MATLAB is used for processing the HRRR data and performing operations on the data.
---

## Python Setup

This project uses **Miniconda** and was developed with **Python 3.12.9**. You’ll need either [Miniconda or Anaconda](https://docs.conda.io/en/latest/miniconda.html#latest-miniconda-installer-links) installed to recreate the environment.

To set up the Python environment:

## MATLAB Setup

Tested with **MATLAB R2024a**  
No additional toolboxes are required.

### Step 1: Download the environment file

Download `DWL_env_full.yml` from this repository.

### Step 2: Create and activate the environment

```bash
conda env create -f DWL_env_full.yml
conda activate DWL_env
```

### Step 3. Download the data

The DWL data is available here: https://doi.org/10.26023/R75F-FGJ8-VG12

Download the data. There will be 24 hourly files per day. naming convention: `Stare_122_YYYYMMDD_hh_.hpl`
These files will be in folders labeled `pwd/YYYYMM/YYYYMMDD`. put these folders in the `DWL_Data` folder

The surface weather station data is available here: https://doi.org/10.26023/30XE-MB6C-SC14

Download the data. There will be one netCDF file per day. Naming convention: `iss2_m2hats_sfcmet_3mtower_YYYYMMDD.nc`
put these files into the `ISS_Data` folder

### Step 4. Run the python DWL decoding script

make sure that `halo_dl_decode.py` is in the same directory as the YYYYMM folders run the code.

***Note*** This code was throwing errors when ran from spyder, but had no issues when ran directly from anaconda prompt

This code will convert the .hpl files into netCDF files in the same folders named: `fp_YYYYMMDD_hhmmss.nc`

### Step 5. Run the MATLAB DWL processing code

The script `process_halo_netCDF.m` code will convert the 24 netCDF files into a .mat file containing the wind and backscatter data integrated to 30 meters and 10 s in local time

***Note*** The function `concatenate_halo_data.m` contains a time correction variable unique for PDT (UTC-7)

### Step 6. Run the MATLAB surface weather station processing code

The script `process_surface_meteorology_data.m` will extract relevant variables from the ISS netCDF files and save them as a .mat file in local time

***Note*** The function `concatenate_halo_data.m` contains a time correction variable unique for PDT (UTC-7)

### Step 7. Run the MATLAB ISFS processing code

The script `process_surface_meteorology_data.m` will extract relevant variables from the ISFS netCDF files and save them as a .mat file in local time

***Note*** The function `concatenate_halo_data.m` contains a time correction variable unique for PDT (UTC-7)

***Note*** The function `compute_average_flux.m` contains the gravitational constant for Tonopah, Nevada

### Step 8. Run the MATLAB mask code

The script `create_masks_halo.m` generates masks for clouds, precipitation, and missing data, and identifies cloud locations. However, the code was written as a quick fix and may not work well for other datasets.

***Note*** keep the `betaAvg.mat` file in the same folder as the main script.

### Step 9. Run the MATLAB temporal variance extraction code

The script `compute_temporal_variance.m` performs an autocovariance extrapolation to extract the real wind variance from noisy lidar data. 

***Note*** the function `compute_autocovariance_extrapolation.m` contains a line that sets the time window to 180 time steps, which correponds to 30 minutes for the processed DWL data.

***Caution*** This script is prone to causing MATLAB to crash, even on high-performance systems.

### Step 10. Run the MATLAB MLH code

The script `find_mlh_dwl.m` will diagnose the mlh from the vertical velocity variance data.

***Note*** SunriseSunsetTable.csv is a table of the sunrise and sunset times for Tonopah, Nevada during the M2HATS experiment. Keep it in the same directory.

---
