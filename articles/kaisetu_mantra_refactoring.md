---
title: "【論文解説】リファクタリング精度を爆上げするマルチエージェントシステム"
emoji: "🦁"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["LLM", "machine learning", "GPT", "リファクタリング"]
published: true
---

Title: **MANTRA: Enhancing Automated Method-Level Refactoring with Contextual RAG and Multi-Agent LLM Collaboration**
Link: http://arxiv.org/abs/2503.14340
Code: https://anonymous.4open.science/r/MUARF-B49B/code/rag/contextual_rag_process.py

# Intro / Conclusion
- 大規模システムではコードのリファクタリングが必須だが，既存コード解析やテストに多大な手間がかかる。LLMベースの手法もあるが，プロンプトベースのリファクタリングに依存しているために以下課題がある。
  - 適用範囲が狭い(関数の呼び出しなどの対象メソッド周辺の文脈が考慮できない)
  - リファクタリング後にテスト実行により検証する仕組みがない(生成→検証→再生成のループに留まっている)
  - 対象とするリファクタリングの種類が限定的
  - LLMが持つ自己反省や自己改善の能力を十分に活用できていない
- また，LLMを用いない既存手法だと「プロジェクト固有の命名規則やライブラリの使い方やドメインロジックを考慮できない」「開発者の感覚値に基づく読みやすいコードと異なるスタイルになりやすい」
- 本論文ではメソッドレベルのリファクタリングを自動化するend-to-endのLLMエージェント手法MANTRA (Multi-AgeNT Code RefActoring) を提案。
- MANTRAを従来のリファクタリング研究で用いられてきた10個のJavaプロジェクトを使って評価。その結果，コンパイル可能且つテストに合格するリファクタリングコードを生成する成功率がMANTRAは82.8%に対してRawGPTは8.7%。また，EM-Assistに対しても50%の性能向上を示した。
ユーザー調査においてもMANTRAと人手によるリファクタリングコードそれぞれの可読性と再利用性のスコアがほぼ同等であることが分かった。
>コミットの中にはバグ修正/機能追加といった無関係のコード変更を伴うためPurityCheckerによりコミットをフィルタリングしている。

# Method
- 提案手法のMANTRAは主に以下3つのモジュールから成る。
  1. Context-Aware Retrieval Augmented Refactored Code Generation: 過去のリファクタリングをfew-shot exampleとして含む検索可能なデータベースを構築し，構築したデータベース×RAGによりfew-shot exampleをプロンプトに含める。
  2. Multi-Agent Refactored Code Generation: マルチエージェントフレームワークによりリファクタリングコードを生成。
  3. Self-Repair Using Verbal Reinforcement Learning: 言語強化学習フレームワークを実装し，生成コードの問題を自動敵に検出・修正。

## 1. Context-Aware Retrieval Augmented Refactored Code Generation
![alt text](/images/kaisetu_mantra_refactoring/image.png)
- コードをRAGする上での課題は以下。
  - 同じようなクラス構成や同じような振る舞いをする関数を持っていても，テスト用か本番用かなどクラスが担う役割が異なる場合があり，その場合の適切なモジュールをRetrievalできなくなる。つまり見た目は似ているが目的が違う例を拾ってしまう。
  - 当該コードがどのリソースに依存しているかを考慮できないため，処理内で使われるフィールドや他メソッドへの参照がどこからきているか分からないと引数設計/戻り値設計を誤り，コンパイルエラーを生む可能性が高まる。
- MANTRAではRAGの検索能力を高めるために以下を行い，コンテキスト付きデータベースを構築する。
  - データベース中のコードに自然言語による説明を加える。
  - プロンプトデータベース中のコードに呼び出し元と呼び出し先のコードの情報を加える。
- コンテキスト付きデータベース構築のために用いるプロンプトは以下。
```
{Code}{Caller/Callee}{Class Info} 　Please give a short, succinct description to situate this code within the context to improve search retrieval of the code.
```

- RAGではsparse retrievalとdense retrievalという2つの検索手法を組み合わせて融合することが一般的である。以上のランキング結果をRRFというアルゴリズムを用いて統合して最終的なFew-shot参照例を選定。
>https://chatgpt.com/share/6825963e-a77c-8009-852b-2dc3d3e34165

## 2. Multi-Agent Refactored Code Generation
![alt text](/images/kaisetu_mantra_refactoring/image-1.png)
- MANTRAは上図に示すようにDeveloper AgentとReview AgentとRepair Agentのマルチエージェントが強調しながらリファクタリングを行う。MANTRAは[mixture-of-agents architecture](https://chatgpt.com/share/68259a24-b114-8009-9c3d-2f4386c5ae2c)を採用している。
  - Developer: コードを生成および改善する。
  - Reviewer: 生成コードをレビューしてフィードバックor提案をDeveloperに返す。
  - Repair: リファクタリングしたコードがコンパイル失敗/テストが通らない場合は，LLMに基づく推論とverbal RLによりコンパイルエラーやテスト失敗の修復を試みる。
### Developer Agent
- Dev-Agent-1: 静的コード解析によるリポジトリおよびソースコード構造抽出
  - 与えられたリファクタリング種別と対象コードに基づき実行すべき解析を自律的に選択する。
  - move methodリファクタリングの場合，get_project_structureを呼び出してプロジェクト全体の構造を取得する。次にget_file_contentを実行して該当ディレクトリからソースコードを取得してメソッドを移動すべきターゲットのクラス/ファイルを評価。解析結果を次のDev-Agentに送る。
- Dev-Agent-2: RAGとchain-of-thoughtによる自動リファクタリングコード生成
  - 前段の解析結果を受けっとった後，①RAGを用いて類似リファクタリングをfew-shot例として取得し，②chain-of-thought推論によってリファクタリング後のコードを生成する。
  - プロンプト例は以下。
  ```
    ### タスク: 指定されたリファクタリングタイプに基づくコードリファクタリング

    #### 手順: 以下のステップバイステップ解析に従ってください

    **ステップ 1: コード解析**
    リファクタリング対象の特定コード片を解析し，リファクタリングすべきコードの簡潔な概要を出力する。

    **ステップ 2: リファクタリング手法の参照**
    RAG システムから最大 3 件の類似リファクタリング例を検索・取得する。

    **ステップ 3: 構造情報の抽出**
    リファクタリングタイプとコード概要に基づき，提供されたツールを使って必要な構造情報を収集する。これにはコード構造情報やプロジェクト構造情報が含まれる。

    **ステップ 4: リファクタリングの実行**
    抽出した構造情報と取得した例を用いてリファクタリング後のコードを生成する。

    ---

    ### 入力: ‘{リファクタリング対象コード, リファクタリングタイプ}’

    ### 応答: ‘{リファクタリング後コード}’
  ```   
  - 指定されたリファクタリングタイプに従っていくつかのリファクタリング候補一覧を生成する。生成した候補一覧から妥当なリファクタリングを選択。

### Reviewer Agent
- Developerが生成したコードを[RefactoringMiner](https://github.com/tsantalis/RefactoringMiner)を用いてコードに指定されたリファクタリング種別が正しく適用されているかを検証。チェックに失敗した場合は検証結果を含むフィードバックを生成した後，Developerに返して修正を促す。
- 検証に合格した場合は[CheckStyle](https://checkstyle.org/)を用いたコードスタイルの一貫性解析を実施。
- リファクタリング検証とコードスタイルチェックの両方を通過したらコードをコンパイルしてテストを実行。失敗がなく全てのテストに合格すれば生成プロセス完了。もし失敗が発生した場合は生成コードとエラーログを次のフェーズに転送。

### Communication between the Developer and Reviewer Agents
- DeveloperとReviewerが反復的に協働し，人間のチーム開発を模倣する。全ての要件を満たすまで上記のサイクルを繰り返す。
- コンパイルエラーやテスト失敗になった場合は問題をRepair Agentにエスカレーションする。

## 3. Self-Repair Using Verbal Reinforcement Learning
- Repair Agentはコンパイルやテストに失敗したコードを[verbal reinforcement learning](https://chatgpt.com/share/6825d5fc-36e4-8009-9ff9-fd87fb8dedac)を活用して反復的に修復する。具体的には[Reflexion](https://chatgpt.com/share/6825d82f-ac6c-8009-b5ce-e78142e5e833)フレームワークを適応。
- ①初期解析でリファクタリングのコードとそのコードを含むクラス全体，および関連するエラーログ調査をして修正パッチを適用後に再コンパイル/再テストを実施→②問題がある場合はコンパイルとテスト結果を批判的に自己レビューする→③自己内省の結果を受けて修正方針を生成→④該方針に基づくパッチを適用してコンパイルとテスト実行。
※テストを通過するかあらかじめ定めた最大修復試行回数に達するまで実施することになる。

# Evaluation
## Dataset
![alt text](/images/kaisetu_mantra_refactoring/image-3.png)
- 上表に示す10件のJavaプロジェクトが評価対象となる。これら10件のプロジェクトは先行研究で変更履歴追跡目的で使用されたものである。
- 以下3つの観点から本プロジェクトを選定した。
  - アプリケーション領域が多彩で実践的なソフトウェア開発が行われている。
  - プロジェクトのコミット履歴が2000件と十分に多く，リファクタリングを含むコミットを検出しやすい。
  - リファクタリング品質を検証するために手動でコンパイル問題を解決できる＆テストを正常に実行できる。
- バグ修正などの無関係な変更によるノイズを除くため，[PurityChecker](https://github.com/pedramnoori/PurityChecker)を使用して純粋リファクタリングを含むコミットのみを選別した。書く純粋リファクタリングコミットおよびその親コミットをコンパイルしてコンパイルとテストが両方とも成功するコミットだけを評価対象として採用した。

## Metrics
- リファクタリング後のコードは**機能的正当性**と**人間らしさ**の2つの観点で評価する。
- 機能的正当性は以下3項目で評価。
  1. コンパイル成功
  2. テスト通過
  3. RefactoringMiner検証
- 人間らしさは以下の指標で測定する。
  - CodeBLEU指標: 人間が書いたリファクタリング後のコードとMANTRAが生成したコードとの文法的・意味的な一貫性を評価する指標。
  - AST Diffの適合率 (Precision): MANTRAが生成したリファクタリング後コードのマッピングと開発者コードの間ピングを比較した際の一致度合いのPrecision
  - AST Diffの再現率 (Recall): MANTRAが生成したリファクタリング後コードのマッピングと開発者コードの間ピングを比較した際の一致度合いのRecall
  
  ※AST Diffは元コードとリファクタリング後のコードの差分を表すマッピング集合。
  ※CodeBLEU と AST Precision／Recall の値は 0 〜 1 の範囲を取り、1 は完全一致を表す。