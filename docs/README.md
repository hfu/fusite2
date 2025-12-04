# fusite2 標高タイル可視化サイト

このサイトは [hfu/fusite](https://github.com/hfu/fusite) の拡張版で、URL インタフェースを通じて地形データとテーマを柔軟に切り替えられます。

## URL フラグメントパラメータ

`#map=xxx&terrain=yyy&theme=zzz&exag=www` 形式のフラグメントを使用して地図の状態を制御します。

注: UI（情報パネル）の `theme` / `terrain` ドロップダウンで選択すると、ページ全体を再読み込みせずに地図表示が即時に切り替わります。URL フラグメントは内部的に更新され、ブラウザの履歴に残したくない場合は同一履歴エントリが置き換えられます。

### パラメータ

| パラメータ | 説明 | デフォルト値 | 例 |
|-----------|------|-------------|-----|
| `map` | MapLibre GL JS の map hash 形式 (`zoom/lat/lng/pitch/bearing`) | `14.74/32.75055/129.87255/16.2/6` | `14/35.6812/139.7671/45/0` |
| `terrain` | Terrarium タイルの場所を指定 (TileJSON を `https://tunnel.optgeo.org/martin/{terrain}` から取得) | `fusi` | `fusi`, `iwaki`, `shimabara` |
| `theme` | 地形の上に載せる地図スタイル | `osm` | `osm`, `gsi`, `gsi-ortho`, `contour`, `lineage` |
| `exag` | 地形強調度（0〜3の範囲で指定可能） | `1` | `1.5`, `2.0` |

注: `lineage` テーマを使うときは、対応する画像タイルを示す TileJSON を `https://tunnel.optgeo.org/martin/{terrain}-lineage` から取得します（例: `terrain=iwaki` と `theme=lineage` の組合せでは `https://tunnel.optgeo.org/martin/iwaki-lineage` を参照します）。

### テーマ

- **`osm`** (デフォルト): OpenStreetMap ラスタータイルを背景に表示。
- **`gsi`**: 国土地理院の基盤地図情報（bvmap）ベクトルタイルを使用。行政区画、道路、鉄道、建物など詳細な地理情報を表示。
- **`gsi-ortho`**: 国土地理院のシームレス写真（seamlessphoto）タイルを背景に表示。航空写真と標高データを組み合わせた立体表示。
- **`contour`**: Terrarium 標高タイルから `maplibre-contour` を使用して動的に等高線を生成。
- **`lineage`**: 地形に対応した Lineage 画像タイルを表示（terrain に "-lineage" を付加したタイル）。

### 使用例

```
# デフォルト（島原周辺、OSMテーマ、fusi地形、強調度1）
https://hfu.github.io/fusite2/

# 東京周辺をGSIテーマで表示
https://hfu.github.io/fusite2/#map=14/35.6812/139.7671/45/0&terrain=fusi&theme=gsi

# 東京周辺を航空写真で表示
https://hfu.github.io/fusite2/#map=14/35.6812/139.7671/45/0&terrain=fusi&theme=gsi-ortho

# いわき周辺を等高線テーマで表示
https://hfu.github.io/fusite2/#map=13/37.0/140.9/30/0&terrain=iwaki&theme=contour

# 島原を異なる地形データで表示（強調度2倍）
https://hfu.github.io/fusite2/#map=14/32.75/129.87/20/0&terrain=shimabara&theme=osm&exag=2.0
```

## コントロール

### 右上のコントロール
- **NavigationControl**: ズーム、回転、ピッチ調整
- **FullscreenControl**: フルスクリーン表示
- **GeolocateControl**: 現在地表示
- **GlobeControl**: Globe/Mercator 投影法の切替（MapLibre GL JS 標準コントロール）

### 左下のコントロール
- **TerrainExaggerationControl**: 地形強調度のスライダー（0〜3倍）

### 左上のパネル
- **情報パネル**: テーマ・地形の切り替えドロップダウン、サイト情報

## データソース

- **標高タイル**: `https://tunnel.optgeo.org/martin/{terrain}/{z}/{x}/{y}`
- **エンコーディング**: Terrarium 形式
- **タイルサイズ**: 512x512 ピクセル
- **ズーム範囲**: 0-16

## 技術スタック

- **地図ライブラリ**: [MapLibre GL JS](https://maplibre.org/) v5.7.0
- **等高線生成**: [maplibre-contour](https://www.npmjs.com/package/maplibre-contour) v0.0.5
- **タイル形式**: WebP (Lossless)
- **エンコーディング**: Terrarium (mapterhorn 互換)

## データのライセンス

測量法に基づく国土地理院長承認（使用）R 6JHs 133

国土地理院が提供する標高データを使用しています。

## 謝辞

このプロジェクトは [Mapterhorn](https://github.com/mapterhorn/mapterhorn) の手法とビジュアライゼーションに触発されました。

## ローカルでの実行

このサイトは静的 HTML/CSS/JavaScript で構成されており、ウェブサーバーで配信できます：

```bash
# Python の HTTP サーバーを使用
cd docs
python3 -m http.server 8000

# または Node.js の http-server を使用
npx http-server docs -p 8000
```

ブラウザで http://localhost:8000 を開いてください。

## GitHub Pages での公開

リポジトリの Settings → Pages から `/docs` ディレクトリを設定してください。
