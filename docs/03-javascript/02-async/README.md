# JavaScript 非同期処理編

JavaScriptの非同期処理について、基礎から実践まで学びます。

## 目次

1. [非同期処理とは](#1-非同期処理とは)
2. [Promise の基礎](#2-promise-の基礎)
3. [async/await](#3-asyncawait)
4. [Fetch API](#4-fetch-api)
5. [AbortController（通信の中断）](#5-abortcontroller通信の中断)
6. [fetch vs axios](#6-fetch-vs-axios)
7. [実践的なパターン](#7-実践的なパターン)

---

## 1. 非同期処理とは

### 同期処理と非同期処理の違い

**同期処理**は、コードが順番に実行され、1つの処理が終わるまで次の処理は待機します。

```javascript
console.log("1番目");
console.log("2番目");
console.log("3番目");
// 出力: 1番目 → 2番目 → 3番目
```

**非同期処理**は、時間のかかる処理を待たずに次の処理を実行します。

```javascript
console.log("1番目");

setTimeout(() => {
  console.log("2番目（1秒後）");
}, 1000);

console.log("3番目");
// 出力: 1番目 → 3番目 → 2番目（1秒後）
```

### なぜ非同期処理が必要か

以下のような**時間のかかる処理**では、非同期処理が必須です：

- サーバーとの通信（API呼び出し）
- ファイルの読み書き
- データベース操作
- タイマー処理

これらを同期的に実行すると、ブラウザがフリーズしてユーザー体験が悪化します。

---

## 2. Promise の基礎

Promiseは、非同期処理をより扱いやすくするための仕組みです。

### Promise の状態

Promiseには3つの状態があります：

1. **Pending（待機中）**: 初期状態、まだ完了していない
2. **Fulfilled（成功）**: 処理が成功した
3. **Rejected（失敗）**: 処理が失敗した

### Promise の作成

```javascript
const promise = new Promise((resolve, reject) => {
  // 非同期処理
  const success = true;

  if (success) {
    resolve("成功しました"); // 成功時
  } else {
    reject("失敗しました");   // 失敗時
  }
});
```

### Promise の使用（then/catch）

```javascript
promise
  .then(result => {
    console.log(result); // "成功しました"
  })
  .catch(error => {
    console.error(error);
  });
```

### 実用例：遅延処理

```javascript
function delay(ms) {
  return new Promise(resolve => {
    setTimeout(resolve, ms);
  });
}

delay(2000)
  .then(() => {
    console.log("2秒経過しました");
  });
```

### Promise チェーン

複数の非同期処理を順番に実行できます。

```javascript
function fetchUser(userId) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve({ id: userId, name: "太郎" });
    }, 1000);
  });
}

function fetchPosts(userId) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve([
        { id: 1, title: "投稿1" },
        { id: 2, title: "投稿2" }
      ]);
    }, 1000);
  });
}

// Promiseチェーン
fetchUser(1)
  .then(user => {
    console.log("ユーザー取得:", user);
    return fetchPosts(user.id);
  })
  .then(posts => {
    console.log("投稿取得:", posts);
  })
  .catch(error => {
    console.error("エラー:", error);
  });
```

### Promise.all（並列実行）

複数の非同期処理を同時に実行し、すべて完了するのを待ちます。

```javascript
const promise1 = fetch("/api/users");
const promise2 = fetch("/api/posts");
const promise3 = fetch("/api/comments");

Promise.all([promise1, promise2, promise3])
  .then(([users, posts, comments]) => {
    console.log("すべての取得完了");
    console.log(users, posts, comments);
  })
  .catch(error => {
    console.error("いずれかが失敗:", error);
  });
```

**重要：** 1つでも失敗すると、Promise.all全体が失敗します。

### Promise.race（最初の完了を待つ）

```javascript
const slow = new Promise(resolve => setTimeout(() => resolve("遅い"), 2000));
const fast = new Promise(resolve => setTimeout(() => resolve("速い"), 1000));

Promise.race([slow, fast])
  .then(result => {
    console.log(result); // "速い"（最初に完了した結果）
  });
```

### Promise.allSettled（全結果を取得）

すべての処理の完了を待ち、成功・失敗に関わらず全結果を取得します。

```javascript
const promises = [
  Promise.resolve("成功1"),
  Promise.reject("失敗"),
  Promise.resolve("成功2")
];

Promise.allSettled(promises)
  .then(results => {
    results.forEach(result => {
      if (result.status === "fulfilled") {
        console.log("成功:", result.value);
      } else {
        console.log("失敗:", result.reason);
      }
    });
  });
```

---

## 3. async/await

async/awaitは、Promiseをより読みやすく書ける構文です。

### 基本構文

```javascript
// Promiseを使った書き方
function fetchData() {
  return fetch("/api/data")
    .then(response => response.json())
    .then(data => {
      console.log(data);
      return data;
    });
}

// async/awaitを使った書き方
async function fetchData() {
  const response = await fetch("/api/data");
  const data = await response.json();
  console.log(data);
  return data;
}
```

### async の役割

`async`をつけた関数は、**自動的にPromiseを返します**。

```javascript
async function greet() {
  return "こんにちは";
}

greet().then(message => {
  console.log(message); // "こんにちは"
});

// 上記は以下と同じ
function greet() {
  return Promise.resolve("こんにちは");
}
```

### await の役割

`await`は、**Promiseの完了を待ちます**。await は async 関数内でのみ使用できます。

```javascript
async function example() {
  console.log("開始");

  // 1秒待つ
  await new Promise(resolve => setTimeout(resolve, 1000));

  console.log("1秒経過");
}

example();
```

### エラーハンドリング（try/catch）

```javascript
async function fetchUser(userId) {
  try {
    const response = await fetch(`/api/users/${userId}`);

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const user = await response.json();
    console.log(user);
    return user;
  } catch (error) {
    console.error("エラーが発生しました:", error);
    throw error; // 必要に応じて再スロー
  }
}
```

### 複数の非同期処理

#### 順次実行（await を順番に）

```javascript
async function sequential() {
  const user = await fetchUser(1);      // 1秒待つ
  const posts = await fetchPosts(user.id); // さらに1秒待つ
  // 合計2秒かかる
}
```

#### 並列実行（Promise.all）

```javascript
async function parallel() {
  const [user, posts] = await Promise.all([
    fetchUser(1),
    fetchPosts(1)
  ]);
  // 1秒で完了（同時実行）
}
```

**重要な注意点：** ループ内で await を使うと、1つずつ順番に実行されます。

```javascript
// ❌ 遅い（順次実行）
async function loadImages(urls) {
  const images = [];
  for (const url of urls) {
    const image = await loadImage(url); // 1つずつ待つ
    images.push(image);
  }
  return images;
}

// ✅ 速い（並列実行）
async function loadImages(urls) {
  const promises = urls.map(url => loadImage(url));
  return await Promise.all(promises);
}
```

### トップレベル await（モジュール内）

モジュール（`type="module"`）では、トップレベルで await が使えます。

```javascript
// モジュールファイル内
const response = await fetch("/api/config");
const config = await response.json();

console.log(config);
```

---

## 4. Fetch API

Fetch APIは、サーバーと通信するためのモダンなブラウザ標準APIです。

### 基本的な GET リクエスト

```javascript
fetch("https://api.example.com/users")
  .then(response => response.json())
  .then(data => {
    console.log(data);
  })
  .catch(error => {
    console.error("エラー:", error);
  });

// async/awaitを使った書き方
async function getUsers() {
  const response = await fetch("https://api.example.com/users");
  const data = await response.json();
  return data;
}
```

### レスポンスの処理

```javascript
async function fetchData(url) {
  const response = await fetch(url);

  // ステータスコードの確認
  console.log(response.status);     // 200, 404, 500など
  console.log(response.ok);         // 200-299ならtrue

  // ヘッダーの取得
  console.log(response.headers.get("Content-Type"));

  // ボディの解析
  const data = await response.json(); // JSON
  // const text = await response.text(); // テキスト
  // const blob = await response.blob(); // バイナリ

  return data;
}
```

### POST リクエスト

```javascript
async function createUser(userData) {
  const response = await fetch("https://api.example.com/users", {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify(userData)
  });

  const result = await response.json();
  return result;
}

// 使用例
createUser({ name: "太郎", age: 25 });
```

### PUT リクエスト（更新）

```javascript
async function updateUser(userId, userData) {
  const response = await fetch(`https://api.example.com/users/${userId}`, {
    method: "PUT",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify(userData)
  });

  return await response.json();
}
```

### DELETE リクエスト

```javascript
async function deleteUser(userId) {
  const response = await fetch(`https://api.example.com/users/${userId}`, {
    method: "DELETE"
  });

  if (response.ok) {
    console.log("削除成功");
  }
}
```

### クエリパラメータの追加

```javascript
const params = new URLSearchParams({
  page: 1,
  limit: 10,
  sort: "name"
});

fetch(`https://api.example.com/users?${params}`)
  .then(response => response.json())
  .then(data => console.log(data));

// URL: https://api.example.com/users?page=1&limit=10&sort=name
```

### 認証トークンの追加

```javascript
async function fetchWithAuth(url, token) {
  const response = await fetch(url, {
    headers: {
      "Authorization": `Bearer ${token}`
    }
  });

  return await response.json();
}
```

### エラーハンドリングの注意点

**重要：** fetchは、ネットワークエラー以外（404、500など）では**Promiseをrejectしません**。

```javascript
async function fetchData(url) {
  try {
    const response = await fetch(url);

    // HTTPエラーのチェックが必要
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json();
    return data;
  } catch (error) {
    // ネットワークエラーまたは手動でスローしたエラー
    console.error("エラー:", error);
    throw error;
  }
}
```

---

## 5. AbortController（通信の中断）

AbortControllerは、fetch リクエストを途中で中断するための機能です。

### 基本的な使い方

```javascript
// コントローラーを作成
const controller = new AbortController();
const signal = controller.signal;

// fetchにsignalを渡す
fetch("https://api.example.com/data", { signal })
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => {
    if (error.name === "AbortError") {
      console.log("リクエストが中断されました");
    } else {
      console.error("エラー:", error);
    }
  });

// 中断する
controller.abort();
```

### タイムアウトの実装

```javascript
async function fetchWithTimeout(url, timeout = 5000) {
  const controller = new AbortController();
  const signal = controller.signal;

  // タイムアウトを設定
  const timeoutId = setTimeout(() => {
    controller.abort();
  }, timeout);

  try {
    const response = await fetch(url, { signal });
    clearTimeout(timeoutId); // タイムアウトをクリア
    return await response.json();
  } catch (error) {
    if (error.name === "AbortError") {
      throw new Error("タイムアウトしました");
    }
    throw error;
  }
}

// 使用例
fetchWithTimeout("https://api.example.com/data", 3000)
  .then(data => console.log(data))
  .catch(error => console.error(error.message));
```

### ユーザーによるキャンセル

```javascript
let controller = null;

// 検索開始
function startSearch(query) {
  // 前回の検索をキャンセル
  if (controller) {
    controller.abort();
  }

  // 新しいコントローラーを作成
  controller = new AbortController();

  fetch(`https://api.example.com/search?q=${query}`, {
    signal: controller.signal
  })
    .then(response => response.json())
    .then(results => {
      displayResults(results);
    })
    .catch(error => {
      if (error.name !== "AbortError") {
        console.error(error);
      }
    });
}

// ユーザーが検索をキャンセル
function cancelSearch() {
  if (controller) {
    controller.abort();
    controller = null;
  }
}
```

### 複数のリクエストを同時にキャンセル

```javascript
const controller = new AbortController();
const signal = controller.signal;

const requests = [
  fetch("/api/users", { signal }),
  fetch("/api/posts", { signal }),
  fetch("/api/comments", { signal })
];

Promise.all(requests)
  .then(responses => Promise.all(responses.map(r => r.json())))
  .then(([users, posts, comments]) => {
    console.log(users, posts, comments);
  })
  .catch(error => {
    if (error.name === "AbortError") {
      console.log("すべてのリクエストが中断されました");
    }
  });

// すべてのリクエストを中断
controller.abort();
```

---

## 6. fetch vs axios

fetch APIとaxiosライブラリの違いを理解しましょう。

### インストール

```javascript
// fetch - インストール不要（ブラウザ標準）

// axios - インストールが必要
// npm install axios
```

### HTTPメソッドの使い方

#### fetch

```javascript
// GET
fetch("/api/users")
  .then(response => response.json())
  .then(data => console.log(data));

// POST
fetch("/api/users", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ name: "太郎" })
})
  .then(response => response.json())
  .then(data => console.log(data));
```

#### axios

```javascript
// GET
axios.get("/api/users")
  .then(response => console.log(response.data));

// POST（自動でJSONに変換）
axios.post("/api/users", { name: "太郎" })
  .then(response => console.log(response.data));
```

### JSONデータの取得

#### fetch

```javascript
// 2段階必要
const response = await fetch("/api/users");
const data = await response.json(); // JSONパースが必要
```

#### axios

```javascript
// 1段階で取得
const response = await axios.get("/api/users");
const data = response.data; // 既にパース済み
```

### エラーハンドリング（最大の違い）

#### fetch

HTTPエラー（404、500など）でも**Promiseはresolveされます**。

```javascript
fetch("/api/users")
  .then(response => {
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return response.json();
  })
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

#### axios

HTTPエラーで**自動的にPromiseがrejectされます**。

```javascript
axios.get("/api/users")
  .then(response => console.log(response.data))
  .catch(error => {
    // 404、500などで自動的にcatchに入る
    console.error(error.response.status);
    console.error(error.response.data);
  });
```

### どちらを使うべきか

**fetch を推奨する場合：**
- 追加のライブラリをインストールしたくない
- シンプルな通信のみで十分
- バンドルサイズを小さく保ちたい

**axios を推奨する場合：**
- 複雑な通信処理が多い
- エラーハンドリングを簡潔に書きたい
- インターセプターやリクエスト変換が必要

---

## 7. 実践的なパターン

### ローディング状態の管理

```javascript
async function fetchDataWithLoading() {
  const loadingElement = document.getElementById("loading");
  const contentElement = document.getElementById("content");

  try {
    // ローディング表示
    loadingElement.style.display = "block";
    contentElement.style.display = "none";

    const response = await fetch("/api/data");
    const data = await response.json();

    // データを表示
    contentElement.textContent = JSON.stringify(data);
    contentElement.style.display = "block";
  } catch (error) {
    contentElement.textContent = "エラーが発生しました";
  } finally {
    // ローディング非表示
    loadingElement.style.display = "none";
  }
}
```

### リトライ処理

```javascript
async function fetchWithRetry(url, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      const response = await fetch(url);
      if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`);
      return await response.json();
    } catch (error) {
      if (i === maxRetries - 1) throw error; // 最後の試行で失敗したらエラーを投げる

      // 待機時間を徐々に増やす（指数バックオフ）
      const delay = Math.pow(2, i) * 1000;
      console.log(`リトライ ${i + 1}/${maxRetries}（${delay}ms後）`);
      await new Promise(resolve => setTimeout(resolve, delay));
    }
  }
}
```

### キャッシュ機能

```javascript
const cache = new Map();

async function fetchWithCache(url, cacheTime = 60000) {
  // キャッシュをチェック
  if (cache.has(url)) {
    const { data, timestamp } = cache.get(url);
    if (Date.now() - timestamp < cacheTime) {
      console.log("キャッシュから取得");
      return data;
    }
  }

  // 新規取得
  console.log("サーバーから取得");
  const response = await fetch(url);
  const data = await response.json();

  // キャッシュに保存
  cache.set(url, { data, timestamp: Date.now() });

  return data;
}
```

### 複数APIの組み合わせ

```javascript
async function fetchUserWithPosts(userId) {
  try {
    // ユーザー情報を取得
    const userResponse = await fetch(`/api/users/${userId}`);
    const user = await userResponse.json();

    // ユーザーの投稿を取得
    const postsResponse = await fetch(`/api/users/${userId}/posts`);
    const posts = await postsResponse.json();

    // 結合して返す
    return {
      ...user,
      posts: posts
    };
  } catch (error) {
    console.error("データ取得エラー:", error);
    throw error;
  }
}
```

### デバウンス検索（入力中の無駄なリクエストを防ぐ）

```javascript
let searchTimeout = null;
let searchController = null;

function searchWithDebounce(query) {
  // 前回のタイマーをキャンセル
  clearTimeout(searchTimeout);

  // 前回のリクエストをキャンセル
  if (searchController) {
    searchController.abort();
  }

  // 新しいタイマーを設定（500ms後に実行）
  searchTimeout = setTimeout(async () => {
    searchController = new AbortController();

    try {
      const response = await fetch(`/api/search?q=${query}`, {
        signal: searchController.signal
      });
      const results = await response.json();
      displayResults(results);
    } catch (error) {
      if (error.name !== "AbortError") {
        console.error(error);
      }
    }
  }, 500);
}

// 使用例：入力イベント
document.getElementById("searchInput").addEventListener("input", (e) => {
  searchWithDebounce(e.target.value);
});
```

---

## まとめ

非同期処理の重要なポイント：

1. **Promise**: 非同期処理の基本的な仕組み
2. **async/await**: Promiseをより読みやすく書ける構文
3. **並列実行**: `Promise.all`で複数の処理を同時実行
4. **順次実行**: awaitを順番に使用
5. **エラーハンドリング**: try/catchでエラーを捕捉
6. **AbortController**: リクエストの中断とタイムアウト
7. **fetch vs axios**: 用途に応じて選択

非同期処理はモダンなWeb開発の核心です。これらのパターンを理解し、適切に使い分けることが重要です。

---

## 次のステップ

- [DOM操作とイベント処理編](../03-dom-events/README.md)へ進む
- [実践的なコード例](../../../examples/)を確認する
