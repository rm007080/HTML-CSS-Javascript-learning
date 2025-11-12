# CSS Layout編 - モダンなレイアウト技術

このセクションでは、モダンなCSSレイアウト技術について学びます。特にCSS Grid Layoutを中心に、実務で使えるレイアウトパターンを習得していきます。

## 目次

1. [CSS Grid Layoutの基礎](#css-grid-layoutの基礎)
2. [Grid Layoutの実践的な活用法](#grid-layoutの実践的な活用法)
3. [CSS Subgrid](#css-subgrid)
4. [中央揃えのテクニック](#中央揃えのテクニック)
5. [論理プロパティによる中央揃え](#論理プロパティによる中央揃え)
6. [特殊なレイアウトパターン](#特殊なレイアウトパターン)

---

## CSS Grid Layoutの基礎

### Grid Layoutとは

CSS Grid Layoutは、縦軸・横軸のある格子状レイアウトを実現するための強力な仕組みです。従来のFloatやFlexboxと比較して、以下の利点があります：

- **HTMLに余計な要素を追加せず、親要素の指定のみでレイアウトを実現**
- **縦横両方向の配置を同時に制御可能**
- **グリッドコンテナーに配置ルールを集約できる**

### 基本構造

Grid Layoutは**グリッドコンテナー**と**グリッドアイテム**の2層構造が基本です。

```html
<div class="container">  <!-- グリッドコンテナー -->
  <div class="item1">Item 1</div>  <!-- グリッドアイテム -->
  <div class="item2">Item 2</div>
  <div class="item3">Item 3</div>
</div>
```

```css
.container {
  display: grid;
  grid-template-columns: 180px 1fr 160px;  /* 列の定義 */
  grid-template-rows: 60px 1fr 90px;       /* 行の定義 */
  gap: 20px;  /* アイテム間の余白 */
}
```

### fr単位の理解

`fr`（fraction）単位は、Grid Layoutで使用される特別な単位です。

**重要な特性:**
> 全体の幅から`fr`以外の単位で指定したものを引き、残りの幅が`fr`で指定された列に配分される

```css
/* 例：左サイドバー固定、コンテンツ可変、右サイドバー固定 */
.container {
  grid-template-columns: 200px 1fr 180px;
}
```

この例では：
1. 左列が200px、右列が180pxで固定
2. 残りの幅がすべて中央列（1fr）に割り当てられる

**複数のfr単位:**

```css
.container {
  grid-template-columns: 1fr 2fr 1fr;
}
```

残りの幅を 1:2:1 の比率で配分します。

### グリッドラインによる配置

グリッド線は自動的に番号が付けられます（1, 2, 3...）。この番号を使ってアイテムを配置できます。

```css
.header {
  grid-column: 1 / 4;  /* 1列目から4列目まで（3列分） */
  grid-row: 1;         /* 1行目 */
}

.sidebar {
  grid-column: 1;      /* 1列目 */
  grid-row: 2 / 4;     /* 2行目から4行目まで */
}

.main {
  grid-column: 2 / 4;  /* 2列目から4列目まで */
  grid-row: 2;
}

.footer {
  grid-column: 1 / 4;
  grid-row: 3;
}
```

**短縮記法:**

```css
/* grid-columnとgrid-rowをまとめて指定 */
.item {
  grid-area: 1 / 2 / 3 / 4;  /* row-start / column-start / row-end / column-end */
}
```

### grid-template-areasによる配置

より直感的な配置方法として、エリア名を使う方法があります。

```css
.container {
  display: grid;
  grid-template-columns: 200px 1fr 180px;
  grid-template-rows: 80px 1fr 100px;
  grid-template-areas:
    "header header  header"
    "sidebar main   aside"
    "footer footer  footer";
}

.header  { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main    { grid-area: main; }
.aside   { grid-area: aside; }
.footer  { grid-area: footer; }
```

この方法の利点：
- レイアウト構造が視覚的に理解しやすい
- メディアクエリでの変更が簡単

```css
/* レスポンシブ対応 */
@media (width < 768px) {
  .container {
    grid-template-columns: 1fr;
    grid-template-areas:
      "header"
      "main"
      "sidebar"
      "aside"
      "footer";
  }
}
```

### repeatとauto-fillの活用

同じサイズの列・行を繰り返す場合は`repeat()`関数を使います。

```css
/* 4列のグリッド */
.container {
  grid-template-columns: repeat(4, 1fr);
}
```

**レスポンシブなカードレイアウト:**

```css
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
}
```

- `auto-fit`: ビューポートサイズに応じて自動的に列数を調整
- `minmax(250px, 1fr)`: 最小250px、最大1frの幅

---

## Grid Layoutの実践的な活用法

### 1. 横並びメニュー（ロゴ固定、メニュー可変幅）

```html
<header class="header">
  <div class="logo">Logo</div>
  <nav class="nav">
    <a href="#">Home</a>
    <a href="#">About</a>
    <a href="#">Contact</a>
  </nav>
</header>
```

```css
.header {
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 20px;
  align-items: center;
}

.logo {
  /* 内容に応じたサイズ */
}

.nav {
  display: flex;
  gap: 20px;
  justify-content: flex-end;
}
```

### 2. カード内の要素を右下に配置

```html
<div class="card">
  <img src="image.jpg" alt="商品画像">
  <h3>商品名</h3>
  <p>商品説明</p>
  <button>購入する</button>
</div>
```

```css
.card {
  display: grid;
  grid-template-rows: auto auto 1fr auto;
  gap: 12px;
  padding: 20px;
}

.card button {
  /* grid-template-rowsで1frを使っているため、
     ボタンは自動的に下部に配置される */
}

/* 特定要素のみ個別配置 */
.card .price {
  justify-self: end;  /* 右寄せ */
  align-self: end;    /* 下寄せ */
}
```

### 3. タイルレイアウト（自動列数調整）

```css
.tiles {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 24px;
}
```

**auto-fitとauto-fillの違い:**

- `auto-fit`: アイテムを引き延ばして余白を埋める
- `auto-fill`: 余白を残す（新しいアイテムのためのスペース確保）

```css
/* 幅いっぱいに広がる */
grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));

/* 最大幅を保つ */
grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
```

### 4. コンテンツ量が少ない場合の中央寄せ

```css
.container {
  display: grid;
  justify-content: center;  /* 水平方向の中央揃え */
  gap: 20px;
}
```

- 1行の場合：中央に配置
- 複数行の場合：左寄せで折り返し

### 5. 要素の重ね合わせ

```html
<div class="hero">
  <img src="hero.jpg" alt="背景画像">
  <div class="hero-text">
    <h1>タイトル</h1>
    <p>説明文</p>
  </div>
</div>
```

```css
.hero {
  display: grid;
  /* 子要素をすべて同じセルに配置 */
}

.hero > * {
  grid-area: 1 / 1;
}

.hero-text {
  z-index: 1;
  place-self: center;  /* 中央配置 */
  color: white;
  text-align: center;
}
```

### 6. ページ全体のレイアウト

```css
body {
  display: grid;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}

header {
  /* 内容に応じた高さ */
}

main {
  /* 残りのスペースをすべて使用 */
}

footer {
  /* 内容に応じた高さ */
}
```

コンテンツが少ない場合でもフッターが下部に固定されます。

---

## CSS Subgrid

### Subgridとは

CSS Subgridは、ネストされたグリッドが親グリッドの行・列に整列する機能です。

**従来の課題:**

複数のカード内で、タイトルや説明文の高さを揃えることがJavaScriptなしでは困難でした。

```html
<div class="cards">
  <div class="card">
    <img src="1.jpg">
    <h3>タイトル1</h3>
    <p>短い説明</p>
    <button>詳細</button>
  </div>
  <div class="card">
    <img src="2.jpg">
    <h3>長いタイトル2が入ります</h3>
    <p>非常に長い説明文がここに入ります...</p>
    <button>詳細</button>
  </div>
</div>
```

各カード内の要素の高さがバラバラになってしまいます。

### Subgridによる解決

**親グリッド:**

```css
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 40px;
}
```

**子グリッド（Subgrid）:**

```css
.card {
  display: grid;
  grid-template-rows: subgrid;  /* 親の行を継承 */
  grid-row: span 4;              /* 4行分を占有 */
  row-gap: 12px;
}
```

これにより、すべてのカードで対応する要素の高さが自動的に揃います。

### Subgridの実装パターン

```css
/* 親グリッド */
.parent {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}

/* 子要素をサブグリッド化 */
.child {
  display: grid;
  grid-template-columns: subgrid;  /* 列を継承 */
  grid-column: span 3;              /* 3列分を使用 */
  
  /* または行を継承 */
  grid-template-rows: subgrid;
  grid-row: span 4;
}

/* サブグリッド内の孫要素は親グリッドの線に整列する */
.grandchild {
  grid-column: 2;  /* 親グリッドの2列目 */
}
```

**使用例：統一されたカードレイアウト**

```css
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 32px;
}

.card {
  display: grid;
  grid-template-rows: subgrid;
  grid-row: span 5;  /* 画像、タイトル、説明、タグ、ボタン */
  row-gap: 16px;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
}

/* すべてのカードで各要素の高さが自動的に揃う */
```

### ブラウザサポート

- Chrome 117以降
- Safari 16以降
- Firefox 71以降
- Edge 117以降

2023年9月以降、全主要ブラウザで使用可能です。

---

## 中央揃えのテクニック

### Grid Layoutによる最短の中央揃え

CSS Gridを使えば、**わずか2行**で完全な中央揃えが実現できます。

#### 1つの要素の中央揃え

```css
.container {
  display: grid;
  place-content: center;
  
  /* 必要に応じて高さを指定 */
  min-height: 100vh;
}
```

`place-content: center` は以下の短縮記法です：
- `justify-content: center`（水平方向）
- `align-content: center`（垂直方向）

#### 複数要素の中央揃え

```css
.container {
  display: grid;
  place-content: center;
  place-items: center;
  gap: 20px;
}
```

- `place-content`: 行列全体の配置
- `place-items`: 各セル内でのアイテム配置

#### 個別要素の調整

```css
.item-special {
  place-self: start;  /* この要素だけ上寄せ */
}
```

### Flexboxとの比較

**Flexboxの場合（3行必要）:**

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

**Grid Layoutの場合（2行）:**

```css
.container {
  display: grid;
  place-content: center;
}
```

Grid Layoutのほうが簡潔で、複雑なレイアウトにも対応しやすくなります。

### 実用例：モーダルの中央表示

```html
<div class="modal-overlay">
  <div class="modal-content">
    <h2>モーダルタイトル</h2>
    <p>コンテンツ</p>
    <button>閉じる</button>
  </div>
</div>
```

```css
.modal-overlay {
  position: fixed;
  inset: 0;  /* top: 0; right: 0; bottom: 0; left: 0; と同じ */
  background: rgba(0, 0, 0, 0.5);
  
  /* 中央揃え */
  display: grid;
  place-content: center;
}

.modal-content {
  background: white;
  padding: 32px;
  border-radius: 8px;
  max-width: 500px;
  max-height: 80vh;
  overflow: auto;
}
```

---

## 論理プロパティによる中央揃え

### margin-inline: auto

従来、要素を水平方向に中央揃えする際は以下のように書いていました：

```css
/* 従来の方法 */
.box {
  margin-left: auto;
  margin-right: auto;
}
```

モダンCSSでは、**論理プロパティ**を使ってより簡潔に書けます：

```css
/* 推奨される方法 */
.box {
  margin-inline: auto;
}
```

### 論理プロパティとは

論理プロパティは、**テキストの流れと同じ方向**に作用するCSSプロパティです。

- `inline`: テキストの流れる方向（英語や日本語横書きでは水平方向）
- `block`: テキストのブロックが積み重なる方向（垂直方向）

| 物理プロパティ | 論理プロパティ |
|--------------|--------------|
| `margin-left`, `margin-right` | `margin-inline` |
| `margin-top`, `margin-bottom` | `margin-block` |
| `padding-left`, `padding-right` | `padding-inline` |
| `padding-top`, `padding-bottom` | `padding-block` |
| `border-left`, `border-right` | `border-inline` |
| `width` | `inline-size` |
| `height` | `block-size` |

### 実装例

```css
/* コンテンツを中央揃え */
.content {
  margin-inline: auto;
  max-inline-size: 1200px;  /* max-width: 1200px; と同等 */
  padding-inline: 20px;     /* 左右のパディング */
}
```

### margin: 0 autoを避けるべき理由

```css
/* 非推奨 */
.box {
  margin: 0 auto;
}
```

**問題点:**
- 上下のマージンも同時に指定される（`margin-top: 0; margin-bottom: 0;`）
- 予期しないスタイル上書きのリスクがある
- 意図が不明確

**推奨:**

```css
/* 推奨 */
.box {
  margin-inline: auto;
}
```

これにより、水平方向の中央揃えという意図が明確になります。

### 論理プロパティのメリット

1. **コードが簡潔になる**
   ```css
   /* Before */
   margin-left: auto;
   margin-right: auto;
   
   /* After */
   margin-inline: auto;
   ```

2. **国際化対応が容易**
   アラビア語（右から左）などの言語でも自動的に対応

3. **意図が明確**
   物理的な方向ではなく、論理的な方向で考える

### ブラウザサポート

- Safari 14.1以降
- Chrome 87以降
- Firefox 66以降
- Edge 87以降

すべてのモダンブラウザで使用可能です。

---

## 特殊なレイアウトパターン

### 左右に広がるレイアウト

コンテンツの一部だけを画面の端まで広げたい場合のテクニックです。

```html
<div class="wrapper">
  <div class="content">
    <p>通常のコンテンツ</p>
  </div>
  <div class="full-width">
    <img src="wide-image.jpg" alt="全幅画像">
  </div>
  <div class="content">
    <p>通常のコンテンツ</p>
  </div>
</div>
```

#### ビューポート単位を使った実装

```css
.wrapper {
  max-width: 1200px;
  margin-inline: auto;
  padding-inline: 20px;
}

.full-width {
  width: 100vw;
  margin-inline: calc((100vw - 100%) / -2);
}
```

**計算の仕組み:**
1. `100vw`: ビューポート全体の幅
2. `100%`: 親要素（wrapper）の幅
3. `(100vw - 100%)`: 両端の余白の合計
4. `/ -2`: 左右それぞれの余白をネガティブマージンで相殺

#### 左側だけ広げる

```css
.extend-left {
  width: 50vw;
  margin-inline-start: calc((50vw - 50%) * -1);
}
```

#### 右側だけ広げる

```css
.extend-right {
  width: 50vw;
  margin-inline-start: 50%;
}
```

### フッター固定レイアウト（Sticky Footer）

コンテンツが少ない場合でも、フッターを画面下部に固定するレイアウトです。

```html
<body>
  <header>ヘッダー</header>
  <main>メインコンテンツ</main>
  <footer>フッター</footer>
</body>
```

```css
body {
  display: grid;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
  margin: 0;
}

header {
  /* ヘッダーは内容に応じた高さ */
}

main {
  /* メインコンテンツが残りのスペースを占める */
}

footer {
  /* フッターは内容に応じた高さ */
}
```

**動作の仕組み:**
1. `min-height: 100vh`: 最低でもビューポート全体の高さを確保
2. `grid-template-rows: auto 1fr auto`:
   - `auto`: ヘッダーとフッターは内容に応じたサイズ
   - `1fr`: メインコンテンツが残りのスペースをすべて使用

### 聖杯レイアウト（Holy Grail Layout）

ヘッダー、フッター、サイドバー2つ、メインコンテンツを持つ古典的なレイアウトです。

```html
<div class="layout">
  <header>ヘッダー</header>
  <nav>ナビゲーション</nav>
  <main>メインコンテンツ</main>
  <aside>サイドバー</aside>
  <footer>フッター</footer>
</div>
```

```css
.layout {
  display: grid;
  grid-template-columns: 200px 1fr 180px;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "header header  header"
    "nav    main    aside"
    "footer footer  footer";
  min-height: 100vh;
  gap: 20px;
}

header { grid-area: header; }
nav    { grid-area: nav; }
main   { grid-area: main; }
aside  { grid-area: aside; }
footer { grid-area: footer; }

/* レスポンシブ対応 */
@media (width < 768px) {
  .layout {
    grid-template-columns: 1fr;
    grid-template-areas:
      "header"
      "nav"
      "main"
      "aside"
      "footer";
  }
}
```

---

## ベストプラクティス

### 1. HTMLの順序と表示順序を一致させる

Grid Layoutでは要素の表示順序を自由に変えられますが、**HTMLの出現順序と視覚的表示順序を変えないこと**が重要です。

**理由:**
- スクリーンリーダー利用者は HTMLの順序でコンテンツを読む
- キーボードナビゲーションもHTMLの順序に従う

```css
/* 避けるべき例 */
.item1 { grid-row: 3; }  /* HTMLでは1番目なのに表示は3番目 */
.item2 { grid-row: 1; }  /* HTMLでは2番目なのに表示は1番目 */
.item3 { grid-row: 2; }
```

### 2. プロパティ名の更新

旧来のプロパティ名ではなく、モダンな名前を使用しましょう。

| 旧プロパティ | 新プロパティ |
|------------|------------|
| `grid-gap` | `gap` |
| `grid-row-gap` | `row-gap` |
| `grid-column-gap` | `column-gap` |

新しいプロパティはFlexboxでも使用できるため、より汎用的です。

### 3. gapプロパティの活用

マージンではなく`gap`プロパティで余白を管理しましょう。

```css
/* 推奨 */
.container {
  display: grid;
  gap: 20px;
}

/* 非推奨 */
.item {
  margin: 10px;  /* 外側にも余白ができてしまう */
}
```

### 4. レスポンシブ対応

```css
/* モバイルファースト */
.container {
  display: grid;
  grid-template-columns: 1fr;
  gap: 16px;
}

/* タブレット以上 */
@media (width >= 768px) {
  .container {
    grid-template-columns: repeat(2, 1fr);
    gap: 24px;
  }
}

/* デスクトップ */
@media (width >= 1024px) {
  .container {
    grid-template-columns: repeat(3, 1fr);
    gap: 32px;
  }
}
```

### 5. 開発ツールの活用

Chrome DevToolsでグリッド線を可視化できます：

1. Elementsパネルで要素を選択
2. `display: grid`が設定された要素の横に「grid」バッジが表示される
3. バッジをクリックしてグリッド線を表示

---

## アンチパターン

### ❌ 過度な入れ子

```css
/* 避けるべき */
.outer {
  display: grid;
}

.inner {
  display: grid;
}

.deepest {
  display: grid;
}
```

HTMLとCSSが複雑になり、保守性が低下します。

### ❌ 固定値の多用

```css
/* 避けるべき */
.container {
  grid-template-columns: 300px 400px 300px;
  grid-template-rows: 100px 200px 150px;
}
```

レスポンシブ対応が困難になります。`fr`やauto、minmax()を活用しましょう。

### ❌ 不必要なwrapper要素

```html
<!-- 避けるべき -->
<div class="grid-container">
  <div class="grid-item-wrapper">
    <div class="actual-content">コンテンツ</div>
  </div>
</div>
```

Grid Layoutを使えば、余計なwrapper要素は不要です。

---

## 参考資料

- [ICS MEDIA - CSS Grid Layout入門](https://ics.media/entry/15649/)
- [CSS Grid Layout実践例](https://zenn.dev/kagan/articles/4f96a97aadfcb8)
- [CSS Subgrid解説](https://zenn.dev/tonkotsuboy_com/articles/css-subgrid-all-browsers)
- [CSS Gridで中央揃え](https://zenn.dev/tonkotsuboy_com/articles/css-grid-centering)
- [margin-inline: auto](https://zenn.dev/tonkotsuboy_com/articles/margin-inline_auto)
- [MDN - CSS Grid Layout](https://developer.mozilla.org/ja/docs/Web/CSS/CSS_Grid_Layout)

---

次のセクション：[02. モダンプロパティ編](../02-modern-properties/README.md)
