# HTML セマンティック要素編

モダンなHTMLのセマンティック要素を学び、意味のあるマークアップを身につけます。

## 目次

1. [セマンティックHTMLとは](#1-セマンティックhtmlとは)
2. [details/summary 要素](#2-detailssummary-要素)
3. [dialog 要素](#3-dialog-要素)
4. [hgroup 要素](#4-hgroup-要素)
5. [search 要素](#5-search-要素)
6. [その他のセマンティック要素](#6-その他のセマンティック要素)

---

## 1. セマンティックHTMLとは

### セマンティックの意味

**セマンティック（semantic）** = 「意味論的な」

HTMLタグが**見た目だけでなく意味を持つ**ことを指します。

### なぜ重要なのか

#### 1. アクセシビリティの向上

スクリーンリーダーなどの支援技術が、コンテンツの構造と意味を理解できます。

```html
<!-- ❌ 意味が不明 -->
<div class="nav">
  <div class="link">ホーム</div>
  <div class="link">About</div>
</div>

<!-- ✅ 意味が明確 -->
<nav>
  <a href="/">ホーム</a>
  <a href="/about">About</a>
</nav>
```

#### 2. SEOの向上

検索エンジンがページの構造を正しく理解できます。

#### 3. 保守性の向上

コードを読んだときに、各部分の役割が明確になります。

#### 4. 標準機能の活用

ブラウザの組み込み機能を活用できます。

---

## 2. details/summary 要素

### 基本的な使い方

`<details>`と`<summary>`でアコーディオン（折りたたみ）UIを作成できます。

```html
<details>
  <summary>詳細を表示</summary>
  <p>ここに詳細な内容が表示されます。</p>
</details>
```

### 特徴

- ✅ JavaScriptなしで動作
- ✅ キーボード操作に対応（Enterキー/スペースキー）
- ✅ スクリーンリーダーに対応
- ✅ ブラウザ内検索に対応（閉じた状態でも検索可能）

### デフォルトで開いた状態

```html
<details open>
  <summary>詳細を表示</summary>
  <p>最初から開いています。</p>
</details>
```

### 複数のアコーディオン

```html
<details>
  <summary>よくある質問 1</summary>
  <p>回答 1</p>
</details>

<details>
  <summary>よくある質問 2</summary>
  <p>回答 2</p>
</details>

<details>
  <summary>よくある質問 3</summary>
  <p>回答 3</p>
</details>
```

### name 属性で排他的動作

**重要：** 同じ`name`属性を持つ`<details>`要素は、1つだけが開きます（アコーディオングループ）。

```html
<details name="faq">
  <summary>質問 1</summary>
  <p>回答 1</p>
</details>

<details name="faq">
  <summary>質問 2</summary>
  <p>回答 2</p>
</details>

<details name="faq">
  <summary>質問 3</summary>
  <p>回答 3</p>
</details>
```

**動作：**
- 質問 2 を開くと、質問 1 は自動的に閉じる
- 常に1つだけが開いた状態になる

### CSS でスタイリング

#### デフォルトの三角形を削除

```css
summary {
  list-style: none; /* Chrome, Safari */
}

summary::-webkit-details-marker {
  display: none; /* Chrome, Safari（古いブラウザ） */
}
```

#### カスタムアイコンの追加

```css
summary {
  list-style: none;
  cursor: pointer;
  position: relative;
  padding-left: 30px;
}

summary::before {
  content: "▶";
  position: absolute;
  left: 0;
  transition: transform 0.3s;
}

details[open] summary::before {
  transform: rotate(90deg);
}
```

#### 開閉アニメーション（CSS のみ・部分対応）

```css
details summary {
  cursor: pointer;
}

details[open] > :not(summary) {
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
```

### JavaScript でアニメーション（全ブラウザ対応）

```html
<details>
  <summary>詳細を表示</summary>
  <div class="details-content">
    <p>ここに詳細な内容が表示されます。</p>
  </div>
</details>

<script>
document.querySelectorAll('details').forEach(details => {
  const summary = details.querySelector('summary');
  const content = details.querySelector('.details-content');

  summary.addEventListener('click', (e) => {
    e.preventDefault();

    if (details.open) {
      // 閉じるアニメーション
      const animation = content.animate([
        { height: content.offsetHeight + 'px', opacity: 1 },
        { height: 0, opacity: 0 }
      ], {
        duration: 300,
        easing: 'ease-out'
      });

      animation.onfinish = () => {
        details.open = false;
      };
    } else {
      // 開くアニメーション
      details.open = true;
      const animation = content.animate([
        { height: 0, opacity: 0 },
        { height: content.offsetHeight + 'px', opacity: 1 }
      ], {
        duration: 300,
        easing: 'ease-out'
      });
    }
  });
});
</script>
```

### 実用例：FAQ セクション

```html
<section>
  <h2>よくある質問</h2>

  <details name="faq">
    <summary>配送料はいくらですか？</summary>
    <p>全国一律500円です。5,000円以上のご購入で送料無料になります。</p>
  </details>

  <details name="faq">
    <summary>返品・交換はできますか？</summary>
    <p>商品到着後7日以内であれば、未開封品に限り返品・交換を承ります。</p>
  </details>

  <details name="faq">
    <summary>支払い方法は何がありますか？</summary>
    <p>クレジットカード、銀行振込、代金引換、コンビニ決済に対応しています。</p>
  </details>
</section>
```

---

## 3. dialog 要素

### 基本的な使い方

`<dialog>`要素は、モーダルダイアログやポップアップを作成するための標準要素です。

```html
<dialog id="myDialog">
  <h2>ダイアログタイトル</h2>
  <p>ダイアログの内容がここに表示されます。</p>
  <button id="closeDialog">閉じる</button>
</dialog>

<button id="openDialog">ダイアログを開く</button>

<script>
const dialog = document.getElementById('myDialog');
const openButton = document.getElementById('openDialog');
const closeButton = document.getElementById('closeDialog');

openButton.addEventListener('click', () => {
  dialog.showModal(); // モーダルとして表示
});

closeButton.addEventListener('click', () => {
  dialog.close();
});
</script>
```

### 2つの表示モード

#### 1. showModal() - モーダルダイアログ

```javascript
dialog.showModal();
```

**特徴：**
- 最上位レイヤーに表示
- 背面が不活性化（inert）
- 背面スクロール防止
- Escキーで自動的に閉じる
- フォーカストラップ（ダイアログ内にフォーカスを閉じ込める）

#### 2. show() - 非モーダルダイアログ

```javascript
dialog.show();
```

**特徴：**
- 通常のレイヤーに表示
- 背面も操作可能
- Escキーで閉じない

### 標準で提供される機能

✅ **自動提供される機能：**
- 最上位レイヤー表示
- 背面コンテンツの不活性化
- Escキーでのクローズ
- フォーカス管理

❌ **自分で実装が必要：**
- 背面スクロール抑制
- オーバーレイクリック時のクローズ
- 開閉アニメーション

### 背面スクロール抑制

```css
/* dialogが開いているとき */
body:has(dialog[open]) {
  overflow: hidden;
}
```

### オーバーレイクリックで閉じる

```javascript
dialog.addEventListener('click', (e) => {
  // dialog要素自体がクリックされた場合（背景クリック）
  if (e.target === dialog) {
    dialog.close();
  }
});
```

### 開閉アニメーション

```css
dialog {
  opacity: 0;
  transform: translateY(-50px);
  transition: opacity 0.3s, transform 0.3s, display 0.3s allow-discrete;
}

dialog[open] {
  opacity: 1;
  transform: translateY(0);
}

/* 背景（backdrop） */
dialog::backdrop {
  background-color: rgba(0, 0, 0, 0);
  transition: background-color 0.3s;
}

dialog[open]::backdrop {
  background-color: rgba(0, 0, 0, 0.5);
}
```

### JavaScript でのアニメーション制御

```javascript
const dialog = document.getElementById('myDialog');

// 開く
function openDialog() {
  dialog.showModal();
  requestAnimationFrame(() => {
    dialog.setAttribute('data-active', 'true');
  });
}

// 閉じる
function closeDialog() {
  dialog.setAttribute('data-active', 'false');
  setTimeout(() => {
    dialog.close();
  }, 300); // アニメーション時間と合わせる
}
```

```css
dialog {
  opacity: 0;
  transform: scale(0.9);
  transition: opacity 0.3s, transform 0.3s;
}

dialog[data-active="true"] {
  opacity: 1;
  transform: scale(1);
}
```

### 完全な実装例

```html
<dialog id="confirmDialog">
  <form method="dialog">
    <h2>確認</h2>
    <p>本当に削除しますか？</p>
    <div class="dialog-buttons">
      <button value="cancel" type="submit">キャンセル</button>
      <button value="confirm" type="submit">削除</button>
    </div>
  </form>
</dialog>

<button id="deleteButton">削除</button>

<script>
const dialog = document.getElementById('confirmDialog');
const deleteButton = document.getElementById('deleteButton');

deleteButton.addEventListener('click', () => {
  dialog.showModal();
});

dialog.addEventListener('close', () => {
  if (dialog.returnValue === 'confirm') {
    console.log('削除が実行されました');
  } else {
    console.log('キャンセルされました');
  }
});

// オーバーレイクリックで閉じる
dialog.addEventListener('click', (e) => {
  if (e.target === dialog) {
    dialog.close('cancel');
  }
});
</script>

<style>
dialog {
  border: none;
  border-radius: 8px;
  padding: 20px;
  max-width: 400px;
}

dialog::backdrop {
  background-color: rgba(0, 0, 0, 0.5);
}

.dialog-buttons {
  display: flex;
  gap: 10px;
  justify-content: flex-end;
  margin-top: 20px;
}
</style>
```

### 重要な注意点

**禁止事項：**
- ❌ `tabindex`属性を使用しない
- ❌ `role`属性を上書きしない

**推奨：**
- ✅ `<form method="dialog">`を使用
- ✅ `close()`メソッドで閉じる
- ✅ `returnValue`で結果を取得

---

## 4. hgroup 要素

### hgroup 要素とは

主題（見出し）と副題をグループ化するための要素です。

### 基本的な使い方

```html
<hgroup>
  <h1>モダンなWeb開発</h1>
  <p>HTML/CSS/JavaScriptの学習ガイド</p>
</hgroup>
```

### 仕様の変遷

**2009年：** 初登場
- 複数の見出し要素（h1, h2）をグループ化

**2022年：** 仕様変更
- **単一の見出し要素**と**複数のp要素**で構成

**2023年：** ARIAロール変更
- `generic`から`group`へ変更
- セマンティック的な価値が確立

### 正しい使い方（現在の仕様）

```html
<!-- ✅ 正しい -->
<hgroup>
  <h1>主題</h1>
  <p>副題テキスト</p>
</hgroup>

<!-- ✅ 複数の副題も可能 -->
<hgroup>
  <h2>記事タイトル</h2>
  <p>サブタイトル</p>
  <p>2024年1月15日</p>
</hgroup>
```

### 間違った使い方

```html
<!-- ❌ 複数の見出し要素（古い仕様） -->
<hgroup>
  <h1>主題</h1>
  <h2>副題</h2>
</hgroup>

<!-- ❌ 見出しのみ -->
<hgroup>
  <h1>タイトルだけ</h1>
</hgroup>
```

### 他の方法との比較

#### 見出し内に副題を含める

```html
<!-- △ セマンティック的に不十分 -->
<h1>
  主題
  <span class="subtitle">副題</span>
</h1>
```

**問題点：**
- 副題がセマンティックに独立していない
- スクリーンリーダーがすべて見出しとして読み上げる

#### CSS疑似要素

```html
<!-- ❌ アクセシビリティの問題 -->
<h1 data-subtitle="副題">主題</h1>
```

```css
h1::after {
  content: attr(data-subtitle);
  display: block;
}
```

**問題点：**
- ブラウザの検索機能に非対応
- スクリーンリーダーが読み上げない
- 翻訳されない

### 実用例

```html
<article>
  <hgroup>
    <h1>JavaScriptの非同期処理</h1>
    <p>Promise、async/awaitを完全理解</p>
  </hgroup>

  <p>この記事では、JavaScriptの非同期処理について...</p>
</article>
```

```html
<section>
  <hgroup>
    <h2>2024年のWeb開発トレンド</h2>
    <p>最新技術と実践的なアプローチ</p>
    <p>更新日：2024年1月15日</p>
  </hgroup>

  <p>2024年のWeb開発において...</p>
</section>
```

---

## 5. search 要素

### search 要素とは

2023年3月に正式に追加された、検索機能を表すセマンティック要素です。

### なぜ重要なのか

**ARIAランドマークロール：**
- banner（`<header>`）
- navigation（`<nav>`）
- main（`<main>`）
- complementary（`<aside>`）
- contentinfo（`<footer>`）
- form（`<form>`）
- region（`<section>`）
- **search** ← これまでネイティブ要素がなかった

### 基本的な使い方

```html
<search>
  <form action="/search" method="get">
    <label for="search">サイト内検索</label>
    <input type="search" id="search" name="q">
    <button type="submit">検索</button>
  </form>
</search>
```

### 従来の方法

```html
<!-- 従来：role属性で代替 -->
<div role="search">
  <form action="/search">
    <input type="search" name="q">
    <button>検索</button>
  </form>
</div>
```

### ネイティブ要素の利点

**WAI-ARIAの第1原則：**
> 「必要なセマンティクスと動作がすでに組み込まれたネイティブのHTML要素や属性を使用できる場合は、ネイティブのものを使用する」

✅ **ネイティブ要素を使うべき理由：**
- ブラウザのサポートが標準化
- role属性の記述が不要
- 将来の機能拡張に対応

### 互換性を考慮した実装

```html
<!-- 古いブラウザとの互換性を保つ -->
<search role="search">
  <form action="/search">
    <input type="search" name="q" aria-label="サイト内検索">
    <button>検索</button>
  </form>
</search>
```

### 実用例

#### サイトヘッダーの検索

```html
<header>
  <h1>サイト名</h1>

  <search>
    <form action="/search" role="search">
      <label for="site-search">サイト内検索</label>
      <input type="search"
             id="site-search"
             name="q"
             placeholder="キーワードを入力">
      <button type="submit">
        <span class="visually-hidden">検索</span>
        🔍
      </button>
    </form>
  </search>

  <nav>
    <a href="/">ホーム</a>
    <a href="/about">About</a>
  </nav>
</header>
```

#### フィルター機能

```html
<search>
  <form>
    <h2>商品を絞り込む</h2>

    <label for="category">カテゴリ</label>
    <select id="category" name="category">
      <option value="">すべて</option>
      <option value="electronics">家電</option>
      <option value="fashion">ファッション</option>
    </select>

    <label for="price-range">価格帯</label>
    <select id="price-range" name="price">
      <option value="">指定なし</option>
      <option value="0-5000">〜5,000円</option>
      <option value="5000-10000">5,000〜10,000円</option>
    </select>

    <button type="submit">絞り込む</button>
  </form>
</search>
```

### 注意点

**search要素の特性：**
- ランドマーク機能のみ提供
- 特別な動作は持たない
- `<header>`や`<footer>`と同じ構造要素

**使用場所：**
- サイト全体の検索
- ページ内の検索
- フィルター機能
- 絞り込み機能

---

## 6. その他のセマンティック要素

### セクション構造

```html
<header>
  <h1>サイト名</h1>
  <nav>
    <a href="/">ホーム</a>
    <a href="/about">About</a>
  </nav>
</header>

<main>
  <article>
    <header>
      <h1>記事タイトル</h1>
      <time datetime="2024-01-15">2024年1月15日</time>
    </header>

    <p>記事の本文...</p>

    <footer>
      <p>著者：太郎</p>
    </footer>
  </article>

  <aside>
    <h2>関連記事</h2>
    <ul>
      <li><a href="#">関連記事1</a></li>
      <li><a href="#">関連記事2</a></li>
    </ul>
  </aside>
</main>

<footer>
  <p>&copy; 2024 サイト名</p>
</footer>
```

### time 要素

```html
<!-- 日付 -->
<time datetime="2024-01-15">2024年1月15日</time>

<!-- 日時 -->
<time datetime="2024-01-15T14:30:00+09:00">
  2024年1月15日 14時30分
</time>

<!-- 期間 -->
<time datetime="P3D">3日間</time>
```

### mark 要素

```html
<p>検索結果：<mark>JavaScript</mark>に関する記事</p>
```

### figure/figcaption 要素

```html
<figure>
  <img src="chart.png" alt="売上グラフ">
  <figcaption>図1：2024年の売上推移</figcaption>
</figure>
```

---

## まとめ

セマンティック要素の重要なポイント：

1. **details/summary**: JavaScriptなしでアコーディオンを実装
2. **dialog**: 標準的なモーダルダイアログ
3. **hgroup**: 主題と副題のグループ化
4. **search**: 検索機能のランドマーク
5. **その他**: header、nav、main、article、aside、footer

これらを使うことで：
- ✅ アクセシビリティが向上
- ✅ SEOが改善
- ✅ コードの可読性が向上
- ✅ 標準機能を活用できる

---

## 次のステップ

- [HTMLアクセシビリティ編](../03-accessibility/README.md)へ進む
- [実践的なコード例](../../../examples/)を確認する
