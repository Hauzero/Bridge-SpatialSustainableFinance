# Data

Only the inputs the notebooks need are tracked here. Large inputs are downloaded by the notebooks; run outputs go to `outputs/` and are not tracked.

## `mine_buffers_10km.geojson`

10 km buffers (WGS84 polygons) around seven nickel mining sites in eastern Indonesia, derived from the mine locations used in the study (Jasansky et al., 2023). Used by notebook 2 as **exclusion zones**: a counterfactual point may not fall inside another mine's buffer.

Properties per feature: `Name` (site name), `IsMine` (always 1 in this file; the notebook excludes features with `IsMine = 1`).

Sites: Weda Bay Nickel/IWIP, Trimegah Bangun Persada, Gag Nikel, Karya Wijaya/Mineral Trobos, Kawei Sejahtera Mining, Anugerah Surya Pratama, Makmur Jaya Lestari.

## `counterfactual_demo/`

Pre-computed counterfactual points for Weda Bay Nickel/IWIP, produced with the method of notebook 2 (AlphaEarth 2018 embeddings, 10 clusters, 30 reference points, 10 matches each). Loaded by notebook 2 when `USE_DEMO_RESULTS = True`.

| File | Content |
|---|---|
| `Weda_Bay_Nickel_IWIP_LAND_counterfactuals.csv` | 300 land points (30 m search, intact forest in the same ecoregion and watershed) |
| `Weda_Bay_Nickel_IWIP_COASTAL_counterfactuals.csv` | 300 coastal water points (300 m search, within 50 km, excluding mine buffers) |

Columns: `latitude`, `longitude` (WGS84), `similarity` (cosine similarity to the reference point; for coastal points it can exceed 1 because a distance-to-shore band is added to the 64 embedding bands, so compare coastal values only with coastal values), `source_cluster` (k-means cluster of the reference point), `ref_index` (reference point number), `site_lat`, `site_lon` (the mine).

The combined file that notebook 2 writes (`*_counterfactuals_combined.csv`) is a run output and is ignored by git.

## `rrs_monthly/` (not tracked)

Monthly Sentinel-3 OLCI water-leaving reflectance files, `east_indonesia_<year>_<month>_rrs.nc`, downloaded by notebook 1 from the Copernicus Marine Service (about 2 GB for April 2016 to December 2025). See `docs/copernicus_marine.md`.
