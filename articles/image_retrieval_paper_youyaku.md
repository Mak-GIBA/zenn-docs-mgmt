---
title: "[論文要約]画像検索分野の研究動向 (2020年-2024年)"
emoji: "📌"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["画像検索", "深層学習", "image retrieval", "computer vision"]
published: false
---
# はじめに
画像検索に関する論文を漁ったので，それら論文の概要を以下に纏めます。

# [Unifying Deep Local and Global Features for  Image Search](https://www.bing.com/ck/a?!&&p=a15e6de4f298f5f064ae8b4cedd15cd9e56747344a730f3dd39e06d45b2b5d8dJmltdHM9MTc0NDQxNjAwMA&ptn=3&ver=2&hsh=4&fclid=36f718b3-72b8-6b12-316c-0d8d737c6a48&psq=Unifying+Deep+Local+and+Global+Features+for++Image+Search&u=a1aHR0cHM6Ly9hcnhpdi5vcmcvYWJzLzIwMDEuMDUwMjc&ntb=1)，ECCV2020
- 画像検索の性能を左右するのは画像をどう特徴ベクトルとして数値化するかという点。この表現には主に2種類ある。
  - グローバル特徴 (Global Features): 画像全体を纏めて1つの特徴ベクトルに圧縮したものであり，画像全体の雰囲気を掴む。高速だが，局所構造を失うため細かなマッチングには弱い。Recallに強い (見逃しが少ない) 。
  - ローカル特徴 (Local Features): 複数の画像領域を小さな特徴ベクトル(＋その位置)に圧縮したもの。精密な位置関係や剛体物体に強いが，処理が重くなりがち。Precisionに強い (間違えにくい)。
- 本論文の貢献は以下。
  - グローバル特徴とローカル特徴を1つのCNNで抽出可能とした。具体的にはGeMプーリングでグローバル特徴を効果的に集約し，Attentive Local Feature Detectionで重要な領域を重点的に検出してローカル特徴を抽出。
  - AutoEncoderをCNNに統合することで低次元のローカル特徴を直接学習可能とした (従来，ローカル特徴は高次元で表現されており，後処理でPCAなどが必要となることが多かった)。これにより高速にend-to-endな学習が可能となった。
  - 画像単位のラベル (シンプルなクラスラベル) だけでend-to-endの学習を実現。
  - 有名なベンチマーク(Revisited Oxford, Revisited Paris, Google Landmarks v2)でSOTA達成。

![alt text](image.png)
- グローバル特徴は同じ対象の画像であれば全体的に「似たベクトル」になるように学習される一方で，ローカル特徴は画像内の局所領域ごとの一致を高精度で判断できるように学習される (グローバル特徴は抽象/意味重視，ローカル特徴は具体/空間重視)。そのため，CNNでこれら異なる性質を同時に学習するのは難しい。
- 本研究で提案するDELG (DEep Local and Global features) は従来の複雑な多段階学習を排除し，シンプルにend-to-endな学習を実現。
- CNNに入力画像を通すと空間的な局所特徴を保持した浅い層の特徴と抽象的で意味的な表現を持つ深い層の特徴の2種類の特徴マップが得られる (深い層ほど空間サイズが小さくチャンネル数が多くなる)。
- 以下式に示す[GeMプーリング](https://paperswithcode.com/method/generalized-mean-pooling)で得たベクトルに全結合層 (全結合層＋バイアス) を通すことで次元変換を行いグローバル特徴を抽出する。

$$
 g = F \cdot \left( \frac{1}{H_D W_D} \sum_{h,w} d_{h,w}^p \right)^{1/p} + b_F
$$

- ローカル特徴は意味のある領域を選択しつつ，低次元化の必要がある (ローカル特徴は数百~数千個に及ぶため高次元だと重すぎる) 。
  - 重要領域の選択: 浅い層から得られる特徴マップを入力としてAttentionによりどの位置が重要かを表すスコアマップを生成。($S \in \mathbb{R}^{H_S \times W_S \times C_S}$ → $A \in \mathbb{R}^{H_S \times W_S}$)
  - AutoEncoderを用いて浅い特徴から低次元の表現を学習。これによりメモリや計算コストを削減。ここで，圧縮後の特徴は表現力を高めるために正負の符号付き。
- 最終的には各位置において，「低次元表現に変換した特徴ベクトル」「Attentionにより得られた信頼度スコア」「位置情報」

![alt text](image-1.png)

- 