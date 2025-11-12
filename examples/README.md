# 実践コード例

各セクションで学んだ内容を実際に動かせるコード例です。

## 目次

### HTML編
1. [レスポンシブ画像の例](#html-1-レスポンシブ画像の例)
2. [picture要素とモダンフォーマット](#html-2-picture要素とモダンフォーマット)
3. [detailsアコーディオンの例](#html-3-detailsアコーディオンの例)
4. [dialog モーダルの例](#html-4-dialog-モーダルの例)
5. [アクセシブルなフォーム](#html-5-アクセシブルなフォーム)

### CSS編
1. [Grid Layoutレイアウト例](#css-1-grid-layoutレイアウト例)
2. [モダンプロパティ活用例](#css-2-モダンプロパティ活用例)
3. [視覚効果の例](#css-3-視覚効果の例)
4. [モダンセレクタの例](#css-4-モダンセレクタの例)
5. [カスタムフォーム要素](#css-5-カスタムフォーム要素)

### JavaScript編
1. [非同期処理の例](#js-1-非同期処理の例)
2. [Intersection Observer の例](#js-2-intersection-observer-の例)
3. [matchMedia の例](#js-3-matchmedia-の例)
4. [モジュールの例](#js-4-モジュールの例)
5. [実用的なパターン](#js-5-実用的なパターン)

---

## HTML編

### HTML-1. レスポンシブ画像の例

srcset と sizes を使ったレスポンシブ画像の実装。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>レスポンシブ画像の例</title>
  <style>
    body {
      font-family: sans-serif;
      margin: 0;
      padding: 20px;
    }

    .container {
      max-width: 1200px;
      margin: 0 auto;
    }

    img {
      max-width: 100%;
      height: auto;
      border-radius: 8px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }

    .info {
      background: #f5f5f5;
      padding: 15px;
      margin: 20px 0;
      border-radius: 4px;
    }

    code {
      background: #e0e0e0;
      padding: 2px 6px;
      border-radius: 3px;
      font-family: monospace;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>レスポンシブ画像の例</h1>

    <div class="info">
      <p>ブラウザの幅を変更してみてください。</p>
      <p>デバイスに応じて、最適なサイズの画像が自動的に選択されます。</p>
      <p>DevToolsのNetworkタブで、どの画像が読み込まれているか確認できます。</p>
    </div>

    <!-- 例1：基本的なレスポンシブ画像 -->
    <h2>例1：基本的なレスポンシブ画像</h2>
    <img src="https://picsum.photos/800/600"
         srcset="https://picsum.photos/400/300 400w,
                 https://picsum.photos/800/600 800w,
                 https://picsum.photos/1200/900 1200w"
         sizes="(max-width: 640px) 100vw,
                (max-width: 1024px) 50vw,
                800px"
         width="800"
         height="600"
         alt="サンプル画像"
         loading="lazy">

    <div class="info">
      <p><strong>使用している技術：</strong></p>
      <ul>
        <li><code>srcset</code>: 複数サイズの画像を提供</li>
        <li><code>sizes</code>: 表示幅を指定</li>
        <li><code>width/height</code>: レイアウトシフト防止</li>
        <li><code>loading="lazy"</code>: 遅延読み込み</li>
      </ul>
    </div>

    <!-- 例2：高解像度ディスプレイ対応 -->
    <h2>例2：高解像度ディスプレイ対応</h2>
    <img src="https://picsum.photos/400/300"
         srcset="https://picsum.photos/400/300 1x,
                 https://picsum.photos/800/600 2x,
                 https://picsum.photos/1200/900 3x"
         width="400"
         height="300"
         alt="Retina対応画像"
         loading="lazy">

    <div class="info">
      <p><strong>Retina対応：</strong></p>
      <p>1x (標準)、2x (Retina)、3x (超高解像度) の3種類を用意</p>
    </div>
  </div>
</body>
</html>
```

### HTML-2. picture要素とモダンフォーマット

AVIF/WebPなどのモダンフォーマットに対応した画像の提供。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>picture要素の例</title>
  <style>
    body {
      font-family: sans-serif;
      max-width: 1000px;
      margin: 0 auto;
      padding: 20px;
    }

    img {
      max-width: 100%;
      height: auto;
      border-radius: 8px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }

    .example {
      margin: 40px 0;
      padding: 20px;
      background: #f9f9f9;
      border-radius: 8px;
    }

    .note {
      background: #e3f2fd;
      padding: 15px;
      margin: 15px 0;
      border-left: 4px solid #2196f3;
      border-radius: 4px;
    }
  </style>
</head>
<body>
  <h1>picture要素の例</h1>

  <div class="note">
    <p><strong>注意：</strong> この例では実際のAVIF/WebP画像の代わりにJPEG画像を使用しています。</p>
    <p>実際の実装では、それぞれのフォーマットで画像を用意してください。</p>
  </div>

  <!-- 例1：モダンフォーマット対応 -->
  <div class="example">
    <h2>例1：モダンフォーマット対応</h2>
    <picture>
      <!-- AVIF: 最新・最高圧縮率 -->
      <source srcset="https://picsum.photos/800/600" type="image/avif">

      <!-- WebP: 広くサポートされた高圧縮フォーマット -->
      <source srcset="https://picsum.photos/800/600" type="image/webp">

      <!-- JPEG: フォールバック -->
      <img src="https://picsum.photos/800/600"
           width="800"
           height="600"
           alt="モダンフォーマット対応画像">
    </picture>

    <p><strong>対応フォーマット：</strong></p>
    <ul>
      <li>AVIF（Chrome 85+、Firefox 93+、Safari 16+）</li>
      <li>WebP（Chrome、Firefox、Edge、Safari 14+）</li>
      <li>JPEG（全ブラウザ対応）</li>
    </ul>
  </div>

  <!-- 例2：レスポンシブ + モダンフォーマット -->
  <div class="example">
    <h2>例2：アートディレクション</h2>
    <p>画面サイズに応じて異なる画像を表示</p>

    <picture>
      <!-- モバイル: 正方形 -->
      <source media="(max-width: 640px)"
              srcset="https://picsum.photos/400/400">

      <!-- タブレット: 4:3 -->
      <source media="(max-width: 1024px)"
              srcset="https://picsum.photos/800/600">

      <!-- デスクトップ: 16:9 -->
      <img src="https://picsum.photos/1200/675"
           width="1200"
           height="675"
           alt="レスポンシブ画像">
    </picture>

    <p>ブラウザの幅を変更すると、異なるアスペクト比の画像に切り替わります。</p>
  </div>
</body>
</html>
```

### HTML-3. detailsアコーディオンの例

JavaScriptなしで動作するアコーディオンUI。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Detailsアコーディオンの例</title>
  <style>
    body {
      font-family: sans-serif;
      max-width: 800px;
      margin: 0 auto;
      padding: 20px;
      background: #f5f5f5;
    }

    h1 {
      text-align: center;
      color: #333;
    }

    .faq {
      background: white;
      border-radius: 8px;
      padding: 20px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }

    details {
      border-bottom: 1px solid #e0e0e0;
      padding: 15px 0;
    }

    details:last-child {
      border-bottom: none;
    }

    summary {
      font-weight: bold;
      font-size: 18px;
      cursor: pointer;
      padding: 10px;
      list-style: none;
      position: relative;
      padding-left: 35px;
      transition: color 0.3s;
    }

    summary:hover {
      color: #2196f3;
    }

    summary::before {
      content: "▶";
      position: absolute;
      left: 10px;
      transition: transform 0.3s;
      color: #2196f3;
    }

    details[open] summary::before {
      transform: rotate(90deg);
    }

    details[open] summary {
      color: #2196f3;
      margin-bottom: 10px;
    }

    .answer {
      padding: 10px 10px 10px 35px;
      color: #666;
      line-height: 1.6;
    }

    .note {
      background: #e3f2fd;
      padding: 15px;
      margin: 20px 0;
      border-radius: 4px;
      border-left: 4px solid #2196f3;
    }
  </style>
</head>
<body>
  <h1>よくある質問（FAQ）</h1>

  <div class="note">
    <p><strong>特徴：</strong></p>
    <ul>
      <li>✅ JavaScriptなしで動作</li>
      <li>✅ キーボード操作対応</li>
      <li>✅ スクリーンリーダー対応</li>
      <li>✅ ブラウザ内検索対応</li>
    </ul>
  </div>

  <div class="faq">
    <!-- name属性で排他的に動作 -->
    <details name="faq">
      <summary>配送料はいくらですか？</summary>
      <div class="answer">
        <p>全国一律500円です。5,000円以上のご購入で送料無料になります。</p>
      </div>
    </details>

    <details name="faq">
      <summary>返品・交換はできますか？</summary>
      <div class="answer">
        <p>商品到着後7日以内であれば、未開封品に限り返品・交換を承ります。</p>
        <p>お客様都合による返品の場合、返送料はお客様負担となります。</p>
      </div>
    </details>

    <details name="faq">
      <summary>支払い方法は何がありますか？</summary>
      <div class="answer">
        <p>以下のお支払い方法に対応しています：</p>
        <ul>
          <li>クレジットカード（VISA、MasterCard、JCB、AMEX）</li>
          <li>銀行振込</li>
          <li>代金引換</li>
          <li>コンビニ決済</li>
        </ul>
      </div>
    </details>

    <details name="faq">
      <summary>会員登録は必要ですか？</summary>
      <div class="answer">
        <p>会員登録なしでもご購入いただけます。</p>
        <p>ただし、会員登録いただくと以下の特典があります：</p>
        <ul>
          <li>購入履歴の確認</li>
          <li>お気に入り機能</li>
          <li>ポイントの獲得・使用</li>
          <li>お届け先の保存</li>
        </ul>
      </div>
    </details>

    <details name="faq">
      <summary>商品の在庫状況を教えてください</summary>
      <div class="answer">
        <p>各商品ページに在庫状況が表示されています。</p>
        <p>「在庫あり」の商品は通常1〜3営業日で発送いたします。</p>
      </div>
    </details>
  </div>
</body>
</html>
```

### HTML-4. dialog モーダルの例

標準のdialog要素を使ったモーダルウィンドウ。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dialogモーダルの例</title>
  <style>
    body {
      font-family: sans-serif;
      max-width: 800px;
      margin: 0 auto;
      padding: 40px 20px;
      background: #f5f5f5;
    }

    h1 {
      text-align: center;
    }

    .buttons {
      display: flex;
      gap: 15px;
      justify-content: center;
      margin: 30px 0;
    }

    button {
      padding: 12px 24px;
      font-size: 16px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      transition: transform 0.2s, box-shadow 0.2s;
    }

    button:hover {
      transform: translateY(-2px);
      box-shadow: 0 4px 12px rgba(0,0,0,0.15);
    }

    .btn-primary {
      background: #2196f3;
      color: white;
    }

    .btn-danger {
      background: #f44336;
      color: white;
    }

    .btn-success {
      background: #4caf50;
      color: white;
    }

    /* Dialog のスタイル */
    dialog {
      border: none;
      border-radius: 12px;
      padding: 0;
      max-width: 500px;
      box-shadow: 0 8px 32px rgba(0,0,0,0.3);
      opacity: 0;
      transform: translateY(-50px);
      transition: opacity 0.3s, transform 0.3s;
    }

    dialog[open] {
      opacity: 1;
      transform: translateY(0);
    }

    dialog::backdrop {
      background: rgba(0, 0, 0, 0.5);
      backdrop-filter: blur(4px);
    }

    .dialog-header {
      padding: 20px;
      background: #2196f3;
      color: white;
      border-radius: 12px 12px 0 0;
    }

    .dialog-header h2 {
      margin: 0;
      font-size: 20px;
    }

    .dialog-content {
      padding: 20px;
    }

    .dialog-footer {
      padding: 20px;
      border-top: 1px solid #e0e0e0;
      display: flex;
      gap: 10px;
      justify-content: flex-end;
    }

    .dialog-footer button {
      padding: 8px 16px;
    }

    .btn-secondary {
      background: #999;
      color: white;
    }

    .features {
      background: white;
      padding: 20px;
      border-radius: 8px;
      margin-top: 30px;
    }

    .features ul {
      list-style: none;
      padding: 0;
    }

    .features li {
      padding: 8px 0;
      padding-left: 25px;
      position: relative;
    }

    .features li::before {
      content: "✓";
      position: absolute;
      left: 0;
      color: #4caf50;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <h1>Dialog要素のモーダル例</h1>

  <div class="buttons">
    <button class="btn-primary" id="openInfo">情報ダイアログ</button>
    <button class="btn-danger" id="openConfirm">確認ダイアログ</button>
    <button class="btn-success" id="openForm">フォームダイアログ</button>
  </div>

  <!-- 情報ダイアログ -->
  <dialog id="infoDialog">
    <div class="dialog-header">
      <h2>お知らせ</h2>
    </div>
    <div class="dialog-content">
      <p>これは情報を表示するダイアログです。</p>
      <p>Escキーまたは背景をクリックで閉じることができます。</p>
    </div>
    <div class="dialog-footer">
      <button class="btn-primary" id="closeInfo">閉じる</button>
    </div>
  </dialog>

  <!-- 確認ダイアログ -->
  <dialog id="confirmDialog">
    <div class="dialog-header" style="background: #f44336;">
      <h2>確認</h2>
    </div>
    <div class="dialog-content">
      <p>本当に削除しますか？</p>
      <p>この操作は取り消せません。</p>
    </div>
    <div class="dialog-footer">
      <button class="btn-secondary" id="cancelConfirm">キャンセル</button>
      <button class="btn-danger" id="confirmDelete">削除する</button>
    </div>
  </dialog>

  <!-- フォームダイアログ -->
  <dialog id="formDialog">
    <form method="dialog">
      <div class="dialog-header" style="background: #4caf50;">
        <h2>お問い合わせ</h2>
      </div>
      <div class="dialog-content">
        <label for="name" style="display: block; margin-bottom: 5px;">お名前</label>
        <input type="text" id="name" name="name" required
               style="width: 100%; padding: 8px; margin-bottom: 15px; border: 1px solid #ddd; border-radius: 4px;">

        <label for="email" style="display: block; margin-bottom: 5px;">メールアドレス</label>
        <input type="email" id="email" name="email" required
               style="width: 100%; padding: 8px; margin-bottom: 15px; border: 1px solid #ddd; border-radius: 4px;">

        <label for="message" style="display: block; margin-bottom: 5px;">メッセージ</label>
        <textarea id="message" name="message" rows="4" required
                  style="width: 100%; padding: 8px; border: 1px solid #ddd; border-radius: 4px; resize: vertical;"></textarea>
      </div>
      <div class="dialog-footer">
        <button type="button" class="btn-secondary" id="cancelForm">キャンセル</button>
        <button type="submit" class="btn-success">送信</button>
      </div>
    </form>
  </dialog>

  <div class="features">
    <h2>標準機能</h2>
    <ul>
      <li>最上位レイヤーに表示</li>
      <li>背面コンテンツの不活性化（inert）</li>
      <li>Escキーで自動クローズ</li>
      <li>フォーカストラップ</li>
      <li>アクセシビリティ対応</li>
    </ul>
  </div>

  <script>
    // 情報ダイアログ
    const infoDialog = document.getElementById('infoDialog');
    document.getElementById('openInfo').addEventListener('click', () => {
      infoDialog.showModal();
    });
    document.getElementById('closeInfo').addEventListener('click', () => {
      infoDialog.close();
    });

    // 確認ダイアログ
    const confirmDialog = document.getElementById('confirmDialog');
    document.getElementById('openConfirm').addEventListener('click', () => {
      confirmDialog.showModal();
    });
    document.getElementById('cancelConfirm').addEventListener('click', () => {
      confirmDialog.close();
    });
    document.getElementById('confirmDelete').addEventListener('click', () => {
      alert('削除しました');
      confirmDialog.close();
    });

    // フォームダイアログ
    const formDialog = document.getElementById('formDialog');
    document.getElementById('openForm').addEventListener('click', () => {
      formDialog.showModal();
    });
    document.getElementById('cancelForm').addEventListener('click', () => {
      formDialog.close();
    });

    formDialog.addEventListener('close', () => {
      if (formDialog.returnValue === 'default') {
        alert('送信しました！');
      }
    });

    // 背景クリックで閉じる
    [infoDialog, confirmDialog, formDialog].forEach(dialog => {
      dialog.addEventListener('click', (e) => {
        if (e.target === dialog) {
          dialog.close();
        }
      });
    });

    // 背面スクロール抑制
    const style = document.createElement('style');
    style.textContent = 'body:has(dialog[open]) { overflow: hidden; }';
    document.head.appendChild(style);
  </script>
</body>
</html>
```

### HTML-5. アクセシブルなフォーム

WAI-ARIAを使ったアクセシブルなフォームの実装。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>アクセシブルなフォーム</title>
  <style>
    body {
      font-family: sans-serif;
      max-width: 600px;
      margin: 0 auto;
      padding: 40px 20px;
      background: #f5f5f5;
    }

    form {
      background: white;
      padding: 30px;
      border-radius: 8px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }

    h1 {
      text-align: center;
      color: #333;
    }

    .form-group {
      margin-bottom: 20px;
    }

    label {
      display: block;
      margin-bottom: 5px;
      font-weight: bold;
      color: #333;
    }

    input[type="text"],
    input[type="email"],
    input[type="password"],
    select,
    textarea {
      width: 100%;
      padding: 10px;
      border: 2px solid #ddd;
      border-radius: 4px;
      font-size: 16px;
      transition: border-color 0.3s;
      box-sizing: border-box;
    }

    input:focus,
    select:focus,
    textarea:focus {
      outline: none;
      border-color: #2196f3;
    }

    .required {
      color: #f44336;
    }

    .hint {
      font-size: 14px;
      color: #666;
      margin-top: 5px;
    }

    .error {
      color: #f44336;
      font-size: 14px;
      margin-top: 5px;
      display: none;
    }

    input:invalid + .error {
      display: block;
    }

    .checkbox-group,
    .radio-group {
      margin-top: 10px;
    }

    .checkbox-group label,
    .radio-group label {
      display: inline;
      font-weight: normal;
      margin-left: 5px;
    }

    .checkbox-group input,
    .radio-group input {
      width: auto;
    }

    button {
      width: 100%;
      padding: 12px;
      background: #2196f3;
      color: white;
      border: none;
      border-radius: 4px;
      font-size: 16px;
      font-weight: bold;
      cursor: pointer;
      transition: background 0.3s;
    }

    button:hover {
      background: #1976d2;
    }

    button:focus {
      outline: 2px solid #2196f3;
      outline-offset: 2px;
    }

    .current-field {
      background: #e3f2fd;
      padding: 15px;
      margin-bottom: 20px;
      border-radius: 4px;
      border-left: 4px solid #2196f3;
    }
  </style>
</head>
<body>
  <h1>ユーザー登録フォーム</h1>

  <div class="current-field" aria-live="polite" id="currentField">
    <strong>現在のフィールド:</strong> <span id="fieldName">フォーム開始</span>
  </div>

  <form aria-label="ユーザー登録フォーム">
    <!-- お名前 -->
    <div class="form-group">
      <label for="name">
        お名前 <span class="required" aria-label="必須">*</span>
      </label>
      <input type="text"
             id="name"
             name="name"
             required
             aria-required="true"
             aria-describedby="name-hint name-error">
      <div id="name-hint" class="hint">フルネームで入力してください</div>
      <div id="name-error" class="error" role="alert">お名前を入力してください</div>
    </div>

    <!-- メールアドレス -->
    <div class="form-group">
      <label for="email">
        メールアドレス <span class="required" aria-label="必須">*</span>
      </label>
      <input type="email"
             id="email"
             name="email"
             required
             aria-required="true"
             aria-describedby="email-hint email-error">
      <div id="email-hint" class="hint">example@domain.com</div>
      <div id="email-error" class="error" role="alert">有効なメールアドレスを入力してください</div>
    </div>

    <!-- パスワード -->
    <div class="form-group">
      <label for="password">
        パスワード <span class="required" aria-label="必須">*</span>
      </label>
      <input type="password"
             id="password"
             name="password"
             required
             minlength="8"
             aria-required="true"
             aria-describedby="password-hint password-error">
      <div id="password-hint" class="hint">8文字以上、英数字と記号を含めてください</div>
      <div id="password-error" class="error" role="alert">8文字以上で入力してください</div>
    </div>

    <!-- 性別 -->
    <div class="form-group">
      <fieldset>
        <legend>性別</legend>
        <div class="radio-group">
          <input type="radio" id="male" name="gender" value="male">
          <label for="male">男性</label>
        </div>
        <div class="radio-group">
          <input type="radio" id="female" name="gender" value="female">
          <label for="female">女性</label>
        </div>
        <div class="radio-group">
          <input type="radio" id="other" name="gender" value="other">
          <label for="other">その他</label>
        </div>
      </fieldset>
    </div>

    <!-- 興味のある分野 -->
    <div class="form-group">
      <fieldset>
        <legend>興味のある分野（複数選択可）</legend>
        <div class="checkbox-group">
          <input type="checkbox" id="web" name="interests" value="web">
          <label for="web">Web開発</label>
        </div>
        <div class="checkbox-group">
          <input type="checkbox" id="mobile" name="interests" value="mobile">
          <label for="mobile">モバイル開発</label>
        </div>
        <div class="checkbox-group">
          <input type="checkbox" id="design" name="interests" value="design">
          <label for="design">デザイン</label>
        </div>
      </fieldset>
    </div>

    <!-- 自己紹介 -->
    <div class="form-group">
      <label for="bio">自己紹介</label>
      <textarea id="bio"
                name="bio"
                rows="4"
                aria-describedby="bio-hint"></textarea>
      <div id="bio-hint" class="hint">簡単な自己紹介をお願いします</div>
    </div>

    <!-- 利用規約 -->
    <div class="form-group">
      <div class="checkbox-group">
        <input type="checkbox"
               id="terms"
               name="terms"
               required
               aria-required="true"
               aria-describedby="terms-error">
        <label for="terms">
          <a href="#" target="_blank">利用規約</a>に同意する
          <span class="required" aria-label="必須">*</span>
        </label>
      </div>
      <div id="terms-error" class="error" role="alert">利用規約に同意してください</div>
    </div>

    <!-- 送信ボタン -->
    <button type="submit">登録する</button>
  </form>

  <script>
    // フォーカス中のフィールド名を表示
    const form = document.querySelector('form');
    const fieldName = document.getElementById('fieldName');

    form.addEventListener('focusin', (e) => {
      const target = e.target;
      if (target.tagName === 'INPUT' || target.tagName === 'TEXTAREA' || target.tagName === 'SELECT') {
        const label = document.querySelector(`label[for="${target.id}"]`);
        if (label) {
          fieldName.textContent = label.textContent.trim();
        }
      }
    });

    // フォーム送信
    form.addEventListener('submit', (e) => {
      e.preventDefault();
      alert('登録が完了しました！');
    });

    // リアルタイムバリデーション
    form.querySelectorAll('input[required], textarea[required]').forEach(input => {
      input.addEventListener('blur', () => {
        if (!input.validity.valid) {
          input.setAttribute('aria-invalid', 'true');
        } else {
          input.setAttribute('aria-invalid', 'false');
        }
      });
    });
  </script>
</body>
</html>
```

---

## CSS編

### CSS-1. Grid Layoutレイアウト例

CSS Grid Layoutを使った実用的なレイアウトパターン。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Grid Layout 実践例</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: sans-serif;
      line-height: 1.6;
      color: #333;
    }

    /* ページ全体のレイアウト */
    .layout {
      display: grid;
      grid-template-columns: 250px 1fr 200px;
      grid-template-rows: auto 1fr auto;
      grid-template-areas:
        "header header  header"
        "sidebar main   aside"
        "footer footer  footer";
      min-height: 100vh;
      gap: 20px;
      padding: 20px;
    }

    .header {
      grid-area: header;
      background: #2196f3;
      color: white;
      padding: 20px;
      border-radius: 8px;
    }

    .sidebar {
      grid-area: sidebar;
      background: #f5f5f5;
      padding: 20px;
      border-radius: 8px;
    }

    .main {
      grid-area: main;
      background: white;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }

    .aside {
      grid-area: aside;
      background: #e3f2fd;
      padding: 20px;
      border-radius: 8px;
    }

    .footer {
      grid-area: footer;
      background: #333;
      color: white;
      padding: 20px;
      text-align: center;
      border-radius: 8px;
    }

    /* カードグリッド */
    .cards {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 20px;
      margin: 20px 0;
    }

    .card {
      background: white;
      border: 1px solid #ddd;
      border-radius: 8px;
      padding: 20px;
      display: grid;
      grid-template-rows: auto 1fr auto;
      gap: 12px;
      transition: transform 0.2s, box-shadow 0.2s;
    }

    .card:hover {
      transform: translateY(-5px);
      box-shadow: 0 10px 20px rgba(0,0,0,0.1);
    }

    .card h3 {
      color: #2196f3;
    }

    .card button {
      justify-self: end;
      padding: 8px 16px;
      background: #2196f3;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }

    /* レスポンシブ対応 */
    @media (width < 1024px) {
      .layout {
        grid-template-columns: 1fr;
        grid-template-areas:
          "header"
          "main"
          "sidebar"
          "aside"
          "footer";
      }
    }

    /* Grid中央揃え */
    .center-demo {
      display: grid;
      place-content: center;
      min-height: 200px;
      background: linear-gradient(135deg, #667eea, #764ba2);
      color: white;
      border-radius: 8px;
      margin: 20px 0;
    }
  </style>
</head>
<body>
  <div class="layout">
    <header class="header">
      <h1>Grid Layout デモ</h1>
      <p>モダンなレイアウトシステム</p>
    </header>

    <nav class="sidebar">
      <h2>ナビゲーション</h2>
      <ul>
        <li><a href="#home">ホーム</a></li>
        <li><a href="#about">About</a></li>
        <li><a href="#services">サービス</a></li>
        <li><a href="#contact">お問い合わせ</a></li>
      </ul>
    </nav>

    <main class="main">
      <h2>メインコンテンツ</h2>
      <p>Grid Layoutを使った実用的なレイアウト例です。</p>

      <div class="center-demo">
        <div>
          <h3>Grid中央揃え</h3>
          <p>place-content: center で簡単に中央揃え</p>
        </div>
      </div>

      <h3>カードグリッド（auto-fit）</h3>
      <div class="cards">
        <div class="card">
          <h3>カード 1</h3>
          <p>レスポンシブに自動調整されるカードレイアウト。画面幅に応じて列数が変わります。</p>
          <button>詳細</button>
        </div>
        <div class="card">
          <h3>カード 2</h3>
          <p>minmax(250px, 1fr)により、最小幅を保ちながら柔軟に配置されます。</p>
          <button>詳細</button>
        </div>
        <div class="card">
          <h3>カード 3</h3>
          <p>JavaScriptなしで、CSSのみで実装できます。</p>
          <button>詳細</button>
        </div>
      </div>
    </main>

    <aside class="aside">
      <h2>サイドバー</h2>
      <p>追加情報をここに表示</p>
      <ul>
        <li>お知らせ 1</li>
        <li>お知らせ 2</li>
        <li>お知らせ 3</li>
      </ul>
    </aside>

    <footer class="footer">
      <p>&copy; 2024 Grid Layout Demo</p>
    </footer>
  </div>
</body>
</html>
```

### CSS-2. モダンプロパティ活用例

ビューポート単位、clamp関数、論理プロパティの活用例。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>モダンプロパティ活用例</title>
  <style>
    :root {
      /* clamp関数でレスポンシブなフォントサイズ */
      --font-size-sm: clamp(0.875rem, 0.8rem + 0.375vw, 1rem);
      --font-size-base: clamp(1rem, 0.9rem + 0.5vw, 1.25rem);
      --font-size-lg: clamp(1.25rem, 1.1rem + 0.75vw, 1.75rem);
      --font-size-xl: clamp(1.5rem, 1.2rem + 1.5vw, 2.5rem);

      /* スペーシング */
      --space-sm: clamp(0.75rem, 0.6rem + 0.75vw, 1.25rem);
      --space-md: clamp(1rem, 0.8rem + 1vw, 1.75rem);
      --space-lg: clamp(1.5rem, 1.2rem + 1.5vw, 2.5rem);
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: sans-serif;
      font-size: var(--font-size-base);
      /* テキストの折り返し制御 */
      overflow-wrap: anywhere;
      word-break: normal;
      line-break: strict;
    }

    /* ヒーローセクション（svh使用） */
    .hero {
      height: 100svh;  /* Small Viewport Height */
      display: grid;
      place-content: center;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white;
      text-align: center;
      padding-inline: var(--space-md);  /* 論理プロパティ */
    }

    .hero h1 {
      font-size: var(--font-size-xl);
      margin-block-end: var(--space-md);  /* 論理プロパティ */
    }

    .hero p {
      font-size: var(--font-size-lg);
    }

    /* コンテナ（論理プロパティ使用） */
    .container {
      max-inline-size: 1200px;  /* max-width */
      margin-inline: auto;       /* margin-left/right: auto */
      padding-inline: var(--space-md);
      padding-block: var(--space-lg);
    }

    .section {
      margin-block-end: var(--space-lg);
    }

    .section h2 {
      font-size: var(--font-size-lg);
      margin-block-end: var(--space-sm);
      color: #2196f3;
    }

    /* currentColorの活用 */
    .icon-button {
      color: #2196f3;
      padding: var(--space-sm);
      border: 2px solid currentColor;  /* 自動的に#2196f3になる */
      border-radius: 8px;
      background: transparent;
      cursor: pointer;
      transition: all 0.3s;
      display: inline-flex;
      align-items: center;
      gap: 8px;
    }

    .icon-button:hover {
      color: #1976d2;
      /* borderとSVGの色も自動的に変わる */
      background: currentColor;
      color: white;
    }

    .icon-button svg {
      fill: currentColor;
    }

    /* カード */
    .card {
      background: white;
      padding-block: var(--space-md);
      padding-inline: var(--space-md);
      border-radius: 12px;
      border-inline-start: 4px solid #2196f3;  /* 論理プロパティ */
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
      margin-block-end: var(--space-md);
    }

    .info-box {
      background: #e3f2fd;
      padding: var(--space-md);
      border-radius: 8px;
      margin-block: var(--space-md);
    }

    .info-box code {
      background: #fff;
      padding: 2px 6px;
      border-radius: 4px;
      color: #2196f3;
      font-family: monospace;
    }
  </style>
</head>
<body>
  <section class="hero">
    <div>
      <h1>モダンCSS プロパティ</h1>
      <p>ビューポート単位・clamp・論理プロパティ</p>
    </div>
  </section>

  <div class="container">
    <section class="section">
      <h2>ビューポート単位（svh）</h2>
      <div class="card">
        <p>上のヒーローセクションは <code>height: 100svh;</code> を使用しています。</p>
        <p>モバイルブラウザでアドレスバーが表示されている状態でも、画面いっぱいに表示されます。</p>
      </div>
    </section>

    <section class="section">
      <h2>clamp関数</h2>
      <div class="card">
        <p>このページのフォントサイズとスペーシングは、すべて <code>clamp()</code> 関数を使用しています。</p>
        <p>ブラウザの幅を変更すると、滑らかにサイズが変化します。メディアクエリは不要です。</p>
        <div class="info-box">
          <p><strong>例：</strong></p>
          <p><code>font-size: clamp(1rem, 0.9rem + 0.5vw, 1.25rem);</code></p>
          <p>最小1rem、最大1.25remの範囲で、ビューポート幅に応じて変化します。</p>
        </div>
      </div>
    </section>

    <section class="section">
      <h2>論理プロパティ</h2>
      <div class="card">
        <p><code>margin-inline</code>、<code>padding-block</code>などの論理プロパティを使用しています。</p>
        <p>テキストの流れる方向に基づいた指定なので、国際化対応が容易です。</p>
        <div class="info-box">
          <p><strong>物理プロパティ vs 論理プロパティ：</strong></p>
          <ul>
            <li><code>margin-left/right</code> → <code>margin-inline</code></li>
            <li><code>margin-top/bottom</code> → <code>margin-block</code></li>
            <li><code>width</code> → <code>inline-size</code></li>
          </ul>
        </div>
      </div>
    </section>

    <section class="section">
      <h2>currentColorの活用</h2>
      <button class="icon-button">
        <svg width="20" height="20" viewBox="0 0 20 20">
          <path d="M10 0L12.245 7.755L20 10L12.245 12.245L10 20L7.755 12.245L0 10L7.755 7.755L10 0Z"/>
        </svg>
        ホバーしてみてください
      </button>
      <div class="info-box" style="margin-top: 1rem;">
        <p>ボタンの <code>border</code> と SVGの <code>fill</code> に <code>currentColor</code> を使用。</p>
        <p><code>color</code> プロパティを変更するだけで、すべての色が連動します。</p>
      </div>
    </section>
  </div>
</body>
</html>
```

### CSS-3. 視覚効果の例

transform独立プロパティ、filter、mix-blend-modeの活用例。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CSS視覚効果の例</title>
  <style>
    body {
      font-family: sans-serif;
      padding: 40px 20px;
      max-width: 1200px;
      margin: 0 auto;
      background: #f5f5f5;
    }

    h1 {
      text-align: center;
      color: #333;
      margin-bottom: 40px;
    }

    .demo-section {
      background: white;
      padding: 30px;
      margin: 30px 0;
      border-radius: 12px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }

    .demo-section h2 {
      color: #2196f3;
      margin-bottom: 20px;
      border-bottom: 2px solid #2196f3;
      padding-bottom: 10px;
    }

    /* 1. transform独立プロパティ */
    .transform-demo {
      display: flex;
      gap: 20px;
      flex-wrap: wrap;
    }

    .transform-card {
      width: 150px;
      height: 150px;
      background: linear-gradient(135deg, #667eea, #764ba2);
      border-radius: 12px;
      display: grid;
      place-content: center;
      color: white;
      font-weight: bold;
      cursor: pointer;

      /* 独立プロパティを使用 */
      translate: 0 0;
      rotate: 0deg;
      scale: 1;

      /* 個別にtransition設定 */
      transition:
        translate 0.3s ease,
        rotate 0.3s cubic-bezier(0.68, -0.55, 0.265, 1.55),
        scale 0.2s ease-out;
    }

    .transform-card:hover {
      translate: 0 -10px;
      rotate: 5deg;
      scale: 1.1;
      box-shadow: 0 20px 40px rgba(0,0,0,0.3);
    }

    /* 2. filterプロパティ */
    .filter-demo {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 20px;
    }

    .filter-card {
      position: relative;
      overflow: hidden;
      border-radius: 12px;
      cursor: pointer;
    }

    .filter-card img {
      width: 100%;
      height: 200px;
      object-fit: cover;
      transition: filter 0.3s ease;
    }

    .filter-card:nth-child(1) img {
      filter: grayscale(1);
    }

    .filter-card:nth-child(1):hover img {
      filter: grayscale(0);
    }

    .filter-card:nth-child(2) img {
      filter: brightness(1);
    }

    .filter-card:nth-child(2):hover img {
      filter: brightness(1.3) contrast(1.2);
    }

    .filter-card:nth-child(3) img {
      filter: sepia(0);
    }

    .filter-card:nth-child(3):hover img {
      filter: sepia(0.8);
    }

    .filter-label {
      position: absolute;
      bottom: 0;
      left: 0;
      right: 0;
      background: rgba(0,0,0,0.7);
      color: white;
      padding: 10px;
      text-align: center;
      font-size: 14px;
    }

    /* 3. mix-blend-mode */
    .blend-demo {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 20px;
    }

    .blend-card {
      position: relative;
      height: 200px;
      background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
      border-radius: 12px;
      display: grid;
      place-content: center;
      overflow: hidden;
    }

    .blend-text {
      font-size: 48px;
      font-weight: bold;
      color: white;
    }

    .blend-card:nth-child(1) .blend-text {
      mix-blend-mode: overlay;
    }

    .blend-card:nth-child(2) .blend-text {
      mix-blend-mode: multiply;
    }

    .blend-card:nth-child(3) .blend-text {
      mix-blend-mode: screen;
    }

    /* 4. clip-path */
    .clippath-demo {
      display: flex;
      gap: 20px;
      flex-wrap: wrap;
    }

    .clippath-shape {
      width: 150px;
      height: 150px;
      background: linear-gradient(135deg, #667eea, #764ba2);
      transition: clip-path 0.3s ease;
    }

    .circle {
      clip-path: circle(50%);
    }

    .circle:hover {
      clip-path: circle(70%);
    }

    .triangle {
      clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
    }

    .triangle:hover {
      clip-path: polygon(50% 20%, 20% 80%, 80% 80%);
    }

    .hexagon {
      clip-path: polygon(25% 0%, 75% 0%, 100% 50%, 75% 100%, 25% 100%, 0% 50%);
    }

    .hexagon:hover {
      clip-path: polygon(30% 10%, 70% 10%, 90% 50%, 70% 90%, 30% 90%, 10% 50%);
    }

    .star {
      clip-path: polygon(
        50% 0%, 61% 35%, 98% 35%, 68% 57%,
        79% 91%, 50% 70%, 21% 91%, 32% 57%,
        2% 35%, 39% 35%
      );
    }
  </style>
</head>
<body>
  <h1>CSS 視覚効果デモ</h1>

  <!-- Transform独立プロパティ -->
  <div class="demo-section">
    <h2>1. Transform独立プロパティ</h2>
    <p>translate、rotate、scaleを個別に制御。ホバーしてみてください。</p>
    <div class="transform-demo">
      <div class="transform-card">カード 1</div>
      <div class="transform-card">カード 2</div>
      <div class="transform-card">カード 3</div>
      <div class="transform-card">カード 4</div>
    </div>
  </div>

  <!-- Filter -->
  <div class="demo-section">
    <h2>2. Filterプロパティ</h2>
    <p>ホバーでフィルター効果が変化します。</p>
    <div class="filter-demo">
      <div class="filter-card">
        <img src="https://picsum.photos/400/300?random=1" alt="画像1">
        <div class="filter-label">Grayscale → Color</div>
      </div>
      <div class="filter-card">
        <img src="https://picsum.photos/400/300?random=2" alt="画像2">
        <div class="filter-label">Brightness + Contrast</div>
      </div>
      <div class="filter-card">
        <img src="https://picsum.photos/400/300?random=3" alt="画像3">
        <div class="filter-label">Sepia</div>
      </div>
    </div>
  </div>

  <!-- Mix Blend Mode -->
  <div class="demo-section">
    <h2>3. Mix Blend Mode</h2>
    <p>異なるブレンドモードの比較。</p>
    <div class="blend-demo">
      <div class="blend-card">
        <div class="blend-text">OVERLAY</div>
      </div>
      <div class="blend-card">
        <div class="blend-text">MULTIPLY</div>
      </div>
      <div class="blend-card">
        <div class="blend-text">SCREEN</div>
      </div>
    </div>
  </div>

  <!-- Clip Path -->
  <div class="demo-section">
    <h2>4. Clip Path</h2>
    <p>様々な形状にクリッピング。ホバーで変形します。</p>
    <div class="clippath-demo">
      <div class="clippath-shape circle"></div>
      <div class="clippath-shape triangle"></div>
      <div class="clippath-shape hexagon"></div>
      <div class="clippath-shape star"></div>
    </div>
  </div>
</body>
</html>
```

### CSS-4. モダンセレクタの例

:has()、:is()、:where()の実践例。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>モダンセレクタの例</title>
  <style>
    body {
      font-family: sans-serif;
      max-width: 1000px;
      margin: 0 auto;
      padding: 20px;
      background: #f5f5f5;
    }

    h1 {
      text-align: center;
      color: #333;
    }

    .section {
      background: white;
      padding: 30px;
      margin: 30px 0;
      border-radius: 12px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }

    .section h2 {
      color: #2196f3;
      margin-bottom: 20px;
    }

    /* 1. :has()セレクタ */

    /* 画像を含むカードのみスタイル適用 */
    .card:has(img) {
      display: flex;
      gap: 20px;
      align-items: center;
    }

    /* 画像を含まないカードは別スタイル */
    .card:not(:has(img)) {
      display: block;
      text-align: center;
    }

    .card {
      padding: 20px;
      margin: 15px 0;
      border: 2px solid #e0e0e0;
      border-radius: 8px;
      transition: all 0.3s;
    }

    .card img {
      width: 100px;
      height: 100px;
      object-fit: cover;
      border-radius: 8px;
    }

    /* フォーカスを含むフォームにスタイル適用 */
    form:has(input:focus) {
      box-shadow: 0 0 0 3px rgba(33, 150, 243, 0.3);
    }

    form {
      padding: 20px;
      border: 2px solid #e0e0e0;
      border-radius: 8px;
      transition: box-shadow 0.3s;
    }

    /* チェックされたチェックボックスを含むラベル */
    label:has(input[type="checkbox"]:checked) {
      background: #e8f5e9;
      border-color: #4caf50;
    }

    label {
      display: block;
      padding: 12px;
      margin: 10px 0;
      border: 2px solid #e0e0e0;
      border-radius: 6px;
      cursor: pointer;
      transition: all 0.3s;
    }

    /* 2. :is()セレクタ */

    /* 複数の見出しに一括スタイル適用 */
    :is(h1, h2, h3, h4) {
      font-family: 'Georgia', serif;
      line-height: 1.3;
    }

    /* 複数のコンテナ内のリンク */
    :is(.header, .sidebar, .footer) a {
      color: #2196f3;
      text-decoration: none;
      transition: color 0.3s;
    }

    :is(.header, .sidebar, .footer) a:hover {
      color: #1976d2;
      text-decoration: underline;
    }

    /* 3. :where()セレクタ */

    /* 詳細度0なので簡単に上書き可能 */
    :where(ul, ol) {
      padding-left: 20px;
      margin: 10px 0;
    }

    /* リセット用スタイル（詳細度0） */
    :where(button, input, select) {
      font-family: inherit;
      font-size: inherit;
    }

    /* 通常のボタンスタイル（簡単に上書きできる） */
    button {
      padding: 10px 20px;
      background: #2196f3;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      margin: 5px;
    }

    input[type="text"],
    input[type="email"] {
      padding: 10px;
      border: 2px solid #ddd;
      border-radius: 4px;
      width: 100%;
      margin: 5px 0;
      transition: border-color 0.3s;
    }

    input:focus {
      outline: none;
      border-color: #2196f3;
    }

    .demo-area {
      background: #f9f9f9;
      padding: 20px;
      border-radius: 8px;
      margin: 20px 0;
    }

    .info {
      background: #e3f2fd;
      padding: 15px;
      border-left: 4px solid #2196f3;
      margin: 15px 0;
      border-radius: 4px;
    }
  </style>
</head>
<body>
  <h1>モダンCSSセレクタ デモ</h1>

  <!-- :has()のデモ -->
  <div class="section">
    <h2>1. :has()セレクタ</h2>
    <div class="info">
      <p><strong>:has()</strong> - 特定の子要素を持つ親要素を選択</p>
    </div>

    <h3>カードのレイアウト（画像の有無で自動切り替え）</h3>
    <div class="card">
      <img src="https://picsum.photos/100/100?random=1" alt="画像">
      <div>
        <h4>画像付きカード</h4>
        <p>.card:has(img) により、フレックスレイアウトが適用されます。</p>
      </div>
    </div>

    <div class="card">
      <h4>画像なしカード</h4>
      <p>.card:not(:has(img)) により、ブロックレイアウトで中央揃えになります。</p>
    </div>

    <h3>フォームのフォーカス検出</h3>
    <div class="demo-area">
      <form>
        <label>お名前</label>
        <input type="text" placeholder="入力欄をクリックしてみてください">
        <label>メール</label>
        <input type="email" placeholder="フォーム全体にスタイルが適用されます">
      </form>
      <p style="margin-top: 10px; font-size: 14px; color: #666;">
        form:has(input:focus) で、入力中のフォーム全体にスタイル適用
      </p>
    </div>

    <h3>チェックボックスの状態検出</h3>
    <div class="demo-area">
      <label>
        <input type="checkbox">
        タスク1を完了する
      </label>
      <label>
        <input type="checkbox">
        タスク2を完了する
      </label>
      <label>
        <input type="checkbox">
        タスク3を完了する
      </label>
      <p style="margin-top: 10px; font-size: 14px; color: #666;">
        label:has(input:checked) で、チェックされたラベルの背景色が変わります
      </p>
    </div>
  </div>

  <!-- :is()のデモ -->
  <div class="section">
    <h2>2. :is()セレクタ</h2>
    <div class="info">
      <p><strong>:is()</strong> - 複数のセレクタを一括指定</p>
    </div>

    <div class="demo-area">
      <p>すべての見出しに統一フォント（Georgia）が適用されています：</p>
      <h3>見出し3のサンプル</h3>
      <h4>見出し4のサンプル</h4>
      <p style="margin-top: 10px; font-size: 14px; color: #666;">
        :is(h1, h2, h3, h4) により、一括でスタイル適用
      </p>
    </div>
  </div>

  <!-- :where()のデモ -->
  <div class="section">
    <h2>3. :where()セレクタ</h2>
    <div class="info">
      <p><strong>:where()</strong> - 詳細度0で簡単に上書き可能</p>
    </div>

    <div class="demo-area">
      <p>:where()で設定されたスタイルは、詳細度が0なので簡単に上書きできます。</p>
      <ul>
        <li>リスト項目1</li>
        <li>リスト項目2</li>
        <li>リスト項目3</li>
      </ul>
      <p style="font-size: 14px; color: #666;">
        :where(ul, ol) でデフォルトスタイルを設定し、必要に応じて上書き
      </p>
    </div>
  </div>
</body>
</html>
```

### CSS-5. カスタムフォーム要素

アクセシブルなカスタムチェックボックスとラジオボタン。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>カスタムフォーム要素</title>
  <style>
    body {
      font-family: sans-serif;
      max-width: 800px;
      margin: 0 auto;
      padding: 40px 20px;
      background: #f5f5f5;
    }

    h1 {
      text-align: center;
      color: #333;
    }

    .form-container {
      background: white;
      padding: 40px;
      border-radius: 12px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }

    .form-section {
      margin: 30px 0;
    }

    .form-section h2 {
      color: #2196f3;
      margin-bottom: 20px;
      font-size: 20px;
    }

    /* Visually Hiddenパターン */
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

    /* カスタムチェックボックス */
    .checkbox-wrapper {
      margin: 15px 0;
    }

    input[type="checkbox"] + label {
      position: relative;
      padding-left: 35px;
      cursor: pointer;
      display: inline-block;
      line-height: 24px;
      user-select: none;
      transition: color 0.3s;
    }

    /* チェックボックスの外枠 */
    input[type="checkbox"] + label::before {
      content: '';
      position: absolute;
      left: 0;
      top: 0;
      width: 24px;
      height: 24px;
      border: 2px solid #999;
      border-radius: 4px;
      background: white;
      transition: all 0.3s;
    }

    /* チェックマーク */
    input[type="checkbox"] + label::after {
      content: '';
      position: absolute;
      left: 8px;
      top: 4px;
      width: 8px;
      height: 14px;
      border: solid white;
      border-width: 0 3px 3px 0;
      transform: rotate(45deg) scale(0);
      transition: transform 0.2s ease-in-out;
    }

    /* チェック状態 */
    input[type="checkbox"]:checked + label::before {
      background: #2196f3;
      border-color: #2196f3;
    }

    input[type="checkbox"]:checked + label::after {
      transform: rotate(45deg) scale(1);
    }

    /* ホバー */
    input[type="checkbox"]:not(:disabled) + label:hover::before {
      border-color: #2196f3;
      box-shadow: 0 0 0 3px rgba(33, 150, 243, 0.1);
    }

    /* フォーカス（キーボード操作時のみ） */
    input[type="checkbox"]:focus-visible + label::before {
      outline: 3px solid #2196f3;
      outline-offset: 2px;
    }

    /* 無効状態 */
    input[type="checkbox"]:disabled + label {
      color: #999;
      cursor: not-allowed;
    }

    input[type="checkbox"]:disabled + label::before {
      background: #f5f5f5;
      border-color: #ddd;
    }

    /* カスタムラジオボタン */
    .radio-wrapper {
      margin: 15px 0;
    }

    input[type="radio"] + label {
      position: relative;
      padding-left: 35px;
      cursor: pointer;
      display: inline-block;
      line-height: 24px;
      user-select: none;
    }

    /* ラジオボタンの外側の円 */
    input[type="radio"] + label::before {
      content: '';
      position: absolute;
      left: 0;
      top: 0;
      width: 24px;
      height: 24px;
      border: 2px solid #999;
      border-radius: 50%;
      background: white;
      transition: all 0.3s;
    }

    /* ラジオボタンの内側の円 */
    input[type="radio"] + label::after {
      content: '';
      position: absolute;
      left: 7px;
      top: 7px;
      width: 12px;
      height: 12px;
      border-radius: 50%;
      background: #2196f3;
      transform: scale(0);
      transition: transform 0.2s ease-in-out;
    }

    /* 選択状態 */
    input[type="radio"]:checked + label::before {
      border-color: #2196f3;
    }

    input[type="radio"]:checked + label::after {
      transform: scale(1);
    }

    /* ホバー */
    input[type="radio"]:not(:disabled) + label:hover::before {
      border-color: #2196f3;
      box-shadow: 0 0 0 3px rgba(33, 150, 243, 0.1);
    }

    /* フォーカス */
    input[type="radio"]:focus-visible + label::before {
      outline: 3px solid #2196f3;
      outline-offset: 2px;
    }

    /* 無効状態 */
    input[type="radio"]:disabled + label {
      color: #999;
      cursor: not-allowed;
    }

    input[type="radio"]:disabled + label::before {
      background: #f5f5f5;
      border-color: #ddd;
    }

    .note {
      background: #e3f2fd;
      padding: 15px;
      border-left: 4px solid #2196f3;
      margin: 20px 0;
      border-radius: 4px;
      font-size: 14px;
    }

    .submit-btn {
      width: 100%;
      padding: 14px;
      background: #2196f3;
      color: white;
      border: none;
      border-radius: 6px;
      font-size: 16px;
      font-weight: bold;
      cursor: pointer;
      margin-top: 20px;
      transition: background 0.3s;
    }

    .submit-btn:hover {
      background: #1976d2;
    }

    .submit-btn:focus-visible {
      outline: 3px solid #2196f3;
      outline-offset: 2px;
    }
  </style>
</head>
<body>
  <h1>アクセシブルなカスタムフォーム要素</h1>

  <div class="note">
    <p><strong>アクセシビリティ重視の実装：</strong></p>
    <ul style="margin: 10px 0; padding-left: 20px;">
      <li>✅ display: noneを使わない（Visually Hidden）</li>
      <li>✅ キーボード操作対応（Tab、Space、Arrow）</li>
      <li>✅ スクリーンリーダー対応</li>
      <li>✅ フォーカス表示（:focus-visible）</li>
      <li>✅ 無効状態の視覚的表現</li>
    </ul>
  </div>

  <div class="form-container">
    <form>
      <!-- チェックボックス -->
      <div class="form-section">
        <h2>チェックボックス</h2>

        <div class="checkbox-wrapper">
          <input type="checkbox" id="checkbox1" name="interests" value="web">
          <label for="checkbox1">Web開発に興味がある</label>
        </div>

        <div class="checkbox-wrapper">
          <input type="checkbox" id="checkbox2" name="interests" value="design">
          <label for="checkbox2">デザインに興味がある</label>
        </div>

        <div class="checkbox-wrapper">
          <input type="checkbox" id="checkbox3" name="interests" value="backend">
          <label for="checkbox3">バックエンド開発に興味がある</label>
        </div>

        <div class="checkbox-wrapper">
          <input type="checkbox" id="checkbox4" name="interests" value="disabled" disabled>
          <label for="checkbox4">無効状態のチェックボックス</label>
        </div>
      </div>

      <!-- ラジオボタン -->
      <div class="form-section">
        <h2>ラジオボタン</h2>

        <fieldset style="border: none; padding: 0;">
          <legend style="font-weight: bold; margin-bottom: 10px;">配送方法を選択</legend>

          <div class="radio-wrapper">
            <input type="radio" id="radio1" name="shipping" value="standard" checked>
            <label for="radio1">通常配送（無料・3〜5日）</label>
          </div>

          <div class="radio-wrapper">
            <input type="radio" id="radio2" name="shipping" value="express">
            <label for="radio2">お急ぎ便（500円・翌日配送）</label>
          </div>

          <div class="radio-wrapper">
            <input type="radio" id="radio3" name="shipping" value="premium">
            <label for="radio3">プレミアム配送（1,000円・当日配送）</label>
          </div>

          <div class="radio-wrapper">
            <input type="radio" id="radio4" name="shipping" value="disabled" disabled>
            <label for="radio4">無効状態のラジオボタン</label>
          </div>
        </fieldset>
      </div>

      <!-- 利用規約 -->
      <div class="form-section">
        <div class="checkbox-wrapper">
          <input type="checkbox" id="terms" name="terms" required>
          <label for="terms">
            <a href="#" style="color: #2196f3;">利用規約</a>に同意する（必須）
          </label>
        </div>
      </div>

      <button type="submit" class="submit-btn">送信する</button>
    </form>
  </div>

  <div class="note" style="margin-top: 30px;">
    <p><strong>キーボード操作：</strong></p>
    <ul style="margin: 10px 0; padding-left: 20px;">
      <li><kbd>Tab</kbd> - 次の要素へ移動</li>
      <li><kbd>Shift + Tab</kbd> - 前の要素へ移動</li>
      <li><kbd>Space</kbd> - チェックボックスの切り替え</li>
      <li><kbd>↑/↓</kbd> - ラジオボタンのグループ内移動</li>
    </ul>
  </div>

  <script>
    document.querySelector('form').addEventListener('submit', (e) => {
      e.preventDefault();

      const interests = Array.from(document.querySelectorAll('input[name="interests"]:checked'))
        .map(cb => cb.nextElementSibling.textContent);

      const shipping = document.querySelector('input[name="shipping"]:checked')
        .nextElementSibling.textContent;

      const terms = document.getElementById('terms').checked;

      if (!terms) {
        alert('利用規約に同意してください');
        return;
      }

      alert(`選択内容:\n\n興味: ${interests.join(', ') || 'なし'}\n配送: ${shipping}`);
    });
  </script>
</body>
</html>
```

---

## JavaScript編

### JS-1. 非同期処理の例

---

## 1. 非同期処理の例

### fetch-example.html

async/awaitとfetchを使ったAPI通信の例。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Fetch API Example</title>
  <style>
    body {
      font-family: sans-serif;
      max-width: 800px;
      margin: 0 auto;
      padding: 20px;
    }
    .user-card {
      border: 1px solid #ddd;
      padding: 15px;
      margin: 10px 0;
      border-radius: 8px;
    }
    .loading {
      color: #666;
      font-style: italic;
    }
    .error {
      color: red;
      padding: 10px;
      background: #fee;
      border-radius: 4px;
    }
    button {
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h1>ユーザー一覧取得（Fetch API）</h1>
  <button id="loadUsers">ユーザーを読み込む</button>
  <button id="clearUsers">クリア</button>
  <div id="status"></div>
  <div id="userList"></div>

  <script>
    const statusEl = document.getElementById("status");
    const userListEl = document.getElementById("userList");
    const loadButton = document.getElementById("loadUsers");
    const clearButton = document.getElementById("clearUsers");

    // ユーザーデータ取得
    async function fetchUsers() {
      try {
        // ローディング表示
        statusEl.innerHTML = '<p class="loading">読み込み中...</p>';
        userListEl.innerHTML = "";

        // JSONPlaceholder APIからデータ取得
        const response = await fetch("https://jsonplaceholder.typicode.com/users");

        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }

        const users = await response.json();

        // 成功メッセージ
        statusEl.innerHTML = `<p style="color: green;">${users.length}件のユーザーを取得しました</p>`;

        // ユーザーリストを表示
        displayUsers(users);
      } catch (error) {
        // エラー表示
        statusEl.innerHTML = `<div class="error">エラーが発生しました: ${error.message}</div>`;
        console.error("Error:", error);
      }
    }

    // ユーザーリスト表示
    function displayUsers(users) {
      userListEl.innerHTML = users.map(user => `
        <div class="user-card">
          <h3>${user.name}</h3>
          <p>Email: ${user.email}</p>
          <p>会社: ${user.company.name}</p>
          <p>都市: ${user.address.city}</p>
        </div>
      `).join("");
    }

    // イベントリスナー
    loadButton.addEventListener("click", fetchUsers);

    clearButton.addEventListener("click", () => {
      userListEl.innerHTML = "";
      statusEl.innerHTML = "";
    });
  </script>
</body>
</html>
```

---

## 2. Intersection Observer の例

### intersection-observer-example.html

スクロール時のフェードインアニメーション。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Intersection Observer Example</title>
  <style>
    body {
      font-family: sans-serif;
      padding: 20px;
    }

    .section {
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      margin: 20px 0;
      border-radius: 10px;
      color: white;
      font-size: 2em;
    }

    .fade-in {
      opacity: 0;
      transform: translateY(50px);
      transition: opacity 0.8s ease, transform 0.8s ease;
    }

    .fade-in.visible {
      opacity: 1;
      transform: translateY(0);
    }

    .info {
      position: fixed;
      top: 20px;
      right: 20px;
      background: white;
      padding: 15px;
      border-radius: 8px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
      font-size: 14px;
    }

    .info h3 {
      margin: 0 0 10px 0;
    }

    .status {
      padding: 5px 10px;
      border-radius: 4px;
      margin: 5px 0;
      background: #f0f0f0;
    }

    .status.active {
      background: #4caf50;
      color: white;
    }
  </style>
</head>
<body>
  <div class="info">
    <h3>表示状態</h3>
    <div id="statusList"></div>
  </div>

  <h1>スクロールしてみてください</h1>

  <div class="section fade-in" data-section="1">
    セクション 1
  </div>

  <div class="section fade-in" data-section="2">
    セクション 2
  </div>

  <div class="section fade-in" data-section="3">
    セクション 3
  </div>

  <div class="section fade-in" data-section="4">
    セクション 4
  </div>

  <div class="section fade-in" data-section="5">
    セクション 5
  </div>

  <script>
    const statusList = document.getElementById("statusList");
    const sections = document.querySelectorAll(".fade-in");

    // ステータス表示を初期化
    sections.forEach(section => {
      const sectionNum = section.dataset.section;
      const statusEl = document.createElement("div");
      statusEl.className = "status";
      statusEl.id = `status-${sectionNum}`;
      statusEl.textContent = `セクション ${sectionNum}: 非表示`;
      statusList.appendChild(statusEl);
    });

    // Intersection Observer を設定
    const observerOptions = {
      root: null,
      rootMargin: "0px",
      threshold: 0.2 // 20%見えたら発火
    };

    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        const sectionNum = entry.target.dataset.section;
        const statusEl = document.getElementById(`status-${sectionNum}`);

        if (entry.isIntersecting) {
          // 画面内に入った
          entry.target.classList.add("visible");
          statusEl.classList.add("active");
          statusEl.textContent = `セクション ${sectionNum}: 表示中`;
          console.log(`セクション ${sectionNum} が表示されました`);
        } else {
          // 画面外に出た
          statusEl.classList.remove("active");
          statusEl.textContent = `セクション ${sectionNum}: 非表示`;
        }
      });
    }, observerOptions);

    // すべてのセクションを監視
    sections.forEach(section => {
      observer.observe(section);
    });
  </script>
</body>
</html>
```

---

## 3. matchMedia の例

### matchmedia-example.html

レスポンシブ対応のメニュー切り替え。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>matchMedia Example</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: sans-serif;
    }

    .header {
      background: #333;
      color: white;
      padding: 15px 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .logo {
      font-size: 24px;
      font-weight: bold;
    }

    .nav {
      display: flex;
      gap: 20px;
    }

    .nav a {
      color: white;
      text-decoration: none;
      padding: 8px 16px;
      border-radius: 4px;
      transition: background 0.3s;
    }

    .nav a:hover {
      background: #555;
    }

    .menu-toggle {
      display: none;
      background: none;
      border: none;
      color: white;
      font-size: 24px;
      cursor: pointer;
    }

    .mobile-nav {
      display: none;
      background: #444;
      padding: 20px;
    }

    .mobile-nav a {
      display: block;
      color: white;
      text-decoration: none;
      padding: 12px;
      margin: 5px 0;
      border-radius: 4px;
      transition: background 0.3s;
    }

    .mobile-nav a:hover {
      background: #555;
    }

    .content {
      padding: 40px 20px;
      max-width: 1200px;
      margin: 0 auto;
    }

    .status {
      background: #e3f2fd;
      padding: 15px;
      border-left: 4px solid #2196f3;
      margin-bottom: 20px;
      border-radius: 4px;
    }

    .status h2 {
      margin-bottom: 10px;
      color: #1976d2;
    }
  </style>
</head>
<body>
  <header class="header">
    <div class="logo">My Site</div>
    <nav class="nav">
      <a href="#home">ホーム</a>
      <a href="#about">About</a>
      <a href="#services">サービス</a>
      <a href="#contact">お問い合わせ</a>
    </nav>
    <button class="menu-toggle">☰</button>
  </header>

  <nav class="mobile-nav" id="mobileNav">
    <a href="#home">ホーム</a>
    <a href="#about">About</a>
    <a href="#services">サービス</a>
    <a href="#contact">お問い合わせ</a>
  </nav>

  <div class="content">
    <div class="status">
      <h2>現在の表示モード</h2>
      <p id="currentMode">判定中...</p>
      <p id="windowSize"></p>
    </div>

    <h1>matchMedia APIの例</h1>
    <p>ブラウザのウィンドウサイズを変更してみてください。</p>
    <p>768px未満でモバイルメニューに切り替わります。</p>
  </div>

  <script>
    const mobileNav = document.getElementById("mobileNav");
    const menuToggle = document.querySelector(".menu-toggle");
    const desktopNav = document.querySelector(".nav");
    const currentModeEl = document.getElementById("currentMode");
    const windowSizeEl = document.getElementById("windowSize");

    // メディアクエリを定義
    const mobileQuery = window.matchMedia("(max-width: 767px)");
    const tabletQuery = window.matchMedia("(min-width: 768px) and (max-width: 1023px)");
    const desktopQuery = window.matchMedia("(min-width: 1024px)");

    // 表示モードを更新
    function updateLayout() {
      // ウィンドウサイズを表示
      windowSizeEl.textContent = `ウィンドウ幅: ${window.innerWidth}px`;

      if (mobileQuery.matches) {
        currentModeEl.textContent = "モバイル表示（767px以下）";
        menuToggle.style.display = "block";
        desktopNav.style.display = "none";
      } else if (tabletQuery.matches) {
        currentModeEl.textContent = "タブレット表示（768px〜1023px）";
        menuToggle.style.display = "none";
        desktopNav.style.display = "flex";
        mobileNav.style.display = "none";
      } else if (desktopQuery.matches) {
        currentModeEl.textContent = "デスクトップ表示（1024px以上）";
        menuToggle.style.display = "none";
        desktopNav.style.display = "flex";
        mobileNav.style.display = "none";
      }
    }

    // 初回実行
    updateLayout();

    // メディアクエリの変化を監視
    mobileQuery.addEventListener("change", updateLayout);
    tabletQuery.addEventListener("change", updateLayout);
    desktopQuery.addEventListener("change", updateLayout);

    // モバイルメニューの開閉
    menuToggle.addEventListener("click", () => {
      if (mobileNav.style.display === "block") {
        mobileNav.style.display = "none";
      } else {
        mobileNav.style.display = "block";
      }
    });

    // ダークモード検出
    const darkModeQuery = window.matchMedia("(prefers-color-scheme: dark)");
    console.log("ダークモード:", darkModeQuery.matches ? "有効" : "無効");

    darkModeQuery.addEventListener("change", (e) => {
      console.log("ダークモード変更:", e.matches ? "有効" : "無効");
    });
  </script>
</body>
</html>
```

---

## 4. モジュールの例

### module-example/

モジュールシステムを使った例。

#### index.html

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Module Example</title>
  <style>
    body {
      font-family: sans-serif;
      max-width: 800px;
      margin: 0 auto;
      padding: 20px;
    }
    .task {
      border: 1px solid #ddd;
      padding: 15px;
      margin: 10px 0;
      border-radius: 8px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .task.completed {
      background: #e8f5e9;
    }
    input[type="text"] {
      padding: 8px;
      width: 300px;
      font-size: 16px;
    }
    button {
      padding: 8px 16px;
      font-size: 16px;
      cursor: pointer;
      border: none;
      border-radius: 4px;
      background: #2196f3;
      color: white;
    }
    button:hover {
      background: #1976d2;
    }
    .delete-btn {
      background: #f44336;
    }
    .delete-btn:hover {
      background: #d32f2f;
    }
  </style>
</head>
<body>
  <h1>タスク管理アプリ（ES Modules）</h1>

  <div>
    <input type="text" id="taskInput" placeholder="新しいタスクを入力">
    <button id="addTaskBtn">追加</button>
  </div>

  <h2>タスク一覧</h2>
  <div id="taskList"></div>

  <!-- type="module"を指定 -->
  <script type="module" src="./main.js"></script>
</body>
</html>
```

#### main.js

```javascript
// モジュールをインポート
import TaskManager from "./TaskManager.js";
import { formatDate, generateId } from "./utils.js";

// タスク管理インスタンスを作成
const taskManager = new TaskManager();

// DOM要素
const taskInput = document.getElementById("taskInput");
const addTaskBtn = document.getElementById("addTaskBtn");
const taskList = document.getElementById("taskList");

// タスクリストを描画
function renderTasks() {
  const tasks = taskManager.getAllTasks();

  if (tasks.length === 0) {
    taskList.innerHTML = "<p>タスクがありません</p>";
    return;
  }

  taskList.innerHTML = tasks.map(task => `
    <div class="task ${task.completed ? "completed" : ""}">
      <div>
        <h3>${task.title}</h3>
        <small>作成日: ${formatDate(task.createdAt)}</small>
      </div>
      <div>
        <button onclick="toggleTask('${task.id}')">
          ${task.completed ? "未完了に戻す" : "完了"}
        </button>
        <button class="delete-btn" onclick="deleteTask('${task.id}')">
          削除
        </button>
      </div>
    </div>
  `).join("");
}

// タスク追加
function addTask() {
  const title = taskInput.value.trim();
  if (!title) {
    alert("タスク名を入力してください");
    return;
  }

  const task = {
    id: generateId(),
    title: title,
    completed: false,
    createdAt: new Date()
  };

  taskManager.addTask(task);
  taskInput.value = "";
  renderTasks();
}

// タスク完了/未完了切り替え（グローバルに公開）
window.toggleTask = (id) => {
  taskManager.toggleTask(id);
  renderTasks();
};

// タスク削除（グローバルに公開）
window.deleteTask = (id) => {
  if (confirm("このタスクを削除しますか？")) {
    taskManager.deleteTask(id);
    renderTasks();
  }
};

// イベントリスナー
addTaskBtn.addEventListener("click", addTask);
taskInput.addEventListener("keypress", (e) => {
  if (e.key === "Enter") {
    addTask();
  }
});

// 初回描画
renderTasks();
```

#### TaskManager.js

```javascript
// タスク管理クラス
export default class TaskManager {
  constructor() {
    this.tasks = this.loadFromStorage();
  }

  // ローカルストレージから読み込み
  loadFromStorage() {
    const data = localStorage.getItem("tasks");
    if (data) {
      return JSON.parse(data).map(task => ({
        ...task,
        createdAt: new Date(task.createdAt)
      }));
    }
    return [];
  }

  // ローカルストレージに保存
  saveToStorage() {
    localStorage.setItem("tasks", JSON.stringify(this.tasks));
  }

  // タスク追加
  addTask(task) {
    this.tasks.push(task);
    this.saveToStorage();
  }

  // タスク削除
  deleteTask(id) {
    this.tasks = this.tasks.filter(task => task.id !== id);
    this.saveToStorage();
  }

  // タスク完了切り替え
  toggleTask(id) {
    const task = this.tasks.find(task => task.id === id);
    if (task) {
      task.completed = !task.completed;
      this.saveToStorage();
    }
  }

  // すべてのタスクを取得
  getAllTasks() {
    return this.tasks;
  }

  // 完了タスクを取得
  getCompletedTasks() {
    return this.tasks.filter(task => task.completed);
  }

  // 未完了タスクを取得
  getPendingTasks() {
    return this.tasks.filter(task => !task.completed);
  }
}
```

#### utils.js

```javascript
// ユーティリティ関数

// 日付をフォーマット
export function formatDate(date) {
  return date.toLocaleDateString("ja-JP", {
    year: "numeric",
    month: "2-digit",
    day: "2-digit",
    hour: "2-digit",
    minute: "2-digit"
  });
}

// ユニークIDを生成
export function generateId() {
  return Date.now().toString(36) + Math.random().toString(36).substring(2);
}

// 配列をシャッフル
export function shuffle(array) {
  const newArray = [...array];
  for (let i = newArray.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [newArray[i], newArray[j]] = [newArray[j], newArray[i]];
  }
  return newArray;
}
```

---

## 5. 実用的なパターン

### debounce-throttle-example.html

デバウンスとスロットルの比較。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Debounce & Throttle Example</title>
  <style>
    body {
      font-family: sans-serif;
      max-width: 1000px;
      margin: 0 auto;
      padding: 20px;
    }

    .demo-box {
      border: 2px solid #ddd;
      padding: 20px;
      margin: 20px 0;
      border-radius: 8px;
    }

    .counter {
      font-size: 24px;
      font-weight: bold;
      color: #2196f3;
      margin: 10px 0;
    }

    input[type="text"] {
      width: 100%;
      padding: 12px;
      font-size: 16px;
      border: 2px solid #ddd;
      border-radius: 4px;
      box-sizing: border-box;
    }

    .log {
      background: #f5f5f5;
      padding: 10px;
      border-radius: 4px;
      max-height: 200px;
      overflow-y: auto;
      font-family: monospace;
      font-size: 12px;
    }

    .log-entry {
      padding: 4px;
      border-bottom: 1px solid #ddd;
    }

    h2 {
      color: #333;
      border-bottom: 2px solid #2196f3;
      padding-bottom: 10px;
    }
  </style>
</head>
<body>
  <h1>Debounce と Throttle の比較</h1>

  <div class="demo-box">
    <h2>1. 通常のイベント（制御なし）</h2>
    <input type="text" id="normalInput" placeholder="入力してみてください">
    <div class="counter">実行回数: <span id="normalCount">0</span></div>
    <div class="log" id="normalLog"></div>
  </div>

  <div class="demo-box">
    <h2>2. Debounce（入力停止後に実行）</h2>
    <p>入力が止まってから500ms後に1回だけ実行されます</p>
    <input type="text" id="debounceInput" placeholder="入力してみてください">
    <div class="counter">実行回数: <span id="debounceCount">0</span></div>
    <div class="log" id="debounceLog"></div>
  </div>

  <div class="demo-box">
    <h2>3. Throttle（一定間隔で実行）</h2>
    <p>500msごとに最大1回実行されます</p>
    <input type="text" id="throttleInput" placeholder="入力してみてください">
    <div class="counter">実行回数: <span id="throttleCount">0</span></div>
    <div class="log" id="throttleLog"></div>
  </div>

  <script>
    // デバウンス関数
    function debounce(func, delay = 300) {
      let timeoutId;
      return function(...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => {
          func.apply(this, args);
        }, delay);
      };
    }

    // スロットル関数
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

    // ログ追加関数
    function addLog(logElement, message) {
      const entry = document.createElement("div");
      entry.className = "log-entry";
      const time = new Date().toLocaleTimeString("ja-JP");
      entry.textContent = `${time} - ${message}`;
      logElement.insertBefore(entry, logElement.firstChild);
    }

    // カウンター
    let normalCount = 0;
    let debounceCount = 0;
    let throttleCount = 0;

    // 1. 通常のイベント
    document.getElementById("normalInput").addEventListener("input", (e) => {
      normalCount++;
      document.getElementById("normalCount").textContent = normalCount;
      addLog(document.getElementById("normalLog"), `入力値: ${e.target.value}`);
    });

    // 2. Debounce
    const debouncedHandler = debounce((value) => {
      debounceCount++;
      document.getElementById("debounceCount").textContent = debounceCount;
      addLog(document.getElementById("debounceLog"), `処理実行: ${value}`);
    }, 500);

    document.getElementById("debounceInput").addEventListener("input", (e) => {
      addLog(document.getElementById("debounceLog"), `入力検知: ${e.target.value}`);
      debouncedHandler(e.target.value);
    });

    // 3. Throttle
    const throttledHandler = throttle((value) => {
      throttleCount++;
      document.getElementById("throttleCount").textContent = throttleCount;
      addLog(document.getElementById("throttleLog"), `処理実行: ${value}`);
    }, 500);

    document.getElementById("throttleInput").addEventListener("input", (e) => {
      throttledHandler(e.target.value);
    });
  </script>
</body>
</html>
```

---

## 使い方

これらの例は、ローカルサーバーで実行してください。

```bash
# Python 3の場合
python -m http.server 8000

# Node.jsの場合
npx http-server

# VSCodeのLive Server拡張機能を使用
```

ブラウザで `http://localhost:8000` を開いて、各例を試してみてください。

---

## 学習のヒント

1. **実際に動かしてみる**: コードを読むだけでなく、実際に動かして動作を確認
2. **コードを変更してみる**: パラメータや値を変えて、挙動の変化を観察
3. **DevToolsを活用**: ブラウザの開発者ツールでコンソールやネットワークを確認
4. **エラーを恐れない**: エラーが出たら、メッセージを読んで原因を探る

---

## 次のステップ

- 各例を自分なりにカスタマイズ
- 新しい機能を追加してみる
- 実際のプロジェクトで応用する
