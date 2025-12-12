# fusite2

[hfu/fusite](https://github.com/hfu/fusite) の拡張版。URL フラグメントを使用して地形データとテーマを柔軟に切り替えられる標高タイル可視化サイトです。

## 概要

fusite2 は `#map=xxx&terrain=yyy&theme=zzz` 形式の URL フラグメントをサポートし、以下の機能を提供します：

- **theme**: 地形の上に載せる地図スタイルを指定
  - `osm` (デフォルト): OpenStreetMap ベース
  - `gsi`: 国土地理院基盤地図情報ベース
  - `contour`: 動的等高線生成
  - `lineage`: 地形に対応した画像タイル（`terrain` に `-lineage` を付加したタイルを使用）
  - `multidirectional`: 複数光源による多重陰影表示（hillshade の複数光源設定）
- **terrain**: Terrarium タイルの場所を指定 (デフォルト: `fusi`、他に `iwaki`, `shimabara` など)
- **map**: MapLibre GL JS の hash 形式による地図位置

詳細は `docs/README.md` を参照してください。

## 技術スタック

- [MapLibre GL JS](https://maplibre.org/)
- [maplibre-contour](https://www.npmjs.com/package/maplibre-contour)

## 多重光源（multidirectional）について

`fusite2` には地形の陰影表示に複数光源を用いる「多重光源（multidirectional）」テーマを含めています。これは標高データの陰影を複数方向から重ねることで稜線や谷、尾根などの地形特徴を把握しやすくする表現です。

- テーマ名: `multidirectional`
- 実装: `docs/index.html` 内の `buildMultidirectionalStyle` 関数で定義。
- 調整可能なパラメータ: `hillshade-highlight-color`, `hillshade-shadow-color`, `hillshade-illumination-direction`, `hillshade-illumination-altitude`, `hillshade-exaggeration` など。

プリセット（例）:

- `Natural`（控えめ、デフォルト）: `exaggeration: 0.3`, `altitude: 30`
- `Vivid`（強調）: `exaggeration: 0.6`, `altitude: 20`, 鮮やかなハイライト/シャドウ配列

注: 表示は MapLibre GL JS の hillshade 機能に依存します。ブラウザや MapLibre のバージョンによってサポート状況が異なるため、期待通りの動作が得られない場合は MapLibre のバージョンを確認してください。

## ライセンス

測量法に基づく国土地理院長承認（使用）R 6JHs 133

