# LUNAR NAVIGATOR — Version 4.0

**Custom lunar mission engineering workbench for terrain inspection, landing-site screening, and descent-dynamics research.**

> Research prototype only. This is not a NASA product or endorsed system, an operational landing-site certification tool, or a flight-control system. It is not qualified for mission-critical decisions.

## Project

Version 4 includes a custom-vehicle and custom-site workflow, a research-grade six-degree-of-freedom (6-DOF) rigid-body propagation step, and an optional NAIF SPICE adapter. The earlier 2-D descent dashboard remains available as a legacy demonstration.

## Run the application

The application has a React/Vite frontend and a Python/FastAPI backend. **The GitHub repository page is the source-code page, not a hosted running application.** Both services must be started for the full application.

### Windows — Backend

Open a terminal:

```bat
cd backend
py -m venv .venv
.venv\Scripts\activate.bat
python -m pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000
```

Backend endpoints:
- API: http://127.0.0.1:8000
- Health: http://127.0.0.1:8000/health
- OpenAPI docs: http://127.0.0.1:8000/docs

### Windows — Frontend

Open a second terminal:

```bat
cd frontend
npm install
npm run dev
```

Open **http://localhost:5173** in your browser. Select **V4 WORKBENCH** in the top bar.

### Production frontend build

```bat
cd frontend
npm install
npm run build
```

The frontend build alone does not host the Python API. For a public live demo, the frontend and backend must both be deployed and configured with the backend API URL. A GitHub Pages link by itself cannot run the FastAPI backend.

## Version 4 capabilities

- Custom vehicle parameter entry and readiness screening.
- Custom lunar site metadata and coordinate-frame requirements.
- 6-DOF rigid-body dynamics step with quaternion attitude representation.
- Optional NAIF SPICE frame-transform adapter.
- Legacy 2-D descent dashboard and synchronized browser-side physics model.
- NASA LOLA Product 90 terrain data integration on a best-effort basis.
- Verification endpoints and engineering documentation.

## V4 API endpoints

- `GET /api/v4/system/status`
- `POST /api/v4/mission/readiness`
- `POST /api/v4/dynamics/6dof/step`
- `GET /api/v4/frames/spice/status`
- `POST /api/v4/frames/spice/transform`

Other API endpoints include:
- `GET /api/terrain/nasa/catalog`
- `POST /api/terrain/nasa/sample`
- `POST /api/terrain/analyze`
- `POST /api/landing/evaluate`
- `POST /api/simulation/step`
- `POST /api/vehicle/analyze`
- `POST /api/guidance/run`
- `POST /api/guidance/validation`
- `GET /api/verification/baseline`
- `GET /api/system/assurance`
- `POST /api/reports/export?format=json|csv`

## NASA LOLA data

Primary data reference: [NASA GSFC Planetary Geosciences Data Archive — Product 90](https://pgda.gsfc.nasa.gov/products/90).

The project references the south-polar LOLA DEM and related slope, elevation-error, effective-resolution, and return-count products. NASA data access depends on network availability and Rasterio/GDAL `/vsicurl/` support. If NASA data cannot be loaded, the dashboard can use procedural terrain and labels it as simulated.

## Verification

Run backend tests:

```bat
cd backend
python -m pytest -q
```

The bundled verification documentation describes the tested environment and known limitations. Passing software tests is not independent validation, mission qualification, or flight certification.

## Documentation

See:
- `docs/v4/V4_ARCHITECTURE.md`
- `docs/v4/PARAMETER_REGISTER.md`
- `docs/v4/CUSTOM_SITE_DATA_CONTRACT.md`
- `docs/v4/VERIFICATION_PLAN.md`
- `docs/v4/RELEASE_VERIFICATION.md`
- `docs/MODEL_CARD.md`
- `docs/HAZARD_LOG.md`
- `docs/DATA_PROVENANCE.md`
- `docs/VERIFICATION_RESULTS.md`

## License and notice

See [LICENSE](LICENSE) and [NOTICE.md](NOTICE.md). NASA is not affiliated with or endorsing this application.
