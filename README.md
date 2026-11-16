# BRIDGE Spatial Sustainable Finance

How to integrate Satellite Earth Observation for local Asset-level Corporate Biodiversity/Water Risk and Impact assessment, read our paper (Hauser et al., 2025):  https://doi.org/10.1029/2024EF005474

Satellite-based workflows for assessing and attributing water and biodiversity risks of individual assets of physical company operations. The case study is the Weda Bay nickel mining and industrial complex in North Maluku, Indonesia.

| Part | What it does | Where | Reference |
|---|----|---|---|
| **Water-risk indicators** | S-3 coastal water-quality indicators | `notebooks/01_s3_water_quality_indicators.ipynb` | Hauser, Santos & Damm (2026), World Biodiversity Forum, Davos, https://doi.org/10.5194/wbf2026-622 |
| **Counterfactual selection** | Find control sites (land/coast) that resembled the asset area before intervention | `notebooks/02_counterfactual_selection_embeddings.ipynb` | Build using Brown et al. (2025), AlphaEarth Foundations, https://arxiv.org/abs/2507.22291 |
| **Dynamic Habitat Indices** | Biodiversity-relevant productivity indices (cumulative, minimum, seasonality) from HLS30 | `dhi/` (mirrored from [Hauzero/dhi-evi-hls](https://github.com/Hauzero/dhi-evi-hls)) | Adopted from Coops et al. (2018), Scientific Reports, 8, 16261 |

These are the materials presented at the ZHAW-GEO workshop (09 November 2026). The slides are included: [`docs/ZHAW-GEO_workshop_2026-11-09_slides.pdf`](docs/ZHAW-GEO_workshop_2026-11-09_slides.pdf).

## Quick start

**Local (recommended):**

```bash
git clone https://github.com/Hauzero/Bridge-SpatialSustainableFinance.git
cd Bridge-SpatialSustainableFinance
conda env create -f environment.yml
conda activate bridge-ssf
jupyter lab
```

A `requirements.txt` is provided for pip-based environments.

**Google Colab:** open a notebook with the badge below. The first cell clones the repository so that `data/` is available.

| Notebook | Open | Accounts needed |
|---|---|---|
| 01 Water-quality indicators | [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Hauzero/Bridge-SpatialSustainableFinance/blob/main/notebooks/01_s3_water_quality_indicators.ipynb) | free Copernicus Marine account, for the one-time data download |
| 02 Counterfactual selection | [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Hauzero/Bridge-SpatialSustainableFinance/blob/main/notebooks/02_counterfactual_selection_embeddings.ipynb) | Google Earth Engine account and Cloud project (also in demo mode) |

## Accounts and first run

- **Copernicus Marine** (notebook 1): register, install the toolbox, run `copernicusmarine login` once. On the first run the notebook downloads the monthly Sentinel-3 files (about 2 GB) into `data/rrs_monthly/`; later runs reuse them. Full instructions: [`docs/copernicus_marine.md`](docs/copernicus_marine.md).
- **Google Earth Engine** (notebook 2): register a Cloud project for Earth Engine and set `GEE_PROJECT` in the parameter cell. `USE_DEMO_RESULTS = True` (default) submits no export tasks and loads the pre-computed Weda Bay results. Full instructions: [`docs/earth_engine.md`](docs/earth_engine.md).

Both notebooks run top to bottom without edits for Weda Bay. To analyse another site, change the site coordinates in the parameter cell of each notebook; the last section of notebook 2 lists what else to check.

## Repository layout

```
notebooks/   the two workflows, with explanations in the markdown cells
dhi/         Dynamic Habitat Indices code, mirrored from the dhi-evi-hls repository (git subtree)
data/        inputs that the notebooks need for demo mode (see data/README.md)
docs/        workshop slides (PDF) and account setup guides for Copernicus Marine and Earth Engine
outputs/     created at run time (annual indicator composites, time series); not tracked
```

## Data

Included: the 10 km buffers of the seven nickel mining sites (exclusion zones for the counterfactual search) and the pre-computed Weda Bay counterfactual points for demo mode. See [`data/README.md`](data/README.md) for file descriptions and provenance.

Not included: the monthly Sentinel-3 reflectance files (notebook 1 downloads them for the area of interest) and any run outputs.

## Workshop slides

The deck ([`docs/ZHAW-GEO_workshop_2026-11-09_slides.pdf`](docs/ZHAW-GEO_workshop_2026-11-09_slides.pdf), 34 slides) gives the context that the notebooks assume:

| Slides | Content | Material in this repository |
|---|---|---|
| 1-5 | Why asset-level Earth observation: the Seven Challenges (Hauser et al., 2025) and an overview of remote-sensing products for water, cryosphere, land, vegetation and atmosphere, marked as core, adjacent or out of scope | background |
| 6-9 | Case study: coastal nickel mining in East Indonesia (Weda Bay), the EO workflow (10 km footprint, six water indicators, annual composites 2017-2025, attribution, translation to risk) and the water-risk indicator framework | notebook 1 |
| 10-13 | Detection and attribution: before-after control-intervention design, counterfactuals, and the similarity search with AlphaEarth embeddings | notebook 2 |
| 14-20 | Results for Weda Bay: Kd490 before/after maps, counterfactual points, time series, 3-D indicator space, covariate balance of counterfactuals | produced by the research pipeline behind notebooks 1 and 2 |
| 21-22 | The two notebooks | `notebooks/` |
| 23-34 | Dynamic Habitat Indices: from EVI to DHIs, evidence from the literature, HLS implementation, expected mining impacts, example output | `dhi/` |

## Data sources

- Copernicus Marine Service, OLCI L4 reflectance: `cmems_obs-oc_glo_bgc-reflectance_my_l4-olci-300m_P1M`
- AlphaEarth Foundations satellite embeddings (Google DeepMind; Earth Engine `GOOGLE/SATELLITE_EMBEDDING/V1/ANNUAL`)
- Hansen et al., Global Forest Change; RESOLVE Ecoregions 2017; HydroSHEDS basins; sat-io global shorelines (all via Earth Engine)
- Mine locations: Jasansky et al. (2023), see the workshop slides for the full reference

Each source has its own terms of use; check them before redistributing derived data.

## Support

- Questions and problems: open an issue in this repository. From 1 December 2026 the repository is no longer actively maintained.
- Contact: Dr. Leon Hauser, Department of Geography, University of Zurich (UZH).

## Disclaimer
Part of the notebook compilation, git package, and code are compiled with the help of LLMs with manual edit and supervision

## Citation and licence

Code is released under the MIT licence (see `LICENSE`). If you use the code or derived data, please cite it as given in `CITATION.cff`. For the underlying concepts, please cite Hauser et al. (2025): https://doi.org/10.1029/2024EF005474
