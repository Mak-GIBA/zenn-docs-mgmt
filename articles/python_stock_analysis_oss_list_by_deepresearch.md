---
title: "株式・暗号資産向けファンダメンタル／テクニカル分析OSS一覧"
emoji: "💹"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["python", "oss", "株", "金融", "暗号資産", "バックテスト"]
published: true
---


以下に、株式および暗号資産を対象に**ファンダメンタル分析**または**テクニカル分析**が可能で、**バックテスト機能**を備えた主要なオープンソース・ソフトウェア（OSS）をリストアップしました。
まだどれも試せていないので使った方は使用感をコメント欄で教えてくださると嬉しいです。

## Backtrader（Python）

- **対応言語**: Python  
- **主な機能**: 豊富なテクニカル指標と分析機能を内蔵し、過去データを用いた**バックテスト**やライブ取引に対応。再利用可能なストラテジーやインジケーターの作成に集中できる設計 ([Welcome - Backtrader](https://www.backtrader.com/#:~:text=Welcome%20to%20backtrader%21))（ファンダメンタル分析機能は特になし）。  
- **対象資産**: **株式**、暗号資産ほか（CSVやAPI経由で任意の市場データを投入可能）  
- **特徴・強み**: 機能が充実したイベント駆動型のバックテスト＆トレーディングフレームワーク。コミュニティも活発で、暗号資産取引所との連携（CCXT経由）にも対応する拡張が存在します。  
- **リンク**: [GitHub:mementum/backtrader](https://github.com/mementum/backtrader)

## Zipline（Python）

- **対応言語**: Python  
- **主な機能**: **イベント駆動型**のアルゴリズムトレーディングシミュレータで、過去データでの戦略**バックテスト**およびライブトレードに対応 ([Zipline — Zipline 3.0 docs](https://zipline.ml4trading.io/#:~:text=Backtest%20your%20Trading%20Strategies))。移動平均などの基本的なテクニカル指標や統計分析をアルゴリズム内で利用可能 ([Zipline — Zipline 3.0 docs](https://zipline.ml4trading.io/#:~:text=,below%20for%20a%20code%20example))。  
- **対象資産**: **株式**（Quantopianでは米国株中心）。暗号資産は公式には未対応ですが、Enigma CatalystなどZiplineを拡張したプロジェクトで対応例あり。  
- **特徴・強み**: Quantopian社によって開発された実績があり、Pandasデータフレームによる入出力や統計・機械学習ライブラリ（matplotlib, SciPy, StatsModels, scikit-learn等）との統合が容易 ([Zipline — Zipline 3.0 docs](https://zipline.ml4trading.io/#:~:text=,written%20algorithm))。アルゴリズムの開発に専念できる「電池付き（batteries included）」の設計が特徴です。  
- **リンク**: [GitHub: quantopian/zipline](https://github.com/quantopian/zipline)

## Backtesting.py（Python）

- **対応言語**: Python  
- **主な機能**: 軽量で高速な**バックテスト**フレームワーク。PandasやNumPy上に構築されており、ローソク足形式の歴史データがあれば**株式・暗号資産・為替**など様々な資産で戦略検証が可能 ([Backtesting.py - Backtest trading strategies in Python](https://kernc.github.io/backtesting.py/#:~:text=))。テクニカル指標ライブラリ（TA-Libやpandas-taなど）と組み合わせた分析、パラメータ最適化、対話的な可視化機能を備えています ([Backtesting.py - Backtest trading strategies in Python](https://kernc.github.io/backtesting.py/#:~:text=Technical%20indicator%20library%20agnostic)) ([Backtesting.py - Backtest trading strategies in Python](https://kernc.github.io/backtesting.py/#:~:text=))。  
- **対象資産**: **株式、暗号資産、為替、先物**など（時系列データがあれば対応） ([Backtesting.py - Backtest trading strategies in Python](https://kernc.github.io/backtesting.py/#:~:text=))  
- **特徴・強み**: **ユーザーフレンドリー**かつ**直感的**なAPIで、数行のコードからバックテストが可能。Jupyterノートブック上でインタラクティブにチャートを操作できる点や、高速なベクトル演算による最適化（数百通りの戦略を秒単位でテスト）など実用性が高いです ([Backtesting.py - Backtest trading strategies in Python](https://kernc.github.io/backtesting.py/#:~:text=in%20the%20future,including%20a%20handful%20of%20tutorials)) ([Backtesting.py - Backtest trading strategies in Python](https://kernc.github.io/backtesting.py/#:~:text=))。  
- **リンク**: [GitHub: kernc/backtesting.py](https://github.com/kernc/backtesting.py)

## QSTrader（Python）

- **対応言語**: Python  
- **主な機能**: オブジェクト指向でモジュール化された**バックテスト**フレームワーク。**長短両建て**のシステムトレード戦略のシミュレーションを主目的としており、株式やETFのポートフォリオを想定した機能を持ちます ([QSTrader](https://www.quantstart.com/qstrader/#:~:text=QSTrader%20is%20an%20open%20source,simulation%20framework%20written%20in%20Python))（ファンダメンタル分析機能は特になし）。イベントスケジューラによる定期リバランスなども実装可能です。  
- **対象資産**: **株式（現物株）、ETF**（主に現物資産にフォーカス） ([QSTrader](https://www.quantstart.com/qstrader/#:~:text=QSTrader%20is%20an%20open%20source,simulation%20framework%20written%20in%20Python))  
- **特徴・強み**: QuantStart（クオンートスタート）チームによって開発・運用されており、プロ向けの拡張性と信頼性があります ([QSTrader](https://www.quantstart.com/qstrader/#:~:text=QSTrader%20is%20an%20open%20source,simulation%20framework%20written%20in%20Python))。オープンソースコミュニティやヘッジファンド内でも利用されている実績があります。  
- **リンク**: [GitHub: mhallsmoore/qstrader](https://github.com/mhallsmoore/qstrader)

## Freqtrade（Python）

- **対応言語**: Python  
- **主な機能**: 暗号資産専用の自動売買ボット。主要取引所への接続に対応し、ユーザ定義の戦略でテクニカル指標を用いた取引が可能です。**バックテスト**機能や収益曲線のプロット、資金管理ツールを備え、機械学習による戦略のパラメータ最適化（ハイパー最適化）にも対応しています ([Freqtrade](https://www.freqtrade.io/en/stable/#:~:text=Freqtrade%20is%20a%20free%20and,strategy%20optimization%20by%20machine%20learning)) ([Freqtrade](https://www.freqtrade.io/en/stable/#:~:text=,loss%20parameters%20for%20your%20strategy))。Telegram経由の遠隔操作やウェブUIも利用可能です。  
- **対象資産**: **暗号資産**（BinanceやCoinbaseなど主要取引所の現物・先物に対応） ([Freqtrade](https://www.freqtrade.io/en/stable/#:~:text=Freqtrade%20is%20a%20free%20and,strategy%20optimization%20by%20machine%20learning))  
- **特徴・強み**: **完全無料かつオープンソース**であり、ドキュメントやサンプル戦略も充実しています。バックテストで戦略の有効性を検証し、そのままペーパー（仮想）トレードや本番取引に移行できる一貫したワークフローを提供します。コミュニティが活発で、ストラテジーの共有リポジトリも存在します。  
- **リンク**: [GitHub: freqtrade/freqtrade](https://github.com/freqtrade/freqtrade)

## Jesse（Python）

- **対応言語**: Python  
- **主な機能**: 暗号資産トレード向けの包括的フレームワーク。**300種類以上**のテクニカル指標を標準搭載し、複数タイムフレーム・複数通貨ペアを組み合わせた戦略の構築、過去データでの**高精度バックテスト**、パラメータ最適化、さらには現物・先物を含むライブ取引まで一貫してサポートします ([GitHub - jesse-ai/jesse: An advanced crypto trading bot written in Python](https://github.com/jesse-ai/jesse#:~:text=Jesse%20is%20an%20advanced%20crypto,backtesting%2C%20optimizing%2C%20and%20live%20trading))。  
- **対象資産**: **暗号資産**（スポット現物および先物）  
- **特徴・強み**: **シンプルな文法**で戦略を定義でき、バックテストエンジンは高速かつ正確（ルックアヘッドバイアス排除）です ([Jesse - The Open-source Python Bot For Trading Cryptocurrencies](https://jesse.trade/#:~:text=Backtest%20with%20confidence))。自己ホスティング型でプライバシーが守られ、Docker環境でも動作可能。コミュニティによる戦略共有やサポートもあり、ドキュメントも整備されています。  
- **リンク**: [GitHub: jesse-ai/jesse](https://github.com/jesse-ai/jesse)

## Lean (QuantConnect Lean)（C#コア／Python対応）

- **対応言語**: C# (エンジン本体)、**Python**/C# (戦略記述言語として利用可能)  
- **主な機能**: QuantConnect社が開発したプロ向けのオープンソース統合プラットフォーム。イベント駆動型で、**バックテスト**・最適化・ライブトレードまで一貫して行えるのが特徴です ([LEAN Algorithmic Trading Engine - QuantConnect.com](https://www.lean.io/#:~:text=LEAN%20is%20the%20world%27s%20leading,trade%20on%20hundreds%20of%20venues))。株式の財務データや代替データを含む様々なデータソースに対応し、**100種類以上のテクニカル指標**を標準搭載 ([LEAN Algorithmic Trading Engine - QuantConnect.com](https://www.lean.io/#:~:text=Technical%20Indicators))。分割や配当など企業アクションの自動処理、ユニバース選択機能など高度な機能も備えます。  
- **対象資産**: **株式、暗号資産、先物、オプション、FX**など主要な**9資産クラス**を同一ポートフォリオ内で扱えます ([LEAN Algorithmic Trading Engine - QuantConnect.com](https://www.lean.io/#:~:text=LEAN%20is%20the%20world%27s%20leading,trade%20on%20hundreds%20of%20venues)) ([LEAN Algorithmic Trading Engine - QuantConnect.com](https://www.lean.io/#:~:text=portfolio%2C%20allowing%20you%20to%20trade,classes%20at%20the%20same%20time))。  
- **特徴・強み**: **機関投資家レベルの性能と拡張性**を持つフレームワークです。スリッページモデルや手数料モデルなどを差し替え可能なプラグインアーキテクチャで、精緻なシミュレーションが可能。QuantConnectクラウドと同じエンジンでありながらオープンソースで自由に使える点も強みです。  
- **リンク**: [GitHub: QuantConnect/Lean](https://github.com/QuantConnect/Lean)

## PyAlgoTrade（Python）

- **対応言語**: Python  
- **主な機能**: **イベント駆動型**のアルゴリズム取引ライブラリ。過去データでの**バックテスト**を主眼に設計され、Yahoo! FinanceやQuandlから取得した株価データ（CSV）やBitcoinの価格データ（Bitstamp対応）で検証が可能です ([GitHub - gbeced/pyalgotrade: Python Algorithmic Trading Library](https://github.com/gbeced/pyalgotrade#:~:text=,Lib%20integration))。テクニカル指標（SMA、EMA、RSI、ボリンジャーバンド等）やフィルター、シャープレシオやドローダウン分析などのパフォーマンス指標も備え、TA-Libとの統合にも対応しています ([GitHub - gbeced/pyalgotrade: Python Algorithmic Trading Library](https://github.com/gbeced/pyalgotrade#:~:text=,Lib%20integration))。一部取引所（Bitstamp）ではペーパートレード・ライブ取引も可能です ([GitHub - gbeced/pyalgotrade: Python Algorithmic Trading Library](https://github.com/gbeced/pyalgotrade#:~:text=PyAlgoTrade%20is%20an%20event%20driven,trading%20is%20now%20possible%20using))。  
- **対象資産**: **株式（Yahoo!などの市場データ）**、**暗号資産（Bitstamp対応）** ([GitHub - gbeced/pyalgotrade: Python Algorithmic Trading Library](https://github.com/gbeced/pyalgotrade#:~:text=,Lib%20integration))  
- **特徴・強み**: シンプルなAPIで扱いやすく、チュートリアルやドキュメントも整っています。※開発者による保守は停止していますが、同開発者から後継のasync対応フレームワーク「Basana」が公開されています。  
- **リンク**: [GitHub: gbeced/pyalgotrade](https://github.com/gbeced/pyalgotrade)

## VectorBT（Python）

- **対応言語**: Python  
- **主な機能**: NumPyやPandasを駆使した**ベクトル化による高速バックテスト**が可能なクオンツ分析ライブラリです。数行のコードで資産配分や売買シグナルのテストを実行でき、テクニカル指標や売買シグナルの生成、ポートフォリオシミュレーション、パラメータ最適化など**定量分析全般**をカバーします ([VectorBT - An Introductory Guide - AlgoTrading101 Blog](https://algotrading101.com/learn/vectorbt-guide/#:~:text=VectorBT%20is%20an%20open,for%20quantitative%20analysis%20and%20backtesting))（ファンダメンタル分析機能は特になし）。Jupyter上での対話的チャート描画やTelegram通知など拡張機能もあります。  
- **対象資産**: **株式、暗号資産、ほか各種資産**（任意の時系列データに対応）  
- **特徴・強み**: **非常に高速**で、大量の注文やシミュレーションを短時間で処理可能（“オープンソース中最速のバックテストエンジン”とうたわれ、100万件の注文を0.1秒程度で執行 ([Features - vectorbt](https://vectorbt.dev/getting-started/features/#:~:text=Modeling%C2%B6))）。オープンソース版は無料で利用でき、必要に応じて上位版（VectorBT Pro）へのアップグレードも可能です ([VectorBT - An Introductory Guide - AlgoTrading101 Blog](https://algotrading101.com/learn/vectorbt-guide/#:~:text=%2A%20VectorBT%20is%20open,interactive%20charting%20via%20Jupyter%20notebooks)) ([VectorBT - An Introductory Guide - AlgoTrading101 Blog](https://algotrading101.com/learn/vectorbt-guide/#:~:text=Is%20VectorBT%20free%3F))。  
- **リンク**: [GitHub: polakowo/vectorbt](https://github.com/polakowo/vectorbt)

## Zenbot（JavaScript/Node.js）

- **対応言語**: JavaScript (Node.js)  
- **主な機能**: CLI上で動作する暗号資産トレーディングボット。**テクニカル分析に基づく自動売買**戦略の構築が可能で、BinanceやBitfinexなど多数の暗号資産取引所に対応しています ([GitHub - DeviaVir/zenbot: Zenbot is a command-line cryptocurrency trading bot using Node.js and MongoDB.](https://github.com/DeviaVir/zenbot#:~:text=%2A%20Fully,further%20exchange%20support%20is%20ongoing))。プラグイン方式で新たな取引所やストラテジーを追加でき、過去データで戦略を検証する**シミュレーター（バックテスト）**やペーパートレード（仮想売買）モードも備えています ([GitHub - DeviaVir/zenbot: Zenbot is a command-line cryptocurrency trading bot using Node.js and MongoDB.](https://github.com/DeviaVir/zenbot#:~:text=Zenbot%20is%20a%20command,It%20features)) ([GitHub - DeviaVir/zenbot: Zenbot is a command-line cryptocurrency trading bot using Node.js and MongoDB.](https://github.com/DeviaVir/zenbot#:~:text=,50%2Fday%20with%205m%20period))。  
- **対象資産**: **暗号資産**（主要取引所の現物取引に幅広く対応） ([GitHub - DeviaVir/zenbot: Zenbot is a command-line cryptocurrency trading bot using Node.js and MongoDB.](https://github.com/DeviaVir/zenbot#:~:text=%2A%20Fully,further%20exchange%20support%20is%20ongoing))  
- **特徴・強み**: 完全自動のシステムトレードが可能で、売買シグナル発生から発注までをフルサイクルで実行します。Node.jsとMongoDBで構築されており、コマンドラインから細かな設定を変更可能です。※開発のアクティブ度は低下していますが、利用者コミュニティによるフォークや改善が引き続き行われています。  
- **リンク**: [GitHub: DeviaVir/zenbot](https://github.com/DeviaVir/zenbot)

## Qlib（Python）

- **対応言語**: Python  
- **主な機能**: Microsoftリサーチが公開した**AI志向の定量投資プラットフォーム**です。機械学習（教師あり学習、強化学習など）を活用したアルファ因子の研究からモデル構築、**バックテスト**まで一貫して行えます ([GitHub - microsoft/qlib: Qlib is an AI-oriented quantitative investment platform that aims to realize the potential, empower research, and create value using AI technologies in quantitative investment, from exploring ideas to implementing productions. Qlib supports diverse machine learning modeling paradigms. including supervised learning, market dynamics modeling, and RL.](https://github.com/microsoft/qlib#:~:text=Qlib%20is%20an%20open,diverse%20machine%20learning%20modeling%20paradigms))。財務データやテキスト情報からのファクターマイニング（例：決算報告分析）機能も備えており、テクニカル指標とファンダメンタル指標の両面から投資戦略を設計可能です。ポイントインタイムデータ管理やリスクモデル、ポートフォリオ最適化までカバーする高度なフレームワークです。  
- **対象資産**: **株式中心**（中国A株や米国株のデータセット例が提供されている）。ユーザがデータを用意すれば暗号資産など他資産にも応用可能。  
- **特徴・強み**: 最新の研究成果を取り入れた**強力なクオンツ研究プラットフォーム**であり、学術論文レベルのモデル（時系列AIモデルや強化学習エージェントなど）を実装・検証できます。オープンソースでありつつ商用利用も可能な柔軟性を持ち、AIを活用したファンダメンタル分析に強みがあります ([GitHub - microsoft/qlib: Qlib is an AI-oriented quantitative investment platform that aims to realize the potential, empower research, and create value using AI technologies in quantitative investment, from exploring ideas to implementing productions. Qlib supports diverse machine learning modeling paradigms. including supervised learning, market dynamics modeling, and RL.](https://github.com/microsoft/qlib#:~:text=Qlib%20is%20an%20open,diverse%20machine%20learning%20modeling%20paradigms))。  
- **リンク**: [GitHub: microsoft/qlib](https://github.com/microsoft/qlib)

