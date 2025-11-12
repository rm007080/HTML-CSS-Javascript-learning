# HTML アクセシビリティ編

すべてのユーザーが利用できるWebサイトを作るためのアクセシビリティの基礎を学びます。

## 目次

1. [アクセシビリティとは](#1-アクセシビリティとは)
2. [WAI-ARIA の基礎](#2-wai-aria-の基礎)
3. [主要な aria 属性](#3-主要な-aria-属性)
4. [WCAG ガイドライン](#4-wcag-ガイドライン)
5. [アクセシブルなタブUI](#5-アクセシブルなタブui)
6. [実践的なチェックリスト](#6-実践的なチェックリスト)

---

## 1. アクセシビリティとは

### アクセシビリティの定義

**アクセシビリティ（Accessibility）** = 「アクセスのしやすさ」

誰もが、どんな環境でも、Webサイトの情報にアクセスでき、利用できることを指します。

### 対象となるユーザー

- **視覚障害者**：スクリーンリーダーを使用
- **聴覚障害者**：動画に字幕が必要
- **運動障害者**：マウスが使えない、キーボードのみで操作
- **認知障害者**：複雑すぎる操作が困難
- **高齢者**：小さい文字が読みにくい
- **一時的な制限**：怪我、環境（明るい屋外、騒がしい場所）

### なぜ重要なのか

#### 1. 法的要件

多くの国で、Webアクセシビリティは法的義務となっています。

#### 2. SEO への影響

アクセシブルなサイトは、検索エンジンにも理解しやすい構造です。

#### 3. ユーザーベースの拡大

障害の有無に関わらず、すべてのユーザーにサービスを提供できます。

#### 4. ユーザー体験の向上

明確な構造と操作性は、すべてのユーザーにとって有益です。

---

## 2. WAI-ARIA の基礎

### WAI-ARIA とは

**WAI-ARIA** = Web Accessibility Initiative - Accessible Rich Internet Applications

HTMLだけでは表現できない情報を、支援技術に伝えるための仕様です。

### WAI-ARIA の原則

**第1原則：**
> ネイティブHTML要素で可能なら、それを使用する

```html
<!-- ❌ 避けるべき -->
<div role="button" tabindex="0">ボタン</div>

<!-- ✅ 推奨 -->
<button>ボタン</button>
```

### ARIAの3つの要素

#### 1. ロール（Role）

要素が何か、何をするかを定義します。

```html
<div role="navigation">
  <a href="/">ホーム</a>
  <a href="/about">About</a>
</div>

<!-- しかし、ネイティブ要素を使うべき -->
<nav>
  <a href="/">ホーム</a>
  <a href="/about">About</a>
</nav>
```

**主なロール：**
- **ランドマークロール**：`banner`, `navigation`, `main`, `complementary`, `contentinfo`, `search`
- **UIパターン**：`button`, `tab`, `tablist`, `tabpanel`, `dialog`, `menu`

#### 2. プロパティ（Property）

要素の性質や特徴を定義します（静的な情報）。

```html
<button aria-label="メニューを開く">☰</button>
<input type="text" aria-required="true">
<div aria-labelledby="heading">...</div>
```

#### 3. ステート（State）

要素の現在の状態を定義します（動的な情報）。

```html
<button aria-pressed="true">選択中</button>
<div aria-expanded="false">折りたたみ</div>
<input type="checkbox" aria-checked="true">
```

### アクセシビリティツリー

ブラウザは以下の流れで情報を処理します：

1. **HTML** → DOMツリー生成
2. **DOMツリー** → アクセシビリティツリー生成
3. **アクセシビリティツリー** → 支援技術が読み取り

ARIA属性は、アクセシビリティツリーに情報を追加します。

---

## 3. 主要な aria 属性

### aria-label

要素に直接ラベルを付けます。

```html
<!-- アイコンのみのボタン -->
<button aria-label="メニューを開く">
  <svg><!-- ハンバーガーアイコン --></svg>
</button>

<!-- 検索フォーム -->
<form role="search">
  <input type="search" aria-label="サイト内検索">
  <button aria-label="検索">🔍</button>
</form>
```

**使用例：**
- アイコンのみのボタン
- 視覚的にラベルがない入力欄
- 装飾的な要素に意味を付与

**注意点：**
- すべてのロールが`aria-label`を受け付けるわけではない
- 可視ラベルがある場合は`aria-labelledby`を使う

### aria-labelledby

他の要素をラベルとして参照します。

```html
<h2 id="dialog-title">ユーザー設定</h2>
<div role="dialog" aria-labelledby="dialog-title">
  <p>ユーザー設定を行なってください</p>
  <!-- ダイアログの内容 -->
</div>
```

**複数要素の参照：**

```html
<h2 id="section-title">商品情報</h2>
<p id="section-desc">以下の商品が利用可能です</p>
<div aria-labelledby="section-title section-desc">
  <!-- コンテンツ -->
</div>
```

### aria-describedby

要素の詳細説明を参照します。

```html
<label for="password">パスワード</label>
<input type="password"
       id="password"
       aria-describedby="password-hint password-requirements">
<p id="password-hint">8文字以上で入力してください</p>
<p id="password-requirements">英数字と記号を含める必要があります</p>
```

### aria-current

現在の要素を示します。

```html
<nav>
  <a href="/" aria-current="page">ホーム</a>
  <a href="/about">About</a>
  <a href="/contact">Contact</a>
</nav>
```

**値の種類：**
- `page`：現在のページ
- `step`：ステップの現在位置
- `location`：現在の場所
- `date`：現在の日付
- `time`：現在の時刻
- `true`：現在の項目

### aria-haspopup

要素がポップアップメニューを持つことを示します。

```html
<button aria-haspopup="menu" aria-expanded="false">
  メニュー
</button>
```

**値の種類：**
- `menu`：メニュー
- `listbox`：リストボックス
- `tree`：ツリー
- `grid`：グリッド
- `dialog`：ダイアログ
- `true`：メニュー（デフォルト）

### aria-expanded

折りたたみ要素の開閉状態を示します。

```html
<button aria-expanded="false" aria-controls="menu">
  メニュー
</button>
<ul id="menu" hidden>
  <li><a href="#">項目1</a></li>
  <li><a href="#">項目2</a></li>
</ul>

<script>
const button = document.querySelector('button');
const menu = document.getElementById('menu');

button.addEventListener('click', () => {
  const isExpanded = button.getAttribute('aria-expanded') === 'true';
  button.setAttribute('aria-expanded', !isExpanded);
  menu.hidden = isExpanded;
});
</script>
```

### aria-hidden

要素をスクリーンリーダーから隠します。

```html
<!-- 装飾的なアイコン -->
<p>
  <span aria-hidden="true">✨</span>
  人気の記事
</p>

<!-- 視覚的には見えるが、読み上げ不要 -->
<button>
  <span aria-hidden="true">×</span>
  <span class="visually-hidden">閉じる</span>
</button>
```

**注意：**
- フォーカス可能な要素に`aria-hidden="true"`を使わない
- 重要な情報を隠さない

### aria-live

動的に更新されるコンテンツを通知します。

```html
<div aria-live="polite">
  <p>検索結果：12件</p>
</div>
```

**値の種類：**
- `off`：通知しない（デフォルト）
- `polite`：ユーザーの操作が終わってから通知
- `assertive`：即座に通知（緊急時のみ使用）

### aria-atomic

更新時に全体を読み上げるか、変更部分のみか。

```html
<div aria-live="polite" aria-atomic="true">
  <h2>タイトル</h2>
  <p>動的に更新される内容</p>
</div>
```

- `true`：全体を読み上げ
- `false`：変更部分のみ読み上げ（デフォルト）

---

## 4. WCAG ガイドライン

### WCAG とは

**WCAG** = Web Content Accessibility Guidelines

W3Cが定めるWebアクセシビリティの国際標準です。

### 4つの原則（POUR）

#### 1. Perceivable（知覚可能）

情報とUIコンポーネントは、ユーザーが知覚できる方法で提示される必要があります。

**実装例：**
- すべての画像に代替テキスト
- 動画に字幕・音声解説
- 十分な色のコントラスト
- テキストのサイズ変更が可能

#### 2. Operable（操作可能）

UIコンポーネントとナビゲーションは操作可能である必要があります。

**実装例：**
- キーボードのみで全機能が使える
- 十分な時間が与えられる
- 発作を引き起こす点滅を避ける
- ナビゲーションが容易

#### 3. Understandable（理解可能）

情報とUIの操作は理解可能である必要があります。

**実装例：**
- テキストが読みやすく理解しやすい
- Webページの動作が予測可能
- ユーザーのエラーを防ぎ、修正を支援

#### 4. Robust（堅牢）

コンテンツは、様々な支援技術を含む多様なユーザーエージェントで解釈できる必要があります。

**実装例：**
- 標準に準拠したHTML
- 適切なセマンティックマークアップ
- 支援技術との互換性

### 重要な達成基準

#### 代替テキスト（レベルA）

```html
<!-- ✅ 適切な代替テキスト -->
<img src="chart.png" alt="2024年の売上推移グラフ。1月から12月まで右肩上がり">

<!-- ✅ 装飾画像 -->
<img src="decoration.png" alt="">

<!-- ❌ 不適切 -->
<img src="chart.png" alt="画像">
```

#### 見出しの階層（レベルA）

```html
<!-- ✅ 正しい階層 -->
<h1>ページタイトル</h1>
  <h2>セクション1</h2>
    <h3>サブセクション1-1</h3>
  <h2>セクション2</h2>

<!-- ❌ 階層が飛んでいる -->
<h1>ページタイトル</h1>
  <h3>セクション1</h3> <!-- h2を飛ばしている -->
```

#### フォームのラベル（レベルA）

```html
<!-- ✅ 明示的な関連付け -->
<label for="email">メールアドレス</label>
<input type="email" id="email" name="email">

<!-- ✅ 暗黙的な関連付け -->
<label>
  メールアドレス
  <input type="email" name="email">
</label>

<!-- ❌ ラベルなし -->
<input type="email" name="email" placeholder="メールアドレス">
```

#### 色のコントラスト（レベルAA）

**最小コントラスト比：**
- 通常テキスト：4.5:1以上
- 大きいテキスト（18pt以上または14pt太字以上）：3:1以上

```css
/* ✅ 十分なコントラスト */
.text {
  color: #333; /* 黒に近い */
  background: #fff; /* 白 */
  /* コントラスト比：12.63:1 */
}

/* ❌ 不十分なコントラスト */
.text-low-contrast {
  color: #999; /* 薄いグレー */
  background: #fff; /* 白 */
  /* コントラスト比：2.85:1 */
}
```

#### キーボード操作（レベルA）

```html
<!-- ✅ キーボード操作可能 -->
<button>クリック</button>
<a href="/page">リンク</a>

<!-- ❌ キーボード操作不可（対策なし） -->
<div onclick="handleClick()">クリック</div>

<!-- ✅ 対策済み -->
<div role="button"
     tabindex="0"
     onclick="handleClick()"
     onkeypress="handleKeyPress(event)">
  クリック
</div>
```

---

## 5. アクセシブルなタブUI

### タブUIの構成要素

- **タブリスト**：タブをグループ化する要素
- **タブ**：パネルを切り替えるボタン
- **タブパネル**：コンテンツを表示する領域

### 適切なARIAロール

```html
<div class="tabs">
  <!-- タブリスト -->
  <div role="tablist" aria-label="コンテンツタブ">
    <button role="tab"
            aria-selected="true"
            aria-controls="panel-1"
            id="tab-1">
      タブ1
    </button>
    <button role="tab"
            aria-selected="false"
            aria-controls="panel-2"
            id="tab-2"
            tabindex="-1">
      タブ2
    </button>
    <button role="tab"
            aria-selected="false"
            aria-controls="panel-3"
            id="tab-3"
            tabindex="-1">
      タブ3
    </button>
  </div>

  <!-- タブパネル -->
  <div role="tabpanel"
       id="panel-1"
       aria-labelledby="tab-1"
       tabindex="0">
    <p>タブ1の内容</p>
  </div>

  <div role="tabpanel"
       id="panel-2"
       aria-labelledby="tab-2"
       tabindex="0"
       hidden>
    <p>タブ2の内容</p>
  </div>

  <div role="tabpanel"
       id="panel-3"
       aria-labelledby="tab-3"
       tabindex="0"
       hidden>
    <p>タブ3の内容</p>
  </div>
</div>
```

### JavaScript実装

```javascript
class AccessibleTabs {
  constructor(container) {
    this.container = container;
    this.tablist = container.querySelector('[role="tablist"]');
    this.tabs = Array.from(this.tablist.querySelectorAll('[role="tab"]'));
    this.panels = Array.from(container.querySelectorAll('[role="tabpanel"]'));

    this.init();
  }

  init() {
    // クリックイベント
    this.tabs.forEach(tab => {
      tab.addEventListener('click', () => this.selectTab(tab));
    });

    // キーボードナビゲーション
    this.tablist.addEventListener('keydown', (e) => this.handleKeyDown(e));
  }

  selectTab(selectedTab) {
    // すべてのタブを非選択にする
    this.tabs.forEach((tab, index) => {
      const isSelected = tab === selectedTab;

      tab.setAttribute('aria-selected', isSelected);
      tab.tabIndex = isSelected ? 0 : -1;
      this.panels[index].hidden = !isSelected;
    });

    selectedTab.focus();
  }

  handleKeyDown(e) {
    const currentIndex = this.tabs.indexOf(document.activeElement);
    let targetIndex;

    switch (e.key) {
      case 'ArrowRight':
      case 'ArrowDown':
        e.preventDefault();
        targetIndex = (currentIndex + 1) % this.tabs.length;
        this.selectTab(this.tabs[targetIndex]);
        break;

      case 'ArrowLeft':
      case 'ArrowUp':
        e.preventDefault();
        targetIndex = (currentIndex - 1 + this.tabs.length) % this.tabs.length;
        this.selectTab(this.tabs[targetIndex]);
        break;

      case 'Home':
        e.preventDefault();
        this.selectTab(this.tabs[0]);
        break;

      case 'End':
        e.preventDefault();
        this.selectTab(this.tabs[this.tabs.length - 1]);
        break;
    }
  }
}

// 初期化
document.querySelectorAll('.tabs').forEach(tabs => {
  new AccessibleTabs(tabs);
});
```

### キーボード操作

**必須の操作：**
- `Tab`：タブリストに入る、タブパネルに移動
- `Arrow Right/Down`：次のタブへ移動
- `Arrow Left/Up`：前のタブへ移動
- `Home`：最初のタブへ移動
- `End`：最後のタブへ移動

---

## 6. 実践的なチェックリスト

### 必須項目（レベルA）

#### 画像
- [ ] すべての画像に適切な`alt`属性
- [ ] 装飾画像は`alt=""`

#### 見出し
- [ ] すべてのページに`h1`が1つ
- [ ] 見出しレベルを飛ばさない

#### リンク
- [ ] リンクテキストが明確
- [ ] "こちら"、"詳細"だけのリンクを避ける

#### フォーム
- [ ] すべての入力欄に`label`
- [ ] 必須項目を明示
- [ ] エラーメッセージが明確

#### キーボード操作
- [ ] すべての機能がキーボードで操作可能
- [ ] フォーカス順序が論理的

### 推奨項目（レベルAA）

#### 色とコントラスト
- [ ] テキストのコントラスト比が4.5:1以上
- [ ] 色だけで情報を伝えない

#### ナビゲーション
- [ ] スキップリンクを提供
- [ ] パンくずリストを提供
- [ ] 現在のページを明示

#### コンテンツ
- [ ] ページタイトルが内容を表している
- [ ] 言語を指定（`lang`属性）

### 検証ツール

**自動チェックツール：**
- [Lighthouse](https://chrome.google.com/webstore/detail/lighthouse/)
- [axe DevTools](https://www.deque.com/axe/devtools/)
- [WAVE](https://wave.webaim.org/)

**手動チェック：**
- キーボードのみで操作してみる
- スクリーンリーダーで確認（NVDA、VoiceOver、ナレーター）
- ブラウザのズームで200%表示

---

## まとめ

アクセシビリティの重要なポイント：

1. **ネイティブHTML優先**：可能な限り標準要素を使用
2. **aria属性の適切な使用**：必要な場所にのみ使用
3. **キーボード操作**：すべての機能が操作可能に
4. **明確なラベル**：すべての要素に適切な名前を付ける
5. **WCAGガイドライン**：国際標準に準拠

アクセシビリティは：
- ✅ すべてのユーザーのため
- ✅ SEOにも有利
- ✅ 法的要件を満たす
- ✅ ユーザー体験の向上

---

## 次のステップ

- [HTMLパフォーマンス編](../04-performance/README.md)へ進む
- [実践的なコード例](../../../examples/)を確認する
