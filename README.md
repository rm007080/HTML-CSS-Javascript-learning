# HTML/CSS/JavaScript 学習教材

モダンなWeb開発のための包括的な学習教材です。初学者から実務者まで、現代のJavaScript開発に必要な知識を体系的に学べます。

## 📚 教材の特徴

- **初学者向け**: 基礎から丁寧に解説
- **実践的**: 実務で使える技術に焦点
- **モダンな内容**: ES2015以降の現代的な書き方
- **豊富なコード例**: すぐに試せる実装例を多数掲載

## 🎯 対象読者

- JavaScriptの基礎を学びたい初学者
- モダンなJavaScriptの書き方を習得したい方
- 非同期処理やモジュールシステムを理解したい方
- 実務で使える実践的なテクニックを学びたい方

## 📖 教材の構成

### HTML編

#### [1. HTML 画像最適化編](./docs/html/01-image-optimization/README.md)

画像のパフォーマンス最適化技術を学びます。

- レスポンシブ画像（srcset/sizes）
- picture要素とモダンフォーマット（AVIF/WebP）
- 遅延読み込み（lazy loading）
- decoding属性
- 画像の幅と高さの指定
- Core Web Vitals対応

**学習時間の目安**: 2-3時間

#### [2. HTML セマンティック要素編](./docs/html/02-semantic-elements/README.md)

モダンなHTMLのセマンティック要素を学びます。

- details/summary（アコーディオン）
- dialog（モーダル）
- hgroup（見出しグループ）
- search（検索）
- セマンティックHTMLの重要性

**学習時間の目安**: 2-3時間

#### [3. HTML アクセシビリティ編](./docs/html/03-accessibility/README.md)

すべてのユーザーが利用できるWebサイトを作る技術を学びます。

- WAI-ARIAの基礎
- aria-label、aria-labelledby、aria-describedby
- aria-current、aria-haspopup、aria-expanded
- WCAGガイドライン
- アクセシブルなタブUI
- 実践的なチェックリスト

**学習時間の目安**: 3-4時間

#### [4. HTML パフォーマンス編](./docs/html/04-performance/README.md)

Webサイトのパフォーマンスを最大化する技術を学びます。

- rel="preload"によるリソース先読み
- Core Web Vitalsの最適化（LCP/FID/CLS）
- サイト高速化の10の手法
- パフォーマンス測定ツール
- ベストプラクティス

**学習時間の目安**: 2-3時間

### JavaScript編

#### [1. JavaScript 基礎編](./docs/javascript/01-basics/README.md)

ES2015以降のモダンな構文を学びます。

- 変数宣言（let と const）
- アロー関数
- テンプレートリテラル
- 分割代入
- スプレッド演算子
- クラス
- for...of ループ

**学習時間の目安**: 3-4時間

#### [2. 非同期処理編](./docs/javascript/02-async/README.md)

JavaScriptの非同期処理を基礎から実践まで学びます。

- 非同期処理の基本概念
- Promise の使い方
- async/await
- Fetch API
- AbortController（通信の中断）
- fetch vs axios
- 実践的なパターン

**学習時間の目安**: 4-5時間

#### [3. DOM操作とイベント処理編](./docs/javascript/03-dom-events/README.md)

DOM操作とモダンなブラウザAPIを学びます。

- スクリプトタグの読み込みタイミング（defer, async）
- DOMContentLoaded と load イベント
- Intersection Observer API
- matchMedia API
- イベント処理のベストプラクティス

**学習時間の目安**: 3-4時間

#### [4. 実践的なテクニック編](./docs/javascript/04-practical/README.md)

実務で役立つテクニックとベストプラクティスを学びます。

- モジュールシステム（type="module"）
- グローバル変数の衝突回避
- レスポンシブ対応のテクニック
- パフォーマンス最適化
- エラーハンドリング
- デバッグテクニック

**学習時間の目安**: 3-4時間

### [実践コード例](./examples/README.md)

各セクションで学んだ内容を実際に動かせるコード例集。

**HTML編：**
- レスポンシブ画像の例（srcset/sizes）
- picture要素とモダンフォーマット
- detailsアコーディオンの例
- dialogモーダルの例
- アクセシブルなフォームの例

**JavaScript編：**
- 非同期処理の例（Fetch API）
- Intersection Observer の例（フェードインアニメーション）
- matchMedia の例（レスポンシブメニュー）
- モジュールの例（タスク管理アプリ）
- デバウンス/スロットルの例

## 🚀 学習の進め方

### 推奨学習順序

**初心者向けルート：**

1. **HTML 画像最適化編** でWebパフォーマンスの基礎を学ぶ
   - レスポンシブ画像の実装
   - モダンフォーマットの活用

2. **HTML セマンティック要素編** で正しいマークアップを学ぶ
   - details/summary、dialog要素の活用
   - セマンティックHTMLの重要性

3. **JavaScript 基礎編** でモダンな構文を学ぶ
   - ES2015の基本構文をしっかり理解する
   - 特に変数宣言、アロー関数、分割代入は重要

4. **非同期処理編** でAPI通信を学ぶ
   - Promiseの仕組みを理解する
   - async/awaitで実際のAPI通信を試す

5. **DOM操作とイベント処理編** でインタラクティブな実装を学ぶ
   - Intersection Observerで実用的な機能を実装
   - matchMediaでレスポンシブ対応を理解

6. **アクセシビリティ編** ですべてのユーザーに配慮する
   - WAI-ARIAの基礎を学ぶ
   - アクセシブルなUI実装

7. **パフォーマンス編・実践的なテクニック編** で総仕上げ
   - Core Web Vitals最適化
   - モジュールシステムでコードを整理

8. **実践コード例** で手を動かす
   - 実際にコードを動かして理解を深める
   - 自分なりにカスタマイズして試す

**経験者向けルート：**

1. 関心のある分野から始める
2. 不足している知識を補う
3. 実践コード例で統合的に学ぶ

### 学習のコツ

1. **コードを写経する**
   - 例文をそのまま打ち込んで動作を確認
   - タイピングすることで理解が深まる

2. **少しずつ改変する**
   - パラメータを変えて挙動を観察
   - 新しい機能を追加してみる

3. **エラーを恐れない**
   - エラーメッセージは学習の機会
   - デバッグの練習になる

4. **実際に使ってみる**
   - 小さなプロジェクトで実践
   - ポートフォリオ作成に活用

## 💻 環境構築

### 必要なもの

1. **ブラウザ**
   - Google Chrome（推奨）
   - Firefox
   - Safari
   - Edge

2. **コードエディタ**
   - Visual Studio Code（推奨）
   - Sublime Text
   - Atom

3. **ローカルサーバー**（モジュールの例を試す場合）
   ```bash
   # Python 3の場合
   python -m http.server 8000

   # Node.jsの場合
   npx http-server

   # VSCodeのLive Server拡張機能
   ```

### 開発者ツールの使い方

ブラウザの開発者ツールを活用しましょう。

- **Windows/Linux**: `F12` または `Ctrl + Shift + I`
- **Mac**: `Cmd + Option + I`

主な機能：
- **Console**: コードの実行とログの確認
- **Elements**: HTMLとCSSの確認・編集
- **Network**: 通信の監視
- **Sources**: デバッグ

## 📝 参考資料

この教材は以下の記事を参考に作成されています：

- [令和のHTML / CSS / JavaScriptの書き方50選](https://zenn.dev/necscat/articles/bc9bba54babaf5)
- 各種技術記事（教材内にリンクあり）

### さらに学びたい方へ

- [MDN Web Docs](https://developer.mozilla.org/ja/) - 公式リファレンス
- [JavaScript.info](https://javascript.info/) - 詳細なチュートリアル
- [ES6 仕様書](https://www.ecma-international.org/ecma-262/) - 言語仕様

## 🤝 貢献

この教材は継続的に改善されています。

- 誤字脱字の報告
- 内容の改善提案
- 新しいコード例の追加

などがございましたら、Issueやプルリクエストをお願いします。

## 📜 ライセンス

MITライセンス - 詳細は [LICENSE](./LICENSE) ファイルを参照してください。

## 📞 お問い合わせ

質問や感想などがございましたら、お気軽にIssueを作成してください。

---

## 学習の目標

この教材を修了すると、以下のことができるようになります：

**HTML編：**
✅ レスポンシブ画像を適切に実装できる（srcset/sizes/picture）
✅ モダンな画像フォーマット（AVIF/WebP）を活用できる
✅ details/dialog などのセマンティック要素を使いこなせる
✅ WAI-ARIAを使ったアクセシブルなWebサイトを作成できる
✅ Core Web Vitalsを最適化できる
✅ rel="preload"などのパフォーマンス最適化技術を活用できる

**JavaScript編：**
✅ モダンなJavaScriptの構文を理解し、使いこなせる
✅ 非同期処理（Promise、async/await）を正しく扱える
✅ Fetch APIでサーバーと通信できる
✅ Intersection Observer APIでスクロール連動の機能を実装できる
✅ matchMediaでレスポンシブ対応のコードが書ける
✅ モジュールシステムでコードを整理できる
✅ パフォーマンスを意識したコードが書ける
✅ エラーハンドリングとデバッグができる

## さあ、始めましょう！

初学者の方は [HTML 画像最適化編](./docs/html/01-image-optimization/README.md) から、

JavaScriptを集中的に学びたい方は [JavaScript 基礎編](./docs/javascript/01-basics/README.md) から始めてください。

頑張ってください！ 🚀
