# HTML ↔ Backend Sync Summary

## Changes Made to `templates/live.html`

### 1. **Detection State Management**
- Added `detectionActive` boolean variable to track the detection state locally in the frontend
- This ensures the frontend respects the backend's pause/resume state

### 2. **Improved `startAutoUpdate()` Function**
- Added check: `if (!detectionActive) { return; }` to prevent API calls when detection is paused
- Better error handling with response validation
- Maintains sync with backend's `/api/crowd_coordinates` endpoint

### 3. **Enhanced `toggleDetection()` Function**
- Now properly updates the `detectionActive` flag from backend response
- Removes the buggy restart of `startAutoUpdate()` on resume
- Updates button text to reflect actual state (⏸️ Pause / ▶️ Resume)
- Better error handling and console logging

## Backend API Endpoints Used

| Endpoint | Method | Response | Purpose |
|----------|--------|----------|---------|
| `/live_feed` | GET | MJPEG Stream | Live YOLO detection video feed |
| `/api/crowd_coordinates` | GET | JSON | Real-time detection coordinates & area counts |
| `/api/toggle_detection` | GET | JSON | Toggle detection on/off |
| `/api/live_stats` | GET | JSON | Overall statistics (not currently used in UI) |
| `/api/area_analytics_precise` | GET | JSON | Area-wise analytics (not currently used in UI) |
| `/api/adjust_precision` | GET | JSON | Precision settings (not currently used in UI) |

## Current Frontend-to-Backend Flow

```
┌─────────────────┐
│   live.html     │
└────────┬────────┘
         │
    ┌────┴─────────┬─────────────────────┐
    │              │                     │
    ▼              ▼                     ▼
/live_feed    /api/crowd_       /api/toggle_
(MJPEG)       coordinates       detection
    │              │                     │
    └──────────────┼─────────────────────┘
                   │
              ┌────▼──────┐
              │  app.py   │
              │  (Flask)  │
              └───────────┘
                   │
         ┌─────────┼──────────┐
         │         │          │
         ▼         ▼          ▼
      YOLOv8   RTSP Camera  Detection
      Model    (IP Cam)     History
```

## Key Features Synced

✅ **Real-time Detection Updates** - Auto-fetches every 1.5 seconds
✅ **Live Video Feed** - Displays YOLO-annotated frames
✅ **Heatmap Visualization** - Shows crowd density on facility layout
✅ **Area Distribution** - Lists detection counts by area
✅ **Pause/Resume Control** - Toggles detection on/off
✅ **Detection Tracking** - Tracks individual persons with IDs
✅ **Court Boundary Constraint** - Keeps detections within facility bounds
✅ **Error Handling** - Shows connection status and errors

## Data Flow

1. **Backend captures video** from IP camera via RTSP
2. **YOLOv8 detects humans** with high-precision confidence scoring
3. **Detections are normalized** to 0-1 coordinate range
4. **Frontend fetches coordinates** and updates visualizations:
   - Heatmap canvas (crowd density)
   - Detection canvas (pulsing person indicators)
   - Area count statistics

## Configuration

### Timing
- **API Update Interval**: 1500ms (1.5 seconds)
- **Animation Frame**: Continuous (for pulsing effects)
- **Court Constraints**: 16.7%-83.3% X, 25%-75% Y

### Detection Parameters
- **Confidence Threshold**: 0.5 (from backend)
- **Frame Resolution**: 1280×720
- **Precision**: 6-decimal coordinate accuracy
- **Tracking Tolerance**: 50 pixels

## Testing Checklist

- [ ] Start Flask app: `python app.py`
- [ ] Open browser: `http://localhost:5000`
- [ ] Verify live feed displays (left panel)
- [ ] Check heatmap updates every ~1.5 seconds
- [ ] Test Pause/Resume button
- [ ] Verify area counts update in real-time
- [ ] Check error handling (stop app and verify error message)
- [ ] Verify WhatsApp messaging in terminal

## Notes

- All frontend-backend communication uses JSON over HTTP
- CORS is not enabled but not needed (same origin)
- Detection can be paused without stopping the server
- Heatmap respects facility layout boundaries
- All data is processed with numpy type conversion for JSON serialization
