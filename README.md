# Under-Ice Argo Float Position Reconstruction

Reconstruction of missing Arctic Argo float trajectories during periods when GNSS positioning is unavailable, particularly under sea ice.

The project uses ocean current data and probabilistic data assimilation to estimate the float trajectory and its associated positional uncertainty.

## Methods

Two smoothing methods are implemented:

* **Ensemble Kalman Smoother (EnKS)**
* **Forward Filtering Backward Smoothing (FFBS)** based on a particle filter

The float position is propagated using ocean current velocities from the **GLORYS12** ocean reanalysis. The reconstruction can also use:

* GNSS positions
* bathymetry
* parking depth
* grounding information
* potential vorticity and speed constraints

The trajectory is integrated using RK45 and represented in the Arctic Polar Stereographic projection (EPSG:3995).

## Data

The code requires:

* Argo float NetCDF files (`prof`, `Rtraj` and `meta`)
* a bathymetry NetCDF file
* GLORYS12 ocean current data (`uo` and `vo`)

A typical data organisation is:

```text
0_data/
├── bathy_isas17.nc
└── Argo/
    └── <WMO>/
        ├── <WMO>_prof.nc
        ├── <WMO>_Rtraj.nc
        └── <WMO>_meta.nc
```

Large datasets are not included in this repository.

## Requirements

The code is written in **Python** and uses the following main packages:

```text
numpy
pandas
xarray
matplotlib
scipy
pyproj
cartopy
```

## Configuration

The main parameters are defined at the beginning of the notebook:

```python
float_name = "3902118"

use_enks = True
use_ffbs = True

Ne_enkf = 1000
Ne_ffbs = 5000
```

The paths to the Argo, bathymetry and GLORYS12 datasets must also be adapted to the local environment.

Potential vorticity and speed constraints can be activated with:

```python
use_pv = True
use_speed = True
```

## Running the code

1. **Download or clone this repository.**

2. **Make sure all required datasets are available**, including the Argo float data, bathymetry and GLORYS12 ocean current data.

3. **Modify the configuration** at the beginning of `under_ice_positionning_EnKS_FFBS.ipynb`:

   * select the float;
   * set the paths to the required datasets;
   * select the reconstruction method(s);
   * adjust the main parameters if necessary.

4. **Run the notebook** from beginning to end.

The notebook loads the float and ocean data, performs the reconstruction, estimates the positional uncertainty and generates the corresponding figures and CSV files.


## Outputs

The main outputs are:

* reconstructed trajectories using EnKS and/or FFBS;
* comparison maps with the recorded trajectory;
* 95% position uncertainty ellipses;
* evolution of the positional uncertainty;
* diagnostic plots;
* reconstructed trajectory and uncertainty data in CSV format.


## Scientific context

This project was developed in the context of Arctic oceanography and Argo float trajectory reconstruction. It aims to recover missing positioning information caused by periods spent beneath sea ice while providing an estimate of the associated uncertainty.

This project was developed as part of a Master's 2 project in Oceanographic Data Science, under the supervision of Nicolas Kolodziejczyk.

This repository is provided as a record of the project. The code is no longer actively maintained and has not been updated since the end of the project.

**Author:** Rozenn Moal
