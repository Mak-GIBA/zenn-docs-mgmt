---
title: "リファクタリングを補助するためのLLMアプリ (OSS) 一覧 "
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["LLM", "OSS", "github", "アプリケーション"]
published: true
---

# はじめに

近年では、LLM（大規模言語モデル）を活用することで、変数名の変更、関数の抽出、コードの整形など、プログラムのリファクタリング作業を自動化できるようになってきました。ここでは、以下の条件を満たす**Python中心の軽量～中規模のオープンソースアプリケーション**を調べました：

- **LLMによるリファクタリング支援**
- **バックエンドがDjangoまたはFastAPI（もしくはPythonベース）**
- **UIがある（WebまたはIDE統合）**
- **コードがGitHubなどで公開されている**

# LLMによるリファクタリングツール (OSS)
## Refact.ai（smallcloudai/refact）

Refactは、リファクタリングやコード補完など、AIによる開発支援ができる**自己ホスト型のオープンソースアシスタント**です。

- **使用LLM：** 複数のモデルに対応可能。OpenAI GPT-4やAnthropic Claude、Code Llama（GPT-4oやo3-miniなど）など、**外部APIやローカルモデルどちらも対応可能**です。
- **バックエンド：** Pythonベースの自己ホスト型サーバー（ポート8008でローカル実行）。Django/FastAPIの明記はありませんが、軽量なWebサーバーやFastAPIベースの可能性があります。GitやDockerと連携してコードの文脈を保持します。
- **フロントエンド：** 専用のWeb UIはなく、**VSCode拡張やJetBrainsプラグインなどIDE統合**が中心。
- **主なリファクタリング機能：** 自然言語で指示することで、**可読性向上や品質改善のためのコードリファクタリング**を実行。他にもコード補完、バグ修正、テスト生成なども可能。
- **UI：** **あり（IDE内チャットUI）**。Web UIはないが、開発者が日常的に使うIDE内で完結。
- **GitHub：** [smallcloudai/refact](https://github.com/smallcloudai/refact)

---

## Aider（paul-gauthier/aider）

Aiderは**CLI（コマンドライン）ベース**で使える軽量なAIコーディングアシスタントで、**Gitリポジトリと連携しながらリアルタイムでコード編集やリファクタリングが可能**です。

- **使用LLM：** OpenAI GPT-3.5とGPT-4をAPI経由で利用（フォーク版ではOllamaによるローカルモデル対応もあり）。
- **バックエンド：** Python製のCLIツール。Webフレームワークは使っていません。ローカルのGitとファイル構成を活用。
- **フロントエンド：** **ターミナル上でのチャットインターフェース**。
- **主なリファクタリング機能：** 自然言語で「この変数名を変えて」「この処理を関数に分離して」などと指示すると、**GPTが提案し、変更を直接コードに適用してGitにコミット**します。複数ファイルにまたがる変更にも対応。
- **UI：** **あり（CLIベース）**。WebやグラフィカルUIはないが、変更差分やコミットメッセージをターミナルに表示。
- **GitHub：** [paul-gauthier/aider](https://github.com/paul-gauthier/aider)

---

## CodeCraftGPT（pmutua/CodeCraftGPT）

CodeCraftGPTは**StreamlitベースのWebアプリ**で、コードリファクタリングやスタイル修正、テスト生成、言語変換などの機能を提供しています。

- **使用LLM：** OpenAIのGPTモデル（API経由）。内部ではLangChainでプロンプト構築や処理を統合。
- **バックエンド：** Python製で**StreamlitをWebフレームワークとして使用**。DBなどの外部ストレージは不要。
- **フロントエンド：** **StreamlitのブラウザUI**。複数タブ構成で、各機能（リファクタ、整形、テストなど）に分かれています。
- **主なリファクタリング機能：** “RefactorRite”というモジュールで、コードの改善提案や修正（構造改善、命名最適化、可読性向上など）を実行。他にも：
  - **StyleSculpt：** コーディング規約に沿ったスタイル修正
  - **TestGenius：** テストケース生成
  - **LangLink：** 異なる言語へのコード変換
- **UI：** **あり（Web UI）**。コード入力欄と出力欄があり、ブラウザで完結。
- **GitHub：** [pmutua/CodeCraftGPT](https://github.com/pmutua/CodeCraftGPT)

---

## 自動リファクタリングパイプライン（FlightVin）

このツールはやや特殊で、**コードスキャン → リファクタリング → GitHub PR作成**までを自動で行うバックグラウンド型サービスです。ユーザー操作型ではなく、**CI/CD的な使い方**が中心です。

- **使用LLM：** OpenAIのCodexやGPT-4などの使用を想定。
- **バックエンド：** Pythonスクリプト。定期実行（cronなど）でリファクタリング処理を実行。
- **フロントエンド：** **なし（自動化処理のみ）**。出力はGitHubのPull Requestとして通知。
- **主なリファクタリング機能：**
  - コードスキャンで**コードスメル（長すぎる関数、深いネストなど）を検出**
  - LLMを用いて構造を改善（関数分離、命名改善など）
  - GitHubに**自動でPull Requestを作成**。どんなリファクタリングを行ったかの説明付き
- **UI：** **なし（GitHub PR経由で確認）**。
- **GitHub：** [FlightVin/automated-refactoring](https://github.com/FlightVin/automated-refactoring)

---

## QuantaAgent（Clay-Ferguson/quantizr）

**QuantaAgent**は、LangChainベースのAIコーディングエージェントで、**Streamlit製のWebチャットUI**を通じて自然言語でのリファクタリング指示を受け取り、プロジェクト全体のコードに対して自動的に変更を適用することができます。

- **使用LLM：** OpenAIのGPTモデル（API経由）。LangChainのエージェントツールを活用して対話・コード変更を実施。
- **バックエンド：** Python製でLangChainを使用。ファイル編集はLangChainの`agent`と`tools`を使って動的に処理。
- **フロントエンド：** **StreamlitベースのWebチャットUI**。簡易的な入力欄と応答表示、変更ログの表示機能付き。
- **主なリファクタリング機能：**
  - 指定ディレクトリ内のプロジェクトコードを**直接読み取り・書き換え**可能
  - 自然言語で「関数を整理して」「全てのAPIパスをv2に変更して」などの**高レベルな指示に対応**
  - ファイルの新規生成や復元、複数ファイルへの変更も自動実行
- **UI：** **あり（WebチャットUI）**。会話形式でリファクタリング指示を行い、実行ログが都度表示される。
- **GitHub：** [Clay-Ferguson/quantizr](https://github.com/Clay-Ferguson/quantizr)

---

## OpenHands（All-Hands-AI/OpenHands）

**OpenHands**は、自律的に動作するAIソフトウェアエージェントプラットフォームで、**コードリファクタリングやプロジェクト生成、ドキュメント整備**などを対話形式で実行可能な**VSCode風のWeb UI**を備えています。

- **使用LLM：** Anthropic Claude 3.5を推奨（その他OpenAIやローカルモデルも切り替え可能）。LangChainを用いてマルチエージェント構成。
- **バックエンド：** Python製。LangGraphで構築された自律型エージェントがコード編集、検索、ブラウジングなどの役割を分担。
- **フロントエンド：** **React＋TypeScript製のWeb UI**。マルチペイン構成（チャット、コードエディタ、ブラウザ、ターミナル）で、VSCode風の使い勝手。
- **主なリファクタリング機能：**
  - 指定リポジトリを対象に「このコードを整理して」などの自然言語指示で**コード全体の構造改善や命名最適化**を実行
  - コードの**自動修正→動作確認→必要なら調査→修正の繰り返し**まで一括で自動化
  - プロトタイプ生成やコードコメント挿入、設計リファクタまで対応
- **UI：** **あり（高機能なWebチャット＋エディタUI）**。エディタにリアルタイムでコード修正が反映され、履歴管理や手動編集も可能。
- **GitHub：** [All-Hands-AI/OpenHands](https://github.com/All-Hands-AI/OpenHands)

---

## LangChain Coder AI（haseeb-heaven/langchain-coder）

**LangChain Coder AI**は、LLMを活用して**コードの生成、修正、スタイル改善、リファクタリング提案**を行う**Streamlit製のWebアプリケーション**です。エディタとコード実行環境を統合したインタラクティブな開発UIが特徴です。

- **使用LLM：** OpenAI（GPT-3.5/4）およびGoogle Vertex AI（PaLM/Codey/Gemini）をLangChain経由で選択・連携可能。
- **バックエンド：** Python製。LangChainを使って各種LLMと接続。ローカル実行可能なコード生成・実行環境を備える。
- **フロントエンド：** **StreamlitベースのWebアプリUI**。コードエディタとテキストプロンプト欄、コード実行ボタン付き。
- **主なリファクタリング機能：**
  - 「この関数を最適化して」「モジュール分割して」「エラーハンドリングを追加して」など自然言語による**スタイル・構造改善**
  - 入力したコードに対して**修正提案・リファクタ済みコードの生成**
  - コードの実行機能付きで、**リファクタ後の動作確認が即可能**
- **UI：** **あり（エディタ＋実行機能付きWeb UI）**。チャット形式ではなく、入力フィールド＋コード出力欄で構成される。
- **GitHub：** [haseeb-heaven/langchain-coder](https://github.com/haseeb-heaven/langchain-coder)

---



## まとめ比較表

| **プロジェクト名** | **LLMの種類** | **バックエンド構成** | **UI/インターフェース** | **主なリファクタリング機能** | **UIの有無** |
|-------------------|----------------|------------------------|----------------------------|-----------------------------|-------------|
| **Refact.ai**<br>[smallcloudai/refact](https://github.com/smallcloudai/refact) | GPT-4 / Claude / Code Llama（選択可） | Python自己ホストサーバー（Git/Docker連携） | **IDE統合（VSCode, JetBrains）** | 可読性・品質向上のためのコード改善提案と適用 | **あり（IDE内）** |
| **Aider**<br>[paul-gauthier/aider](https://github.com/paul-gauthier/aider) | OpenAI GPT-3.5 / 4 | Python製CLIツール（Git連携） | **CLIチャット** | 自然言語での指示によるコード編集＆Gitコミット | **あり（CLI）** |
| **CodeCraftGPT**<br>[pmutua/CodeCraftGPT](https://github.com/pmutua/CodeCraftGPT) | OpenAI GPT（LangChain経由） | Streamlit（Python） | **ブラウザUI（Streamlit）** | コード改善提案、スタイル修正、テスト生成、コード変換 | **あり（Web）** |
| **FlightVin**<br>[automated-refactoring](https://github.com/FlightVin/automated-refactoring) | GPTベース（Codexなど） | Pythonスクリプト（定期実行） | **なし（GitHub PR）** | 自動的にコードスメル検出 → PR作成による修正提案 | **なし（PR経由）** |
| **QuantaAgent**<br>[Clay-Ferguson/quantizr](https://github.com/Clay-Ferguson/quantizr) | OpenAI GPT（LangChain経由） | Python（LangChainエージェント） | **WebチャットUI（Streamlit）** | ディレクトリ内のコードを直接編集。自然言語で指示→自動変更・生成・復元が可能 | **あり（Web）** |
| **OpenHands**<br>[All-Hands-AI/OpenHands](https://github.com/All-Hands-AI/OpenHands) | Claude 3.5 / OpenAI / ローカルLLM（選択可） | Python（LangGraph構成のエージェント） | **VSCode風Web UI（React/TS）** | チャットでコードリファクタ、複数ファイル編集、検索・実行も自動化 | **あり（Web）** |
| **LangChain Coder AI**<br>[haseeb-heaven/langchain-coder](https://github.com/haseeb-heaven/langchain-coder) | OpenAI GPT-3.5 / 4, Google Codey（LangChain経由） | Python＋LangChain（Streamlit上） | **コードエディタ＋実行可能なWeb UI** | コード生成、修正、スタイル改善、最適化提案（即時実行も可能） | **あり（Web）** |

