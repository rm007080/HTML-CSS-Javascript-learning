# JavaScript 実践コード例

各セクションで学んだ内容を実際に動かせるコード例です。

## 目次

1. [非同期処理の例](#1-非同期処理の例)
2. [Intersection Observer の例](#2-intersection-observer-の例)
3. [matchMedia の例](#3-matchmedia-の例)
4. [モジュールの例](#4-モジュールの例)
5. [実用的なパターン](#5-実用的なパターン)

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
