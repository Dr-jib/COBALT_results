# MultiCon data bundle

## Purpose

This data bundle contains the selected final solver outputs, final derived
microphysics/optics, solver-consumed ERA5 forcing and winds, six fixed-scale
flight animations, and one representative waypoint summary for **CYYZ -> CYYC**.

Scenario ID from COBALT: `ccfa1d55-088b-451e-b758-75d6f7064146`  
Solver horizon: 10 h per initialized waypoint plume  
Default animation sampling: 5 simulated min/frame at 5 display frames/s
Animation endpoint: final initialized plume's modeled 10-hour endpoint; no inactive tail

Note: Row is the nomenclature used in the solver for a waypoint.

## Contents

- `data/ccfa1d55-088b-451e-b758-75d6f7064146_multicon_science.nc` — CF-1.11 NetCDF scientific dataset (154 waypoints).
- `animations/` — IWC, number concentration, optical depth, signed settling
  velocity, effective radius, and shape-index GIFs.
- `snapshot/row_0039_scientific_summary.png` — row 39 summary.

## NetCDF field groups

Native solver fields: `F_out_t`, `M_out_t`, `S_i_out_t`, `Phi_out_t`, `settling_t`, `Tau_contrail`, `Rf_net`, `Rf_lw`, `Rf_sw`.

Final derived science: `ice_water_content_raw`, `r_effective`, `optical_number_t`, `beta_ext_t`, `mass_weighted_relative_altitude_t`, `mass_weighted_altitude_t`.

ERA5 forcing: `T_vec`, `si_mat`, the advected forcing coordinates, forcing
altitudes/pressures, and `forcing_eastward_wind` / `forcing_northward_wind`.
The winds are the exact interval winds recovered by inverting the solver's saved
WGS84 path update. The last forcing sample is intentionally NaN because it has no
following position.

### Optical-depth animation legends

The optical GIF intentionally uses two separately labelled physical fields:

- Horizontal map: `Column optical depth tau [-]` (`Tau_contrail`).
- Vertical projection: `Extinction coefficient beta_ext [m^-1]` (`beta_ext_t`).

Their units and numerical ranges differ, so they must not share one color
scale. Integrating the vertical field over layer thickness reconstructs the
saved column optical depth.

## NetCDF variable nomenclature

| Variable | Units | Dimensions | Brief explanation |
|---|---|---|---|
| `row_index` | `—` | `row` | Original waypoint row number in the processed flight input. |
| `row_time` | `seconds since 1970-01-01 00:00:00 UTC` | `row` | UTC time at which the waypoint plume is initialized. |
| `row_longitude` | `degrees_east` | `row` | Waypoint longitude at plume initialization. |
| `row_latitude` | `degrees_north` | `row` | Waypoint latitude at plume initialization. |
| `row_altitude` | `m` | `row` | Aircraft altitude at plume initialization. |
| `z` | `m` | `z` | Vertical displacement relative to the initialization altitude. |
| `z_bounds` | `m` | `z, bounds` | Lower and upper bounds of each relative-altitude layer. |
| `dz_layer` | `m` | `z` | Thickness of each vertical model layer. |
| `altitude` | `m` | `row, z` | Absolute altitude of each row-specific vertical model level. |
| `altitude_bounds` | `m` | `row, z, bounds` | Lower and upper absolute-altitude bounds of each layer. |
| `time` | `s` | `time` | Elapsed plume age since initialization. |
| `valid_time` | `seconds since 1970-01-01 00:00:00 UTC` | `row, time` | Absolute UTC time for each row and plume-age sample. |
| `forcing_level` | `—` | `forcing_level` | Index of the vertical meteorological forcing level. |
| `forcing_time` | `s` | `forcing_time` | Elapsed time of each meteorological forcing sample. |
| `forcing_valid_time` | `seconds since 1970-01-01 00:00:00 UTC` | `row, forcing_time` | Absolute UTC time of each meteorological forcing sample. |
| `F_out_t` | `m-1` | `row, z, time` | Normalized vertical particle-number profile evolved by the solver; not a volumetric concentration. |
| `M_out_t` | `kg m-1` | `row, z, time` | Particle-mass-weighted vertical profile evolved by the solver; not volumetric IWC. |
| `S_i_out_t` | `1` | `row, z, time` | Ice supersaturation profile used and evolved by the plume solver. |
| `Phi_out_t` | `1` | `row, z, time` | Ice-crystal shape index: values below 1 indicate plates and values above 1 indicate columns. |
| `settling_t` | `m s-1` | `row, z, time` | Signed vertical ice-crystal settling velocity; negative is downward. |
| `Tau_contrail` | `1` | `row, time` | Total column optical depth of the contrail. |
| `Rf_net` | `W m-2` | `row, time` | Net radiative-forcing diagnostic proxy (longwave plus shortwave). |
| `Rf_lw` | `W m-2` | `row, time` | Longwave radiative-forcing diagnostic proxy. |
| `Rf_sw` | `W m-2` | `row, time` | Shortwave radiative-forcing diagnostic proxy. |
| `ice_water_content_raw` | `mg m-3` | `row, z, time` | Same-time volumetric ice mass concentration reconstructed from the conserved mass profile. |
| `r_effective` | `m` | `row, z, time` | Fixed-density volume-equivalent ice-particle radius; not a size-distribution effective radius. |
| `optical_number_t` | `m-3` | `row, z, time` | Same-time volumetric ice-particle number concentration used by the optical calculation. |
| `beta_ext_t` | `m-1` | `row, z, time` | Local contrail extinction coefficient at each altitude and plume age. |
| `mass_weighted_relative_altitude_t` | `m` | `row, time` | Same-time mass-weighted plume-center displacement relative to initialization altitude. |
| `mass_weighted_altitude_t` | `m` | `row, time` | Same-time mass-weighted absolute plume-center altitude. |
| `wingspan_m` | `m` | `row` | Aircraft wingspan used to initialize the plume. |
| `aircraft_mass_kg` | `kg` | `row` | Aircraft mass used to initialize the plume. |
| `T_vec` | `K` | `row, forcing_time` | ERA5 temperature time series consumed by the plume solver. |
| `si_mat` | `1` | `row, forcing_level, forcing_time` | Vertical/time humidity-derived ice-supersaturation forcing consumed by the solver. |
| `forcing_latitude` | `degrees_north` | `row, forcing_time` | Latitude of the wind-advected meteorological sampling path. |
| `forcing_longitude` | `degrees_east` | `row, forcing_time` | Longitude of the wind-advected meteorological sampling path. |
| `forcing_level_altitude` | `m` | `row, forcing_level` | Absolute altitude of each supersaturation forcing level. |
| `forcing_level_pressure` | `hPa` | `row, forcing_level` | Standard-atmosphere pressure corresponding to each supersaturation forcing altitude. |
| `forcing_eastward_wind` | `m s-1` | `row, forcing_time` | ERA5 eastward wind recovered from consecutive saved forcing-path positions. |
| `forcing_northward_wind` | `m s-1` | `row, forcing_time` | ERA5 northward wind recovered from consecutive saved forcing-path positions. |
| `forcing_wind_reconstruction_status` | `1` | `row, forcing_time` | QC code indicating reconstructed winds or an unavailable terminal sample. |

## Open the data

Python/xarray:

```python
import xarray as xr
ds = xr.open_dataset("data/ccfa1d55-088b-451e-b758-75d6f7064146_multicon_science.nc", engine="h5netcdf")
print(ds)
```

MATLAB: `ncdisp('data/ccfa1d55-088b-451e-b758-75d6f7064146_multicon_science.nc')`  
R: `ncdf4::nc_open('data/ccfa1d55-088b-451e-b758-75d6f7064146_multicon_science.nc')`

Use `row_index` for the original flight row, `row_time` for plume birth time,
`time` for plume age, `valid_time` for absolute solver time, and `altitude` for
the row-specific absolute vertical coordinate.

