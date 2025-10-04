# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

Project overview
- This repository is a single-page, static web app (index.html) that renders a Leaflet map and overlays a physics-based planetary impact simulation. There are no package manifests, build tools, or tests configured.

Commands
- Run locally (static server)
  - Using Python (recommended if available):
    ```bash path=null start=null
    python -m http.server 8000
    ```
    Open http://localhost:8000 and load index.html.
  - Using Node (via npx, no install needed):
    ```bash path=null start=null
    npx serve -n -l 8000 .
    ```
    Then open http://localhost:8000.
- Build: Not applicable (no build step configured).
- Lint: Not applicable (no lint tooling configured).
- Test: Not applicable (no test tooling configured).

High-level architecture
- UI layout
  - A full-viewport Leaflet map (#map) using OpenStreetMap tiles.
  - An input panel (fixed, top-right) with fields for asteroid diameter (m), impact velocity (km/s), latitude, and longitude. A “Run Impact Simulation” button triggers calculations.
  - A results panel (fixed, lower area) shows: energy (megatons TNT), crater radius (m), seismic magnitude (Mw), and damage radii for severe/moderate/light overpressure thresholds.
- Physics and simulation (inline <script> in index.html)
  - Constants: gravitational acceleration, asteroid bulk density, joules-to-megatons conversion, Earth radius, seismic coupling factor, and crater scaling parameters.
  - Core functions:
    - massFromDiameter(d_m, density): sphere mass from diameter and density.
    - kineticEnergyJoules(d_m, v_kmps, density): 0.5·m·v^2 with km/s to m/s conversion.
    - joulesToMegatons(joules): converts joules to MT TNT equivalent.
    - estimateCraterRadiusM(energy_j): simple scaling law R = C·E^{1/3.4}.
    - estimateSeismicMagnitude(energy_j): Mw ≈ (2/3)·log10(E_seis[J]) − 3.2 with E_seis = coupling·E.
    - overpressureFromEnergyAtDistance(E_mt, distance_km): approximate blast scaling; computes incident overpressure in kPa from scaled distance.
    - calculateDamageRadii(E_mt): bisection search to find distances (km) at which overpressure falls to 35, 10, and 1 kPa.
    - simulateImpact(diameter_m, velocity_kms, lat, lon): returns inputs, constants, and computed results (energy, crater, seismic Mw, damage radii).
  - Rendering and interaction:
    - Map initialized with OpenStreetMap tile layer and default view (Mumbai area) at zoom 6.
    - Clicking “Run Impact Simulation” parses inputs, runs simulateImpact, recenters the map, updates the results panel, and renders Leaflet overlays:
      - Marker at impact point.
      - One small circle for crater radius (meters).
      - Three larger circles for severe, moderate, and light damage radii (kilometers converted to meters).
    - Overlays are cleared and redrawn on each simulation.
- External dependencies
  - Tailwind CSS via CDN for styling.
  - Leaflet CSS/JS via CDN for mapping and vector overlays.
  - OpenStreetMap tile layer for basemap (attribution included in the map’s configuration).

Notes
- No README.md, WARP.md, or tool-specific rule files (Claude/Cursor/Copilot) are present. If you introduce build, lint, or test tooling later, update this file with the corresponding commands (build, lint, run tests, and run a single test).
