# CrateCamDB

A curated, open camera sensor database for VFX, matchmove, and virtual production work.

CrateCamDB is the camera reference database used by [Crate](https://github.com/CrateTools/Crate), a Nuke pipeline tool. It is maintained as a standalone repository so it can be used, contributed to, and integrated into other tools independently of Crate itself.

## What's in here

A single JSON file — `crate_cameras.json` — containing sensor specifications for hundreds of digital cinema cameras, broadcast cameras, drones, mobile phones, film formats, and bare sensors.

Each camera entry includes:

- Manufacturer and model name
- Sensor type and shutter mechanism (where known)
- Camera category (cinema, photo, drone, mobile, film)
- One or more sensor modes, each with:
  - Mode label (e.g., "Open Gate 4.6K", "Super 35 4K", "Anamorphic 4:3")
  - Active sensor dimensions in millimeters (width × height)
  - Resolution in pixels (width × height)

## Schema

The JSON has two top-level keys: `_metadata` and `cameras`.

```json
{
  "_metadata": {
    "version": "2.0.0",
    "last_updated": "2026-05-15",
    "camera_count": 334,
    "credits": "Camera reference data derived in part from VFXCamDB.com (Tony D'Agostino), used with permission. Curation and additional data by CrateTools.",
    "license": "CC-BY-4.0",
    "license_url": "https://creativecommons.org/licenses/by/4.0/",
    "source": "https://github.com/CrateTools/CrateCamDB"
  },
  "cameras": {
    "arri-alexa-35": {
      "name": "ARRI Alexa 35",
      "manufacturer": "ARRI",
      "sensor_type": "CMOS",
      "shutter": "Electronic",
      "cam_type": "cinema",
      "sensor_dimensions": [
        {
          "label": "Open Gate 4.6K",
          "res_w": 4608,
          "res_h": 3164,
          "width_mm": 27.99,
          "height_mm": 19.22
        }
      ],
      "url": ""
    }
  }
}
```

Camera keys are slugs: lowercase, hyphen-separated, machine-stable. They are intended as reliable lookup identifiers and should not be renamed once published. The human-readable name lives in the `name` field.

## How to use this database

### In your own tool

Pull `crate_cameras.json` directly. The schema is stable and documented above. Python's standard library is sufficient — no external dependencies required.

```python
import json

with open("crate_cameras.json") as f:
    db = json.load(f)

cameras = db["cameras"]
alexa = cameras.get("arri-alexa-35")
print(alexa["name"], alexa["sensor_dimensions"][0]["width_mm"])
```

### In Crate

Crate ships with a snapshot of CrateCamDB. The Settings → Camera Database panel includes an **"Import JSON from CrateTools"** button that pulls the latest version from this repository on demand, merging new and corrected entries into the curator's local database.

### As a fetch endpoint

For tools that want to pull the latest version programmatically, the raw URL is:
