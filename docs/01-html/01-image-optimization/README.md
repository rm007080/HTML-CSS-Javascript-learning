# HTML 画像最適化編

Webサイトのパフォーマンスを左右する画像の最適化技術を学びます。

## 目次

1. [画像最適化の重要性](#1-画像最適化の重要性)
2. [レスポンシブ画像（srcset/sizes）](#2-レスポンシブ画像srcsetsizes)
3. [picture 要素とモダン画像フォーマット](#3-picture-要素とモダン画像フォーマット)
4. [遅延読み込み（lazy loading）](#4-遅延読み込みlazy-loading)
5. [decoding 属性](#5-decoding-属性)
6. [画像の幅と高さの指定](#6-画像の幅と高さの指定)
7. [実践的なベストプラクティス](#7-実践的なベストプラクティス)

---

## 1. 画像最適化の重要性

### なぜ画像最適化が重要なのか

**驚くべき統計：**
- 95.9%のウェブサイトが画像を含んでいる
- **LCP要素の約70%が画像である**
- サイト容量の半分以上は画像が占めている

つまり、**画像の最適化 = サイト速度改善の最大のポイント**です。

### Core Web Vitals との関係

Googleが検索評価に組み込んだ3つの指標：

#### 1. LCP（Largest Contentful Paint）
- **目標：2.5秒以内**
- メインコンテンツの読み込み速度
- **画像が最大の影響を与える指標**

#### 2. CLS（Cumulative Layout Shift）
- **目標：0.1未満**
- 予期しないレイアウトのズレを防止
- **サイズ未指定の画像が主な原因**

#### 3. FID（First Input Delay）
- **目標：100ミリ秒未満**
- ユーザー操作への反応速度
- 画像の読み込みが重いと遅延する

### 画像最適化の4つの柱

1. **適切なフォーマット選択**（AVIF > WebP > JPEG）
2. **適切なサイズ提供**（srcset/sizes）
3. **遅延読み込み**（loading="lazy"）
4. **レイアウトシフト防止**（width/height指定）

---

## 2. レスポンシブ画像（srcset/sizes）

### 問題：同じ画像をすべてのデバイスに配信

```html
<!-- ❌ 非効率 -->
<img src="large-image-2000w.jpg" alt="商品画像">
```

スマートフォンで2000px幅の画像は不要です。データ通信量の無駄になります。

### 解決策：srcset 属性

複数サイズの画像を用意し、ブラウザに最適なものを選ばせます。

```html
<img src="image-800w.jpg"
     srcset="image-400w.jpg 400w,
             image-800w.jpg 800w,
             image-1200w.jpg 1200w"
     alt="商品画像">
```

**記法の説明：**
- `400w` = この画像は幅400pxです
- `800w` = この画像は幅800pxです
- ブラウザがデバイスに合わせて自動選択

### sizes 属性で表示幅を指定

```html
<img src="image-800w.jpg"
     srcset="image-400w.jpg 400w,
             image-800w.jpg 800w,
             image-1200w.jpg 1200w"
     sizes="(max-width: 640px) 100vw,
            (max-width: 1024px) 50vw,
            800px"
     alt="商品画像">
```

**sizes の意味：**
- 640px以下：画面幅の100%で表示
- 1024px以下：画面幅の50%で表示
- それ以上：800pxで表示

### 完全な実装例

```html
<img src="keyboard-800w.jpg"
     srcset="keyboard-400w.jpg 400w,
             keyboard-800w.jpg 800w,
             keyboard-1200w.jpg 1200w,
             keyboard-1600w.jpg 1600w"
     sizes="(max-width: 640px) 400px,
            (max-width: 1024px) 800px,
            1200px"
     width="800"
     height="600"
     alt="メカニカルキーボード">
```

### 高解像度ディスプレイ対応

Retina ディスプレイなどの高密度画面にも対応できます。

```html
<img src="image.jpg"
     srcset="image-1x.jpg 1x,
             image-2x.jpg 2x,
             image-3x.jpg 3x"
     alt="商品画像">
```

---

## 3. picture 要素とモダン画像フォーマット

### モダン画像フォーマットの利点

**AVIF（最新）：**
- 最も高い圧縮率
- 同じ品質でファイルサイズが最小
- 対応ブラウザ：Chrome 85+、Firefox 93+、Safari 16+

**WebP：**
- 高い圧縮率
- AVIFより広くサポートされている
- 対応ブラウザ：Chrome、Firefox、Edge、Safari 14+

**JPEG/PNG（従来）：**
- すべてのブラウザで対応
- フォールバック用に必須

### picture 要素の基本

```html
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="商品画像">
</picture>
```

**動作：**
1. ブラウザは上から順に確認
2. AVIF対応 → image.avif を使用
3. AVIF非対応でWebP対応 → image.webp を使用
4. どちらも非対応 → image.jpg を使用（フォールバック）

### レスポンシブ + モダンフォーマット

```html
<picture>
  <!-- AVIF: モバイル用 -->
  <source media="(max-width: 640px)"
          srcset="image-small.avif"
          type="image/avif">

  <!-- AVIF: デスクトップ用 -->
  <source media="(min-width: 641px)"
          srcset="image-large.avif"
          type="image/avif">

  <!-- WebP: モバイル用 -->
  <source media="(max-width: 640px)"
          srcset="image-small.webp"
          type="image/webp">

  <!-- WebP: デスクトップ用 -->
  <source media="(min-width: 641px)"
          srcset="image-large.webp"
          type="image/webp">

  <!-- フォールバック -->
  <img src="image.jpg"
       alt="商品画像"
       width="800"
       height="600">
</picture>
```

### アートディレクション

デバイスごとに異なる画像を表示する。

```html
<picture>
  <!-- モバイル：正方形にトリミング -->
  <source media="(max-width: 640px)"
          srcset="hero-mobile.jpg">

  <!-- タブレット：16:9 -->
  <source media="(max-width: 1024px)"
          srcset="hero-tablet.jpg">

  <!-- デスクトップ：ワイド -->
  <img src="hero-desktop.jpg"
       alt="ヒーロー画像"
       width="1920"
       height="600">
</picture>
```

---

## 4. 遅延読み込み（lazy loading）

### loading 属性（ネイティブサポート）

最もシンプルで推奨される方法。

```html
<img src="image.jpg"
     loading="lazy"
     width="800"
     height="600"
     alt="商品画像">
```

### 動作の仕組み

- **loading="lazy"**: ビューポート近くに来るまで読み込まない
- **loading="eager"**: 即座に読み込む（デフォルト）
- **省略時**: ブラウザが自動判断

### 適用すべき画像

✅ **lazy を使うべき画像：**
- ページ下部の画像
- スクロールしないと見えない画像
- ギャラリーの後半の画像

❌ **lazy を使わない画像：**
- ファーストビューの画像（LCP対象）
- ロゴや重要なUI要素
- ページ上部の画像

### 実装例

```html
<!-- ヒーロー画像：即座に読み込む -->
<img src="hero.jpg"
     width="1920"
     height="600"
     alt="ヒーロー画像">

<!-- ページ下部：遅延読み込み -->
<img src="feature-1.jpg"
     loading="lazy"
     width="800"
     height="600"
     alt="機能1">

<img src="feature-2.jpg"
     loading="lazy"
     width="800"
     height="600"
     alt="機能2">
```

### iframe にも適用可能

```html
<iframe src="https://www.youtube.com/embed/VIDEO_ID"
        loading="lazy"
        width="560"
        height="315"
        title="動画タイトル">
</iframe>
```

### ブラウザ対応

- Chrome 77+
- Firefox 75+
- Safari 15.4+
- Edge 79+

古いブラウザでは単に無視されるだけなので、安全に使用できます。

---

## 5. decoding 属性

### decoding 属性の役割

画像のデコード（解析）処理を制御します。

```html
<img src="image.jpg"
     decoding="async"
     alt="商品画像">
```

### 3つの値

#### async（非同期）
```html
<img src="image.jpg" decoding="async" alt="画像">
```
- デコード処理を非同期で実行
- ページのレンダリングをブロックしない
- **用途：** 重要でない装飾画像

#### sync（同期）
```html
<img src="hero.jpg" decoding="sync" alt="ヒーロー画像">
```
- デコード完了を待ってから表示
- ちらつきを防止
- **用途：** LCP対象画像

#### auto（自動・デフォルト）
```html
<img src="image.jpg" alt="画像">
```
- ブラウザが自動判断
- **推奨：** 基本的にはautoで問題ない

### 重要な注意点

**decoding="async"の問題：**
キャッシュされた画像で「ちらつき」が発生することがある。

**推奨：**
```html
<!-- 特別な理由がなければ、decoding属性は省略 -->
<img src="image.jpg"
     loading="lazy"
     width="800"
     height="600"
     alt="商品画像">
```

### loading と decoding の違い

| 属性 | 制御対象 | 目的 |
|------|---------|------|
| loading | ダウンロードタイミング | 必要になるまでダウンロードしない |
| decoding | デコード方式 | デコード処理の同期/非同期 |

---

## 6. 画像の幅と高さの指定

### 問題：レイアウトシフト（CLS）

サイズ未指定の画像は、読み込み完了後にレイアウトがガタつく。

```html
<!-- ❌ レイアウトシフトが発生 -->
<img src="image.jpg" alt="商品画像">
```

**起こること：**
1. ページ表示（画像の場所は0px×0px）
2. 画像読み込み完了（800px×600pxに拡大）
3. 下のコンテンツが押し下げられる ← **ユーザー体験が悪い**

### 解決策：width と height を指定

```html
<!-- ✅ レイアウトシフトを防止 -->
<img src="image.jpg"
     width="800"
     height="600"
     alt="商品画像">
```

**動作：**
1. ページ表示時にブラウザが800px×600pxの領域を確保
2. 画像読み込み中も領域は確保されたまま
3. 画像が表示されても下のコンテンツは動かない

### CSS との組み合わせ

```html
<img src="image.jpg"
     width="1600"
     height="900"
     alt="商品画像"
     style="max-width: 100%; height: auto;">
```

または CSS で：

```css
img {
  max-width: 100%;
  height: auto;
}
```

**動作：**
- HTML属性でアスペクト比を指定（16:9）
- CSSで実際の表示サイズを制御
- レスポンシブにも対応

### aspect-ratio プロパティ（モダンCSS）

```css
img {
  width: 100%;
  aspect-ratio: attr(width) / attr(height);
}
```

HTML属性から自動的にアスペクト比を計算します。

---

## 7. 実践的なベストプラクティス

### 完全な実装例

```html
<picture>
  <!-- AVIF: モバイル -->
  <source media="(max-width: 640px)"
          srcset="product-400.avif 400w,
                  product-800.avif 800w"
          sizes="100vw"
          type="image/avif">

  <!-- AVIF: デスクトップ -->
  <source media="(min-width: 641px)"
          srcset="product-800.avif 800w,
                  product-1200.avif 1200w,
                  product-1600.avif 1600w"
          sizes="(max-width: 1024px) 50vw, 800px"
          type="image/avif">

  <!-- WebP: フォールバック -->
  <source srcset="product-400.webp 400w,
                  product-800.webp 800w,
                  product-1200.webp 1200w"
          sizes="(max-width: 640px) 100vw,
                 (max-width: 1024px) 50vw,
                 800px"
          type="image/webp">

  <!-- JPEG: 最終フォールバック -->
  <img src="product-800.jpg"
       srcset="product-400.jpg 400w,
               product-800.jpg 800w,
               product-1200.jpg 1200w"
       sizes="(max-width: 640px) 100vw,
              (max-width: 1024px) 50vw,
              800px"
       width="800"
       height="600"
       loading="lazy"
       alt="商品名 - 詳細な説明">
</picture>
```

### チェックリスト

#### 必須項目

- [ ] すべての画像に `width` と `height` を指定
- [ ] ファーストビュー以外の画像に `loading="lazy"` を指定
- [ ] すべての画像に適切な `alt` テキストを記述

#### 推奨項目

- [ ] 複数サイズの画像を用意し `srcset` を使用
- [ ] `sizes` 属性で表示幅を指定
- [ ] モダンフォーマット（AVIF/WebP）を提供
- [ ] 画像を圧縮（TinyPNG、Squooshなど）

#### パフォーマンス向上

- [ ] LCP対象画像を特定
- [ ] LCP画像に `rel="preload"` を使用
- [ ] 不要な画像を削除
- [ ] CDNを活用

### ツール

**画像圧縮：**
- [TinyPNG](https://tinypng.com/) - PNG/JPEGの圧縮
- [Squoosh](https://squoosh.app/) - 各種フォーマット対応

**画像変換：**
- [Convertio](https://convertio.co/ja/) - フォーマット変換
- ImageMagick - コマンドラインツール

**パフォーマンス測定：**
- [PageSpeed Insights](https://pagespeed.web.dev/)
- [Lighthouse](https://chrome.google.com/webstore/detail/lighthouse/)

### 画像フォーマット選択ガイド

| 用途 | 推奨フォーマット | フォールバック |
|------|----------------|---------------|
| 写真 | AVIF → WebP | JPEG |
| イラスト（少色） | AVIF → WebP | PNG-8 |
| イラスト（多色） | AVIF → WebP | PNG-24 |
| ロゴ・アイコン | SVG | PNG |
| アニメーション | AVIF → WebP | GIF |
| 透過が必要 | AVIF → WebP | PNG |

---

## まとめ

画像最適化の重要なポイント：

1. **srcset/sizes**: デバイスに最適なサイズを提供
2. **picture + モダンフォーマット**: AVIF/WebPで容量削減
3. **loading="lazy"**: 必要になるまで読み込まない
4. **width/height指定**: レイアウトシフトを防止
5. **適切な圧縮**: 品質を保ちながらファイルサイズ削減

これらを実装することで：
- ✅ ページ読み込み速度が向上
- ✅ Core Web Vitals のスコアが改善
- ✅ SEO評価が向上
- ✅ ユーザー体験が向上

---

## 次のステップ

- [HTMLセマンティック要素編](../02-semantic-elements/README.md)へ進む
- [実践的なコード例](../../../examples/)を確認する
