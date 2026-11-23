# Lunar Navigator V4

**A lunar mission engineering research workbench for terrain inspection, landing-site screening, and descent-dynamics exploration.**

[![Version](https://img.shields.io/badge/version-4.0-blue)](CHANGELOG.md)
[![Frontend](https://img.shields.io/badge/frontend-React%20%2B%20Vite-61DAFB)](frontend/)
[![Backend](https://img.shields.io/badge/backend-FastAPI-009688)](backend/)
[![Status](https://img.shields.io/badge/status-research%20prototype-orange)](#important-use-notice)

> **Important use notice**  
> Lunar Navigator is an independent research prototype. It is not a NASA product or NASA-endorsed system, a certified landing-site assessment tool, or a flight-control system. It is not validated or qualified for operational or mission-critical decisions.

---

## At a glance

Lunar Navigator V4 brings together a browser-based lunar descent dashboard and a custom mission engineering workbench. V4 introduces configurable vehicle and site inputs, a six-degree-of-freedom (6-DOF) rigid-body propagation step, and an optional NAIF SPICE frame-transform adapter. The earlier 2-D descent dashboard remains available as a separate legacy demonstration.

### Main capabilities

| Area | What it provides |
|---|---|
| Mission workbench | Custom vehicle and lunar-site parameter entry with readiness screening |
| 6-DOF dynamics | Rigid-body propagation using position, velocity, attitude quaternion and angular rates |
| Terrain data | Best-effort access to NASA LOLA Product 90 south-polar terrain products |
| Legacy descent demo | Interactive 2-D descent simulation and telemetry visualisation |
| SPICE adapter | Optional NAIF SPICE-based frame transformations with user-supplied kernels |
| Engineering evidence | Verification endpoints, model notes, data provenance and hazard documentation |

## Run locally

The complete application uses **two services**: a Python/FastAPI backend and a React/Vite frontend. Start both services to use the full application.

### 1. Requirements

- Windows 10/11 (commands below use Windows Command Prompt or PowerShell)
- Python 3.10 or newer
- Node.js 18 or newer and npm
- Internet access for installing dependencies and attempting remote NASA data access

### 2. Start the backend

Open a terminal in the project root:

```powershell
cd backend
py -m venv .venv
.venv\Scripts\activate
python -m pip install --upgrade pip
pip install -r requirements.txt
uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```

Keep this terminal running. The API will be available at:

- API root: http://127.0.0.1:8000
- Health check: http://127.0.0.1:8000/health
- Interactive API documentation: http://127.0.0.1:8000/docs

### 3. Start the frontend

Open a **second** terminal in the project root:

```powershell
cd frontend
npm install
npm run dev
```

Open **http://localhost:5173** in your browser. The V4 workbench is available from the top navigation as **V4 WORKBENCH**.

### 4. Build the frontend

To create a production frontend bundle:

```powershell
cd frontend
npm install
npm run build
```

The generated static files are written to `frontend/dist`.

## Can I run it directly from the GitHub link?

**Not from the repository URL alone.** GitHub displays the source code; it does not automatically run this full-stack application. The frontend calls a FastAPI backend, so a public demo requires both parts to be deployed:

1. Deploy the Python API to a Python-compatible hosting service.
2. Deploy the built frontend to a static hosting service.
3. Configure the frontend to call the deployed API URL and configure backend CORS for the frontend domain.
4. Test the live API health endpoint and the application workflows.

GitHub Pages can host static frontend files, but it cannot host this FastAPI backend. A GitHub Pages URL by itself would therefore not provide the complete working application. **No public live-demo URL is claimed until both services have been deployed and verified.**

## V4 API

The V4-specific endpoints include:

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/v4/system/status` | V4 subsystem status |
| POST | `/api/v4/mission/readiness` | Evaluate supplied mission configuration |
| POST | `/api/v4/dynamics/6dof/step` | Propagate one 6-DOF dynamics step |
| GET | `/api/v4/frames/spice/status` | Check optional SPICE adapter status |
| POST | `/api/v4/frames/spice/transform` | Request a configured frame transformation |

The backend also exposes terrain, landing evaluation, simulation, guidance, verification, assurance and report-export endpoints. See the API documentation at `/docs` after starting the backend.

## NASA LOLA terrain data

The project references [NASA GSFC Planetary Geosciences Data Archive — Product 90](https://pgda.gsfc.nasa.gov/products/90), including south-polar LOLA elevation and associated terrain products.

Remote data access is best-effort and depends on network availability and Rasterio/GDAL support for remote Cloud-Optimized GeoTIFF access. If remote terrain data cannot be obtained, the application may retain a procedural demonstration terrain; it should not be mistaken for observed NASA terrain data.

## Verification

Run the backend test suite from the project root:

```powershell
cd backend
python -m pytest -q
```

Passing automated tests indicates that the included test cases passed in that environment. It does **not** establish independent scientific validation, mission qualification, or flight readiness.

## Engineering documentation

The project documentation includes:

- V4 architecture and design conventions
- Custom vehicle parameter register
- Custom-site data contract
- Verification plan and release verification notes
- Model card, hazard log and data provenance
- Engineering notes and verification results

Refer to the `docs/` directory in the source distribution.

## Repository layout

```text
Luner-Navigation-V4/
├── backend/       # FastAPI application, dynamics and tests
├── frontend/      # React/Vite user interface
├── docs/          # Engineering, verification and provenance documents
├── .github/       # CI workflow
├── README.md
├── CHANGELOG.md
├── LICENSE
└── NOTICE.md
```

## Optional NAIF SPICE support

SPICE support is optional. Install `spiceypy` only when you have selected and reviewed appropriate mission-specific kernels. Configure `LUNAR_SPICE_KERNEL_DIR` to point to the kernel directory, then restart the backend. Kernel discovery alone does not verify kernel provenance, coverage, or frame suitability.

## License and attribution

Review the repository's `LICENSE` and `NOTICE.md` before reuse. NASA is not affiliated with or endorsing this project.
