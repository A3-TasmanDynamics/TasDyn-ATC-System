# TasDyn-ATC-System 📡

![Arma 3](https://img.shields.io/badge/Arma%203-Mod-green)
![Build System](https://img.shields.io/badge/Built%20With-HEMTT-blue)
![Vue.js](https://img.shields.io/badge/Vue.js-35495E?style=flat&logo=vuedotjs&logoColor=4FC08D)
![Node.js](https://img.shields.io/badge/Node.js-43853D?style=flat&logo=nodedotjs&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=flat&logo=typescript&logoColor=white)
![License](https://img.shields.io/badge/License-APL--SA-orange)



An integrated, real-time Air Traffic Control (ATC) and airspace monitoring system for Arma 3. The Tasman Dynamics ATC extracts live telemetry from the game engine and broadcasts it to a tactical web interface, providing command elements with total airspace visibility.

## ⚙️ System Architecture

This system is decoupled across three core environments:

1. **Arma 3 Server Mod (SQF):** Sweeps the `CfgVehicles` for active aircraft, extracting coordinates, altitude, speed, heading, and pilot data. Built using HEMTT.
2. **Backend Bridge (Node.js/Express):** A lightweight TypeScript server that receives HTTP POST data from the Arma 3 server and broadcasts it instantly via WebSockets.
3. **STRATCOM WebApp (Vue 3/Vite):** A dark-mode, CRT-styled frontend utilizing Jetelain's `GameMapStorage` architecture to render live radar blips over high-resolution, custom-tiled map extracts natively using Leaflet (`L.CRS.Simple`).

---

## 🛠️ Prerequisites

* **Node.js** (v18+ recommended)
* **Arma 3 Dedicated Server or Listen Server**
* **extREST** or **ArmaRequests** (Arma 3 Server-side HTTP extension)
* **HEMTT** (For building the Arma 3 mod PBO)
* **Jetelain's GameMapStorage Tools** (For extracting custom map topography and serving web tiles)

---

## 🚀 Installation & Setup

### 1. The Backend Bridge
The central nervous system. It hosts the map tiles and routes WebSocket traffic.

```bash
cd backend
npm install
npm run start
```
Ensure your exported map folder (containing the tiles and config.json) is placed in backend/public/maps/<worldName>/.

### 2. STRATCOM Frontend
The tactical UI. It connects to the backend to display the live radar.

```Bash
cd frontend
npm install
npm run dev
```
Configure your backend IP and port in frontend/public/web.json.

### 3. Arma 3 Server Mod
1. Navigate to the arma3-mod directory.
2. Build the .pbo using HEMTT: hemtt build
3. Load the compiled mod on your server.
4. Ensure your server is running an HTTP extension (like extREST) and that the IP/Port in initServer.sqf matches your Node.js backend.

---

## 🗺️ Custom Map Extraction Pipeline
To use custom modded maps, you must extract the topography from the Arma 3 engine using Jetelain's updated GameMapStorage ecosystem.
1. **Extract & Tile:** Utilize Jetelain's exporter tools. Open your desired terrain in the Eden Editor, and follow the mod's specific extraction commands to generate your tile pyramid.
2. **Host:** The exporter will generate a folder containing subfolders of tiles (0, 1, 2, etc.) and a config.json file. Move this entire folder into backend/public/maps/<worldName>/.
3. **Deploy:** When the Arma 3 server initializes, the frontend will automatically fetch the new config.json and seamlessly load the correct terrain and coordinate system via Leaflet and GameMapStorage.

---

## 🛡️ License
This project is licensed under the Arma Public License Share Alike (APL-SA).
You are free to adapt (modify, rework, or update) and share (copy, distribute, or transmit) the material under the following conditions:

. Attribution: You must attribute the material to Tasman Dynamics.
. Noncommercial: You may not use this material for any commercial purposes.
. Arma Only: You may not convert or adapt this material to be used in games other than Arma.
. Share Alike: If you adapt, or build upon this material, you may distribute the resulting material only under the same license.

For the full legal text of the license, please visit the Bohemia Interactive Licenses page.
