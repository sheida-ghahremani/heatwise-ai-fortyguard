# HeatWise AI

HeatWise AI is a personalized thermal-risk-aware route planner for the Texas
A&M and RELLIS campuses. It compares Fastest, Lowest Heat Risk, and Balanced
routes and explains the physical causes of exposure through a LiDAR-based 3D
urban heat view.

**Core temperature source: FortyGuard Temperature API®**

## Scientific outputs

- **UTCI category:** standardized environmental heat stress based on air
  temperature, mean radiant temperature, humidity, and wind.
- **Personal Heat Risk (PET):** activity-, age-, and clothing-sensitive
  physiological heat stress using PET with user-selected clothing insulation,
  metabolic rate, age, MRT, humidity, and wind.
- **Exposure load:** stress above the selected threshold integrated over route
  duration, reported in degree-minutes.
- **Dynamic shade:** LiDAR canopy/building heights projected using the selected
  hour's solar elevation and azimuth.
- **Pedestrian wind:** 10 m weather wind adjusted to 2 m with a local
  logarithmic roughness profile derived from canopy and building height.

These are route-comparison and heat-screening outputs, not clinical diagnoses
or medical advice.

## Data sources

| Component | Source |
|---|---|
| Hyperlocal temperature | FortyGuard Temperature API® |
| Humidity, wind, radiation, cloud, precipitation | Open-Meteo hourly API |
| Walk/bike networks and building footprints | OpenStreetMap |
| Canopy and building height | USGS 3DEP LiDAR-derived rasters |
| Geocoding and basemap | Mapbox |

The interface reports the requested and fetched timestamps. An empty current
FortyGuard response is never silently described as a current observation; the
latest available layer is explicitly labeled as cached.

## Run locally

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
streamlit run app.py
```

Add private credentials only to `.env`. It is excluded from Git.

```dotenv
FORTYGUARD_API_KEY=...
FORTYGUARD_BASE_URL=https://api.fortyguard.com
MAPBOX_ACCESS_TOKEN=...
GROQ_API_KEY=... # optional conversational assistant
```

## Tests

```bash
python -m pytest -q
```

## Repository layout

- `app.py` — Streamlit application
- `heatwise/` — temperature integration, routing, shade, SVF, wind, UTCI/PET,
  geocoding, maps, 3D view, and assistant
- `data/osm/` — processed campus routing networks and footprints
- `data/lidar/` — compact final canopy/building height rasters
- `data/boundaries/` — study-area boundaries
- `fortyguard/` — FortyGuard async API client
- `tests/` — scientific and integration checks
- `DEPLOYMENT.md` — public deployment and secret-management instructions

## Public deployment

See [DEPLOYMENT.md](DEPLOYMENT.md). The public deployment must store API keys
in the host's encrypted secrets panel and must never commit `.env` or
`.streamlit/secrets.toml`.

## Attribution

Developed by the Urban Climate and Health (UCH) Research Group, Department of
Geography, Texas A&M University, for FortyGuard Hackathon '26.
