# Map Symbology Optimizer (geoskill-map-symbology-optimizer)

> Optimize map symbology using color theory, contrast, visual hierarchy and accessible palettes

---

## 1. Overview

Optimizes map symbol colors based on color theory and visual perception: **WCAG contrast** (picks the optimal black/white text color for each class), **color-vision accessibility** (class colors must remain distinguishable after deuteranopia simulation), and **visual hierarchy** (a color-distance metric between classes). Classification uses the Okabe-Ito / Tol accessible palettes, and outputs a complete symbology scheme JSON and a color map.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-map-symbology-optimizer.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `symbology.png` | PNG | Color-classified map + legend (primary output) |
| `classes.tif` | GeoTIFF | Class index raster (verifiable output) |
| `symbology.json` | JSON | Breaks / colors / contrast / color-vision-safety QA |

Each run also produces `output-manifest.json` (run manifest).


## 6. Technical Principle

relative_luminance / contrast_ratio (WCAG, (L1+.05)/(L2+.05)) → simulate_deuteranopia linear matrix → min_pairwise_separation determines color-vision safety → best_text_color picks text color → optimize_symbology assembles the scheme.

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
