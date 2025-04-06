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

## まとめ比較表

| **プロジェクト名** | **LLMの種類** | **バックエンド構成** | **UI/インターフェース** | **主なリファクタリング機能** | **UIの有無** |
|-------------------|----------------|------------------------|----------------------------|-----------------------------|-------------|
| **Refact.ai**<br>[smallcloudai/refact] | GPT-4 / Claude / Code Llama（選択可） | Python自己ホストサーバー（Git/Docker連携） | **IDE統合（VSCode, JetBrains）** | 可読性・品質向上のためのコード改善提案と適用 | **あり（IDE内）** |
| **Aider**<br>[paul-gauthier/aider] | OpenAI GPT-3.5 / 4 | Python製CLIツール（Git連携） | **CLIチャット** | 自然言語での指示によるコード編集＆Gitコミット | **あり（CLI）** |
| **CodeCraftGPT**<br>[pmutua/CodeCraftGPT] | OpenAI GPT（LangChain経由） | Streamlit（Python） | **ブラウザUI（Streamlit）** | コード改善提案、スタイル修正、テスト生成、コード変換 | **あり（Web）** |
| **FlightVin**<br>[automated-refactoring] | GPTベース（Codexなど） | Pythonスクリプト（定期実行） | **なし（GitHub PR）** | 自動的にコードスメル検出 → PR作成による修正提案 | **なし（PR経由）** |

