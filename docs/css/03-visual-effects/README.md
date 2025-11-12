# CSS 視覚効果編

このセクションでは、CSSで実現できる視覚効果について学びます。JavaScriptを使わずに、洗練されたデザイン表現を実現するテクニックを習得していきます。

## 目次

1. [transform独立プロパティ](#transform独立プロパティ)
2. [filterプロパティ](#filterプロパティ)
3. [mix-blend-mode](#mix-blend-mode)
4. [clip-path](#clip-path)

---

## transform独立プロパティ

### 従来のtransformの課題

従来、CSS transformは複数の変形を1つの値として扱っていました。

```css
.element {
  transform: translateX(100px) rotate(45deg) scale(1.2);
}
```

**問題点:**
- 個別にアニメーションできない
- それぞれ異なるタイミング関数を設定できない
- JavaScriptから個別の値を取得/変更しにくい

### 独立したプロパティ

最新CSSでは、translate、rotate、scaleが個別のプロパティになりました。

```css
.element {
  translate: 100px 0;
  rotate: 45deg;
  scale: 1.2;
}
```

### translate

要素を移動させます。

**構文:**

```css
/* X軸のみ */
translate: 100px;

/* X軸とY軸 */
translate: 100px 50px;

/* X軸、Y軸、Z軸 */
translate: 100px 50px 20px;

/* パーセント値も使用可能 */
translate: 50% -50%;
```

**実装例:**

```css
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  translate: -50% -50%;  /* 中央揃え */
}
```

### rotate

要素を回転させます。

**構文:**

```css
/* Z軸周りの回転（2D回転） */
rotate: 45deg;

/* 特定の軸周りの回転（3D回転） */
rotate: x 45deg;
rotate: y 45deg;
rotate: z 45deg;

/* 任意の軸周りの回転 */
rotate: 1 1 1 45deg;  /* X, Y, Z, 角度 */
```

**実装例:**

```css
.card {
  rotate: y 0deg;
  transition: rotate 0.3s ease;
}

.card:hover {
  rotate: y 15deg;  /* Y軸周りに回転 */
}
```

### scale

要素を拡大/縮小します。

**構文:**

```css
/* 均等拡大 */
scale: 1.5;

/* X軸とY軸を個別に */
scale: 1.5 2;

/* X軸、Y軸、Z軸 */
scale: 1.5 2 1;
```

**実装例:**

```css
.button {
  scale: 1;
  transition: scale 0.2s ease;
}

.button:hover {
  scale: 1.05;
}

.button:active {
  scale: 0.95;
}
```

### 複数タイムラインの活用

独立プロパティの最大の利点は、**それぞれに異なるアニメーションを設定できる**ことです。

```css
.element {
  animation-name: moveAnimation, scaleAnimation;
  animation-duration: 3s, 0.25s;
  animation-timing-function: linear, ease-in-out;
  animation-iteration-count: infinite, infinite;
  animation-direction: alternate, alternate;
}

@keyframes moveAnimation {
  from { translate: 0 0; }
  to { translate: 300px 0; }
}

@keyframes scaleAnimation {
  from { scale: 1; }
  to { scale: 1.2; }
}
```

要素が横移動しながら、別のタイミングで拡大縮小を繰り返します。

### 実用的なパターン

#### ホバー時の複合エフェクト

```css
.card {
  translate: 0 0;
  scale: 1;
  rotate: 0deg;
  transition: 
    translate 0.3s ease,
    scale 0.2s ease-out,
    rotate 0.3s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

.card:hover {
  translate: 0 -10px;
  scale: 1.05;
  rotate: 2deg;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
}
```

#### ローディングアニメーション

```css
.loader {
  inline-size: 50px;
  block-size: 50px;
  border: 4px solid #f3f3f3;
  border-top: 4px solid #3498db;
  border-radius: 50%;
  rotate: 0deg;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to { rotate: 360deg; }
}
```

#### 3Dカードフリップ

```css
.card-container {
  perspective: 1000px;
}

.card {
  transition: rotate 0.6s;
  transform-style: preserve-3d;
}

.card:hover {
  rotate: y 180deg;
}

.card-front,
.card-back {
  backface-visibility: hidden;
}

.card-back {
  rotate: y 180deg;
}
```

### JavaScriptとの連携

```javascript
const element = document.querySelector('.element');

// 個別のプロパティを設定
element.style.translate = `${x}px ${y}px`;
element.style.rotate = `${angle}deg`;
element.style.scale = `${scaleValue}`;

// CSSがビジュアルを担当し、JSは座標計算に専念できる
```

### ブラウザサポート

- Chrome 104以降
- Safari 14.1以降
- Firefox 72以降
- Edge 104以降

---

## filterプロパティ

### filterとは

CSSフィルタープロパティは、画像やブラウザ要素にグラフィカルな効果を適用します。

### 基本的なフィルター関数

#### brightness（明るさ）

```css
.image {
  filter: brightness(1);  /* 通常 */
}

.image:hover {
  filter: brightness(1.3);  /* 明るく */
}
```

- `0`: 真っ黒
- `1`: 通常（デフォルト）
- `1以上`: 明るく

#### contrast（コントラスト）

```css
.image {
  filter: contrast(1.2);
}
```

- `0`: グレー
- `1`: 通常
- `1以上`: コントラスト強

#### grayscale（グレースケール）

```css
.image {
  filter: grayscale(0);  /* カラー */
}

.image:hover {
  filter: grayscale(1);  /* モノクロ */
}
```

- `0`: カラー
- `1`: 完全なグレースケール

#### sepia（セピア）

```css
.image {
  filter: sepia(0.8);
}
```

レトロな写真風の効果。

#### saturate（彩度）

```css
.image {
  filter: saturate(1.5);  /* 鮮やかに */
}
```

- `0`: 完全にグレー
- `1`: 通常
- `1以上`: 彩度を上げる

#### hue-rotate（色相回転）

```css
.image {
  filter: hue-rotate(90deg);
}
```

色相環を回転させて色を変更。

#### invert（反転）

```css
.image {
  filter: invert(1);  /* ネガポジ反転 */
}
```

ダークモードの実装などに使用。

#### blur（ぼかし）

```css
.background {
  filter: blur(10px);
}
```

背景のぼかし効果。

#### opacity（不透明度）

```css
.image {
  filter: opacity(0.5);
}
```

`opacity`プロパティと似ていますが、filterとして指定すると複合効果を作りやすくなります。

#### drop-shadow（ドロップシャドウ）

```css
.icon {
  filter: drop-shadow(2px 2px 4px rgba(0, 0, 0, 0.3));
}
```

`box-shadow`と異なり、透過部分を考慮した影を作成。

### 複数フィルターの組み合わせ

```css
.image {
  filter: brightness(1.1) contrast(1.2) saturate(1.3);
}
```

スペース区切りで複数のフィルターを適用できます。

### 実用的なパターン

#### ホバー時の明るさ変更

```css
.button {
  background-image: url('button-bg.jpg');
  filter: brightness(1);
  transition: filter 0.3s ease;
}

.button:hover {
  filter: brightness(1.2);
}

.button:active {
  filter: brightness(0.9);
}
```

1つの画像だけでホバー/アクティブ状態を表現できます。

#### グレースケールからカラーへ

```css
.gallery-item {
  filter: grayscale(1);
  transition: filter 0.3s ease;
}

.gallery-item:hover {
  filter: grayscale(0);
}
```

#### ぼかしとオーバーレイ

```css
.modal-overlay {
  backdrop-filter: blur(10px) brightness(0.7);
}
```

`backdrop-filter`を使うと、背景にフィルターを適用できます。

#### アイコンの色変更

```css
.icon {
  filter: hue-rotate(0deg);
  transition: filter 0.3s ease;
}

.icon.success {
  filter: hue-rotate(120deg);  /* 緑へ */
}

.icon.warning {
  filter: hue-rotate(60deg);   /* 黄色へ */
}

.icon.error {
  filter: hue-rotate(0deg);    /* 赤のまま */
}
```

#### フォーカス時の強調

```css
img {
  filter: brightness(1) contrast(1);
  transition: filter 0.2s ease;
}

img:focus {
  filter: brightness(1.1) contrast(1.2);
  outline: 3px solid #3498db;
}
```

### CSS変数との組み合わせ

```css
:root {
  --filter-brightness: 1;
  --filter-contrast: 1;
  --filter-saturation: 1;
}

.image {
  filter: 
    brightness(var(--filter-brightness))
    contrast(var(--filter-contrast))
    saturate(var(--filter-saturation));
  transition: filter 0.3s ease;
}

/* JavaScriptで動的に変更 */
```

### パフォーマンス

モダンブラウザでは、filterプロパティはGPUアクセラレーションが有効で、60fpsでのアニメーションが可能です。

### ブラウザサポート

- Chrome 53以降
- Safari 9.1以降
- Firefox 35以降
- Edge 12以降

---

## mix-blend-mode

### mix-blend-modeとは

`mix-blend-mode`は、重なった要素の**ブレンド方法**を制御するプロパティです。PhotoshopやFigmaなどのデザインツールと同様の効果をCSSで実現できます。

### 基本的な使い方

```css
.overlay {
  position: absolute;
  mix-blend-mode: multiply;
}
```

**重要:** ブレンドモードは、`z-index`で上位にある要素に指定します。

### 主なブレンドモード

#### 暗くする系

##### multiply（乗算）

```css
.text {
  color: white;
  mix-blend-mode: multiply;
}
```

白は透明に、黒はそのまま。背景を暗くします。

##### darken（比較（暗））

```css
.overlay {
  mix-blend-mode: darken;
}
```

各ピクセルの暗いほうを採用。

#### 明るくする系

##### screen（スクリーン）

```css
.light-effect {
  mix-blend-mode: screen;
}
```

光を重ねたような効果。黒が透明になります。

##### lighten（比較（明））

```css
.highlight {
  mix-blend-mode: lighten;
}
```

各ピクセルの明るいほうを採用。

#### コントラスト系

##### overlay（オーバーレイ）

```css
.text {
  color: white;
  mix-blend-mode: overlay;
}
```

**最も一般的な選択肢**。コントラストが上がり、見栄えが向上します。

##### soft-light（ソフトライト）

```css
.overlay {
  mix-blend-mode: soft-light;
}
```

overlayより柔らかい効果。

##### hard-light（ハードライト）

```css
.overlay {
  mix-blend-mode: hard-light;
}
```

overlayより強い効果。

#### 差分系

##### difference（差の絶対値）

```css
.text {
  color: white;
  mix-blend-mode: difference;
}
```

背景色の反対色になります。

##### exclusion（除外）

```css
.overlay {
  mix-blend-mode: exclusion;
}
```

differenceより柔らかい反転効果。

#### 色操作系

##### hue（色相）

```css
.colorize {
  mix-blend-mode: hue;
}
```

上位レイヤーの色相のみを適用。

##### saturation（彩度）

```css
.saturate {
  mix-blend-mode: saturation;
}
```

上位レイヤーの彩度のみを適用。

##### color（カラー）

```css
.colorize {
  background: red;
  mix-blend-mode: color;
}
```

上位レイヤーの色相と彩度を適用。

##### luminosity（輝度）

```css
.luminance {
  mix-blend-mode: luminosity;
}
```

上位レイヤーの輝度のみを適用。

### 実用的なパターン

#### 画像上のテキスト

```html
<div class="hero">
  <img src="background.jpg" alt="背景">
  <h1>タイトル</h1>
</div>
```

```css
.hero {
  position: relative;
}

.hero img {
  width: 100%;
  display: block;
}

.hero h1 {
  position: absolute;
  inset: 0;
  display: grid;
  place-content: center;
  color: white;
  font-size: 4rem;
  mix-blend-mode: overlay;
}
```

#### デュアルトーン効果

```html
<div class="duotone">
  <img src="photo.jpg" alt="写真">
  <div class="overlay"></div>
</div>
```

```css
.duotone {
  position: relative;
}

.duotone img {
  display: block;
  filter: grayscale(1) contrast(1.2);
}

.duotone .overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  mix-blend-mode: screen;
}
```

#### ホバー時のカラーオーバーレイ

```css
.card {
  position: relative;
  overflow: hidden;
}

.card::after {
  content: '';
  position: absolute;
  inset: 0;
  background: #3498db;
  mix-blend-mode: multiply;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.card:hover::after {
  opacity: 0.3;
}
```

#### クロスフェード

```css
.crossfade-container {
  position: relative;
}

.image-1,
.image-2 {
  position: absolute;
  inset: 0;
}

.image-2 {
  opacity: 0;
  mix-blend-mode: plus-lighter;
  transition: opacity 1s ease;
}

.crossfade-container:hover .image-2 {
  opacity: 1;
}
```

`plus-lighter`モードを使うと、クロスフェード時に不透明度が正しく考慮されます。

### isolation: isolate

ブレンドモードの影響範囲を制限する場合に使用します。

```css
.container {
  isolation: isolate;
}

.child {
  mix-blend-mode: multiply;
  /* containerの背景とのみブレンドされる */
}
```

### background-blend-modeとの違い

- `mix-blend-mode`: 要素同士のブレンド
- `background-blend-mode`: 背景画像同士のブレンド

```css
.element {
  background-image: url('texture.png'), linear-gradient(135deg, #667eea, #764ba2);
  background-blend-mode: multiply;
}
```

### ブラウザサポート

- Chrome 41以降
- Safari 8以降
- Firefox 32以降
- Edge 79以降

---

## clip-path

### clip-pathとは

`clip-path`プロパティは、要素を特定の形状にクリッピング（切り抜き）します。長方形以外の複雑な形状を作成できます。

### 基本的な形状

#### circle（円）

```css
.circle {
  clip-path: circle(50%);
}
```

**構文:** `circle(半径 at 中心X 中心Y)`

```css
/* 中心を指定 */
clip-path: circle(100px at 50% 50%);

/* 左上に円 */
clip-path: circle(50px at 0 0);
```

#### ellipse（楕円）

```css
.ellipse {
  clip-path: ellipse(50% 30%);
}
```

**構文:** `ellipse(X半径 Y半径 at 中心X 中心Y)`

#### inset（内側の長方形）

```css
.inset {
  clip-path: inset(10px 20px 30px 40px);
}
```

**構文:** `inset(上 右 下 左 round border-radius)`

```css
/* 角を丸める */
clip-path: inset(20px round 10px);
```

#### polygon（多角形）

```css
.triangle {
  clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
}
```

**構文:** `polygon(X1 Y1, X2 Y2, X3 Y3, ...)`

座標はパーセントまたはピクセルで指定。

### 実用的な形状

#### 三角形

```css
.triangle-up {
  clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
}

.triangle-down {
  clip-path: polygon(0% 0%, 100% 0%, 50% 100%);
}

.triangle-left {
  clip-path: polygon(100% 0%, 100% 100%, 0% 50%);
}

.triangle-right {
  clip-path: polygon(0% 0%, 100% 50%, 0% 100%);
}
```

#### 台形

```css
.trapezoid {
  clip-path: polygon(20% 0%, 80% 0%, 100% 100%, 0% 100%);
}
```

#### 平行四辺形

```css
.parallelogram {
  clip-path: polygon(25% 0%, 100% 0%, 75% 100%, 0% 100%);
}
```

#### ひし形

```css
.rhombus {
  clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
}
```

#### 五角形

```css
.pentagon {
  clip-path: polygon(50% 0%, 100% 38%, 82% 100%, 18% 100%, 0% 38%);
}
```

#### 六角形

```css
.hexagon {
  clip-path: polygon(25% 0%, 75% 0%, 100% 50%, 75% 100%, 25% 100%, 0% 50%);
}
```

#### 星形

```css
.star {
  clip-path: polygon(
    50% 0%, 61% 35%, 98% 35%, 68% 57%,
    79% 91%, 50% 70%, 21% 91%, 32% 57%,
    2% 35%, 39% 35%
  );
}
```

#### 吹き出し

```css
.speech-bubble {
  clip-path: polygon(
    0% 0%, 100% 0%, 100% 75%,
    75% 75%, 75% 100%, 50% 75%,
    0% 75%
  );
}
```

### アニメーション

**重要:** 同じ数のポイントを持つ形状間でアニメーション可能です。

```css
.shape {
  clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
  transition: clip-path 0.3s ease;
}

.shape:hover {
  clip-path: polygon(50% 20%, 80% 50%, 50% 80%, 20% 50%);
}
```

### 実用例

#### ホバー時の展開効果

```css
.card {
  clip-path: inset(0 0 0 0);
  transition: clip-path 0.3s ease;
}

.card:hover {
  clip-path: inset(-10px -10px -10px -10px);
}
```

#### 画像の切り抜き

```css
.profile-image {
  clip-path: circle(50%);
}
```

#### セクションの装飾

```css
.section {
  background: linear-gradient(135deg, #667eea, #764ba2);
  clip-path: polygon(0 0, 100% 0, 100% 90%, 0 100%);
  padding: 80px 20px;
}
```

傾斜した下端を作成。

#### ボタンの装飾

```css
.button {
  clip-path: polygon(
    0% 20%, 10% 0%, 90% 0%, 100% 20%,
    100% 80%, 90% 100%, 10% 100%, 0% 80%
  );
  background: #3498db;
  padding: 15px 30px;
  color: white;
}
```

### Clippyツールの活用

複雑な形状を作成する際は、[Clippy - CSS clip-path maker](https://bennettfeely.com/clippy/)が便利です。

**機能:**
- ビジュアルエディタで形状を作成
- カスタムポリゴンの作成
- CSSコードの自動生成
- 様々なプリセット形状

### SVGパスの使用

より複雑な形状には、SVGパスを使用できます。

```css
.complex-shape {
  clip-path: path('M 0,0 L 100,0 L 100,100 C 75,100 25,100 0,100 Z');
}
```

### ブラウザサポート

- Chrome 55以降
- Safari 9.1以降
- Firefox 54以降
- Edge 79以降

---

## ベストプラクティス

### 1. パフォーマンスを考慮する

```css
/* GPU アクセラレーション */
.animated {
  will-change: transform, filter;
}

/* アニメーション終了後は削除 */
.animated.done {
  will-change: auto;
}
```

### 2. フォールバックを用意する

```css
.element {
  background: #3498db;
  
  /* グラデーションがサポートされていない場合の備え */
  @supports (mix-blend-mode: overlay) {
    background: linear-gradient(135deg, #667eea, #764ba2);
  }
}
```

### 3. 過度な使用を避ける

視覚効果は控えめに使用し、ユーザー体験を損なわないようにします。

### 4. アクセシビリティを考慮

```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation: none !important;
    transition: none !important;
  }
}
```

---

## アンチパターン

### ❌ 過度なフィルター

```css
/* 避けるべき */
.image {
  filter: brightness(1.5) contrast(1.5) saturate(2) hue-rotate(180deg);
}
```

効果を重ねすぎると不自然になります。

### ❌ 不必要な複雑さ

```css
/* 避けるべき */
.simple-circle {
  clip-path: polygon(
    /* 100個以上の座標で円を描く */
  );
}

/* 推奨 */
.simple-circle {
  clip-path: circle(50%);
}
```

### ❌ アニメーションの濫用

```css
/* 避けるべき */
.element {
  animation: spin 0.5s linear infinite,
             pulse 0.3s ease infinite,
             bounce 0.7s ease infinite;
}
```

ユーザーの注意を散漫にします。

---

## 参考資料

- [transform独立プロパティ](https://ics.media/entry/230309/)
- [filterプロパティ](https://ics.media/entry/15393/)
- [mix-blend-mode解説](https://ics.media/entry/7258/)
- [Clippy - clip-path maker](https://bennettfeely.com/clippy/)
- [MDN - CSS Filters](https://developer.mozilla.org/ja/docs/Web/CSS/filter)
- [MDN - mix-blend-mode](https://developer.mozilla.org/ja/docs/Web/CSS/mix-blend-mode)

---

次のセクション：[04. セレクタとアクセシビリティ編](../04-selectors-accessibility/README.md)
