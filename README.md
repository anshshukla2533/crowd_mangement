# Real-Time Person Detection Dashboard

This project provides a full-stack, real-time crowd monitoring dashboard with live YOLO-based person detection, tracking, heatmap visualization, and WhatsApp alerts.

## Features
- Live video stream with YOLOv8 person detection (bounding boxes, IDs)
- Real-time tracking (DeepSORT/ByteTrack/SORT)
- Pixel-to-real-world coordinate mapping (homography calibration)
- Live heatmap with tracked coordinates and trajectories
- Floating stats button (people count, crowd status, hotspots, last update)
- Automated WhatsApp alerts every 4 minutes
- Modern React + Tailwind frontend
- Flask + Socket.IO backend

## Directory Structure
```
backend/    # Python backend (Flask, YOLO, tracking, WhatsApp, calibration)
frontend/   # React + Tailwind frontend
.github/    # Copilot instructions
.env        # Environment variables (API keys, camera URL, etc.)
README.md   # Project documentation
```

## Quick Start
1. See backend/README.md and frontend/README.md for setup instructions.
2. Calibrate camera using the provided tool for accurate coordinate mapping.
3. Configure WhatsApp API credentials in `.env`.
4. Run backend and frontend. Access dashboard in your browser.

---

Replace all placeholder values and follow the detailed setup in each subdirectory for a working deployment.
