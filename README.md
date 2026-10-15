# opentw.app

Open TW 官方網站入口（純靜態 HTML，部署在 Cloudflare Workers Static Assets）。

## 結構
```
public/
  index.html          首頁（含 meta / OG / JSON-LD）
  404.html            自訂 404（noindex）
  robots.txt / sitemap.xml
  og.png              社群分享圖 1200x630（原稿紙樣式）
  favicon.svg / apple-touch-icon.png / icon-192.png / icon-512.png
  site.webmanifest
  _headers            安全性與快取標頭
wrangler.jsonc
```

## 開發與部署
```bash
npm install
npm run dev      # http://localhost:8787
npx wrangler login
npm run deploy   # 綁定 opentw.app（DNS 需在 Cloudflare）
```

## 新增一個服務
1. 在 `index.html` 的 `#projects` 複製一段 `<article class="project">`，改名稱、說明、連結，並給它 `id`（repo 名稱，例如 `id="lite-wiki"`）。需要新顏色時在 `:root` 與兩段暗色模式各加一個 `--c-xxx`。
2. 在 JSON-LD 的 `ItemList` 加一筆（`url` 指向 `https://opentw.app/#<id>`），並更新 `numberOfItems` 與 `WebPage.dateModified`。
3. 更新 `<meta name="description">`、`og:description`、`twitter:description`，以及頁尾導覽連結。
4. 更新 `sitemap.xml` 的 `lastmod`。
5. 若有獨立頁面（例如 `/cwa-sdk`），新增 `public/cwa-sdk.html` 並加入 `sitemap.xml`，更新 `lastmod`。

## 上線後 SEO 待辦
- Google Search Console / Bing Webmaster 驗證網域並提交 `https://opentw.app/sitemap.xml`
- Cloudflare 後台將 `www.opentw.app` 用 Redirect Rule 301 到 `https://opentw.app`
