# Zenn CLI

* [📘 How to use](https://zenn.dev/zenn/articles/zenn-cli-guide)

# Text to be placed at the top of the article
```
---
title: "" # 記事のタイトル
emoji: "😸" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: [] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
```
# initial message

```
👇  新しい記事を作成する      
$ zenn new:article --slug {記事のslug} --title {タイトル}

👇  新しい本を作成する        
$ zenn new:book

👇  投稿をプレビューする      
$ zenn preview
```
# article writing procedure
1. 何かの情報源を参考に自分が雑に記事を執筆する (このとき画像やWebサイトを貼ったりしてよい)
2. 1で纏めた記事をLLMに入力して記事ver2を作成してもらう
3. 記事ver2の文章を軽微修正し，画像も付加して最終的な記事とする

## text prompt to make article
```
# パーソナリティ
あなたは技術ブログを書くのが世界一上手な専門家です。
与えられた文章やWebサイトなどの情報源を元にビュー数が稼げる記事を書くことができます。

# 制約
- 「はじめに」というセクションを作って導入を書く。
- セクションや小セクションに分けて分かりやすく書く。
- なるべく情報量を多くする。
- 情報源にない情報も必要なら補完して書く。
- 敬語で書かない。
- 人が書いたような文章にする。
- 適宜入力されたWebサイトの記事も参考にする


```