# Copernicus Marine: account and download setup

Notebook 1 (`01_s3_water_quality_indicators.ipynb`) reads monthly Sentinel-3 OLCI water-leaving reflectance from the Copernicus Marine Service (CMEMS). The files are not in the repository (about 2 GB); the notebook downloads them with the `copernicusmarine` toolbox. The download needs a free account and a one-time login. Nothing else in the package needs it.

## 1. Create a free account

1. Open <https://data.marine.copernicus.eu/register>.
2. Fill in the form (name, email, organisation, intended use) and submit.
3. Confirm the email you receive. The account is free; no payment details are asked for.

Your username is the one shown in the confirmation email (it is usually not the email address itself).

## 2. Install the toolbox

The toolbox is part of the project environment:

```bash
conda env create -f environment.yml
conda activate bridge-ssf
```

If you use your own environment instead:

```bash
pip install copernicusmarine
```

## 3. Log in once

In a terminal with the environment active:

```bash
copernicusmarine login
```

Enter your username and password when asked. The toolbox stores them in a file named `.copernicusmarine-credentials` in the `.copernicusmarine` folder of your home directory, so you do not repeat this step on the same machine.

Check that the stored credentials work:

```bash
copernicusmarine login --check-credentials-valid
```

### Alternatives

- **From Python** (for example in a notebook cell), which prompts in the same way:

  ```python
  import copernicusmarine
  copernicusmarine.login()
  ```

- **Environment variables**, useful on Google Colab or any machine where you do not want a credentials file. Set `COPERNICUSMARINE_SERVICE_USERNAME` and `COPERNICUSMARINE_SERVICE_PASSWORD` before the download cell runs. On Colab, Secrets (the key icon in the left bar) are the safest place to keep them:

  ```python
  import os
  from google.colab import userdata
  os.environ["COPERNICUSMARINE_SERVICE_USERNAME"] = userdata.get("CMEMS_USER")
  os.environ["COPERNICUSMARINE_SERVICE_PASSWORD"] = userdata.get("CMEMS_PASSWORD")
  ```

Never write the password into the notebook itself.

## 4. What the notebook downloads

| Setting | Value |
|---|---|
| Product | `cmems_obs-oc_glo_bgc-reflectance_my_l4-olci-300m_P1M` (OLCI L4, monthly, 1/180° grid) |
| Bands | RRS490, RRS510, RRS560, RRS620, RRS665, RRS681, RRS709 |
| Period | April 2016 to December 2025 (117 monthly files) |
| Area | bounding box of the 50 km search circle around the site (set in the notebook) |
| Target folder | `data/rrs_monthly/` (ignored by git) |

When `data/rrs_monthly/` contains no files, the notebook sets `DOWNLOAD = True` by itself and fetches every month; months already on disk are skipped. An interrupted download can be resumed by running the cell again.

## 5. Shortcuts

- **Reuse existing files.** If you received the monthly files from a colleague, copy them into `data/rrs_monthly/`. The notebook then skips the download, and no account is needed.
- **Shorter period.** For a live session, set `START` and `END` in step 2 of the notebook to a shorter window. The annual composites and figures then cover only those years.
- **Other sites.** Change `MINE_LAT`, `MINE_LON` in the setup cell; the bounding box and the download follow the new site. Files are named `east_indonesia_<year>_<month>_rrs.nc` regardless of the site, so keep one `data/rrs_monthly/` folder per site.

## 6. Troubleshooting

| Symptom | What to do |
|---|---|
| `copernicusmarine login` reports invalid credentials | Log in once on <https://data.marine.copernicus.eu> in the browser to confirm the account is active; check that you use the username, not the email, if the email fails. |
| The notebook prints `failed <month>` for some months | Usually a transient network error. Run the cell again; only the missing months are fetched. |
| `ModuleNotFoundError: copernicusmarine` | The environment is not active, or the package is not installed: `pip install copernicusmarine`. |
| Download is slow | The full set is about 2 GB. Shorten `START` / `END`, or run the download before the session and share the folder. |
| Credentials file is not found on a shared machine | Use the environment variables above, or pass `--credentials-file <path>` to the CLI. |

Toolbox documentation: <https://toolbox-docs.marine.copernicus.eu>. Help centre: <https://help.marine.copernicus.eu>.
