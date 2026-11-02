---
name: geoskill-map-symbology-optimizer
description: 'Optimize map symbology using color theory, contrast, visual hierarchy and accessible palettes'
---

# 地图符号优化 | Map Symbology Optimizer

Optimizes map symbology colors based on color theory and visual perception: **WCAG contrast** (selects the optimal black/white text color for each class), **color-vision accessibility** (class colors must remain distinguishable after deuteranopia simulation) and **visual hierarchy** (inter-class color distance metric).

Classification uses the Okabe-Ito / Tol accessible palettes, and outputs a complete symbology scheme as JSON plus a color scheme figure.

## Core Algorithm / 核心算法

relative_luminance/contrast_ratio (WCAG, (L1+.05)/(L2+.05)) → simulate_deuteranopia linear matrix → min_pairwise_separation determines color-vision safety → best_text_color picks the text color → optimize_symbology assembles the scheme.

## Dependencies / 依赖

```bash
pip install numpy rasterio scipy matplotlib geopandas shapely pillow
```

## Usage / 使用方法

### Example 1 (synthetic data, offline)

```bash
python geoskill-map-symbology-optimizer.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### Example 2 (equal-interval classification)

```bash
python geoskill-map-symbology-optimizer.py --input landcover.tif --method equal_interval --classes 6
```

### Example 3 (Tol muted palette)

```bash
python geoskill-map-symbology-optimizer.py --input landcover.tif --palette tol-muted
```

### Example 4 (synthetic, 4 classes)

```bash
python geoskill-map-symbology-optimizer.py --bbox 116 39 117 40 --synthetic --classes 4
```

### Example 5 (quantile + Okabe-Ito)

```bash
python geoskill-map-symbology-optimizer.py --input landcover.tif --method quantile --palette okabe-ito
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `symbology.png` | PNG | Color-classified map + legend (main deliverable) |
| `classes.tif` | GeoTIFF | Class index raster (verifiable deliverable) |
| `symbology.json` | JSON | Breaks/colors/contrast/color-vision-safety QA |

Each run also produces `output-manifest.json` (run manifest).

## Data Source / 数据源 / Source

Local GeoTIFF / vector files; `--synthetic` mode generates physically consistent simulated data, fully offline.

## Privacy / 隐私声明 / Privacy

- Runs offline by default; `--synthetic` mode requires no network at all.
- All processing is performed locally; no user data is uploaded.

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
