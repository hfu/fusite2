# fusite2

[hfu/fusite](https://github.com/hfu/fusite) の拡張版。URL フラグメントを使用して地形データとテーマを柔軟に切り替えられる標高タイル可視化サイトです。

## 概要

fusite2 は `#map=xxx&terrain=yyy&theme=zzz` 形式の URL フラグメントをサポートし、以下の機能を提供します：

- **theme**: 地形の上に載せる地図スタイルを指定
  - `osm` (デフォルト): OpenStreetMap ベース
  - `gsi`: 国土地理院基盤地図情報ベース
  - `contour`: 動的等高線生成
- **terrain**: Terrarium タイルの場所を指定 (デフォルト: `fusi`、他に `iwaki`, `shimabara` など)
- **map**: MapLibre GL JS の hash 形式による地図位置

詳細は [docs/README.md](docs/README.md) を参照してください。

## 技術スタック

- [MapLibre GL JS](https://maplibre.org/) v5.7.0
- [maplibre-contour](https://www.npmjs.com/package/maplibre-contour) v0.0.5

## ライセンス

測量法に基づく国土地理院長承認（使用）R 6JHs 133
