# CSS セレクタとアクセシビリティ編

このセクションでは、モダンなCSSセレクタとアクセシビリティに関するベストプラクティスについて学びます。

## 目次

1. [モダンなセレクタ（:has, :is, :where）](#モダンなセレクタhas-is-where)
2. [Media Query Range記法](#media-query-range記法)
3. [hoverメディア特性](#hoverメディア特性)
4. [Visually Hidden](#visually-hidden)
5. [フォーム要素のカスタマイズ](#フォーム要素のカスタマイズ)

---

## モダンなセレクタ（:has, :is, :where）

### :has()セレクタ

`:has()`は、特定の子要素や状態を持つ親要素を選択する**親セレクタ**です。

#### 基本構文

```css
親要素:has(子要素) {
  /* スタイル */
}
```

#### 基本的な使用例

```css
/* 画像を含むカードのみスタイル適用 */
.card:has(img) {
  display: flex;
}

/* 画像を含まないカードは別のスタイル */
.card:not(:has(img)) {
  display: block;
}
```

#### 実用的なパターン

**フォームのフォーカス状態検出:**

```css
/* フォーム内の入力欄にフォーカスがある場合、フォーム全体にスタイル適用 */
form:has(input:focus) {
  box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.3);
}
```

**隣接要素の存在判定:**

```css
/* 見出しの後に段落がある場合、見出しのマージンを調整 */
h2:has(+ p) {
  margin-block-end: 0.5rem;
}

/* 見出しの後に段落がない場合 */
h2:not(:has(+ p)) {
  margin-block-end: 2rem;
}
```

**チェックボックスの状態による制御:**

```css
/* チェックされたチェックボックスを含むラベルのスタイル */
label:has(input[type="checkbox"]:checked) {
  background-color: #e8f5e9;
  border-color: #4caf50;
}
```

**空要素の検出:**

```css
/* 子要素がない要素を非表示 */
.container:not(:has(*)) {
  display: none;
}
```

**特定クラスを持つ子孫要素の検出:**

```css
/* .error クラスを持つ要素を含むフォーム */
form:has(.error) {
  border-color: #e74c3c;
}
```

#### より複雑な例

**ナビゲーションの階層表示:**

```css
/* サブメニューを持つナビゲーションアイテム */
.nav-item:has(> ul) {
  position: relative;
}

.nav-item:has(> ul)::after {
  content: '▼';
  margin-inline-start: 0.5em;
}
```

**カードグリッドの最後の行の調整:**

```css
/* 最後の行に1つだけアイテムがある場合 */
.grid:has(> .item:last-child:nth-child(4n + 1)) > .item:last-child {
  grid-column: 1 / -1;
  max-width: 300px;
  margin-inline: auto;
}
```

### :is()セレクタ

`:is()`は、複数のセレクタを一括指定し、コードを簡潔にします。

#### 基本構文

```css
:is(セレクタ1, セレクタ2, セレクタ3) {
  /* スタイル */
}
```

#### 使用例

**従来の方法:**

```css
h1, h2, h3, h4, h5, h6 {
  color: #2c3e50;
  font-family: 'Georgia', serif;
}
```

**:is()を使った方法:**

```css
:is(h1, h2, h3, h4, h5, h6) {
  color: #2c3e50;
  font-family: 'Georgia', serif;
}
```

**入れ子構造での活用:**

```css
/* 従来 */
.header a,
.sidebar a,
.footer a {
  color: #3498db;
}

/* :is()使用 */
:is(.header, .sidebar, .footer) a {
  color: #3498db;
}
```

**複雑なセレクタの簡略化:**

```css
/* 従来 */
article > h2,
section > h2,
aside > h2 {
  font-size: 1.5rem;
}

/* :is()使用 */
:is(article, section, aside) > h2 {
  font-size: 1.5rem;
}
```

#### Sassとの組み合わせ

```scss
.container {
  :is(p, ul, ol) {
    margin-block: 1rem;
    
    // 特定の条件を後置修飾で追加
    &:is(:first-child) {
      margin-block-start: 0;
    }
    
    &:is(:last-child) {
      margin-block-end: 0;
    }
  }
}
```

### :where()セレクタ

`:where()`は、`:is()`と似ていますが、**詳細度が0**になる点が異なります。

#### 詳細度の違い

```css
/* :is() - 最も高い詳細度を持つ */
:is(#id, .class) {
  color: blue;  /* 詳細度: 1,0,0 */
}

/* :where() - 詳細度: 0 */
:where(#id, .class) {
  color: blue;  /* 詳細度: 0,0,0 */
}
```

#### 使用場面

**リセットCSSやベーススタイル:**

```css
/* 詳細度0なので、後から簡単に上書きできる */
:where(h1, h2, h3, h4, h5, h6) {
  margin-block: 0;
  font-weight: normal;
}

/* 簡単に上書き可能 */
h1 {
  font-weight: bold;  /* 詳細度: 0,0,1 で勝つ */
}
```

**ユーティリティクラス:**

```css
:where(.mt-0) { margin-block-start: 0; }
:where(.mt-1) { margin-block-start: 0.25rem; }
:where(.mt-2) { margin-block-start: 0.5rem; }

/* 他のスタイルで簡単に上書きできる */
.special {
  margin-block-start: 2rem;  /* ユーティリティより優先される */
}
```

### 後置修飾パターン

`:is()`と`:where()`を使うと、「前置修飾」を「後置修飾」に変換できます。

**前置修飾（従来）:**

```css
.container p {
  color: #333;
}

.container p:first-child {
  margin-block-start: 0;
}

.container p + p {
  margin-block-start: 1rem;
}
```

**後置修飾（モダン）:**

```css
.container p {
  color: #333;
  
  /* すべての条件を後ろにまとめられる */
  &:is(:first-child) {
    margin-block-start: 0;
  }
  
  &:is(h2 + *) {
    margin-block-start: 1rem;
  }
}
```

Sassでの管理が容易になり、関連するスタイルを1箇所にまとめられます。

### ブラウザサポート

- `:has()`: Chrome 105、Safari 15.4、Firefox 121以降
- `:is()`: Chrome 88、Safari 14、Firefox 78以降
- `:where()`: Chrome 88、Safari 14、Firefox 78以降

---

## Media Query Range記法

### 従来の記法の問題

従来のMedia Queryは冗長で複雑でした。

```css
/* 従来の方法 */
@media (min-width: 600px) and (max-width: 799px) {
  /* スタイル */
}
```

**問題点:**
- 記述が長い
- 「未満」と「以上」の表現が直感的でない
- ブレークポイント設定時に小数点の調整が必要

### Range記法（推奨）

数学的で直感的な記法が使えるようになりました。

```css
/* 新しいRange記法 */
@media (600px <= width < 800px) {
  /* スタイル */
}
```

### 使用できる記号

| 記号 | 意味 |
|------|------|
| `<=` | 以下 |
| `<`  | 未満 |
| `>=` | 以上 |
| `>`  | より大きい |
| `=`  | 等しい |

### 実装パターン

#### 最小幅の指定

```css
/* 従来 */
@media (min-width: 768px) {
  /* ... */
}

/* Range記法 */
@media (width >= 768px) {
  /* ... */
}
```

#### 最大幅の指定

```css
/* 従来 */
@media (max-width: 767px) {
  /* ... */
}

/* Range記法 */
@media (width < 768px) {
  /* ... */
}
```

#### 範囲の指定

```css
/* 従来 */
@media (min-width: 600px) and (max-width: 1199px) {
  /* ... */
}

/* Range記法 */
@media (600px <= width < 1200px) {
  /* ... */
}
```

#### 複数条件

```css
/* 幅と高さの両方を指定 */
@media (width >= 768px) and (height >= 600px) {
  /* ... */
}

/* Range記法で範囲指定 */
@media (768px <= width < 1200px) and (height >= 600px) {
  /* ... */
}
```

### 推奨される記述順序

「小さい数 < 大きい数」の順序で記述するのが読みやすいです。

```css
/* 推奨 */
@media (600px <= width < 1200px) {
  /* ... */
}

/* 避けるべき（逆順） */
@media (width < 1200px) and (width >= 600px) {
  /* 読みにくい */
}
```

### レスポンシブデザインの実装例

```css
/* モバイル（デフォルト） */
.container {
  padding: 1rem;
}

/* タブレット */
@media (width >= 768px) {
  .container {
    padding: 2rem;
  }
}

/* デスクトップ */
@media (width >= 1024px) {
  .container {
    padding: 3rem;
    max-width: 1200px;
    margin-inline: auto;
  }
}

/* 大画面 */
@media (width >= 1920px) {
  .container {
    max-width: 1600px;
  }
}
```

### 高さの制御

```css
/* 縦長の画面 */
@media (height >= 800px) {
  .hero {
    min-height: 100svh;
  }
}

/* 横長の画面 */
@media (height < 600px) {
  .hero {
    min-height: 400px;
  }
}
```

### ブラウザサポート

- Chrome 104以降
- Safari 16.4以降
- Firefox 63以降
- Edge 104以降

**フォールバック:**

PostCSSの`postcss-media-minmax`プラグインで、古いブラウザ向けに自動変換できます。

---

## hoverメディア特性

### タップデバイスでのhover問題

スマートフォンなどのタップデバイスでは、hover状態が問題を引き起こします。

**問題点:**
- タップ後もhover状態が継続する
- 意図しないUI動作が発生
- ダブルタップが必要になる場合がある

### hoverメディア特性

デバイスがhover操作に対応しているかを判定します。

```css
@media (hover: hover) {
  .button:hover {
    background-color: #2980b9;
  }
}
```

### any-hoverメディア特性（推奨）

**`any-hover`を使う理由:**
- マウスやトラックパッドなど、*いずれかの*入力デバイスがhoverをサポートしているかを判定
- Magic Keyboard接続時のiPadにも対応
- より包括的な判定が可能

```css
/* 推奨 */
@media (any-hover: hover) {
  .button:hover {
    background-color: var(--background-hover);
  }
}
```

### 実装パターン

#### 基本的なhover無効化

```css
/* すべてのデバイス共通 */
.button {
  background-color: #3498db;
  color: white;
  padding: 12px 24px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

/* hover対応デバイスのみ */
@media (any-hover: hover) {
  .button:hover {
    background-color: #2980b9;
  }
}
```

#### focus-visibleとの組み合わせ

```css
.button {
  background-color: #3498db;
  transition: background-color 0.2s ease;
}

/* hover対応デバイス */
@media (any-hover: hover) {
  .button:hover {
    background-color: #2980b9;
  }
}

/* キーボードフォーカス時 */
.button:focus-visible {
  background-color: #2980b9;
  outline: 3px solid #3498db;
  outline-offset: 2px;
}
```

#### transition-propertyの明確化

```css
/* 推奨: 変化するプロパティを明示 */
.button {
  background-color: #3498db;
  transform: scale(1);
  transition-property: background-color, transform;
  transition-duration: 0.2s;
  transition-timing-function: ease;
}

/* 避けるべき: allは予期しない動作を引き起こす可能性 */
.button {
  transition: all 0.2s ease;
}
```

### 画面幅での判定を避ける理由

```css
/* 避けるべき: iPadでも動作してしまう */
@media (width < 640px) {
  .button:hover {
    /* タブレットでも適用される */
  }
}

/* 推奨: デバイスの能力で判定 */
@media (any-hover: hover) {
  .button:hover {
    /* hover対応デバイスのみ */
  }
}
```

### pointer メディア特性

ポインティングデバイスの精度を判定します。

```css
/* 粗いポインタ（タッチスクリーン） */
@media (pointer: coarse) {
  .button {
    min-height: 44px;  /* タップしやすいサイズ */
    min-width: 44px;
  }
}

/* 細かいポインタ（マウスなど） */
@media (pointer: fine) {
  .button {
    min-height: 32px;
  }
}
```

### 組み合わせパターン

```css
/* タッチデバイス用の大きなボタン */
@media (any-hover: none) and (pointer: coarse) {
  .button {
    min-height: 48px;
    padding: 16px 32px;
    font-size: 1.125rem;
  }
}

/* デスクトップ用 */
@media (any-hover: hover) and (pointer: fine) {
  .button {
    min-height: 36px;
    padding: 10px 20px;
    font-size: 1rem;
  }
  
  .button:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  }
}
```

---

## Visually Hidden

### Visually Hiddenとは

画面上では非表示にしつつ、スクリーンリーダーでは読み上げられる要素を実装するテクニックです。

### 従来の方法の問題点

#### display: none の問題

```css
/* ❌ スクリーンリーダーも読まない */
.hidden {
  display: none;
}
```

#### visibility: hidden の問題

```css
/* ❌ スクリーンリーダーも読まない */
.hidden {
  visibility: hidden;
}
```

### 推奨される実装

```css
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}
```

または、モダンな実装:

```css
.visually-hidden {
  position: fixed;
  width: 4px;
  height: 4px;
  opacity: 0;
  overflow: hidden;
  border: none;
  margin: 0;
  padding: 0;
  display: block;
  visibility: visible;
}
```

### 使用場面

#### アイコンのみのボタン

```html
<button class="icon-button">
  <svg><!-- アイコン --></svg>
  <span class="visually-hidden">メニューを開く</span>
</button>
```

#### スキップリンク

```html
<a href="#main-content" class="visually-hidden focusable">
  メインコンテンツへスキップ
</a>
```

**フォーカス時に表示:**

```css
.focusable:focus {
  position: static;
  width: auto;
  height: auto;
  margin: 0;
  overflow: visible;
  clip: auto;
}
```

#### フォームラベル

```html
<form>
  <label for="search" class="visually-hidden">検索</label>
  <input id="search" type="search" placeholder="検索...">
</form>
```

#### 見出しの補足

```html
<section>
  <h2 class="visually-hidden">商品一覧</h2>
  <!-- 視覚的には見出しが不要だが、構造上は必要 -->
  <div class="products">...</div>
</section>
```

### ユーティリティクラスとして実装

```css
/* 基本 */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}

/* フォーカス可能 */
.sr-only-focusable:focus {
  position: static;
  width: auto;
  height: auto;
  padding: 0;
  margin: 0;
  overflow: visible;
  clip: auto;
  white-space: normal;
}
```

---

## フォーム要素のカスタマイズ

### display: noneを避けるべき理由

チェックボックスやラジオボタンをカスタマイズする際、`display: none`で隠すのは**避けるべき**です。

**問題点:**
- スクリーンリーダーに認識されない
- キーボードナビゲーションで選択できない
- フォーカス表示が機能しない
- JavaScriptでサイズ情報を取得できない

### 推奨される方法

#### Visually Hiddenパターン

```css
/* チェックボックス/ラジオボタンを視覚的に隠す */
input[type="checkbox"],
input[type="radio"] {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

/* ラベルをカスタムデザインに */
input[type="checkbox"] + label {
  position: relative;
  padding-inline-start: 2em;
  cursor: pointer;
}

/* 疑似要素でカスタムチェックボックスを作成 */
input[type="checkbox"] + label::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0.2em;
  width: 1.2em;
  height: 1.2em;
  border: 2px solid #999;
  border-radius: 3px;
  background: white;
}

/* チェック状態 */
input[type="checkbox"]:checked + label::before {
  background: #3498db;
  border-color: #3498db;
}

/* チェックマーク */
input[type="checkbox"]:checked + label::after {
  content: '';
  position: absolute;
  left: 0.4em;
  top: 0.4em;
  width: 0.4em;
  height: 0.8em;
  border: solid white;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
}

/* フォーカス表示 */
input[type="checkbox"]:focus-visible + label::before {
  outline: 3px solid #3498db;
  outline-offset: 2px;
}
```

#### ラジオボタンのカスタマイズ

```css
input[type="radio"] {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

input[type="radio"] + label {
  position: relative;
  padding-inline-start: 2em;
  cursor: pointer;
}

/* 外側の円 */
input[type="radio"] + label::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0.2em;
  width: 1.2em;
  height: 1.2em;
  border: 2px solid #999;
  border-radius: 50%;
  background: white;
}

/* 選択状態の内側の円 */
input[type="radio"]:checked + label::after {
  content: '';
  position: absolute;
  left: 0.4em;
  top: 0.6em;
  width: 0.6em;
  height: 0.6em;
  border-radius: 50%;
  background: #3498db;
}

/* フォーカス表示 */
input[type="radio"]:focus-visible + label::before {
  outline: 3px solid #3498db;
  outline-offset: 2px;
}
```

### 実装例

```html
<div class="form-group">
  <input type="checkbox" id="agree" name="agree">
  <label for="agree">利用規約に同意する</label>
</div>

<fieldset class="form-group">
  <legend>配送方法を選択</legend>
  
  <div>
    <input type="radio" id="standard" name="shipping" value="standard">
    <label for="standard">通常配送（無料）</label>
  </div>
  
  <div>
    <input type="radio" id="express" name="shipping" value="express">
    <label for="express">お急ぎ便（500円）</label>
  </div>
</fieldset>
```

### アクセシビリティのチェックポイント

1. **ラベルの関連付け**
   ```html
   <input type="checkbox" id="unique-id">
   <label for="unique-id">ラベル</label>
   ```

2. **キーボード操作**
   - Tabキーでフォーカス移動
   - Spaceキーで選択/解除
   - 矢印キーでラジオボタンのグループ内移動

3. **フォーカス表示**
   ```css
   input:focus-visible + label::before {
     outline: 3px solid #3498db;
     outline-offset: 2px;
   }
   ```

4. **グループ化**
   ```html
   <fieldset>
     <legend>質問内容</legend>
     <!-- ラジオボタン群 -->
   </fieldset>
   ```

---

## ベストプラクティス

### 1. モダンセレクタを積極的に使用

```css
/* :has()で条件付きスタイル */
form:has(.error) {
  border-color: #e74c3c;
}

/* :is()でコード簡潔化 */
:is(h1, h2, h3) {
  font-family: 'Georgia', serif;
}

/* :where()でリセットCSS */
:where(ul, ol) {
  padding-inline-start: 0;
  list-style: none;
}
```

### 2. Range記法でメディアクエリを明確に

```css
/* 読みやすく直感的 */
@media (768px <= width < 1024px) {
  /* タブレット用スタイル */
}
```

### 3. hover対応を適切に判定

```css
/* デバイスの能力で判定 */
@media (any-hover: hover) {
  .interactive:hover {
    /* ホバー効果 */
  }
}

/* タッチデバイス用のサイズ調整 */
@media (pointer: coarse) {
  .button {
    min-height: 44px;
  }
}
```

### 4. アクセシビリティを最優先

```css
/* Visually Hiddenで支援技術に情報提供 */
.icon-only-button .label {
  position: absolute;
  width: 1px;
  height: 1px;
  /* ... */
}

/* フォーカス表示を確保 */
button:focus-visible {
  outline: 3px solid currentColor;
  outline-offset: 2px;
}
```

---

## アンチパターン

### ❌ display: noneでフォーム要素を隠す

```css
/* 避けるべき */
input[type="checkbox"] {
  display: none;
}
```

### ❌ 画面幅でhoverを判定

```css
/* 避けるべき */
@media (width >= 768px) {
  .button:hover {
    /* iPadでも動作してしまう */
  }
}
```

### ❌ 詳細度の戦争

```css
/* 避けるべき */
.button.button.button {
  /* 詳細度を無理やり上げる */
}

/* 推奨: :where()で詳細度を下げる */
:where(.button) {
  /* 簡単に上書き可能 */
}
```

### ❌ 過度な:has()のネスト

```css
/* 避けるべき: パフォーマンス低下 */
.container:has(.item:has(.child:has(.grandchild))) {
  /* ... */
}
```

---

## 参考資料

- [CSS新セレクタ解説](https://b-risk.jp/blog/2023/06/new-selector/)
- [:is()/:where()活用法](https://zenn.dev/kagan/articles/css-is-where-tips)
- [Media Query Range記法](https://zenn.dev/tonkotsuboy_com/articles/css-range-syntax)
- [hoverメディア特性](https://www.tak-dcxi.com/article/disable-hover-on-mobile-and-hover-implementation-example)
- [Visually Hidden解説](https://qiita.com/randy39/items/fca820d500dfe9ec1a52)
- [フォーム要素カスタマイズ](https://web-guided.com/1695/)
- [MDN - :has()](https://developer.mozilla.org/ja/docs/Web/CSS/:has)
- [MDN - :is()](https://developer.mozilla.org/ja/docs/Web/CSS/:is)
- [MDN - :where()](https://developer.mozilla.org/ja/docs/Web/CSS/:where)

---

これでCSS学習教材のすべてのセクションが完了です。次は実践的なコード例を作成し、プロジェクト全体をまとめていきましょう！
