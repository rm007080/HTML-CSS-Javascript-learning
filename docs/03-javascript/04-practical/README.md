# JavaScript 実践的なテクニック編

実務で役立つモダンなJavaScriptのテクニックとベストプラクティスを学びます。

## 目次

1. [モジュールシステム（type="module"）](#1-モジュールシステムtypemodule)
2. [グローバル変数の衝突回避](#2-グローバル変数の衝突回避)
3. [レスポンシブ対応のテクニック](#3-レスポンシブ対応のテクニック)
4. [パフォーマンス最適化](#4-パフォーマンス最適化)
5. [エラーハンドリングのベストプラクティス](#5-エラーハンドリングのベストプラクティス)
6. [デバッグテクニック](#6-デバッグテクニック)

---

## 1. モジュールシステム（type="module"）

ES6モジュールを使うことで、コードを整理し、名前空間の衝突を防げます。

### 基本的な使い方

#### HTMLでの読み込み

```html
<!-- type="module"を指定 -->
<script type="module" src="./main.js"></script>
```

#### エクスポート（export）

**名前付きエクスポート：**

```javascript
// utils.js
export const API_URL = "https://api.example.com";

export function formatDate(date) {
  return date.toLocaleDateString("ja-JP");
}

export function calculateTotal(items) {
  return items.reduce((sum, item) => sum + item.price, 0);
}

// または一括エクスポート
const API_URL = "https://api.example.com";
function formatDate(date) { /* ... */ }
function calculateTotal(items) { /* ... */ }

export { API_URL, formatDate, calculateTotal };
```

**デフォルトエクスポート：**

```javascript
// User.js
export default class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  greet() {
    return `こんにちは、${this.name}さん`;
  }
}
```

#### インポート（import）

**名前付きインポート：**

```javascript
// main.js
import { API_URL, formatDate, calculateTotal } from "./utils.js";

console.log(API_URL);
console.log(formatDate(new Date()));

const items = [{ price: 100 }, { price: 200 }];
console.log(calculateTotal(items)); // 300
```

**デフォルトインポート：**

```javascript
import User from "./User.js";

const user = new User("太郎", "taro@example.com");
console.log(user.greet());
```

**エイリアスを使用：**

```javascript
import { formatDate as formatJapaneseDate } from "./utils.js";

console.log(formatJapaneseDate(new Date()));
```

**すべてをインポート：**

```javascript
import * as utils from "./utils.js";

console.log(utils.API_URL);
console.log(utils.formatDate(new Date()));
```

**混在：**

```javascript
import User, { API_URL, formatDate } from "./module.js";
```

### モジュールの特徴

#### 1. スコープが独立

モジュール内の変数はグローバルスコープを汚染しません。

```javascript
// module1.js
const name = "太郎";
console.log(name); // OK

// module2.js
const name = "花子"; // エラーにならない（スコープが別）
console.log(name); // OK
```

#### 2. strictモードが自動

モジュールは自動的にstrictモードで実行されます。

```javascript
// type="module"のスクリプト内
// "use strict"; は不要
x = 10; // エラー：変数を宣言せずに使用できない
```

#### 3. deferと同じ動作

モジュールは自動的に遅延実行されます（deferと同じ）。

```html
<!-- 自動的にdefer扱い -->
<script type="module" src="main.js"></script>
```

#### 4. CORSが必要

ローカルファイル（file://）ではモジュールが動作しません。ローカルサーバーが必要です。

```bash
# 簡易サーバーの起動例
python -m http.server 8000
# または
npx http-server
```

### 動的インポート

実行時に必要に応じてモジュールを読み込めます。

```javascript
// ボタンクリック時に読み込み
document.getElementById("loadButton").addEventListener("click", async () => {
  const { default: User } = await import("./User.js");
  const user = new User("太郎", "taro@example.com");
  console.log(user.greet());
});

// 条件付きで読み込み
if (condition) {
  const module = await import("./heavy-module.js");
  module.initialize();
}
```

---

## 2. グローバル変数の衝突回避

複数のJavaScriptファイルを読み込む場合、変数名の衝突を避ける方法を学びます。

### 問題の例

```html
<!-- common.js -->
<script src="/js/common.js"></script>
<!-- 内容: const name = "共通"; -->

<!-- page.js -->
<script src="/js/page.js"></script>
<!-- 内容: const name = "ページ"; -->
<!-- エラー：nameが既に宣言されている -->
```

### 解決策1: type="module"を使用（推奨）

```html
<!-- スコープが独立するため衝突しない -->
<script src="/js/common.js"></script>
<script type="module" src="/js/page.js"></script>
```

**page.js:**
```javascript
// モジュールスコープ内なので衝突しない
const name = "ページ";
console.log(name); // "ページ"
```

### 解決策2: 即時関数（IIFE）

```javascript
// common.js
(function() {
  const name = "共通";
  console.log(name);
})();

// page.js
(function() {
  const name = "ページ";
  console.log(name);
})();
```

### 解決策3: 名前空間オブジェクト

```javascript
// グローバルに1つだけオブジェクトを作成
const MyApp = {};

// common.js
MyApp.common = {
  name: "共通",
  init: function() {
    console.log(this.name);
  }
};

// page.js
MyApp.page = {
  name: "ページ",
  init: function() {
    console.log(this.name);
  }
};

// 使用
MyApp.common.init();
MyApp.page.init();
```

### 解決策4: ブロックスコープ

```javascript
// ブロックで囲む
{
  const name = "ブロック1";
  console.log(name);
}

{
  const name = "ブロック2"; // エラーにならない
  console.log(name);
}
```

---

## 3. レスポンシブ対応のテクニック

### Viewport Extraライブラリ

小さい画面（320px以下）でレイアウトが崩れるのを防ぐライブラリです。

#### 問題

iPhone 8（375px）向けにデザインしたサイトが、iPhone SE（320px）で崩れる。

#### 解決策

Viewport Extraを使用すると、最小幅を設定し、それ以下の端末では縮小表示されます。

```html
<!DOCTYPE html>
<html>
<head>
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <meta name="viewport-extra" content="min-width=375">
  <script async src="https://cdn.jsdelivr.net/npm/viewport-extra@2.0.1/dist/iife/viewport-extra.min.js"></script>
</head>
<body>
  <!-- 375px未満の端末では縮小表示される -->
</body>
</html>
```

#### NPMでの使用

```bash
npm install viewport-extra
```

```javascript
import ViewportExtra from "viewport-extra";

ViewportExtra.create({
  minWidth: 375,
  maxWidth: 1920
});
```

### コンテナクエリ（モダンな手法）

親要素のサイズに応じてスタイルを変更できます。

```css
.card-container {
  container-type: inline-size;
}

@container (min-width: 400px) {
  .card {
    display: grid;
    grid-template-columns: 1fr 2fr;
  }
}
```

### ビューポート単位

```css
/* ビューポートの幅/高さに対する割合 */
.hero {
  width: 100vw;  /* ビューポート幅の100% */
  height: 100vh; /* ビューポート高さの100% */
}

/* 小さい方/大きい方 */
.box {
  width: 50vmin;  /* vwとvhの小さい方の50% */
  height: 50vmax; /* vwとvhの大きい方の50% */
}
```

---

## 4. パフォーマンス最適化

### 1. デバウンス（Debounce）

連続するイベントを制御し、最後の呼び出しのみ実行します。

```javascript
function debounce(func, delay = 300) {
  let timeoutId;
  return function(...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => {
      func.apply(this, args);
    }, delay);
  };
}

// 使用例：検索入力
const searchInput = document.getElementById("search");
const handleSearch = debounce((value) => {
  console.log("検索:", value);
  // API呼び出しなど
}, 500);

searchInput.addEventListener("input", (e) => {
  handleSearch(e.target.value);
});
```

### 2. スロットル（Throttle）

一定時間に1回だけ実行します。

```javascript
function throttle(func, delay = 300) {
  let lastCall = 0;
  return function(...args) {
    const now = Date.now();
    if (now - lastCall >= delay) {
      lastCall = now;
      func.apply(this, args);
    }
  };
}

// 使用例：スクロールイベント
const handleScroll = throttle(() => {
  console.log("スクロール位置:", window.scrollY);
}, 200);

window.addEventListener("scroll", handleScroll);
```

### 3. 遅延評価（Lazy Evaluation）

必要になるまで処理を遅延します。

```javascript
class LazyValue {
  constructor(initializer) {
    this.initializer = initializer;
    this.cached = null;
  }

  get value() {
    if (this.cached === null) {
      console.log("初回計算実行");
      this.cached = this.initializer();
    }
    return this.cached;
  }
}

// 使用例
const expensiveCalculation = new LazyValue(() => {
  // 重い計算
  let sum = 0;
  for (let i = 0; i < 1000000; i++) {
    sum += i;
  }
  return sum;
});

console.log("値を取得する前");
console.log(expensiveCalculation.value); // ここで初めて計算
console.log(expensiveCalculation.value); // キャッシュから取得
```

### 4. requestAnimationFrame

DOM操作やアニメーションの最適化。

```javascript
// ❌ 非効率
window.addEventListener("scroll", () => {
  document.querySelector(".header").style.top = window.scrollY + "px";
});

// ✅ 効率的
let ticking = false;

window.addEventListener("scroll", () => {
  if (!ticking) {
    requestAnimationFrame(() => {
      document.querySelector(".header").style.top = window.scrollY + "px";
      ticking = false;
    });
    ticking = true;
  }
});
```

### 5. Web Workers（重い処理をバックグラウンドで）

```javascript
// worker.js
self.addEventListener("message", (e) => {
  const result = heavyCalculation(e.data);
  self.postMessage(result);
});

function heavyCalculation(data) {
  // 重い計算
  let sum = 0;
  for (let i = 0; i < data; i++) {
    sum += i;
  }
  return sum;
}
```

```javascript
// main.js
const worker = new Worker("worker.js");

worker.postMessage(1000000);

worker.addEventListener("message", (e) => {
  console.log("計算結果:", e.data);
});
```

---

## 5. エラーハンドリングのベストプラクティス

### カスタムエラークラス

```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = "ValidationError";
  }
}

class NetworkError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.name = "NetworkError";
    this.statusCode = statusCode;
  }
}

// 使用例
function validateEmail(email) {
  if (!email.includes("@")) {
    throw new ValidationError("無効なメールアドレスです");
  }
}

try {
  validateEmail("invalid-email");
} catch (error) {
  if (error instanceof ValidationError) {
    console.error("バリデーションエラー:", error.message);
  } else {
    console.error("予期しないエラー:", error);
  }
}
```

### グローバルエラーハンドリング

```javascript
// 未処理のエラーをキャッチ
window.addEventListener("error", (event) => {
  console.error("エラーが発生しました:", event.error);
  // エラーログをサーバーに送信
  logErrorToServer({
    message: event.error.message,
    stack: event.error.stack,
    url: window.location.href
  });
});

// Promiseの未処理のrejectをキャッチ
window.addEventListener("unhandledrejection", (event) => {
  console.error("未処理のPromise reject:", event.reason);
  logErrorToServer({
    message: event.reason,
    url: window.location.href
  });
});
```

### エラーバウンダリ（React風）

```javascript
class ErrorBoundary {
  constructor(callback) {
    this.callback = callback;
  }

  async execute(fn) {
    try {
      return await fn();
    } catch (error) {
      this.callback(error);
      return null;
    }
  }
}

// 使用例
const boundary = new ErrorBoundary((error) => {
  console.error("エラーが発生しました:", error);
  document.getElementById("error").textContent = "エラーが発生しました";
});

boundary.execute(async () => {
  const response = await fetch("/api/data");
  const data = await response.json();
  renderData(data);
});
```

---

## 6. デバッグテクニック

### console のテクニック

```javascript
// 通常のログ
console.log("メッセージ");

// スタイル付きログ
console.log("%c重要なメッセージ", "color: red; font-size: 20px; font-weight: bold");

// テーブル表示
const users = [
  { name: "太郎", age: 25 },
  { name: "花子", age: 30 }
];
console.table(users);

// グループ化
console.group("ユーザー情報");
console.log("名前:", "太郎");
console.log("年齢:", 25);
console.groupEnd();

// 時間計測
console.time("処理時間");
// 重い処理
for (let i = 0; i < 1000000; i++) {}
console.timeEnd("処理時間"); // 処理時間: 10ms

// アサーション
console.assert(1 === 2, "1は2と等しくありません");

// スタックトレース
console.trace("ここに至った経路");
```

### デバッガの使用

```javascript
function calculateTotal(items) {
  debugger; // ここで実行が停止
  return items.reduce((sum, item) => sum + item.price, 0);
}
```

### パフォーマンス計測

```javascript
// Performance API
const start = performance.now();

// 計測したい処理
for (let i = 0; i < 1000000; i++) {}

const end = performance.now();
console.log(`実行時間: ${end - start}ms`);

// マーク&メジャー
performance.mark("start");
// 処理
performance.mark("end");
performance.measure("処理時間", "start", "end");

const measure = performance.getEntriesByName("処理時間")[0];
console.log(`処理時間: ${measure.duration}ms`);
```

### メモリリークの検出

```javascript
// ❌ メモリリーク
let largeArray = [];
setInterval(() => {
  largeArray.push(new Array(1000000));
}, 1000);

// ✅ 適切な管理
let intervalId = setInterval(() => {
  console.log("実行中");
}, 1000);

// 不要になったら停止
function cleanup() {
  clearInterval(intervalId);
  intervalId = null;
}
```

---

## まとめ

実践的なテクニックの重要なポイント：

1. **モジュールシステム**: コードの整理と名前空間の管理
2. **衝突回避**: type="module"やIIFEの活用
3. **レスポンシブ**: matchMedia、Viewport Extraの活用
4. **パフォーマンス**: debounce、throttle、遅延評価
5. **エラーハンドリング**: カスタムエラー、グローバルハンドリング
6. **デバッグ**: console、debugger、Performance APIの活用

これらのテクニックを実践することで、プロフェッショナルなJavaScriptコードが書けるようになります。

---

## 次のステップ

- [JavaScript基礎編](../01-basics/README.md)を復習
- [実践的なコード例](../../../examples/)を確認する
- 実際のプロジェクトで応用する
