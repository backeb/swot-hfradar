# swot-hfradar

Code to reproduce the results in:

> *Evaluation of SWOT-Derived Surface Geostrophic Currents Using HF Radar Observations in the Southern Agulhas Current* (in preparation for the GRL special collection [Science from the Surface Water and Ocean Topography Satellite Mission](https://agupubs.onlinelibrary.wiley.com/hub/journal/19448007/homepage/call-for-papers/si-2023-001028)).

The study compares SWOT L3 KaRIn geostrophic currents with HF radar surface currents (HYDDOA v2) off the South African south coast (20-28°E, 33-38°S), using Copernicus currents and ERA5 winds to examine sources of disagreement.

## Repository contents

```text
notebooks/00_paper_results.ipynb   All results, figures and tables in the paper (start here)
notebooks/exploratory/             Earlier exploratory notebooks; not needed to reproduce the paper
environment.yml                    Minimal conda environment
environment.lock.yml               Exact package versions used to produce the results
data/raw/, data/processed/         Created locally by the notebook; not tracked by Git
figures/                           Local output; not tracked by Git
```

## Reproducing the results

1. Create the environment.
   ```bash
   mamba env create -f environment.yml      # or environment.lock.yml for the exact versions used
   mamba activate swot-hfradar
   ```
2. Obtain credentials for AVISO+ (SWOT L3) and the Copernicus Climate Data Store (ERA5, `~/.cdsapirc`). Credentials are stored outside the repository.
3. Place the HF radar and Copernicus Marine files as described under *Data sources*.
4. Run `notebooks/00_paper_results.ipynb` from top to bottom. Paths are resolved relative to the repository root. The SWOT download cell (`DOWNLOAD = True`) retrieves about 5 GB; set it to `False` if the files are already in `data/raw/swot_l3_expert/`. Intermediate Zarr archives (about 1.6 GB) are written to `data/processed/`.

The lock file was exported from the environment used for the analysis (Python 3.11, xarray 2026.7.0, zarr 3.1.6). `environment.yml` has not been solved from scratch on a clean machine; use the lock file if it fails.

## Data sources

All input data are publicly available. None are redistributed here.

| Dataset | Used for | Where to obtain it |
|---|---|---|
| SWOT L3 LR SSH Expert, v3.0 (CNES/CLS, AVISO+) | Geostrophic currents (`ugos_filtered`, `vgos_filtered`) | https://doi.org/10.24400/527896/A01-2023.018 (DOI as given in the file metadata); downloaded by the notebook, requires a free AVISO+ account |
| HF radar sea surface velocity, HYDDOA v2 | Reference currents | https://doi.org/10.15493/DEA.MIMS.20250406 (SAEON; catalogue: https://odp.saeon.ac.za/mims/catalog/10.15493/DEA.MIMS.20250406). Unpack into `data/raw/hf_radar/` |
| Copernicus Marine `cmems_obs-mob_glo_phy-cur_nrt_0.25deg_PT1H-i` | Tidal, Ekman and geostrophic correction fields | Copernicus Marine Service (product MULTIOBS_GLO_PHY_NRT_015_003). Subset to 20.12-27.88°E, 37.88-33.12°S, 0-15 m, 2023-07-26 to 2026-09-11 into `data/raw/copernicusmarine/` |
| Copernicus Marine `cmems_mod_glo_wav_anfc_0.083deg_PT3H-i` (variables VSDX, VSDY) | Stokes drift | https://doi.org/10.48670/moi-00017; subset to 20-28°E, 38-33°S, same period, into `data/raw/copernicusmarine/` |
| ERA5 hourly single-level 10 m winds | Wind-conditioned analysis | https://doi.org/10.24381/cds.adbb2d47; downloaded by the notebook via `cdsapi` |
| Natural Earth 1:10m land polygons | Coastline in maps | https://www.naturalearthdata.com/; place `ne_10m_land.zip` in `data/raw/basemaps/` |

## Availability statement (draft for the manuscript)

> The SWOT L3 LR SSH Expert product is available from AVISO+ (CNES/CLS) at https://doi.org/10.24400/527896/A01-2023.018. HF radar surface currents processed with HYDDOA v2 are available from the South African Earth Observation Network (SAEON) at https://doi.org/10.15493/DEA.MIMS.20250406. Copernicus Marine products are available from https://data.marine.copernicus.eu, and ERA5 from the Copernicus Climate Data Store at https://doi.org/10.24381/cds.adbb2d47. The code, environment files and notebook used to produce all results, figures and tables are archived at [Zenodo DOI to be added] and developed at https://github.com/backeb/swot-hfradar. The processed Zarr archives derived from these inputs can be regenerated with the notebook.

## Remaining actions before submission

- Archive a tagged release on Zenodo (GitHub integration) and insert the DOI in `CITATION.cff` and the statement above.
- Cite the code and each dataset in the manuscript reference list (AGU requires this in addition to the availability statement).
- Consider depositing the small matched dataset (`swot_hfradar_zarr_matches.zarr`, under 1 MB) on Zenodo. The larger archives are reproducible from the inputs.
- Confirm the DOI for the Copernicus currents product (MULTIOBS_GLO_PHY_NRT_015_003); it could not be verified automatically.
- Confirm the GitHub URL in `CITATION.cff` and this file.
- Delete this section once complete.

## Licence

Code: MIT (see [LICENSE](LICENSE)). Input datasets remain under their providers' licences.
