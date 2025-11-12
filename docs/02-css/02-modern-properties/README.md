# CSS モダンプロパティ編

このセクションでは、モダンCSSで利用可能な新しいプロパティや単位について学びます。これらの機能を使うことで、JavaScriptに頼らず、より柔軟で保守性の高いスタイリングが実現できます。

## 目次

1. [ビューポート単位（svh, dvh, lvh）](#ビューポート単位svh-dvh-lvh)
2. [clamp関数による流動的なサイズ指定](#clamp関数による流動的なサイズ指定)
3. [currentColorの活用](#currentcolorの活用)
4. [テキストの折り返し制御](#テキストの折り返し制御)
5. [論理プロパティ](#論理プロパティ)

---

## ビューポート単位（svh, dvh, lvh）

### 従来の100vhの問題

`100vh`を使って画面いっぱいに要素を表示しようとすると、モバイルブラウザで問題が発生します。

```css
/* 問題のあるコード */
.hero {
  height: 100vh;
}
```

**問題点:**
- iOS SafariやAndroid Chromeでは、アドレスバーの表示/非表示でビューポートの高さが変わる
- スクロール時に不要な縦スクロールが発生する
- ユーザー体験が悪化する

### 新しいビューポート単位

CSS 2022で導入された新しい単位により、この問題が解決されました。

| 単位 | 正式名称 | 説明 |
|-----|---------|------|
| `svh` | Small Viewport Height | ビューポートが**最小**時の高さ |
| `lvh` | Large Viewport Height | ビューポートが**最大**時の高さ |
| `dvh` | Dynamic Viewport Height | **現在の**ビューポート高さ |

同様に、幅にも対応する単位があります：
- `svw`, `lvw`, `dvw`（幅）
- `svmin`, `lvmin`, `dvmin`（高さと幅の小さい方）
- `svmax`, `lvmax`, `dvmax`（高さと幅の大きい方）

### 推奨される実装

#### ヒーローイメージ（推奨）

```css
.hero {
  height: 100svh;  /* Small Viewport Height */
}
```

**svhを使う理由:**
- アドレスバーが表示されている状態（最小ビューポート）でも画面いっぱいに表示
- スクロール時にコンテンツがはみ出ない
- ほとんどのケースでこれが最適

#### モーダルウィンドウ

```css
.modal {
  height: 100dvh;  /* Dynamic Viewport Height */
  overflow: auto;
}
```

**dvhを使う理由:**
- スクロール時にビューポートサイズが変わっても追従
- モーダルが常に画面いっぱいに表示される

#### 実装例：ヒーローセクション

```html
<section class="hero">
  <div class="hero-content">
    <h1>タイトル</h1>
    <p>説明文</p>
    <button>詳細を見る</button>
  </div>
</section>
```

```css
.hero {
  height: 100svh;
  display: grid;
  place-content: center;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  text-align: center;
}

.hero-content {
  padding: 20px;
}
```

### 従来の回避策との比較

#### 100%の問題

```css
/* 非推奨: 祖先要素すべてに高さを設定する必要がある */
html, body {
  height: 100%;
}

.container {
  height: 100%;
}

.hero {
  height: 100%;
}
```

煩雑で保守性が低い。

#### -webkit-fill-availableの問題

```css
/* 非推奨: Safariのみ対応 */
.hero {
  height: -webkit-fill-available;
}
```

ChromeやFirefoxで動作しない。

#### 新しい単位の利点

```css
/* 推奨: シンプルで全ブラウザ対応 */
.hero {
  height: 100svh;
}
```

- 1行で完結
- すべてのモダンブラウザで動作
- 意図が明確

### ブラウザサポート

- Chrome 108以降
- Safari 15.4以降
- Firefox 101以降
- Edge 108以降

2023年以降、全主要ブラウザで使用可能です。

---

## clamp関数による流動的なサイズ指定

### clamp関数とは

`clamp()`関数は、最小値、推奨値、最大値を指定して、ビューポートサイズに応じた流動的なサイズ指定を実現します。

```css
clamp(最小値, 推奨値, 最大値)
```

**動作:**
- ビューポートが小さい場合：最小値を使用
- ビューポートが中間の場合：推奨値（可変）を使用
- ビューポートが大きい場合：最大値を使用

### 基本的な使用例

#### レスポンシブなフォントサイズ

```css
h1 {
  font-size: clamp(1.5rem, 5vw, 3rem);
}
```

**説明:**
- 最小サイズ: `1.5rem`（24px）
- 推奨サイズ: ビューポート幅の5%
- 最大サイズ: `3rem`（48px）

ビューポートサイズに応じて、24px〜48pxの範囲で滑らかにスケールします。

#### レスポンシブな余白

```css
.section {
  padding-block: clamp(2rem, 5vh, 5rem);
}
```

画面の高さに応じてパディングが変化します。

#### レスポンシブな幅

```css
.container {
  width: clamp(320px, 90vw, 1200px);
  margin-inline: auto;
}
```

- 最小幅: 320px
- 推奨幅: ビューポート幅の90%
- 最大幅: 1200px

### 線形補間の計算

より細かい制御が必要な場合、線形補間式を使用します。

```css
font-size: clamp(1rem, 0.818rem + 0.91vw, 1.5rem);
```

**計算式の構造:**
```
推奨値 = 基本値 + (ビューポート幅に応じた可変値)
```

### 実用的なパターン

#### タイポグラフィシステム

```css
:root {
  --font-size-sm: clamp(0.875rem, 0.8rem + 0.375vw, 1rem);
  --font-size-base: clamp(1rem, 0.9rem + 0.5vw, 1.25rem);
  --font-size-lg: clamp(1.25rem, 1.1rem + 0.75vw, 1.75rem);
  --font-size-xl: clamp(1.5rem, 1.2rem + 1.5vw, 2.5rem);
  --font-size-2xl: clamp(2rem, 1.5rem + 2.5vw, 3.5rem);
}

body {
  font-size: var(--font-size-base);
}

h1 {
  font-size: var(--font-size-2xl);
}

h2 {
  font-size: var(--font-size-xl);
}
```

#### スペーシングシステム

```css
:root {
  --space-xs: clamp(0.5rem, 0.4rem + 0.5vw, 0.75rem);
  --space-sm: clamp(0.75rem, 0.6rem + 0.75vw, 1.25rem);
  --space-md: clamp(1rem, 0.8rem + 1vw, 1.75rem);
  --space-lg: clamp(1.5rem, 1.2rem + 1.5vw, 2.5rem);
  --space-xl: clamp(2rem, 1.5rem + 2.5vw, 4rem);
}

.section {
  padding-block: var(--space-xl);
  gap: var(--space-md);
}
```

### 計算ツールの活用

手動で計算するのが難しい場合は、以下のツールを使用できます：

- [Min-Max-Value Interpolation Calculator](https://min-max-calculator.9elements.com/)

**使い方:**
1. 最小値と最大値を入力（pxまたはrem）
2. ビューポート範囲を指定（例: 320px〜1920px）
3. clamp()式が自動生成される

### メディアクエリとの使い分け

#### メディアクエリ（段階的な変化）

```css
.title {
  font-size: 1.5rem;
}

@media (width >= 768px) {
  .title {
    font-size: 2rem;
  }
}

@media (width >= 1024px) {
  .title {
    font-size: 2.5rem;
  }
}
```

ブレークポイントで急激に変化します。

#### clamp（滑らかな変化）

```css
.title {
  font-size: clamp(1.5rem, 3vw, 2.5rem);
}
```

ビューポートサイズに応じて滑らかに変化します。

**どちらを使うべきか:**
- デザインの変更が必要な場合：メディアクエリ
- サイズのみを調整したい場合：clamp

---

## currentColorの活用

### currentColorとは

`currentColor`は、要素の`color`プロパティの値を他のプロパティで参照できるCSS変数のようなキーワードです。

```css
.element {
  color: red;
  border: 1px solid currentColor;  /* border色もredになる */
}
```

### 基本的な使用例

#### ボーダーの色を自動継承

```css
.button {
  color: #3498db;
  border: 2px solid currentColor;
}

.button:hover {
  color: #2980b9;  /* ボーダーの色も自動的に変わる */
}
```

#### box-shadowの色を継承

```css
.card {
  color: #2c3e50;
  box-shadow: 0 4px 6px currentColor;
}
```

#### 背景色として使用

```css
.badge {
  color: #e74c3c;
  background-color: currentColor;
  color: white;  /* 上書き */
}
```

### 実用的なパターン

#### SVGアイコンの色制御

```html
<button class="icon-button">
  <svg width="24" height="24">
    <path fill="currentColor" d="M12 2L2 7..."/>
  </svg>
  ボタン
</button>
```

```css
.icon-button {
  color: #3498db;
  display: flex;
  align-items: center;
  gap: 8px;
}

.icon-button:hover {
  color: #2980b9;  /* アイコンとテキストの色が同時に変わる */
}

/* SVGの色はcurrentColorで自動的にテキスト色と揃う */
```

これにより、複数の色でアイコンを使い回せます。

#### リンクのアンダーライン

```css
a {
  color: #3498db;
  text-decoration: underline;
  text-decoration-color: currentColor;
  text-decoration-thickness: 2px;
}

a:hover {
  color: #2980b9;
  /* アンダーラインの色も自動的に変わる */
}
```

#### カスタムフォーカスリング

```css
button {
  color: #3498db;
}

button:focus-visible {
  outline: 3px solid currentColor;
  outline-offset: 2px;
}
```

### currentColorの利点

1. **DRY原則の実現**
   色の指定が一箇所で済む

2. **保守性の向上**
   色を変更する際、`color`プロパティのみを修正すればよい

3. **一貫性の担保**
   テキストとアイコン、ボーダーなどの色が自動的に揃う

### CSS変数との組み合わせ

```css
:root {
  --color-primary: #3498db;
  --color-secondary: #2ecc71;
  --color-danger: #e74c3c;
}

.button-primary {
  color: var(--color-primary);
  border: 2px solid currentColor;
}

.button-secondary {
  color: var(--color-secondary);
  border: 2px solid currentColor;
}
```

### ブラウザサポート

CSS3から利用可能で、すべてのモダンブラウザでサポートされています。

---

## テキストの折り返し制御

### 推奨される設定

テキストの折り返しを適切に制御することで、レイアウト崩れを防ぎ、読みやすさを向上させます。

```css
body {
  overflow-wrap: anywhere;
  word-break: normal;
  line-break: strict;
}
```

この設定により、開発者が細かい調整をしなくても、ほとんどのケースで適切な折り返しが実現されます。

### overflow-wrap

長い単語やURLがレイアウトを突き破るのを防ぎます。

```css
.text {
  overflow-wrap: anywhere;
}
```

**値の説明:**

| 値 | 説明 |
|----|------|
| `normal` | 単語の途中で折り返さない（デフォルト） |
| `anywhere` | 必要に応じて単語の途中で折り返す |
| `break-word` | anywhereと同様だが、最小コンテンツサイズの計算が異なる |

**推奨:** `anywhere`（2020年以降のブラウザでサポート）

#### 実装例

```html
<div class="card">
  <p>https://example.com/very-long-url-that-could-break-layout</p>
</div>
```

```css
.card {
  max-width: 300px;
  padding: 20px;
  overflow-wrap: anywhere;
}
```

URLが長くてもカードの幅を超えません。

### word-break

英単語の折り返し方を制御します。

```css
.text {
  word-break: normal;
}
```

**値の説明:**

| 値 | 説明 |
|----|------|
| `normal` | 通常の折り返しルール（推奨） |
| `break-all` | すべての文字の間で折り返し可能 |
| `keep-all` | CJK（中日韓）テキストで単語内の折り返しを禁止 |

**注意:** `break-all`は英文を読みにくくするため、通常は使用しません。

```css
/* 非推奨 */
.text {
  word-break: break-all;
}
```

結果：
```
This is a ver
y long senten
ce.
```

単語の途中で不自然に折り返されます。

### line-break

日本語の禁則処理を制御します。

```css
.text {
  line-break: strict;
}
```

**値の説明:**

| 値 | 説明 |
|----|------|
| `auto` | ブラウザのデフォルト |
| `loose` | 緩い禁則処理 |
| `normal` | 通常の禁則処理 |
| `strict` | 厳密な禁則処理（推奨） |
| `anywhere` | 禁則処理なし |

**strictを使う理由:**
- 句点（。）が行頭に来ない
- 読みやすさが向上
- 読み間違いを防ぐ

#### 禁則処理の例

```
【loose】
これは長い文章です。
句点が行頭に来ることがあります
。

【strict】
これは長い文章です。句点が
行頭に来ることはありません。
```

### 実装パターン

#### グローバル設定（推奨）

```css
/* すべてのテキストに適用 */
body {
  overflow-wrap: anywhere;
  word-break: normal;
  line-break: strict;
}
```

#### 特定要素のみに適用

```css
/* ユーザー入力のテキストなど */
.user-content {
  overflow-wrap: anywhere;
  line-break: strict;
}
```

#### コードブロックの例外

```css
/* コードは折り返さない */
code, pre {
  overflow-wrap: normal;
  word-break: normal;
  white-space: pre;
  overflow-x: auto;
}
```

### hyphensプロパティ（ハイフネーション）

英文でハイフンを使った折り返しを有効にします。

```css
.text {
  hyphens: auto;
  lang: en;  /* HTML側で言語を指定 */
}
```

**注意:**
- 日本語では通常使用しない
- 英文サイトで有用
- `lang`属性の指定が必要

---

## 論理プロパティ

### 論理プロパティとは

論理プロパティは、物理的な方向（top、right、bottom、left）ではなく、**テキストの流れに基づいた方向**を指定するプロパティです。

- **inline方向**: テキストが流れる方向（横書きなら水平方向）
- **block方向**: ブロックが積み重なる方向（横書きなら垂直方向）

### 主要な論理プロパティ

| 物理プロパティ | 論理プロパティ | 説明 |
|--------------|--------------|------|
| `margin-left`, `margin-right` | `margin-inline` | インライン方向のマージン |
| `margin-top`, `margin-bottom` | `margin-block` | ブロック方向のマージン |
| `padding-left`, `padding-right` | `padding-inline` | インライン方向のパディング |
| `padding-top`, `padding-bottom` | `padding-block` | ブロック方向のパディング |
| `border-left`, `border-right` | `border-inline` | インライン方向のボーダー |
| `border-top`, `border-bottom` | `border-block` | ブロック方向のボーダー |
| `width` | `inline-size` | インライン方向のサイズ |
| `height` | `block-size` | ブロック方向のサイズ |
| `max-width` | `max-inline-size` | インライン方向の最大サイズ |
| `max-height` | `max-block-size` | ブロック方向の最大サイズ |

### 開始・終了の指定

方向を個別に指定する場合は、`-start`と`-end`を使います。

```css
.element {
  margin-inline-start: 20px;   /* 左（LTRの場合） */
  margin-inline-end: 40px;     /* 右（LTRの場合） */
  margin-block-start: 10px;    /* 上 */
  margin-block-end: 10px;      /* 下 */
}
```

### 実装例

#### コンテンツの中央揃え

```css
.container {
  max-inline-size: 1200px;  /* max-width */
  margin-inline: auto;       /* 水平方向の中央揃え */
  padding-inline: 20px;      /* 左右のパディング */
}
```

#### カードコンポーネント

```css
.card {
  padding-block: 24px;   /* 上下のパディング */
  padding-inline: 20px;  /* 左右のパディング */
  border-block-start: 3px solid #3498db;  /* 上ボーダー */
}
```

#### スペーシング

```css
.section {
  margin-block-end: 40px;  /* 下マージン */
}

.button {
  padding-inline: 24px;  /* 左右のパディング */
  padding-block: 12px;   /* 上下のパディング */
}
```

### 論理プロパティの利点

#### 1. 国際化対応

アラビア語やヘブライ語など、右から左（RTL）に読む言語でも自動的に対応します。

```css
/* 論理プロパティ */
.button {
  margin-inline-start: 20px;
}
```

- LTR（左から右）: 左マージン
- RTL（右から左）: 右マージン

物理プロパティでは、RTL対応に追加のスタイルが必要になります。

#### 2. 意図が明確

```css
/* 物理プロパティ */
padding-left: 20px;
padding-right: 20px;

/* 論理プロパティ */
padding-inline: 20px;
```

論理プロパティのほうが、「水平方向のパディング」という意図が明確です。

#### 3. コードが簡潔

```css
/* Before */
margin-top: 10px;
margin-bottom: 10px;

/* After */
margin-block: 10px;
```

### 書字方向の制御

HTMLで書字方向を指定できます。

```html
<!-- 左から右（デフォルト） -->
<div dir="ltr">English text</div>

<!-- 右から左 -->
<div dir="rtl">نص عربي</div>

<!-- 縦書き -->
<div style="writing-mode: vertical-rl">
  縦書きテキスト
</div>
```

論理プロパティは、これらの書字方向に自動的に対応します。

### ブラウザサポート

- Chrome 87以降
- Safari 14.1以降
- Firefox 66以降
- Edge 87以降

すべてのモダンブラウザで使用可能です。

---

## ベストプラクティス

### 1. グローバル設定の活用

```css
:root {
  /* ビューポート単位 */
  --vh: 100svh;
  
  /* フォントサイズ */
  --font-size-base: clamp(1rem, 0.9rem + 0.5vw, 1.25rem);
  
  /* スペーシング */
  --space-md: clamp(1rem, 0.8rem + 1vw, 1.75rem);
}

/* テキストの折り返し */
body {
  overflow-wrap: anywhere;
  word-break: normal;
  line-break: strict;
  font-size: var(--font-size-base);
}
```

### 2. 論理プロパティを優先

```css
/* 推奨 */
.element {
  margin-inline: auto;
  padding-block: 20px;
  border-inline-start: 3px solid blue;
}

/* 非推奨（特別な理由がない限り） */
.element {
  margin-left: auto;
  margin-right: auto;
  padding-top: 20px;
  padding-bottom: 20px;
  border-left: 3px solid blue;
}
```

### 3. currentColorで色の一貫性を保つ

```css
.button {
  color: var(--color-primary);
  border: 2px solid currentColor;
}

.button svg {
  fill: currentColor;
}
```

### 4. clampでメディアクエリを減らす

```css
/* メディアクエリなしでレスポンシブ */
h1 {
  font-size: clamp(2rem, 5vw, 4rem);
  margin-block: clamp(1rem, 3vw, 2rem);
}
```

---

## アンチパターン

### ❌ 100vhの使用（モバイルで問題）

```css
/* 避けるべき */
.hero {
  height: 100vh;
}
```

### ❌ 固定値の多用

```css
/* 避けるべき */
h1 {
  font-size: 48px;
}

@media (width < 768px) {
  h1 {
    font-size: 32px;
  }
}
```

clampを使えば、メディアクエリなしで滑らかにスケールできます。

### ❌ word-break: break-allの使用

```css
/* 避けるべき */
.text {
  word-break: break-all;
}
```

英文が読みにくくなります。`overflow-wrap: anywhere`を使いましょう。

### ❌ margin: 0 autoの使用

```css
/* 避けるべき */
.container {
  margin: 0 auto;
}

/* 推奨 */
.container {
  margin-inline: auto;
}
```

---

## 参考資料

- [CSS svh/dvh/lvh解説](https://zenn.dev/tonkotsuboy_com/articles/svh-dvh-lvh-for-all-browser)
- [Min-Max-Value Interpolation Calculator](https://min-max-calculator.9elements.com/)
- [currentColor活用法](https://zenn.dev/rabee/articles/css-tips-currentcolor)
- [テキストの折り返し制御](https://ics.media/entry/240411/)
- [MDN - CSS Logical Properties](https://developer.mozilla.org/ja/docs/Web/CSS/CSS_Logical_Properties)

---

次のセクション：[03. 視覚効果編](../03-visual-effects/README.md)
