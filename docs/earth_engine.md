# Google Earth Engine: account and project setup

Notebook 2 (`02_counterfactual_selection_embeddings.ipynb`) runs on Google Earth Engine (GEE): the AlphaEarth embeddings, the clustering, the maps and the similarity search are all computed there. Earth Engine access is needed even in demo mode, which only skips the export step. The related repository [`dhi-evi-hls`](https://github.com/Hauzero/dhi-evi-hls) (Dynamic Habitat Indices) needs the same setup.

## 1. Register for Earth Engine

1. Sign in with a Google account at <https://code.earthengine.google.com/register>.
2. Choose the non-commercial or research option if it applies to you (academic use is free). Commercial use needs a paid plan.
3. Create a new Google Cloud project or pick an existing one, and register it for Earth Engine when asked. Note the **project ID** (for example `my-eo-project-123`); the notebook needs it, not the display name.

If your organisation already has a registered project, ask the owner to add you to it instead.

## 2. Install the API

The API is part of the project environment (`environment.yml`). Otherwise:

```bash
pip install earthengine-api
```

## 3. First run

In the parameter cell of the notebook, set:

```python
GEE_PROJECT = "your-project-id"
```

The setup cell tries `ee.Initialize(project=GEE_PROJECT)` and, if that fails, calls `ee.Authenticate()`. Authentication opens a browser page where you sign in with the Google account and paste a code back. This is needed once per machine; afterwards `ee.Initialize` works directly.

On Google Colab the same flow applies; the browser step happens in the notebook output.

## 4. Demo mode and full runs

| `USE_DEMO_RESULTS` | What happens |
|---|---|
| `True` (default) | Geometry, clusters, reference points and maps are computed on Earth Engine. No export task is submitted. Section D loads the pre-computed Weda Bay results from `data/counterfactual_demo/`. |
| `False` | Two export tasks (land and coastal) are submitted. Earth Engine writes the CSVs to the Google Drive folder `EE_Counterfactuals` (usually 5 to 30 minutes). Section C waits for them; section D reads them from Drive (Colab) or from a local folder you set in `RESULTS_DIR`. |

Running tasks can be watched and cancelled at <https://code.earthengine.google.com/tasks>.

## 5. Troubleshooting

| Symptom | What to do |
|---|---|
| `ee.ee_exception.EEException: ... not registered` or a permission error on `Initialize` | The project ID is wrong or not registered for Earth Engine. Check it in the Cloud console and on the registration page. |
| `Authenticate` keeps being asked | The stored token was created for another project or account. Run `ee.Authenticate()` again and sign in with the account that has access. |
| Maps do not render in the notebook | `folium` is missing (`pip install folium`), or the notebook is viewed in a static viewer. In Jupyter Lab, trust the notebook (File > Trust Notebook). |
| Export never finishes | Check the task on the tasks page. Large search areas can take longer; the notebook can be interrupted without stopping the task. |
| Quota or "too many concurrent aggregations" | Wait and re-run the cell; free accounts have modest limits on parallel requests. |

Documentation: <https://developers.google.com/earth-engine>. The AlphaEarth dataset: <https://developers.google.com/earth-engine/datasets/catalog/GOOGLE_SATELLITE_EMBEDDING_V1_ANNUAL>.
