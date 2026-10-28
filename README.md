# Bangladesh Flood & Flash-Flood Precursor Monitoring Prototype

**Status:** ongoing exploratory prototype. It is not yet a trained or validated flood-forecasting model and must not be used to issue warnings.

## What is implemented

- Historical daily precipitation and temperature estimates from the Open-Meteo Archive API for selected Bangladesh river-corridor points.
- SRTM-derived elevation lookup through OpenTopoData.
- Feature engineering: 3-day and 7-day rolling rainfall sums, an exponentially weighted antecedent precipitation index, and cyclical month encodings.
- Retry/backoff, local caching, partial-failure handling, CSV outputs, and plots.
- Hourly forecast precipitation retrieval, including next 6-hour, 24-hour and 72-hour rainfall summaries and the maximum forecast precipitation probability over the next 24 hours.
- Optional daily modelled river-discharge forecast retrieval from Open-Meteo's Global Flood API (GloFAS), including ensemble p25/p75 where the API returns them.
- Three approximate regional weather-forecast context points around Meghalaya and Tripura to support future investigation of northeastern Bangladesh flash-flood precursors. These are not rain gauges, are not areal rainfall estimates, and are not yet linked to verified catchments.

## What it does NOT do

- It does not train an ML model, generate supervised flood labels, or calculate calibrated flood probabilities.
- The relative index is hand-designed and unvalidated; it is not a river-level warning threshold.
- The weather forecast and GloFAS outputs are forecast/model products, not ground-truth observations. GloFAS grid-cell selection can choose an unintended nearby river, so check the returned model-grid coordinates and verify the river.
- The current point locations are representative exploratory locations, not all official gauge stations.
- It does not currently ingest satellite images.
- Climate change is the motivation for exploring extreme/rapid-onset flood events; the code does not attribute an event to climate change or make future climate projections.

## Data sources

- Open-Meteo Historical Weather API: https://open-meteo.com/en/docs/historical-weather-api
- Open-Meteo Forecast API: https://open-meteo.com/en/docs
- Open-Meteo Global Flood API (GloFAS modelled discharge): https://open-meteo.com/en/docs/flood-api
- OpenTopoData API and SRTM: https://www.opentopodata.org/api/
- Bangladesh FFWC API landing page: https://api.ffwc.gov.bd/?format=json
- Bangladesh FFWC observed water levels: https://ffwc.gov.bd/app/observed-water-level
- NASA GPM IMERG precipitation: https://gpm.nasa.gov/data/imerg
- ESA Sentinel-1 flood extent capability and revisit considerations: https://knowledge-hub-gda.esa.int/eo_capability/flood-extent/

## Next research steps

1. Verify access and coverage of FFWC/BWDB observed station water levels and official danger levels.
2. Choose one target: short-lead flood forecasting (e.g., future danger-level exceedance at a named gauge) or flood-extent segmentation/mapping from satellite imagery.
3. For flash floods, focus on catchment-aligned upstream rainfall/soil moisture and observed/forecast river conditions in a selected northeastern basin; delineate basins rather than treating nearby points as catchment averages.
4. Construct labels from observations, use chronological and event/location holdouts, and compare simple baselines before applying ML.
5. Evaluate PR-AUC, precision, recall, false-alarm rate and lead time—not accuracy alone.
6. Use GPM IMERG as a rapid precipitation-estimation/nowcasting input where appropriate. Use Sentinel-1 primarily for observed inundation mapping, event analysis, and reference masks because revisit intervals can limit its usefulness as the sole signal for a fast flash-flood forecast.
7. Make climate-change attribution or projection a separate, explicitly designed analysis.
