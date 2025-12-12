
# fusite2

[hfu/fusite](https://github.com/hfu/fusite) の拡張版。URL フラグメントを使用して地形データとテーマを柔軟に切り替えられる標高タイル可視化サイトです。

## 目次

- 概要
- サポートされているテーマ
- サポートされている地形（terrain）
- URL フラグメントでの操作
- コントロールと操作
- データソースとライセンス
- ローカルでの実行
- 技術スタック
- 貢献について
- CHANGELOG

## 概要

`fusite2` はブラウザ上で Terrarium 形式の標高タイルを可視化する静的な Web ページです。URL のフラグメント（ハッシュ）を使って `map` / `terrain` / `theme` / `exag` を指定し、ページをリロードせずに表示を切り替えられます。

## サポートされているテーマ

`docs/index.html` の実装で利用できる主なテーマ（`theme` パラメータ）:

- `osm` — OpenStreetMap のラスタタイルを背景に表示します。
- `gsi` — 国土地理院の最適化ベクトルタイル（bvmap-overdrive / MLT）を読み込み、行政区画や道路等のベクトル情報を重ねます。
- `gsi-ortho` — 国土地理院のシームレス航空写真タイル（seamlessphoto）を背景に使用します。
- `contour` — `maplibre-contour` を使って Terrarium 標高タイルから動的に等高線を生成します。
- `lineage` — 地形に対応した Lineage 画像タイルを表示します（`terrain` 名に `-lineage` を付加した TileJSON を参照）。
- `multidirectional` — 複数光源を用いた hillshade 表示（ハイライト/シャドウ色配列、光源方向、高度、強調度を調整可能）。

※ 一部テーマは UI のドロップダウンに表示されない場合がありますが、URL の `theme` を直接指定すれば利用可能です。

## サポートされている地形（terrain）

UI（`docs/index.html` の `<select id="terrain-select">`）で明示的に用意されている地形:

- `fusi`
- `iwaki`
- `shimabara`
- `fuji`
- `dem10`

これらは TileJSON を `https://tunnel.optgeo.org/martin/{terrain}` から取得し、`tiles` 配列をソースとして利用します。TileJSON が取得できない場合はフォールバックで URL パターンを組み立てます。

## URL フラグメントでの操作（ユーザー向け）

ページの状態は URL のフラグメントで表現できます。形式:

```
#map=<zoom>/<lat>/<lng>/<pitch>/<bearing>&terrain=<terrain>&theme=<theme>&exag=<exaggeration>
```

例:

- デフォルト: `https://<your-host>/fusite2/`（UI から操作）
- 東京を GSI テーマで: `...#map=14/35.6812/139.7671/45/0&terrain=fusi&theme=gsi`
- 島原を等高線テーマで: `...#map=14/32.75/129.87/20/0&terrain=shimabara&theme=contour&exag=2.0`

UI の `テーマ` / `地形` ドロップダウンで選択すると、ページの再読み込みなしに地図が更新され、URL フラグメントは `history.replaceState` によって更新されます。

## コントロールと操作

- 右上: ナビゲーション（ズーム/回転/ピッチ） / フルスクリーン / 現在地 / GlobeControl
- 左下: 地形強調（Terrain Exaggeration）のスライダー（0〜3）
- 左上: 情報パネル（`テーマ` / `地形` の選択、説明、出典表示）

## データソースとライセンス

- 標高タイル: Terrarium 形式（tileSize 512）を `https://tunnel.optgeo.org/martin/{terrain}` から取得
- GSI ベクトルタイル（bvmap-overdrive）は `https://tunnel.optgeo.org/martin/bvmap-overdrive/{z}/{x}/{y}` を利用
- 国土地理院データ利用に関する承認表示: R 6JHs 133

## ローカルでの実行

静的ファイルなので簡単にローカルサーバで確認できます:

```bash
cd docs
python3 -m http.server 8000
# ブラウザで http://localhost:8000 を開く
```

または:

```bash
npx http-server docs -p 8000
```

## 技術スタック

- Map rendering: `maplibre-gl` (loaded from `https://unpkg.com/maplibre-gl@^5.13.0` in `docs/index.html`)
- Contour generation: `maplibre-contour` (v0.0.5, loaded from unpkg in `docs/index.html`)

## 貢献について

小さな修正（ドキュメント、スタイル調整、プリセット追加など）はプルリクエスト歓迎です。大きな機能追加や API 変更は Issue で事前に相談してください。

## CHANGELOG

直近の更新は `CHANGELOG.md` を参照してください。

## ライセンス

測量法に基づく国土地理院長承認（使用）R 6JHs 133

詳細は `docs/README.md` を参照してください。

## 技術スタック

- [MapLibre GL JS](https://maplibre.org/)
- [maplibre-contour](https://www.npmjs.com/package/maplibre-contour)

## 多重光源（multidirectional）について

`fusite2` には地形の陰影表示に複数光源を用いる「多重光源（multidirectional）」テーマを含めています。これは標高データの陰影を複数方向から重ねることで稜線や谷、尾根などの地形特徴を把握しやすくする表現です。

## ライセンス

測量法に基づく国土地理院長承認（使用）R 6JHs 133

