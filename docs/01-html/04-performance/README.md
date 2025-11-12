# HTML パフォーマンス編

Webサイトのパフォーマンスを最大化するHTML実装技術を学びます。

## 目次

1. [パフォーマンスの重要性](#1-パフォーマンスの重要性)
2. [rel="preload" によるリソース先読み](#2-relpreload-によるリソース先読み)
3. [Core Web Vitals の最適化](#3-core-web-vitals-の最適化)
4. [サイト高速化の10の手法](#4-サイト高速化の10の手法)
5. [パフォーマンス測定](#5-パフォーマンス測定)

---

## 1. パフォーマンスの重要性

### サイト速度が与える影響

**ユーザー体験：**
- 1秒遅れるとコンバージョン率が7%低下
- 3秒以上かかると53%のユーザーが離脱

**SEO：**
- Googleの検索ランキング要因
- Core Web Vitalsが評価指標

**ビジネス：**
- Amazon：100ms遅れると売上が1%減少
- Pinterest：待ち時間を40%改善で新規登録が15%増加

### パフォーマンスの4つの決定要因

1. **リクエスト回数**
   - ダウンロードするファイル数

2. **リクエストサイズ**
   - 各ファイルの容量

3. **リクエストタイミング**
   - ファイルの読み込み順序

4. **HTMLパース速度**
   - ブラウザの処理速度

---

## 2. rel="preload" によるリソース先読み

### preload とは

重要なリソースを事前にダウンロードし、キャッシュする機能です。

```html
<link rel="preload" href="style.css" as="style">
<link rel="preload" href="main.js" as="script">
<link rel="preload" href="hero.jpg" as="image">
```

### 基本構文

```html
<link rel="preload"
      href="リソースのパス"
      as="リソースタイプ">
```

**必須属性：**
- `href`：リソースのパス
- `as`：リソースの種類

### as 属性の値

| 値 | 用途 | 例 |
|----|------|-----|
| style | CSS | `as="style"` |
| script | JavaScript | `as="script"` |
| image | 画像 | `as="image"` |
| font | フォント | `as="font"` |
| fetch | Fetch API | `as="fetch"` |
| audio | 音声 | `as="audio"` |
| video | 動画 | `as="video"` |
| track | 字幕 | `as="track"` |

### CSS の先読み

```html
<link rel="preload" href="critical.css" as="style">
<link rel="stylesheet" href="critical.css">
```

### JavaScript の先読み

```html
<link rel="preload" href="app.js" as="script">
<script src="app.js" defer></script>
```

### フォントの先読み

```html
<!-- crossorigin属性が必須 -->
<link rel="preload"
      href="fonts/myfont.woff2"
      as="font"
      type="font/woff2"
      crossorigin>
```

**重要：** フォントには`crossorigin`属性が必須です。

### 画像の先読み

```html
<!-- LCP画像を先読み -->
<link rel="preload" href="hero.jpg" as="image">
<img src="hero.jpg" alt="ヒーロー画像">
```

### レスポンシブ画像の先読み

```html
<link rel="preload"
      href="hero-mobile.jpg"
      as="image"
      media="(max-width: 640px)">

<link rel="preload"
      href="hero-desktop.jpg"
      as="image"
      media="(min-width: 641px)">
```

### 複数フォーマットの先読み

```html
<!-- AVIF対応ブラウザ用 -->
<link rel="preload"
      href="image.avif"
      as="image"
      type="image/avif">

<!-- WebP対応ブラウザ用 -->
<link rel="preload"
      href="image.webp"
      as="image"
      type="image/webp">
```

### JavaScript での動的 preload

```javascript
const link = document.createElement('link');
link.rel = 'preload';
link.as = 'image';
link.href = 'image.jpg';
document.head.appendChild(link);
```

### preload を使うべき場面

✅ **推奨：**
- LCP対象の画像
- ファーストビューのフォント
- クリティカルCSS
- 初期表示に必要なスクリプト

❌ **非推奨：**
- すべてのリソース（逆効果）
- 実際に使わないリソース
- 低優先度のリソース

### preload vs prefetch vs preconnect

```html
<!-- preload：このページで使う（高優先度） -->
<link rel="preload" href="critical.css" as="style">

<!-- prefetch：次のページで使うかも（低優先度） -->
<link rel="prefetch" href="next-page.js">

<!-- preconnect：外部ドメインへの接続を事前確立 -->
<link rel="preconnect" href="https://fonts.googleapis.com">
```

---

## 3. Core Web Vitals の最適化

### Core Web Vitals とは

Googleが定めた、ユーザー体験を測る3つの重要指標です。

#### 1. LCP（Largest Contentful Paint）

**目標：2.5秒以内**

最大のコンテンツ要素が表示されるまでの時間。

**LCP対象要素：**
- `<img>`要素
- `<image>`内の`<svg>`要素
- `<video>`要素
- CSS背景画像
- テキストを含むブロック要素

**改善方法：**

```html
<!-- 1. LCP画像を先読み -->
<link rel="preload" href="hero.jpg" as="image">

<!-- 2. 適切なサイズを提供 -->
<img src="hero.jpg"
     srcset="hero-400.jpg 400w,
             hero-800.jpg 800w,
             hero-1200.jpg 1200w"
     sizes="100vw"
     width="1200"
     height="600"
     alt="ヒーロー画像">

<!-- 3. 遅延読み込みしない -->
<!-- loading="lazy"を付けない -->
```

**CSS最適化：**

```html
<!-- クリティカルCSSをインライン化 -->
<style>
  /* ファーストビューのスタイルのみ */
  .hero { /* ... */ }
</style>

<!-- 非クリティカルCSSを遅延読み込み -->
<link rel="preload" href="non-critical.css" as="style"
      onload="this.onload=null;this.rel='stylesheet'">
```

#### 2. FID（First Input Delay）

**目標：100ミリ秒未満**

ユーザーの最初の操作から、ブラウザが応答するまでの時間。

**改善方法：**

```html
<!-- 1. JavaScriptを最小化 -->
<script src="main.min.js" defer></script>

<!-- 2. コード分割 -->
<script type="module">
  // 必要な時だけ読み込む
  button.addEventListener('click', async () => {
    const module = await import('./heavy-feature.js');
    module.init();
  });
</script>

<!-- 3. Web Workerで重い処理を分離 -->
<script>
  const worker = new Worker('worker.js');
  worker.postMessage({/* データ */});
</script>
```

#### 3. CLS（Cumulative Layout Shift）

**目標：0.1未満**

予期しないレイアウトのズレを防止。

**改善方法：**

```html
<!-- 1. 画像にサイズを指定 -->
<img src="image.jpg"
     width="800"
     height="600"
     alt="画像">

<!-- 2. アスペクト比を保持 -->
<style>
  img {
    width: 100%;
    height: auto;
    aspect-ratio: attr(width) / attr(height);
  }
</style>

<!-- 3. フォント読み込みの最適化 -->
<link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin>

<style>
  @font-face {
    font-family: 'MyFont';
    src: url('font.woff2') format('woff2');
    font-display: swap; /* フォールバックフォントを先に表示 */
  }
</style>

<!-- 4. 広告・埋め込みに領域を確保 -->
<div style="min-height: 250px;">
  <!-- 広告が入る -->
</div>
```

### 包括的な最適化例

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>最適化されたページ</title>

  <!-- フォントの先読み -->
  <link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin>

  <!-- LCP画像の先読み -->
  <link rel="preload" href="hero.jpg" as="image">

  <!-- クリティカルCSS -->
  <style>
    /* インライン化したクリティカルCSS */
    body { margin: 0; font-family: sans-serif; }
    .hero { width: 100%; height: auto; }
  </style>

  <!-- 非クリティカルCSSの遅延読み込み -->
  <link rel="preload" href="styles.css" as="style"
        onload="this.onload=null;this.rel='stylesheet'">
  <noscript><link rel="stylesheet" href="styles.css"></noscript>
</head>
<body>
  <!-- LCP要素 -->
  <img src="hero.jpg"
       srcset="hero-400.jpg 400w, hero-800.jpg 800w, hero-1200.jpg 1200w"
       sizes="100vw"
       width="1200"
       height="600"
       alt="ヒーロー画像"
       class="hero">

  <main>
    <!-- コンテンツ -->
  </main>

  <!-- JavaScriptの遅延読み込み -->
  <script src="main.js" defer></script>
</body>
</html>
```

---

## 4. サイト高速化の10の手法

### 1. 画像の最適化

**実装：**

```html
<!-- AVIF/WebP形式 + 圧縮 + レスポンシブ -->
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg"
       srcset="image-400.jpg 400w, image-800.jpg 800w"
       sizes="(max-width: 640px) 100vw, 800px"
       width="800"
       height="600"
       loading="lazy"
       alt="画像">
</picture>
```

**効果：** 容量を50-80%削減

### 2. 不要なファイルの削除

```html
<!-- ❌ 使っていないCSS/JSを削除 -->
<!-- <link rel="stylesheet" href="unused.css"> -->
<!-- <script src="unused.js"></script> -->
```

**効果：** リクエスト数削減

### 3. ファイルの最小化（minify）

```html
<!-- 圧縮版を使用 -->
<link rel="stylesheet" href="style.min.css">
<script src="app.min.js"></script>
```

**効果：** ファイルサイズを30-40%削減

### 4. gzip/Brotli圧縮

サーバー設定で有効化：

```nginx
# Nginx設定例
gzip on;
gzip_types text/css application/javascript image/svg+xml;
```

**効果：** 転送サイズを70-80%削減

### 5. CDN の活用

```html
<!-- 外部ライブラリはCDNから -->
<script src="https://cdn.jsdelivr.net/npm/vue@3/dist/vue.global.js"></script>
```

**効果：** 配信速度の向上、サーバー負荷の軽減

### 6. ブラウザキャッシュの活用

```html
<!-- サーバー設定でキャッシュを有効化 -->
```

```nginx
# Nginx設定例
location ~* \.(jpg|jpeg|png|gif|css|js)$ {
    expires 1y;
    add_header Cache-Control "public, immutable";
}
```

**効果：** 2回目以降の表示が高速化

### 7. クリティカルレンダリングパスの最適化

```html
<head>
  <!-- 1. クリティカルCSSをインライン -->
  <style>/* ファーストビューのCSS */</style>

  <!-- 2. 非クリティカルCSSを遅延 -->
  <link rel="preload" href="rest.css" as="style"
        onload="this.rel='stylesheet'">

  <!-- 3. JSはdeferで -->
  <script src="app.js" defer></script>
</head>
```

**効果：** 初期表示の高速化

### 8. DNS Prefetch / Preconnect

```html
<!-- 外部ドメインへの接続を事前確立 -->
<link rel="dns-prefetch" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
```

**効果：** 外部リソースの読み込み高速化

### 9. HTTPリクエストの削減

```html
<!-- CSSスプライト、アイコンフォント、SVGスプライト -->
<svg style="display: none;">
  <symbol id="icon-home" viewBox="0 0 24 24">
    <path d="M10 20v-6h4v6h5v-8h3L12 3 2 12h3v8z"/>
  </symbol>
</svg>

<svg><use href="#icon-home"></use></svg>
```

**効果：** リクエスト数の大幅削減

### 10. Service Worker によるキャッシュ

```javascript
// service-worker.js
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open('v1').then((cache) => {
      return cache.addAll([
        '/',
        '/style.css',
        '/app.js',
        '/image.jpg'
      ]);
    })
  );
});

self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      return response || fetch(event.request);
    })
  );
});
```

```html
<!-- メインページで登録 -->
<script>
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/service-worker.js');
}
</script>
```

**効果：** オフライン対応、高速な再訪問

---

## 5. パフォーマンス測定

### 測定ツール

#### 1. Lighthouse

Chrome DevToolsに標準搭載。

**使い方：**
1. Chrome DevToolsを開く（F12）
2. Lighthouseタブを選択
3. 「レポートを生成」をクリック

**測定項目：**
- Performance
- Accessibility
- Best Practices
- SEO
- PWA

#### 2. PageSpeed Insights

オンラインツール：https://pagespeed.web.dev/

**特徴：**
- 実ユーザーデータ（フィールドデータ）を取得
- モバイル/デスクトップ両方を測定
- 具体的な改善提案

#### 3. WebPageTest

https://www.webpagetest.org/

**特徴：**
- 詳細な分析
- ウォーターフォールチャート
- 複数地点からの測定

#### 4. Chrome DevTools - Performance タブ

**使い方：**
1. DevToolsを開く
2. Performanceタブを選択
3. 記録開始 → ページ操作 → 停止

**確認できること：**
- タイムライン
- ネットワーク活動
- JavaScript実行時間
- レンダリング処理

### パフォーマンスバジェット

目標値を設定し、それを超えないようにする。

**例：**
- ページ総サイズ：1MB以下
- JavaScriptサイズ：300KB以下
- 画像サイズ：500KB以下
- LCP：2.5秒以内
- FID：100ms以内
- CLS：0.1未満

### モニタリング

**リアルユーザーモニタリング（RUM）：**

```javascript
// Web Vitals の測定
import {getCLS, getFID, getLCP} from 'web-vitals';

getCLS(console.log);
getFID(console.log);
getLCP(console.log);
```

**定期的な確認：**
- 週次でLighthouseスコアを確認
- デプロイ前にパフォーマンス測定
- CIに組み込んで自動チェック

---

## まとめ

パフォーマンス最適化の重要なポイント：

1. **preload**: 重要なリソースを先読み
2. **Core Web Vitals**: LCP、FID、CLSを最適化
3. **画像最適化**: 最大の改善効果
4. **ファイルサイズ削減**: minify、圧縮、不要なファイル削除
5. **クリティカルパス**: 初期表示に必要なものを優先

これらを実装することで：
- ✅ ユーザー体験が向上
- ✅ SEO評価が向上
- ✅ コンバージョン率が向上
- ✅ サーバー負荷が軽減

パフォーマンスは継続的な改善が重要です。定期的に測定し、ボトルネックを特定して改善していきましょう。

---

## 次のステップ

- [HTML画像最適化編](../01-image-optimization/README.md)を復習
- [実践的なコード例](../../../examples/)を確認する
- 自分のサイトでLighthouseを実行してみる
