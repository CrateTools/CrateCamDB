# CrateCamDB under contruction

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

```
https://raw.githubusercontent.com/CrateTools/CrateCamDB/main/crate_cameras.json
```

Please cache responsibly — GitHub serves these for free but is not a database backend.

## Contributing

Corrections, additions, and clarifications are welcome.

**To add a new camera:**

1. Fork the repository
2. Add an entry to `crate_cameras.json` following the schema above
3. Open a pull request including:
   - The camera's full official name (as the manufacturer markets it)
   - Source(s) for the sensor dimensions (manufacturer spec sheet, official handbook, press release, etc.)
   - At minimum one sensor mode; more is better

**To correct an existing entry:**

1. Open an issue describing the correction with your source, or
2. Open a pull request directly if the fix is obvious and well-sourced

**Slug conventions:**

- Lowercase, hyphen-separated
- Brand prefix when possible (`sony-venice-ii-86k`, not `venice-86k`)
- Stable: once a slug is published, it should not change. Renaming a slug breaks every tool that depends on this database.

## License

CrateCamDB is licensed under the [Creative Commons Attribution 4.0 International License (CC-BY-4.0)](https://creativecommons.org/licenses/by/4.0/).

The full license text is in the [`LICENSE`](./LICENSE) file.

You are free to:

- **Use** this database for any purpose, including commercial products and pipelines
- **Modify** entries, add new cameras, correct existing ones
- **Redistribute** the database, in whole or in part, original or modified

Under one condition:

- **Attribution** — give appropriate credit, link to the license, and indicate if changes were made.

### How to credit CrateCamDB

When using this database in your tool, include a credit line in your About page, documentation, or `_metadata` block. Suggested wording:

> Camera reference data from CrateCamDB (https://github.com/CrateTools/CrateCamDB) — licensed under CC-BY-4.0.

If you've modified entries:

> Camera reference data based on CrateCamDB (https://github.com/CrateTools/CrateCamDB), with modifications. Licensed under CC-BY-4.0.

### Upstream attribution

This database is derived in part from **[VFXCamDB](https://vfxcamdb.com)** maintained by **Tony D'Agostino** and **[Matchmove machine](https://camdb.matchmovemachine.com/)** maintained by **Matchmove machine**.

We are genuinely grateful and the Camera module would not exist in its current form without Tony D'Agostino and Matchmove machine work. If you find this data useful, please consider [supporting VFXCamDB](https://vfxcamdb.com/donate/) and [supporting Matchmove machine](https://camdb.matchmovemachine.com/donate) directly.

VFXCamDB and Matchmove machine are the foundational community reference for digital cinema sensor specifications and the work CrateCamDB builds on. Downstream users — whether integrating this data into a tool, publishing derivative works, or simply redistributing copies — are strongly encouraged to preserve credit to VFXCamDB and Matchmove machine alongside CrateCamDB.

A combined attribution might read:

> Camera reference data from CrateCamDB (https://github.com/CrateTools/CrateCamDB), licensed under CC-BY-4.0. Includes data derived from VFXCamDB.com (Tony D'Agostino) and Matchmove machine.

### Camera manufacturer trademarks

Camera, sensor, and manufacturer names (ARRI, Sony, RED, Blackmagic, Canon, etc.) referenced in this database are trademarks of their respective owners. Their use here is purely descriptive — to identify the specific products whose specifications are documented — and does not imply endorsement of CrateCamDB or of any tool that uses it.

## Disclaimer

Camera specifications are provided as-is for reference. While care has been taken to verify accuracy against manufacturer documentation, errors are possible. For production-critical applications (matchmove, virtual production, lens calibration), users are advised to verify sensor dimensions against the camera manufacturer's official specifications before committing them to a shot.

If you find an error, please open an issue — accuracy is the whole point.

## Acknowledgments

- **Tony D'Agostino** of [VFXCamDB.com](https://vfxcamdb.com), whose decade-plus of community work made this database possible.
- **[Matchmove machine](https://camdb.matchmovemachine.com/)** by **Matchmove machine**.
- The VFX, matchmove, and virtual production community, whose collective tracking, calibration, and bug-reporting work is the substrate this kind of reference data rests on.

---

Maintained by [CrateTools](https://github.com/CrateTools).
