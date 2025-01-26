---
title: "物体検出の進化 (2017年-2020年)"
emoji: "🦔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['物体検出','Computer Vision','画像処理']
published: true
---

# 物体検出手法の進化 (2017年-2020年)

## はじめに
2017年から2020年の間に、物体検出技術は大きく進化しました。特に、特徴ピラミッド構造(FPN)、アンカーフリー手法、クロスステージパーシャル(CSP)構造などが登場し、それぞれのアプローチで精度と計算効率のバランスが改善されました。本記事では、この時期の主なモデルの進化を振り返りながら、それぞれの技術の特徴を解説します。
![](![](https://storage.googleapis.com/zenn-user-upload/b4ac1f3f9c70-20250126.png))

---

## 2017年 - RetinaNetとFPN
### 特徴ピラミッド構造 (FPN)
- 物体検出モデルの精度向上のため、FPNが提案されました。
- 小物体の検出精度が大幅に向上。
- 特に、ResNet-101-FPNをbackboneに用いたRetinaNetの$AP_s$が高い結果を示しました。

### RetinaNetの登場
- RetinaNetはFPNを採用し、小物体検出精度を向上。
- YOLOv3と比較すると、全般的にRetinaNetの方が高い精度を達成。

---

## 2018年 - CornerNetとCenterNet
### Hourglassネットワーク
- 人間のポーズ推定タスクで使用されていたHourglassネットワークが、CornerNetやCenterNetのbackboneとして採用される。
- 砂時計のような構造で、アップサンプリングとダウンサンプリングを繰り返し、異なるレベルの特徴マップ情報を取り込む。
- 異なるサイズの画像に柔軟に対応可能だが、計算量が多く、推論速度やリソースに影響を与える。
- データ量が不足している場合や分布が一様でない場合には、オーバーフィットのリスクがある。

---

## 2019年 - FCOSとアンカーフリー手法
### FCOSと4種類のbackbone
- FCOSはアンカーフリーの物体検出手法。
- 4種類の異なるbackboneを使用した実験で、backboneの選択が検出精度に影響を与えることが確認された。
- **HRNetの特徴**:
  - 異なる解像度の特徴マップを計算可能。
  - 高解像度の画像特徴を保持できるため、詳細な情報を捉えやすい。
  - ただし、複数の解像度を処理するため計算量が増加。

---

## 2020年 - YOLOv4とCSPNet
### YOLOv4の改良点
- YOLOv4はCSPNetをbackboneに導入し、精度向上を実現。
- CSPNetの特徴:
  - DenseNetを改良し、計算量を10〜20%削減しながら精度を向上。
  - 入力の特徴マップをチャンネル方向に2分割し、片方のみDenseBlockのように変換、もう片方はそのまま保持。
  - ResNetにも適用可能。

### PAN (Path Aggregation Network)
- FPNの改良版としてPANを採用。
- より効果的に特徴情報を集約。

### CIoU loss
- YOLOv4ではCIoU lossを使用し、より適切なバウンディングボックスの回帰を実現。

### Bag of Specials
- 推論コストを少しだけ上げつつ、物体検出精度を大幅に向上させる技術。

---

## おわりに
2017年から2020年にかけて、物体検出技術はFPN、アンカーフリー手法、CSPNetなどの新技術によって大きく進化しました。これらの技術は、以降の物体検出モデルにも大きな影響を与えています。今後もさらなる精度向上と計算効率のバランスを追求した研究が続くことでしょう。

## 参考記事
### Hourglassの詳細
- [Stacked Hourglass Network解説](https://cvml-expertguide.net/terms/dl/human-pose-estiamtion/stacked-hourglass-network/)
- [Hourglassネットワークの解説記事](https://yusuke-ujitoko.hatenablog.com/entry/2017/07/22/000523)

### CornerNetとCenterNetの記事
- [CornerNetの解説](https://metrica-tech.hatenablog.jp/entry/2019/08/10/000000)
- [CenterNetの解説](https://metrica-tech.hatenablog.jp/entry/2019/12/07/173032)
- [CenterNetに関する詳細記事](https://metrica-tech.hatenablog.jp/entry/2019/08/17/000000)

### YOLOv4に関する詳細記事
- [YOLOv4の解説](https://qiita.com/hnishi/items/e1b84ecd4025fe4e5ccf)
- [YOLOv4の詳細](https://qiita.com/kindamu24005/items/9ad336354bbc2dfd14ce)
- [CSPNetの解説](https://nakamura-shogo.gitbook.io/dev-wiki/cv/aiml_cv_classification/cspnet)

---



