---
title: "[論文要約]NeurIPS2024で気になった物体検出分野の研究まとめ"
emoji: "🙆"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: 
  - "deep learning"
  - "深層学習"
  - "NeurIPS"
  - "物体検出"
  - "Object Detection"
  - "論文"
published: true
---

NeurIPS2024の発表の中で気になった物体検出分野の研究をピックアップ。勉強がてらそれぞれの研究の概要を以下に纏めた。

# [Adaptive Important Region Selection with Reinforced Hierarchical Search for Dense Object Detection](https://neurips.cc/virtual/2024/poster/94227)
- dense object detectionは様々なドメインに適用されている一方で非常に難しいタスクである。アンカーベースの1ステージ型物体検出器は，複雑かつノイズの多い背景を持つ場合に，多様なタイプの候補アンカーを捉えられず，多くの偽陽性アンカーが生じてしまう。
- 本研究では，独自に設計された報酬関数を用いてEvidential Q-learningを行うエージェントにより適応重要領域選択(AIRS:Adaptive Important Region Selection)を実施することを提案する。本手法は物体があるかどうかはっきりしない不確実性が高い領域を積極的に探索するように設計されている，かつむだな探索を減らすような仕組みになっているため高速に収束させつつ精度向上をさせられる。

![](/images/neurips2024-detection/image.png)

- FPNの各スケール(P5が最も低い解像度でP3が最も高い解像度)のパッチからRLエージェントが有効なパッチを選択，それが特徴抽出器を通過し，次にRNNを通じて状態表現が生成される。
- 状態表現はEvidential Q-networkを通過してEvidential Normal-Inverse Gaussian(NIG)を形成した後可能なアクションのQ値を推定する。NIGはQ値に加えてその認識不確実性もモデル化する
- 推定Q値に対応する認識不確実性を組み合わせることによって不確実正が高いパッチを優先的に探索しつつトレーニングが進につれて不確実性を減らし，価値の高いアクションに収束させていくことができる。

# [Progressive Exploration-Conformal Learning for Sparsely Annotated Object Detection in Aerial Images](https://neurips.cc/virtual/2024/poster/95684)

![](/images/neurips2024-detection/image-01.png)

- 大量のデータにアノテーションするのは高コストなため，限られたアノテーション付きサンプルと大量の未アノテーションサンプルを利用して性能を向上させる半教師あり物体検出(SSOD:semi-superbised object detection)が提案されている。SSODは一般画像に商店を当てており，航空画像における独自の画像特性を考慮することができない(上図に示すように一般画像と比べて航空画像は物体が密で類似している)。
- 本研究では，航空画像におけるスパースにラベル付けされた物体検出タスク(SAOD:sparsely annotated object detection)に焦点を当てる。つまり，ごく一部の物体しかアノテーションされていない画像からうまく物体検出するタスク。
- 従来手法だと疑似ラベルを用いた手法が一般的で，固定の閾値により疑似ラベルを付ける対象をフィルタリングする。しかし，この方法だと大きくて目立つ物体はモデルの推定確率が高くなりやすい一方，小さい物体は推定確率が低くなりがちである。つまり，小さい閾値を設定すると小さい物体の疑似ラベルがとりこぼされる（逆閾値を低くすると誤検出した物体も大量に疑似ラベルとして採用されてしまうリスクが高まる）。
航空画像だと飛行機のような特徴が明瞭な画像だと検出スコアが高いが，小型の車両だとスコアが低くなりがち。そのため，大きい物体と小さい物体で信頼度が異なるなどのクラス毎の不均衡を考慮しながら疑似ラベルを適切に探索・活用できるPECL(Progressive Exploration-Conformable Learning)を提案。

![](/images/neurips2024-detection/image-1.png)
- PECL(Progressive Exploration-Conformable Learning)フレームワークは以下を口語に繰り返す閉ループ型の反復学習を行う。
  - Conformal Pseud-label Exploration (疑似ラベル生成・洗練)
  - Detector Updating (検出器の更新)
- 具体的には以下のような処理を繰り返す
  1. 事前学習済検出器(pre-trained aerial detector)の準備: 訓練データに含まれるスパースにラベル付けされた物体だけを利用して航空物体検出器$\Theta$を初期モデルとして学習する。
  2. クラス別プロトタイプ(class-wise prototypes)の構築: 正解ラベルが付いている物体の全結合層の特徴ベクトルを取得し(全結合層を通してベクトルを取得するって言っている)，それをクラス単位でまとめる (これらプロトタイプは疑似ラベルを生成/評価する際に各クラスがどのような特徴を持つかを判断する指標として使われる)。
  3. 各入力画像に対する物体候補セットの取得: 入力画像に対してモデル(1で学習した事前学習済検出器)が検出した候補ボックスを集めた候補セットを用意する。この候補野中には実際の正例もあれば誤検出もある。
  4. **Conformal Pseudo-label Exploration (CPE) の適用**: 候補設定に対してクラス別のプロトタイプ情報などを活用しながらカテゴリ毎の特性を考慮して適切に高品質な疑似ラベルを選び出す
  5. **Pseudo-labelとともにDetectorを更新 (Detector Updating)**: 上記で得た疑似ラベルを新たな教師データの一部として利用し，モデルを再学習する。

# [YOLOv10: Real-Time End-to-End Object Detection](https://neurips.cc/virtual/2024/poster/93301)
- リアルタイム物体検出タスクは低レイテンシで動くことが重要で様々な実アプリで使われている。
- YOLOシリーズは精度と速度のバランスが良いため，人気が高いが，YOLOは以下2つの構成により物体検出するため(NMSを活用するため)「処理時間がかかる」「ハイパーパラメータ次第で精度がブレる」「完全なend-to-end推論ではない (NMSが別工程)」などの問題が出る。DETRやCNNベースのone-to-one検出によりNMSを不要とするアプローチが出てきているが推論時の計算コスト増加や精度低下の面で課題が残る。
  1. Forward: 入力画像から予測を行う。1つの物体に複数の予測を対応させるone-to-many label assignment。
  2. NMS: 重複予測を間引いて最良のものを選択。

- また，YOLOの精度と速度に最も大きな影響を与えるモデル構造設計に関しても完全に最適化されておらず，課題が残る。具体的にはBackbone(特徴抽出)→Neck(マルチスケール特徴を統合する部分)→モデルスケーリング/再パラメータ化の構造で物体検出するが，これに無駄がある。
- 本研究では，精度と速度をより向上させるために「NMSの排除」「モデル構造の抜本的見直し」という2つの観点から改善を図った。

![](/images/neurips2024-detection/image-02.png)

- NMSを不要とするためのDual Assignment Strategyを提案。本手法は，one-to-many assignmentの利点(多くの監視信号が得られるためモデルがより安定して高性能に学習される)とone-to-one matchingの利点(各物体に1つの予測のみを割り当てるため推論時にNMSが不要となり，冗長な予測を抑えられる)を活かすものである。
- 本手法の実装構造は「one-to-many head (従来のYOLOのまま)」「one-to-one head (新しく追加)」の2つのheadを持ち，学習時は両方のheadを同時に最適化 (joint training) する。これによりBackboneとNeck (PAN) はone-to-manyの豊富な信号を活かすことができ，学習の安定性・精度が向上。
- 推論時はone-to-one headのみを使用。これにより冗長な出力が存在せずNMSが不要に。one-to-oneマッチングはHungarian Matchingが主に使われるが，計算コストが高いため，各物体についてスコアが最も高い予測を1つだけ選ぶTop-1選択戦略を採用。
- 以上で説明した2つの出力headが同じmetricに基づくことで両者の学習を整合させるためにConsistent Matching Metricを提案。これは，両headが同じ予測とGTのペアを最重要と認識するようにすることで学習方向を整合させるもの。両headとも以下のスコアで予測とGTをマッチングさせる。詳細→https://zenn.dev/link/comments/a23a1f69d7d392

$$
m(\alpha, \beta) = s \cdot p^\alpha \cdot \text{IoU}(\hat{b}, b)^\beta
$$

| 項目 | 意味 |
|------|------|
| $s$ | 予測のアンカーポイントがGTの中にあるか（空間的優先） |
| $p$ | クラス分類スコア（信頼度） |
| $\hat{b}, b$ | 予測ボックスとGTボックス |
| $\alpha, \beta$ | 分類と回帰のバランスをとるハイパーパラメータ |

>Appendix:
>https://zenn.dev/giba/scraps/bd364ef8e2112e