Extract real-time XML **danmaku (弹幕 / bullet comments)**, exact video appearance timestamps (`videoTimeFormatted`), high-energy **peak reaction minutes (`peakMinute`)**, and sender hashes from **Bilibili (哔哩哔哩 / bilibili.com)** — China's premier Gen-Z video, gaming, ACG, and tech review platform.

Built for **AI/LLM multimodal dialogue training corpora, video highlight detection, gaming & tech launch reception analysis, and automated RAG pipelines** (n8n, Make, Dify, Claude MCP). **100% no login, no cookies, and no VPN required.** Users on the **Apify Free plan ($5 monthly credit) can extract up to 1,111 Bilibili danmaku bullet comments per month for free**.

---

## What Can This Bilibili Danmaku Scraper Do? (Features)

| Bilibili Danmaku Capability | Built-In Advantage |
| :--- | :--- |
| **Native XML Danmaku Stream Parsing (`list.so`)** | Directly extracts Bilibili's real-time bullet comment pool by `bvid`, `aid`, or video URL without browser overhead or login cookies. |
| **Peak High-Energy Minute Detection (`peakMinute`)** | Automatically buckets bullet comments across the video timeline to pinpoint the exact 1-minute window (e.g., `"02:00-02:59"`) where viewer reactions peak. |
| **Smart Density Sampling (Zero Opening Spam)** | Instead of only grabbing `"前排"` spam from the first `00:00` second, the engine samples across peak-density moments and the full video timeline sorted chronologically. |
| **Danmaku Keyword Filtering (`danmakuKeyword`)** | Optionally filter bullet comments by specific substrings (e.g., `"高能"`, `"泪目"`, `"遥遥领先"`, `"价格"`) to isolate targeted viewer reactions. |
| **Automatic Keyword Video Discovery** | Don't have specific `BV` IDs? Leave `videoIds` empty and enter any English or Chinese topic in `searchQuery` to auto-discover top videos and harvest their danmaku. |
| **256 MB RAM Real-Time Streaming** | Streams bullet records to your Apify Dataset in real-time batches of 10 rows on just **256 MB RAM**, supporting scheduled runs (`deltaMode`) and **JSON, CSV, Excel, or Parquet** exports. |

---

## How to Scrape Bilibili Danmaku in 3 Easy Steps

1. **Enter Target Videos (`videoIds`)** — Paste one or more Bilibili `BV` IDs (e.g., `BV1r94y1H77U`), `av` numbers, or full video URLs; OR leave `videoIds` empty and enter a topic in `searchQuery`.
2. **Set `maxResults` & Optional Filter** — Choose how many bullet comments to extract (`1` to `10,000`) and optionally enter a `danmakuKeyword` filter.
3. **Click "Start"** — Download your time-aligned danmaku dataset in **JSON, CSV, Excel, or Parquet**, or connect via REST API, n8n, Make, or Claude MCP.

---

## Bilibili Danmaku Scraper Use Cases

| Buyer Profile | Bilibili Danmaku & Timestamp Intelligence Use Case |
| :--- | :--- |
| **AI / LLM & Multimodal Training Teams** | Build time-aligned video-to-text reaction corpora pairing exact video timestamps (`videoSecond`) with colloquial Chinese crowd commentary |
| **Gaming & Consumer Tech Brands** | Pinpoint which exact second of a product keynote, game trailer (`Black Myth Wukong`), or smartphone teardown triggered the biggest audience reaction (`peakMinute`) |
| **Video Editors & MCN Agencies** | Automatically detect viral highlight moments and crowd catchphrases across long-form Bilibili videos |
| **Academic NLP & Cultural Researchers** | Study real-time synchronous bullet-curtain (`弹幕`) linguistics, subculture slang, and viewer engagement dynamics |

---

## Input

All parameters are configured in the **Input** tab. Below is the complete input parameter reference table matching the schema and JSON input examples:

| Input Field | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **`videoIds`** | `array` | `["BV1r94y1H77U"]` | Target Bilibili BV IDs, av numbers, or full video URLs. Leave empty `[]` to auto-discover videos via `searchQuery`. |
| **`danmakuKeyword`** | `string` | `""` | Optional substring filter to only return bullet comments containing specific words. Leave empty for full peak-density sampling. |
| **`maxResults`** | `integer` | `100` | Maximum number of danmaku bullet comment rows to return (`1` to `10,000`). |
| **`searchQuery`** | `array` | `["黑神话悟空"]` | Fallback keywords in English or Chinese used to auto-discover active videos when `videoIds` is empty. |
| **`autoLocalize`** | `boolean` | `true` | Automatically translates English brand/game keywords into Simplified Chinese during video auto-discovery. |
| **`deltaMode`** | `boolean` | `false` | Remembers scraped `danmakuId` values in Key-Value Store across scheduled runs so only new bullet comments are returned. |
| **`mode`** | `string` | `"danmaku"` | Locked to `"danmaku"` extraction mode. |

### 1. Extract Peak & Timeline Danmaku from Specific Videos (JSON)
```json
{
  "mode": "danmaku",
  "videoIds": [
    "BV1r94y1H77U",
    "https://www.bilibili.com/video/BV1xrnz6gEVD"
  ],
  "danmakuKeyword": "",
  "maxResults": 200,
  "deltaMode": false
}
```

### 2. Filter Danmaku by Keyword or Auto-Discover Videos (JSON)
```json
{
  "mode": "danmaku",
  "videoIds": ["BV1r94y1H77U"],
  "danmakuKeyword": "高能",
  "maxResults": 100
}
```

---

## Output

All scraped bullet comments are stored in your Apify **Dataset** and can be exported to JSON, CSV, Excel, XML, or Parquet. Below is a sample JSON output matching the `"BV1r94y1H77U"` input above:

```json
{
  "mode": "danmaku",
  "danmakuId": "16123456789012345",
  "bvid": "BV1r94y1H77U",
  "aid": 1130123456,
  "cid": 1612345678,
  "title": "《黑神话：悟空》最终预告 | 8月20日，重走西游",
  "text": "前方高能！四大天王这压迫感绝了！",
  "videoSecond": 125.4,
  "videoTimeFormatted": "02:05",
  "peakMinute": "02:00-02:59",
  "danmakuMode": 1,
  "fontSize": 25,
  "colorHex": "#FFFFFF",
  "senderHash": "a1b2c3d4",
  "videoUrl": "https://www.bilibili.com/video/BV1r94y1H77U",
  "createdAtIso": "2024-08-08T02:18:45Z",
  "scrapedAtIso": "2026-10-28T10:30:00Z"
}
```

### Output Field Definitions
* **`danmakuId`**: Unique Bilibili bullet comment ID (used for deduplication in `deltaMode`).
* **`bvid` / `aid` / `cid`**: Parent video BV ID, numeric archive ID, and video player cid.
* **`text`**: Clean bullet comment text in Simplified Chinese.
* **`videoSecond` / `videoTimeFormatted`**: Exact timestamp in the video where the bullet comment appears (in seconds `125.4` and `MM:SS` format `"02:05"`).
* **`peakMinute`**: The 1-minute window in the parent video with the highest overall danmaku density (e.g., `"02:00-02:59"`).
* **`colorHex` / `fontSize` / `danmakuMode`**: Visual styling metadata (`#FFFFFF`, font size, scrolling vs. top/bottom pinned mode).
* **`senderHash`**: Anonymized CRC32 hash of the bullet sender's account ID.

---

## Automated Daily Danmaku Monitoring (Schedules & Delta Mode)

Track new bullet comments on viral videos automatically:
1. Go to **Apify Console → Schedules → Create new schedule** and select **`axon-studio/bilibili-danmaku-scraper`**.
2. Set **`deltaMode: true`** in your input JSON. Previously delivered `danmakuId` records are stored in persistent storage and skipped on future runs—so you only pay for newly posted bullet comments.

### Python & Synchronous REST API Integration

```python
from apify_client import ApifyClient

client = ApifyClient("YOUR_APIFY_API_TOKEN")
run = client.actor("axon-studio/bilibili-danmaku-scraper").call(run_input={
    "mode": "danmaku",
    "videoIds": ["BV1r94y1H77U"],
    "maxResults": 100,
})
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(item["videoTimeFormatted"], item["peakMinute"], item["text"])
```

```bash
curl -X POST "https://api.apify.com/v2/acts/axon-studio~bilibili-danmaku-scraper/run-sync-get-dataset-items?token=YOUR_APIFY_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"mode":"danmaku","videoIds":["BV1r94y1H77U"],"maxResults":50}'
```

---

## How Much Does It Cost to Scrape Bilibili Danmaku? (Pricing)

**$4.50 per 1,000 danmaku items (`$0.0045` per bullet comment) + `$0.02` per Actor start (Pay-Per-Event)**

* **Free Plan Credits**: Every Apify account includes **$5.00 in free monthly credits**, covering up to **1,111 Bilibili danmaku bullet comments every month for free**.
* **Volume Discounts**: Automatic lower unit prices for Apify **Bronze / Starter**, **Silver / Scale**, and **Gold / Business** plan subscribers.
* **No Extra Server Compute Fees**: Runs on just 256 MB RAM with platform compute usage fully included.
* **Typical Run Costs**: 100 bullet comments = **`$0.47`** · 500 bullet comments = **`$2.27`** · 1,000 bullet comments = **`$4.52`**.

---

## FAQ & Legal Compliance

### Do I need a Bilibili login or `SESSDATA` cookie to extract XML danmaku?
**No.** This Actor resolves the video's player `cid` and parses Bilibili's public XML danmaku stream automatically without requiring user credentials.

### Is it legal to scrape public Bilibili danmaku?
This Actor extracts publicly visible bullet comments and anonymized sender hashes displayed on `bilibili.com`. Under **GDPR** and **PIPL**, ensure you have a lawful basis for NLP research or market analytics. *Not affiliated with Bilibili Inc.*

---

## More Chinese Social Intelligence Scrapers by Axon Studio

* **[Bilibili Scraper 哔哩哔哩 (No Cookie) — All-in-One Videos, Danmaku, Comments & Creators](https://apify.com/axon-studio/bilibili-scraper)** — Full 8-mode Bilibili intelligence suite.
* **[Bilibili Comments & IP Geolocation Scraper](https://apify.com/axon-studio/bilibili-comments-scraper)** — Extract Bilibili comment threads and English IP province locations (`regionEn`).
* **[Bilibili Creator Videos & UP Profile Scraper](https://apify.com/axon-studio/bilibili-creator-videos-scraper)** — Extract UP creator video uploads and KOL follower profiles without `-352` blocks.
* **[Weibo Scraper 微博 (No Cookie) — Search, Posts, Comments & BlackCat](https://apify.com/axon-studio/weibo-scraper)** — 9-mode Sina Weibo & BlackCat social listening suite.
* **[BlackCat Scraper 黑猫投诉 — China Consumer Complaints API](https://apify.com/axon-studio/blackcat-complaints-scraper)** — Extract Chinese consumer disputes from `tousu.sina.com.cn`.
