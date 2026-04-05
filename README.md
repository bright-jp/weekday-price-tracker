# Weekday Price Tracker

[![Bright Data](https://img.shields.io/badge/Powered%20by-Bright%20Data-blue?style=flat-square)](https://brightdata.jp)
[![Weekday Price Tracker](https://img.shields.io/badge/Weekday%20Price%20Tracker-Managed%20Solution-orange?style=flat-square)](https://brightdata.jp/products/insights/price-tracker/weekday)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow?style=flat-square)](https://python.org)

[![Bright Insights Price Tracker](https://raw.githubusercontent.com/danielshashko/bright-insights-assets/main/price-tracker-hero-v2.png)](https://brightdata.jp/products/insights/price-tracker/weekday)

リアルタイムのWeekday価格トラッキング - H&M Group傘下のスウェーデンのストリートウェアブランド。開始方法は2通りあります: **フルマネージド**のインテリジェンスプラットフォーム、または Bright Data の AI Scraper Builder で構築する**カスタム scraper**です。

---

## Option 1: Bright Insights - AI搭載の価格トラッキング（推奨）

**[Bright Insights](https://brightdata.jp/products/insights/price-tracker/weekday)** は、Bright Data のフルマネージドなリテールインテリジェンスプラットフォームです。scraper を構築する必要も、インフラを保守する必要もありません。構造化され、分析にそのまま使える価格データが、ダッシュボード、data feed、または BI tools に提供されます。

**チームが Bright Insights を選ぶ理由:**
- 🚀 **セットアップ不要** - すぐに使えるダッシュボードと data feed で数分で稼働開始
- 🤖 **AI搭載のレコメンデーション** - 対話型AIアシスタントが数百万のデータポイントを即座に実用的なインサイトへ変換
- ⚡ **リアルタイム監視** - 1時間ごとから日次までの更新頻度と即時アラート（email、Slack、webhook）
- 🌍 **無制限のスケール** - あらゆる website、あらゆる地域、あらゆる更新頻度に対応
- 🔗 **プラグアンドプレイ統合** - AWS、GCP、Databricks、Snowflake など
- 🛡️ **フルマネージド** - Bright Data が schema 変更、site 更新、データ品質を自動で処理

**主なユースケース:**
- ✅ Weekday の**季節割引**やセールイベントを追跡
- ✅ **競合価格を監視**し、トレンドを特定
- ✅ 商品が目標価格を下回った際の**価格アラートを自動化**
- ✅ MAPポリシー準拠を監視し、価格違反を検出
- ✅ 競合のプロモーションと販促動向を追跡
- ✅ クリーンで統合済みのデータを dynamic pricing algorithm や AI model に直接投入

> **月額 $250 から - [お見積もりはこちら →](https://brightdata.jp/products/insights/price-tracker/weekday)**

---

## Option 2: 独自の Weekday scraper を構築する

事前構築済みの Weekday scraper API がない？問題ありません。Bright Data の **AI Scraper Builder** なら、数クリックでカスタム Weekday scraper を生成できます — コーディングは不要です。

### 数分で Weekday scraper を構築

**[Weekday AI Scraper Builder を開く →](https://brightdata.jp/products/web-scraper/weekday)**

ドメインを選択し、必要なデータ要件を指定すれば、AI scraper builder が自動的に API を作成します。

1. **必要なデータを平易な英語で記述**
2. **AI が即座に scraper API を生成**
3. **API request を実行してすぐに結果を取得**
4. 必要に応じて、**組み込み IDE でコードを編集**

構築が完了すると、scraper に **Web Scraper ID** (`gd_xxxxxxxxxxxx`) が付与されます — 以下の Setup 手順で使うためにコピーしてください。

### 前提条件

- Python 3.9 以上
- [Bright Data account](https://brightdata.jp)（無料トライアルあり）
- Bright Data の **API token**（[取得方法](https://docs.brightdata.jp/general/account/account-settings#api-token)）
- Weekday 用の **Web Scraper ID**（上記の構築ステップで取得）

### Setup

1. **この repository を clone**

   ```bash
   git clone https://github.com/bright-jp/weekday-price-tracker.git
   cd weekday-price-tracker
   ```

2. **依存関係をインストール**

   ```bash
   pip install -r requirements.txt
   ```

3. **認証情報を設定**

   `.env.example` を `.env` にコピーし、値を入力します:

   ```bash
   cp .env.example .env
   ```

   ```env
   BRIGHTDATA_API_TOKEN=your_api_token_here
   BRIGHTDATA_DATASET_ID=your_dataset_id_here
   ```

   > **Your Web Scraper ID**
   > [AI Scraper Builder dashboard](https://brightdata.jp/products/web-scraper/weekday) から Web Scraper ID をコピーし、
   > `BRIGHTDATA_DATASET_ID` に貼り付けてください（形式: `gd_xxxxxxxxxxxx`）。

---

## 使い方

Weekday scraper の構築が完了し、Web Scraper ID が `.env` に設定されると、Python interface は同じ方法で利用できます。

### 1. URLで特定の商品を追跡

Weekday の商品URLのリストを渡して、構造化された価格データを取得します:

```python
from price_tracker import track_prices

urls = [
    "https://www.weekday.com/en/products/sample-product-123456",
    # Add more product URLs here
]

results = track_prices(urls)
for item in results:
    print(f"{item.get('title')} - {item.get('final_price', item.get('price'))} {item.get('currency', '')}")
```

または直接実行:

```bash
python price_tracker.py
```

### 2. キーワードで商品を検索

キーワード検索に一致する商品を見つけます:

```python
from price_tracker import discover_by_keyword

results = discover_by_keyword("laptop", limit=50)
```

### 3. カテゴリURLで商品を閲覧

Weekday のカテゴリページからすべての商品を収集します:

```python
from price_tracker import discover_by_category

results = discover_by_category(
    "https://weekday.com/category/example",
    limit=100,
)
```

---

## 出力フィールド

各結果レコードには以下のフィールドが含まれます:

| Field | Description |
|-------|-------------|
| `url` | 商品ページURL |
| `title` | 商品名 |
| `brand` | ブランド |
| `initial_price` | 元の価格 |
| `final_price` | セール価格 / 現在価格 |
| `currency` | 通貨コード |
| `discount` | 割引率 |
| `in_stock` | 在庫状況 |
| `color` | 利用可能なカラー |
| `size` | 利用可能なサイズ |
| `category` | 商品カテゴリ / department |
| `images` | 商品画像URL |
| `description` | 商品説明 |
| `timestamp` | 収集タイムスタンプ |

### 出力例

```json
[
  {
    "url": "https://www.weekday.com/en/products/sample-product-123456",
    "title": "Example Product Name",
    "brand": "Example Brand",
    "initial_price": 59.99,
    "final_price": 44.99,
    "currency": "USD",
    "discount": "25%",
    "in_stock": true,
    "rating": 4.5,
    "reviews_count": 1234,
    "images": ["https://weekday.com/images/product1.jpg"],
    "description": "Product description text...",
    "timestamp": "2025-01-15T10:30:00Z"
  }
]
```

---

## 高度なオプション

`trigger_collection()` 関数は、データ収集を制御するためのオプションパラメータを受け付けます:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | - | 返却するレコードの最大数 |
| `include_errors` | boolean | `true` | 結果に error report を含める |
| `notify` | string (URL) | - | snapshot の準備完了時に呼び出す webhook URL |
| `format` | string | `json` | 出力形式: `json`、`csv`、または `ndjson` |

オプション付きの例:

```python
from price_tracker import trigger_collection, get_results

inputs = [{"url": "https://www.weekday.com/en/products/sample-product-123456"}]
snapshot_id = trigger_collection(inputs, limit=200, notify="https://your-webhook.com/hook")
results = get_results(snapshot_id)
```

---

## リソース

- 🌟 [Weekday Price Tracker - Bright Insights (Managed)](https://brightdata.jp/products/insights/price-tracker/weekday)
- 🏗️ [Build a Weekday Scraper](https://brightdata.jp/products/web-scraper/weekday)
- 📖 [Bright Data Web Scraper API Documentation](https://docs.brightdata.jp/scraping-automation/web-scraper-api/overview)
- 🗄️ [Web Scrapers Control Panel](https://brightdata.jp/cp/scrapers)
- 🔑 [How to get an API token](https://docs.brightdata.jp/general/account/account-settings#api-token)
- 🌐 [Bright Data Homepage](https://brightdata.jp)

---

*[Bright Data](https://brightdata.jp) を使用して構築 - 業界をリードする web data platform。*