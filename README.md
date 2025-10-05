🌍 Planetary Impact Simulation — Interactive Web App

Simulate asteroid impacts on Earth 🌎 with a Leaflet map and simple physics-based models. Single-page, static app — no backend required.

🚀 Features

Simulate impacts by inputting:

Diameter (m)

Velocity (km/s)

Latitude / Longitude

Outputs:

Energy (MT TNT)

Crater radius (m)

Seismic magnitude (Mw)

Damage zones

⚙️ Run Locally

Python:

python -m http.server 8000

Visit: http://localhost:8000

Node (no install):

npx serve -n -l 8000 .

🧠 Simulation

Physics Functions: Mass, Kinetic Energy, Crater Radius, Seismic Magnitude, Damage Radii

Map Interaction:

Click Run Impact Simulation → updates map & results

Shows impact marker, crater, and damage zones

📦 Dependencies

Tailwind CSS (CDN)

Leaflet (CSS + JS via CDN)

OpenStreetMap tiles

💡 Notes

Educational & visualization purposes only.

No build or test setup — runs in browser.
