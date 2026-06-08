# HAR Tag Event Inspector

HARファイルから広告・解析タグの発火状況を媒体横断で一覧化するブラウザ完結ツールです。

🔗 **[Open Tool](https://AzuWatsan.github.io/har-tag-inspector/)**

---

## 特徴

- **ブラウザ完結** — アップロードしたHARファイルは外部に送信されません
- **8媒体に対応** — GA4 / Google Ads / Meta / Criteo / LINE / Microsoft Ads / Yahoo Ads / TikTok
- **重複検出** — 同一媒体・ID・イベントの二重発火を自動検出
- **警告一覧** — HTTPエラー、ID未取得、複数ID混在などを自動チェック
- **エクスポート** — CSV / Markdown / JSON で結果をダウンロード

## 使い方

1. Chrome DevTools または Charles などで HARファイルを取得する
2. ツールを開き、HARファイルをドロップまたは選択する
3. **Run Inspection** をクリック
4. **Summary / Matched Tags / Warnings** タブで結果を確認する

## 対応媒体

| 媒体 | 検出ホスト | 取得情報 |
|---|---|---|
| GA4 | `google-analytics.com/g/collect` | Measurement ID / イベント名 |
| Google Ads | `googleadservices.com` | コンバージョンID / タイプ |
| Meta Pixel | `facebook.com/tr` | Pixel ID / イベント名 |
| Criteo | `criteo.com/event` | アカウントID / イベント名 |
| LINE Tag | `tr.line.me` | タグID / イベント名 |
| Microsoft Ads | `bat.bing.com/action` | タグID / イベント名 |
| Yahoo Ads | `yahoo.co.jp` 系 | サイトID |
| TikTok Pixel | `analytics.tiktok.com/pixel/track` | Pixel ID / イベント名 |

## HARファイルの取得方法

**Chrome DevTools**
1. DevTools を開く（F12）
2. Network タブを選択
3. 計測したいページを操作する
4. Network タブ内を右クリック → "Save all as HAR with content"

## 新しい媒体を追加する

`index.html` 内の detector 関数を1つ追加し、`DETECTORS` 配列に追記するだけです。

```js
function detectXxx(ctx) {
  if (!ctx.url.hostname.includes("example.com")) return null;
  const params = { ...ctx.queryParams, ...ctx.postParams };
  return makeEvent("Xxx", {
    platformId: params["id"],
    eventName: params["event"],
    eventType: "pixel_event",
    rawParams: params,
  }, ctx);
}

const DETECTORS = [
  // ... 既存 ...
  detectXxx, // ← 追加
];
```

## 技術スタック

- React 18 (CDN / UMD)
- Vanilla JS + CSS Custom Properties
- 外部依存: React / ReactDOM のみ（cdnjs.cloudflare.com）
- ビルド不要・単一HTMLファイル

## ライセンス

MIT
