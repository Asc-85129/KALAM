# Project Kalam  
### National Runner-up (2nd Place) — ISRO Bhartiya Antariksh Hackathon 2025  
Built by **Team DOMinators**

---

##  Overview

**Project Kalam** is a near-term atmospheric nowcasting and visualization system developed during the ISRO Bhartiya Antariksh Hackathon 2025, where it secured **2nd place nationally**.

The system combines:

-  Python-based ML inference pipeline (PyTorch + ONNX)  
-  Node.js/Express backend with WebSocket live logs  
-  Cloudflare R2 storage integration  
-  MongoDB persistence  
-  Modern React + Vite + Tailwind UI  

It enables short-term satellite frame prediction, real-time log streaming, rich map visualization, and operational UX.

---

# Live Focus

## Nowcasting
Predict short-term satellite frames:
- T+1  
- T+2  
- T+3  

## Visualization
- Multi-band exploration
- Timeline animations
- Overlay views
- Model performance reports

## Operational UX
- File uploads to Cloudflare R2
- MongoDB persistence
- WebSocket streaming of Python inference logs
- Model test + validation UI

---

# Tech Stack

## Frontend
- React 19  
- Vite  
- Tailwind CSS  
- react-router  
- lucide-react  

## Backend
- Node.js  
- Express  
- WebSocket (ws)  
- Multer  
- MongoDB (Mongoose)  

## Storage
- Cloudflare R2 (AWS SDK v3)

## ML / Inference
- Python  
- PyTorch  
- ONNX  

---

# Monorepo Layout

```
frontend/         → React/Vite UI (animations, maps, reports)
backend/          → Express API + WebSocket + R2 + MongoDB
Python-Backend/   → Training & inference utilities
```

---

# Quick Start

## Prerequisites

- Node.js 18+
- Python 3.10+
- MongoDB (local or Atlas)
- Cloudflare R2 bucket + credentials

---

## Clone the Repository

```bash
git clone <this-repo>
cd <this-repo>
```

---

## Backend Setup

```bash
cd backend
npm install
```

Create a `.env` file (see below).

Run:

```bash
npm run dev
```

- Express → `http://localhost:3000`
- WebSocket → `ws://localhost:3001`

---

## Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

Open:

```
http://localhost:5173
```

---

# Environment Variables

Create `backend/.env`

## Server
```
PORT=3000
NODE_ENV=development
```

## Database
```
MONGO_URL=mongodb://localhost:27017/project-kalam
```
(or Atlas URI)

## Cloudflare R2
```
R2_ENDPOINT=https://<your-account-id>.r2.cloudflarestorage.com
R2_ACCESS_KEY_ID=<your-access-key>
R2_SECRET_ACCESS_KEY=<your-secret-key>
R2_BUCKET_NAME=<your-bucket>
R2_PUBLIC_URL=https://pub-<id>.r2.dev/<your-bucket>
```

On startup, backend verifies R2 configuration and logs health summary.

---

# Important: Update Local Paths

There are hard-coded Windows paths from hackathon development.

### Python Script Path
```
backend/controllers/modelTest.controller.js:20
const pythonScriptPath = "d:\\Hackathon\\ISRO\\pre_final\\test1.py";
```

### Static Predicted Images Path
```
backend/app.js:28
backend/controllers/modelTest.controller.js:255, 445
```

Change:
```
D:\\Hackathon\\ISRO\\pre_final\\inference_outputs\\test
```

To your local inference output directory.

---

# Run Flow

1. Start backend (`:3000`) and WebSocket (`:3001`)
2. Start frontend (`:5173`)
3. Use the UI:

### “Chase The Cloud”
- Animate frames
- Request predictions for T+1..T+3

### “Test Model”
- Stream Python logs via WebSocket

### 🗺 “Visualize On Map”
- Explore overlays (if configured)

---

# API Overview

Base URL:
```
http://localhost:3000
```

---

## Core Endpoints

### `GET /`
Service info + advertised endpoints

### `GET /api/prediction-images/<relative-path>`
Serves predicted images from `testOutputPath`

---

## R2 Uploads (`/api/v1`)

### `POST /upload`
Upload single file  
- Field name: `file`  
- Validates type/size  
- Stores in R2  
- Persists metadata in MongoDB  

### `POST /test-upload`
Multer sanity check

### `GET /health`
R2 configuration + health snapshot

---

## Model Test & Predictions (`/api/v1`)

### `POST /folder-path`
Triggers Python validation job  
Logs stream via:
```
ws://localhost:3001
```

### `POST /predict-frames`

Body:
```json
{
  "timeWindow": [4 numbers],
  "selectedDirectory": "string",
  "bands": ["string"],
  "windowSize": number
}
```

Returns:
- Predicted frames metadata
- Performance aggregates

---

### `GET /available-sequences`
Lists available `sequence_****` folders under `testOutputPath`

---

# Frontend Highlights

- Multi-band timeline with keyboard navigation
- Single-cycle animation
- Predicted vs ground-truth comparison (T+1..T+3)
- Metrics display panel
- File System Access API integration
- Dark/light theme toggle
- Toast notifications
- Polished UI components

---

# Key Entrypoints

```
frontend/src/App.jsx
frontend/src/pages/SatelliteAnimationPage.jsx
frontend/src/components/ModelTestAndTerminalPreview.jsx
frontend/src/libs/axios.js
```

---

# Backend Structure

```
backend/app.js
backend/routes/r2upload.routes.js
backend/controllers/r2upload.controller.js
backend/routes/modelTest.routes.js
backend/controllers/modelTest.controller.js
backend/config/r2.config.js
backend/utils/db.js
backend/models/File.model.js
Python-Backend/*
```

---

# 🧩 Troubleshooting

### Images Not Loading
Update `testOutputPath` in:
- `backend/app.js`
- `modelTest.controller.js`

### WebSocket Not Connecting
Ensure:
```
ws://localhost:3001
```
is reachable.

### Uploads Failing
- Verify `.env` R2 variables
- Check bucket permissions
- Test:
```
GET /api/v1/health
```

### Mongo Errors
Check `MONGO_URL` and confirm MongoDB is running.

### Python Errors
- Fix `pythonScriptPath`
- Activate Python environment
- Run script manually to verify

---

# Achievement

**ISRO Bhartiya Antariksh Hackathon 2025**  
National Runner-up (2nd Place)

---

# Acknowledgements

Special thanks to:
- Mentors  
- Organizers  
- Open-source community  

---

# License

MIT License  
See `LICENSE` for details.
