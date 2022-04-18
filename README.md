# OSM Address Normalize

OpenStreetMap のためのマッピング支援ツール

## Abstract

住所正規化と大まかな緯度経度を計算できるライブラリ[@geolonia/normalize-japanese-addresses](https://github.com/geolonia/normalize-japanese-addresses)を活用し、
住所のリストを GeoJSON に変換します。

生成された GeoJSON には、正規化され都道府県、市区町村、町丁目、番地、…に分割された住所が格納されており、これらは OpenStreetMap のタグ`addr:*`に対応しています。
生成された GeoJSON を[JOSM](https://josm.openstreetmap.de/)で読み込めば、タグ付け済みのポイントが表示されます。
なお、出力されるポイントデータの緯度経度は大まかな値ですので、そのままアップロードすることは推奨されません。
自動生成されるタグ情報にも誤りがある可能性がありますので、JOSM 上で適宜修正の上ご利用ください。

本ライブラリはあくまでマッピングのための補助ツールとして利用されることを想定しており、**マッピングの自動化を目的としたものではない**ことをおことわりいたします。

OpenStreetMap プロジェクトの Wiki には自動編集への考え方や外部データをインポートする際の注意点がまとめられています。こちらもご参照ください。

- https://wiki.openstreetmap.org/wiki/JA:Automated_Edits_code_of_conduct
- https://wiki.openstreetmap.org/wiki/JA:%E3%82%A4%E3%83%B3%E3%83%9D%E3%83%BC%E3%83%88/%E3%82%AC%E3%82%A4%E3%83%89%E3%83%A9%E3%82%A4%E3%83%B3

## Usage

```javascript
const fs = require("fs/promises");
const { addresses2geojson } = require("osm-address-normalize/dist/lib");

const addressList = [
	"東京都千代田区霞が関２丁目１−３",
	"京都府京都市上京区薮之内町１",
	"広島県広島市中区基町５−２５",
];
addresses2geojson(addressList).then((geojson) => {
	fs.writeFile("./output.geojson", JSON.stringify(geojson, null, "\t"));
});
```

```javascript
// output.geojson
{
	"type": "FeatureCollection",
	"features": [
		{
			"type": "Feature",
			"properties": {
				"addr:country": "JP",
				"addr:province": "東京都",
				"addr:suburb": "千代田区",
				"addr:quarter": "霞が関",
				"addr:neighbourhood": "二丁目",
				"addr:block_number": "1",
				"addr:housenumber": "3"
			},
			"geometry": {
				"type": "Point",
				"coordinates": [
					139.750413,
					35.675551
				]
			}
		},
		{
			"type": "Feature",
			"properties": {
				"addr:country": "JP",
				"addr:province": "京都府",
				"addr:city": "京都市",
				"addr:suburb": "上京区",
				"addr:quarter": "藪之内町",
				"addr:block_number": "1"
			},
			"geometry": {
				"type": "Point",
				"coordinates": [
					135.755632,
					35.02141
				]
			}
		},
		{
			"type": "Feature",
			"properties": {
				"addr:country": "JP",
				"addr:province": "広島県",
				"addr:city": "広島市",
				"addr:suburb": "中区",
				"addr:quarter": "基町",
				"addr:block_number": "5",
				"addr:housenumber": "25"
			},
			"geometry": {
				"type": "Point",
				"coordinates": [
					132.456506,
					34.401095
				]
			}
		}
	]
}
```

## Develop

```bash
git clone git@github.com:YUUKIToriyama/osm-address-normalize.git
cd osm-address-normalize
yarn install
```
