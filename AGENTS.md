# MyParisTrip 單頁切版交接規格

## 專案概述

本專案為 MyParisTrip「小法旅」主題式法國旅遊單頁網站切版。頁面使用 Bootstrap 5 作為版面框架，搭配自訂 CSS 完成設計稿視覺與 RWD。

目前主要頁面為 `index.html`，自訂樣式已獨立於 `css/style.css`，Bootstrap CSS/JS 皆使用本地端檔案，不依賴 CDN。

## 目錄結構

```text
websiteai/
├─ index.html
├─ PROJECT_SPEC.md
├─ 0622011.jpg
├─ css/
│  ├─ bootstrap.min.css
│  └─ style.css
├─ js/
│  └─ bootstrap.bundle.min.js
└─ images/
   ├─ logo.png
   └─ paris.jpg
```

## 主要檔案說明

`index.html`

網站主要 HTML 結構，包含導覽列、封面、主題旅行、即將出團輪播、景點找行程、頁尾，以及景點切換用的 JavaScript。

`css/style.css`

自訂樣式檔。包含品牌色、導覽列、hero、主題區、卡片輪播、SVG 地圖、景點內容區、頁尾與 RWD 設定。

`css/bootstrap.min.css`

本地 Bootstrap 5 CSS。

`js/bootstrap.bundle.min.js`

本地 Bootstrap 5 JS，已包含 Popper，供 navbar collapse 與 carousel 使用。

`images/logo.png`

頁首與頁尾 logo。

`images/paris.jpg`

目前用於「花園午後一日遊」卡片圖片。

`0622011.jpg`

原 UI/UX 設計稿參考圖，目前未直接載入頁面。

## 頁面區塊

1. 導覽列

位置：`index.html` 的 `<nav class="navbar ...">`

功能：
- 顯示 logo。
- 導覽連結錨點包含 `#themes`、`#tours`、`#spots`、`#footer`。
- 手機版使用 Bootstrap collapse。

2. 封面 Hero

位置：`<header class="hero">`

功能：
- 顯示主標語與搜尋列視覺。
- 背景圖目前寫在 `css/style.css` 的 `.hero` background 中。

3. 五大主題旅行系列

位置：`<section class="theme-section" id="themes">`

功能：
- 顯示五個主題分類。
- 圖示目前使用自訂 `.bi` CSS 文字符號模擬，未使用 Bootstrap Icons CDN。

4. 即將出團

位置：`<section class="tours-section" id="tours">`

功能：
- 使用 Bootstrap Carousel。
- 輪播 ID：`tourCarousel`。
- 每個 carousel item 內有 6 張 card，使用 Bootstrap row cols 做 RWD。
- 「花園午後一日遊」圖片引用 `images/paris.jpg`。

新增卡片時，建議沿用：

```html
<div class="col">
  <article class="card tour-card">
    <img src="..." alt="...">
    <div class="card-body">
      <h3><i class="bi bi-airplane me-1"></i>行程名稱</h3>
      <div class="tour-meta"><span>分類</span><span>地點</span><span>特色</span></div>
      <p>行程簡介</p>
      <div class="tour-date"><i class="bi bi-calendar2-week"></i> 日期</div>
    </div>
  </article>
</div>
```

5. 景點找行程

位置：`<section class="spot-section" id="spots">`

功能：
- 左側為內嵌 SVG 地圖。
- 地圖上的 `<g class="landmark-btn">` 可以點擊或鍵盤 Enter/Space 操作。
- 右側文字與圖片由頁尾 inline script 的 `spots` 物件控制。

目前景點 key：
- `arc`：凱旋門
- `louvre`：羅浮宮
- `eiffel`：艾菲爾鐵塔
- `mont`：聖米歇爾山

若要新增景點，需要同步更新三處：
- SVG 內新增一組 `.landmark-btn`，並設定 `data-spot="新key"`。
- 右側 `.spot-tabs` 新增一顆按鈕，並設定相同 `data-spot`。
- JavaScript `spots` 物件新增同名 key 的資料。

6. 頁尾

位置：`<footer class="footer" id="footer">`

內容：
- 聯絡電話
- Email
- 版權宣告
- logo
- 社群連結
- 公司地址與統編

## CSS 維護規則

自訂樣式統一放在 `css/style.css`。

載入順序必須維持：

```html
<link href="css/bootstrap.min.css" rel="stylesheet">
<link href="css/style.css" rel="stylesheet">
```

原因是 `style.css` 需要覆蓋 Bootstrap 預設樣式。

主要 CSS 區塊：
- `:root`：品牌色與共用色票。
- `.bi`：本地化簡易圖示符號。
- `.site-shell`、`.navbar`、`.brand-*`：整體容器與導覽。
- `.hero`：封面。
- `.theme-section`：五大主題。
- `.tours-section`、`.tour-card`：即將出團輪播。
- `.spot-section`、`.france-map`、`.landmark-btn`：景點找行程與 SVG 地圖。
- `.footer`：頁尾。
- `@media`：平板與手機 RWD。

## JavaScript 維護規則

目前 JavaScript 寫在 `index.html` 底部，包含：
- `spots` 景點資料物件。
- `setSpot(key)` 切換景點內容。
- 點擊與鍵盤事件綁定。

若後續頁面變多，建議將這段 inline script 獨立成 `js/main.js`，並於 Bootstrap 後載入：

```html
<script src="js/bootstrap.bundle.min.js"></script>
<script src="js/main.js"></script>
```

## 圖片資源規則

專案圖片統一放入 `images/`。

建議命名：
- logo：`logo.png`
- 行程卡片：`tour-名稱.jpg`
- 景點圖片：`spot-名稱.jpg`
- 背景圖片：`bg-用途.jpg`

目前仍有部分圖片使用 Unsplash 遠端網址。若要完全離線化，請下載對應圖片放入 `images/`，再將 `index.html` 與 `css/style.css` 內的遠端圖片路徑改成本地路徑。

## 本地化狀態

已本地化：
- Bootstrap CSS：`css/bootstrap.min.css`
- Bootstrap JS：`js/bootstrap.bundle.min.js`
- Logo：`images/logo.png`
- 花園午後一日遊卡片圖：`images/paris.jpg`

尚未完全本地化：
- Hero 背景圖仍在 `css/style.css` 中使用遠端圖片。
- 多數行程卡片圖片仍使用遠端圖片。
- 景點切換圖片仍使用遠端圖片。
- 紙質背景紋理仍使用遠端圖片。

## 後續建議

1. 將所有遠端圖片下載到 `images/`，避免離線或網路不穩時圖片失效。
2. 若有正式 Bootstrap Icons 檔案，可改用本地 `bootstrap-icons.css` 與 fonts，取代目前 `.bi` 簡易符號。
3. 若網站會增加更多頁面，建議拆出共用 header/footer，或至少建立 HTML 模板規則。
4. 若互動功能增加，建議將頁尾 inline JavaScript 拆成 `js/main.js`。
