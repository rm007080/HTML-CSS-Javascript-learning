# 実践コード例

各セクションで学んだ内容を実際に動かせるコード例です。

## 目次

### HTML編
1. [レスポンシブ画像の例](#html-1-レスポンシブ画像の例)
2. [picture要素とモダンフォーマット](#html-2-picture要素とモダンフォーマット)
3. [detailsアコーディオンの例](#html-3-detailsアコーディオンの例)
4. [dialog モーダルの例](#html-4-dialog-モーダルの例)
5. [アクセシブルなフォーム](#html-5-アクセシブルなフォーム)

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
