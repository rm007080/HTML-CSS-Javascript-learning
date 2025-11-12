# JavaScript DOM操作とイベント処理編

DOM操作、イベント処理、そしてモダンなブラウザAPIについて学びます。

## 目次

1. [スクリプトタグの読み込みタイミング](#1-スクリプトタグの読み込みタイミング)
2. [DOMContentLoaded と load イベント](#2-domcontentloaded-と-load-イベント)
3. [Intersection Observer API](#3-intersection-observer-api)
4. [matchMedia API](#4-matchmedia-api)
5. [イベント処理のベストプラクティス](#5-イベント処理のベストプラクティス)

---

## 1. スクリプトタグの読み込みタイミング

JavaScriptの読み込み方法によって、ページのパフォーマンスと動作が大きく変わります。

### 3つの読み込み方法

#### 1. デフォルト（同期的読み込み）

```html
<!DOCTYPE html>
<html>
<head>
  <script src="./script.js"></script>
</head>
<body>
  <h1>こんにちは</h1>
</body>
</html>
```

**動作：**
- スクリプトに到達すると、HTMLのパースが**停止**
- スクリプトのダウンロードと実行が完了するまで待機
- その後、HTMLのパースを再開

**問題点：**
- ページの表示が遅れる
- 大きなスクリプトがあると、画面が真っ白な時間が長くなる

#### 2. async 属性（非同期読み込み）

```html
<script src="./script.js" async></script>
```

**動作：**
- スクリプトを**バックグラウンドでダウンロード**
- ダウンロード完了後、**即座に実行**
- 実行中はHTMLのパースが停止

**特徴：**
- 複数のスクリプトの**実行順序は保証されない**
- ダウンロードが速く完了したものから実行される

**用途：**
- 他のスクリプトに依存しない独立したスクリプト
- Google Analyticsなどのトラッキングスクリプト

```html
<!-- 実行順序は不定 -->
<script src="analytics.js" async></script>
<script src="tracking.js" async></script>
```

#### 3. defer 属性（遅延読み込み・推奨）

```html
<script src="./script.js" defer></script>
```

**動作：**
- スクリプトを**バックグラウンドでダウンロード**
- HTMLのパースが完了するまで実行を**待機**
- DOMContentLoadedイベントの**直前に実行**

**特徴：**
- 複数のスクリプトの**実行順序が保証される**
- 記述した順番に実行される

**用途（推奨）：**
- DOM操作を行うスクリプト
- 他のスクリプトに依存するスクリプト
- ほとんどの通常のJavaScriptファイル

```html
<!-- 確実に順番通りに実行される -->
<script src="library.js" defer></script>
<script src="main.js" defer></script>
```

### 比較表

| 属性 | HTMLパース | ダウンロード | 実行タイミング | 実行順序 |
|------|----------|------------|--------------|---------|
| なし | 停止 | 同期的 | 即座 | 保証 |
| async | 継続 | 非同期 | ダウンロード完了後すぐ | 不定 |
| defer | 継続 | 非同期 | HTMLパース完了後 | 保証 |

### ベストプラクティス

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>ページタイトル</title>
  <!-- CSSは通常通りheadに配置 -->
  <link rel="stylesheet" href="style.css">

  <!-- defer属性を付けてheadに配置（推奨） -->
  <script src="script.js" defer></script>
</head>
<body>
  <h1>こんにちは</h1>
  <!-- スクリプトはここに書かなくてもOK -->
</body>
</html>
```

**理由：**
1. HTMLパースをブロックしない
2. DOM構築完了後に確実に実行される
3. 複数ファイルの実行順序が保証される

---

## 2. DOMContentLoaded と load イベント

ページ読み込みに関する2つの重要なイベントがあります。

### DOMContentLoaded

**発火タイミング：** HTMLのパースが完了した直後

```javascript
document.addEventListener("DOMContentLoaded", () => {
  console.log("DOMの準備完了");
  // DOM要素にアクセス可能
  const heading = document.querySelector("h1");
  console.log(heading.textContent);
});
```

**特徴：**
- 画像、CSS、サブフレームの読み込みを**待たない**
- DOM要素へのアクセスが可能になった時点で発火
- ページの初期化処理に最適

### load イベント

**発火タイミング：** すべてのリソース（画像、CSS、JSなど）の読み込みが完了した後

```javascript
window.addEventListener("load", () => {
  console.log("すべてのリソース読み込み完了");
  // 画像のサイズなども確定している
  const img = document.querySelector("img");
  console.log(img.naturalWidth, img.naturalHeight);
});
```

**特徴：**
- すべてのリソースの読み込みを**待つ**
- 画像のサイズ計算などが必要な場合に使用

### 比較

```html
<!DOCTYPE html>
<html>
<head>
  <script>
    console.log("1: scriptタグ実行");

    document.addEventListener("DOMContentLoaded", () => {
      console.log("3: DOMContentLoaded");
    });

    window.addEventListener("load", () => {
      console.log("4: load");
    });
  </script>
</head>
<body>
  <h1>こんにちは</h1>
  <img src="large-image.jpg" alt="大きな画像">
  <script>
    console.log("2: body内のscript実行");
  </script>
</body>
</html>
```

**出力順序：**
```
1: scriptタグ実行
2: body内のscript実行
3: DOMContentLoaded  ← HTMLパース完了
4: load              ← 画像など全リソース読み込み完了
```

### defer を使用する場合

```html
<script src="script.js" defer></script>
```

deferを使用すると、DOMContentLoadedの直前に実行されるため、多くの場合イベントリスナーは不要です。

```javascript
// defer付きスクリプト内
// すでにDOMは利用可能
console.log("DOMにアクセス可能");
const heading = document.querySelector("h1");
console.log(heading.textContent);
```

---

## 3. Intersection Observer API

Intersection Observer APIは、要素がビューポート（画面）に入ったかどうかを効率的に監視できるAPIです。

### 従来の方法の問題点

スクロールイベントを使った監視は、パフォーマンスに問題がありました。

```javascript
// ❌ 非推奨：スクロールイベントは頻繁に発火する
window.addEventListener("scroll", () => {
  const element = document.querySelector(".target");
  const rect = element.getBoundingClientRect();

  // ビューポート内にあるかチェック
  if (rect.top < window.innerHeight && rect.bottom > 0) {
    console.log("画面内に表示されています");
  }
});
```

**問題点：**
- スクロールのたびに実行される（パフォーマンス低下）
- ビューポートサイズ変更時の再計算が必要
- コードが複雑になる

### Intersection Observer の基本

```javascript
// オプション設定
const options = {
  root: null,        // nullの場合はビューポート
  rootMargin: "0px", // 判定範囲の調整（CSSのmarginと同じ書式）
  threshold: 0       // 交差割合（0〜1）
};

// コールバック関数
const callback = (entries, observer) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      console.log("要素が画面内に入りました");
      console.log(entry.target);
    } else {
      console.log("要素が画面外に出ました");
    }
  });
};

// Observerを作成
const observer = new IntersectionObserver(callback, options);

// 監視対象の要素を指定
const target = document.querySelector(".target");
observer.observe(target);

// 監視を停止する場合
// observer.unobserve(target);
// observer.disconnect(); // すべての監視を停止
```

### オプションの詳細

#### root

監視の基準となる要素を指定します。

```javascript
// ビューポートを基準（デフォルト）
{ root: null }

// 特定の要素を基準
const container = document.querySelector(".container");
{ root: container }
```

#### rootMargin

判定範囲を拡張・縮小します。**単位が必須**です。

```javascript
// 上下50px早めに判定
{ rootMargin: "50px 0px" }

// 画面中央でのみ判定（上下50%縮小）
{ rootMargin: "-50% 0px" }

// 上：100px、右：0px、下：-100px、左：0px
{ rootMargin: "100px 0px -100px 0px" }
```

#### threshold

要素の何%が見えたら発火するかを指定します。

```javascript
// 1pxでも見えたら発火
{ threshold: 0 }

// 50%見えたら発火
{ threshold: 0.5 }

// 完全に見えたら発火
{ threshold: 1.0 }

// 複数の閾値を指定
{ threshold: [0, 0.25, 0.5, 0.75, 1.0] }
```

### 実用例1：フェードインアニメーション

```html
<style>
.fade-in {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.6s, transform 0.6s;
}

.fade-in.visible {
  opacity: 1;
  transform: translateY(0);
}
</style>

<script>
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add("visible");
      // 一度表示したら監視を停止（再度フェードインさせない）
      observer.unobserve(entry.target);
    }
  });
}, {
  threshold: 0.1 // 10%見えたら発火
});

// すべての .fade-in 要素を監視
document.querySelectorAll(".fade-in").forEach(el => {
  observer.observe(el);
});
</script>
```

### 実用例2：遅延画像読み込み（Lazy Loading）

```html
<img data-src="image1.jpg" alt="画像1" class="lazy">
<img data-src="image2.jpg" alt="画像2" class="lazy">
<img data-src="image3.jpg" alt="画像3" class="lazy">

<script>
const imageObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src; // data-srcからsrcへ
      img.classList.remove("lazy");
      imageObserver.unobserve(img); // 読み込んだら監視停止
    }
  });
}, {
  rootMargin: "50px" // 画面に近づいたら読み込み開始
});

document.querySelectorAll(".lazy").forEach(img => {
  imageObserver.observe(img);
});
</script>
```

**注意：** 現代のブラウザでは、`<img loading="lazy">`が標準でサポートされています。

```html
<!-- モダンな方法 -->
<img src="image.jpg" loading="lazy" alt="画像">
```

### 実用例3：無限スクロール

```html
<div id="content">
  <!-- コンテンツ -->
</div>
<div id="sentinel" style="height: 20px;"></div>

<script>
let page = 1;

const sentinelObserver = new IntersectionObserver(async (entries) => {
  const entry = entries[0];
  if (entry.isIntersecting) {
    console.log("さらに読み込み中...");

    // 次のページを読み込み
    const data = await fetchNextPage(page);
    renderContent(data);
    page++;
  }
});

sentinelObserver.observe(document.getElementById("sentinel"));

async function fetchNextPage(page) {
  const response = await fetch(`/api/posts?page=${page}`);
  return await response.json();
}

function renderContent(data) {
  const content = document.getElementById("content");
  data.forEach(item => {
    const div = document.createElement("div");
    div.textContent = item.title;
    content.appendChild(div);
  });
}
</script>
```

### 実用例4：目次のハイライト

```html
<nav id="toc">
  <a href="#section1" data-target="section1">セクション1</a>
  <a href="#section2" data-target="section2">セクション2</a>
  <a href="#section3" data-target="section3">セクション3</a>
</nav>

<section id="section1">セクション1の内容</section>
<section id="section2">セクション2の内容</section>
<section id="section3">セクション3の内容</section>

<script>
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    const id = entry.target.id;
    const tocLink = document.querySelector(`[data-target="${id}"]`);

    if (entry.isIntersecting) {
      // すべてのリンクからactiveを削除
      document.querySelectorAll("#toc a").forEach(a => {
        a.classList.remove("active");
      });
      // 現在のセクションをハイライト
      tocLink.classList.add("active");
    }
  });
}, {
  rootMargin: "-50% 0px" // 画面中央を基準
});

// すべてのセクションを監視
document.querySelectorAll("section").forEach(section => {
  observer.observe(section);
});
</script>
```

---

## 4. matchMedia API

matchMediaは、CSSメディアクエリの条件をJavaScriptで判定し、変化を監視できるAPIです。

### 従来の問題点

resizeイベントを使った監視は、頻繁に実行されパフォーマンスが低下します。

```javascript
// ❌ 非推奨：リサイズのたびに実行される
window.addEventListener("resize", () => {
  if (window.innerWidth >= 768) {
    console.log("タブレット以上");
  } else {
    console.log("モバイル");
  }
});
```

### matchMedia の基本

```javascript
// メディアクエリを作成
const mediaQuery = window.matchMedia("(min-width: 768px)");

console.log(mediaQuery.matches); // true/false

// 変化を監視
mediaQuery.addEventListener("change", (e) => {
  if (e.matches) {
    console.log("768px以上になりました");
  } else {
    console.log("768px未満になりました");
  }
});
```

### MediaQueryList オブジェクト

```javascript
const mql = window.matchMedia("(min-width: 768px)");

console.log(mql.matches); // true/false（現在マッチしているか）
console.log(mql.media);   // "(min-width: 768px)"（クエリ文字列）
```

### レスポンシブ対応の実装

```javascript
// ブレークポイント定義
const breakpoints = {
  mobile: window.matchMedia("(max-width: 767px)"),
  tablet: window.matchMedia("(min-width: 768px) and (max-width: 1023px)"),
  desktop: window.matchMedia("(min-width: 1024px)")
};

function handleBreakpointChange() {
  if (breakpoints.mobile.matches) {
    console.log("モバイル表示");
    initMobileMenu();
  } else if (breakpoints.tablet.matches) {
    console.log("タブレット表示");
    initTabletMenu();
  } else if (breakpoints.desktop.matches) {
    console.log("デスクトップ表示");
    initDesktopMenu();
  }
}

// 初回実行
handleBreakpointChange();

// 変化を監視
Object.values(breakpoints).forEach(mq => {
  mq.addEventListener("change", handleBreakpointChange);
});
```

### 実用例1：レスポンシブ画像の切り替え

```javascript
const mobileQuery = window.matchMedia("(max-width: 767px)");
const img = document.querySelector("#hero-image");

function updateImage(mq) {
  if (mq.matches) {
    img.src = "hero-mobile.jpg";
  } else {
    img.src = "hero-desktop.jpg";
  }
}

// 初回設定
updateImage(mobileQuery);

// 変化を監視
mobileQuery.addEventListener("change", updateImage);
```

### 実用例2：ダークモードの検出

```javascript
const darkModeQuery = window.matchMedia("(prefers-color-scheme: dark)");

function handleDarkModeChange(e) {
  if (e.matches) {
    document.body.classList.add("dark-mode");
    console.log("ダークモードが有効");
  } else {
    document.body.classList.remove("dark-mode");
    console.log("ライトモードが有効");
  }
}

// 初回設定
handleDarkModeChange(darkModeQuery);

// 変化を監視
darkModeQuery.addEventListener("change", handleDarkModeChange);
```

### 実用例3：印刷スタイルの検出

```javascript
const printQuery = window.matchMedia("print");

printQuery.addEventListener("change", (e) => {
  if (e.matches) {
    console.log("印刷が開始されました");
    // 印刷用の処理
  } else {
    console.log("印刷がキャンセルされました");
  }
});
```

### 実用例4：オリエンテーション（縦横）の検出

```javascript
const portraitQuery = window.matchMedia("(orientation: portrait)");

function handleOrientationChange(e) {
  if (e.matches) {
    console.log("縦向き");
  } else {
    console.log("横向き");
  }
}

handleOrientationChange(portraitQuery);
portraitQuery.addEventListener("change", handleOrientationChange);
```

### addListener vs addEventListener

**注意：** `addListener()` は非推奨です。必ず `addEventListener()` を使用してください。

```javascript
const mq = window.matchMedia("(min-width: 768px)");

// ❌ 非推奨
// mq.addListener(handler);

// ✅ 推奨
mq.addEventListener("change", handler);
```

---

## 5. イベント処理のベストプラクティス

### イベント委譲（Event Delegation）

多数の要素にイベントリスナーを追加する代わりに、親要素で監視します。

```javascript
// ❌ 非効率：各要素にリスナーを追加
document.querySelectorAll(".item").forEach(item => {
  item.addEventListener("click", (e) => {
    console.log(e.target.textContent);
  });
});

// ✅ 効率的：親要素で監視
document.querySelector(".list").addEventListener("click", (e) => {
  if (e.target.classList.contains("item")) {
    console.log(e.target.textContent);
  }
});
```

**利点：**
- パフォーマンスが向上
- 動的に追加される要素にも対応

### イベントリスナーの削除

```javascript
function handleClick() {
  console.log("クリックされました");
}

const button = document.querySelector("button");

// リスナーを追加
button.addEventListener("click", handleClick);

// リスナーを削除
button.removeEventListener("click", handleClick);
```

**注意：** アロー関数や無名関数は削除できません。

```javascript
// ❌ 削除できない
button.addEventListener("click", () => {
  console.log("クリック");
});
// removeEventListener で同じ関数を指定できない

// ✅ 削除可能
const handler = () => console.log("クリック");
button.addEventListener("click", handler);
button.removeEventListener("click", handler);
```

### once オプション（1回だけ実行）

```javascript
button.addEventListener("click", () => {
  console.log("初回のみ実行");
}, { once: true });
// 1回実行後、自動的にリスナーが削除される
```

### passive オプション（スクロールパフォーマンス）

```javascript
// スクロールイベントで preventDefault() を使わない場合
document.addEventListener("scroll", (e) => {
  // preventDefault() を呼ばない
  console.log("スクロール中");
}, { passive: true });
```

---

## まとめ

DOM操作とイベント処理の重要なポイント：

1. **defer属性**: ほとんどの場合、スクリプトにはdeferを使用
2. **DOMContentLoaded**: DOM準備完了時の処理
3. **Intersection Observer**: 要素の表示監視（スクロール連動）
4. **matchMedia**: レスポンシブ対応の実装
5. **イベント委譲**: 効率的なイベント処理

これらの技術を使うことで、パフォーマンスが高く、保守性の良いコードを書くことができます。

---

## 次のステップ

- [実践的なテクニック編](../04-practical/README.md)へ進む
- [実践的なコード例](../../../examples/)を確認する
