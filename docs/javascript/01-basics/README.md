# JavaScript 基礎編

モダンなJavaScript（ES2015/ES6以降）の基本構文を学びます。

## 目次

1. [変数宣言（let と const）](#1-変数宣言)
2. [アロー関数](#2-アロー関数)
3. [テンプレートリテラル](#3-テンプレートリテラル)
4. [分割代入](#4-分割代入)
5. [スプレッド演算子](#5-スプレッド演算子)
6. [クラス](#6-クラス)
7. [for...of ループ](#7-forof-ループ)

---

## 1. 変数宣言

### 従来の問題点（var）

ES5以前では、変数は`var`で宣言していましたが、以下のような問題がありました：

- スコープが関数単位で、ブロックスコープがない
- 意図しない変数の上書きが発生しやすい
- 巻き上げ（hoisting）による予期しない動作

### let と const の登場

ES2015で導入された`let`と`const`は、これらの問題を解決します。

#### const（推奨）

**再代入が不要な変数**は`const`で宣言します。

```javascript
const name = "太郎";
const age = 25;
const PI = 3.14159;

// エラー：再代入できない
// name = "花子";
```

**特徴：**
- 再代入不可
- ブロックスコープ
- 宣言時に初期化が必須

**重要な注意点：** `const`で宣言したオブジェクトや配列は、中身の変更は可能です。

```javascript
const user = { name: "太郎" };
user.name = "花子"; // OK：プロパティの変更は可能
user.age = 25;      // OK：新しいプロパティの追加も可能

// エラー：オブジェクト自体の再代入は不可
// user = { name: "次郎" };
```

#### let

**再代入が必要な変数**のみ`let`で宣言します。

```javascript
let count = 0;
count = 1;  // OK：再代入可能
count++;    // OK

let message;
message = "こんにちは"; // OK：後から代入可能
```

**特徴：**
- 再代入可能
- ブロックスコープ
- 宣言後に初期化可能

#### ブロックスコープの利点

```javascript
// ブロックスコープの例
if (true) {
  const x = 10;
  let y = 20;
  console.log(x, y); // 10 20
}
// console.log(x); // エラー：xはブロック外では参照できない
// console.log(y); // エラー：yもブロック外では参照できない
```

### ベストプラクティス

1. **デフォルトは`const`を使用**
2. **再代入が必要な場合のみ`let`を使用**
3. **`var`は使用しない**（レガシーコードの理解のみ）

---

## 2. アロー関数

アロー関数は、従来の`function`キーワードを使った関数宣言をより簡潔に書ける構文です。

### 基本構文

```javascript
// 従来の関数
function add(a, b) {
  return a + b;
}

// アロー関数
const add = (a, b) => {
  return a + b;
};

// さらに簡潔に（単一式の場合、returnとブロック{}を省略可能）
const add = (a, b) => a + b;
```

### 引数が1つの場合

引数が1つの場合、括弧を省略できます。

```javascript
// 引数が1つ
const double = n => n * 2;

// 引数なし（括弧が必要）
const greet = () => "こんにちは";

// 引数が複数（括弧が必要）
const multiply = (a, b) => a * b;
```

### オブジェクトを返す場合

オブジェクトリテラルを返す場合は、括弧で囲む必要があります。

```javascript
// エラーになる書き方（ブロックと解釈される）
// const createUser = name => { name: name };

// 正しい書き方
const createUser = name => ({ name: name });

// またはこう書く
const createUser = name => {
  return { name: name };
};
```

### this の扱いが異なる（重要）

アロー関数の最大の特徴は、**`this`が定義された場所のスコープを継承する**ことです。

#### 従来の関数の問題

```javascript
const person = {
  name: "太郎",
  hobbies: ["読書", "映画", "料理"],

  printHobbies: function() {
    this.hobbies.forEach(function(hobby) {
      // ここでのthisはpersonを指さない！
      console.log(this.name + "の趣味：" + hobby);
    });
  }
};

person.printHobbies();
// "undefinedの趣味：読書"（エラーではないが意図しない動作）
```

#### アロー関数で解決

```javascript
const person = {
  name: "太郎",
  hobbies: ["読書", "映画", "料理"],

  printHobbies: function() {
    this.hobbies.forEach(hobby => {
      // アロー関数はthisを外側のスコープから継承
      console.log(this.name + "の趣味：" + hobby);
    });
  }
};

person.printHobbies();
// "太郎の趣味：読書"
// "太郎の趣味：映画"
// "太郎の趣味：料理"
```

### アロー関数を使うべきでない場合

オブジェクトのメソッドとして定義する場合、アロー関数は適していません。

```javascript
// ❌ 避けるべき
const person = {
  name: "太郎",
  greet: () => {
    console.log("こんにちは、" + this.name);
    // thisはpersonを指さない
  }
};

// ✅ 推奨
const person = {
  name: "太郎",
  greet() {
    console.log("こんにちは、" + this.name);
  }
};
```

---

## 3. テンプレートリテラル

テンプレートリテラルは、バッククォート（`` ` ``）を使った文字列表現で、変数の埋め込みや複数行の文字列を簡単に扱えます。

### 変数の埋め込み

```javascript
// 従来の方法（文字列連結）
const name = "太郎";
const age = 25;
const message = "私の名前は" + name + "で、" + age + "歳です。";

// テンプレートリテラル
const message = `私の名前は${name}で、${age}歳です。`;
```

### 式の評価

`${}`の中には、式も書けます。

```javascript
const a = 10;
const b = 20;
console.log(`合計は${a + b}です`); // "合計は30です"

const price = 1000;
const tax = 0.1;
console.log(`税込価格：${price * (1 + tax)}円`); // "税込価格：1100円"
```

### 複数行の文字列

従来は`\n`で改行を表現していましたが、テンプレートリテラルでは直接改行できます。

```javascript
// 従来の方法
const message = "1行目\n2行目\n3行目";

// テンプレートリテラル
const message = `1行目
2行目
3行目`;

// HTML文字列の作成に便利
const html = `
  <div class="card">
    <h2>${title}</h2>
    <p>${description}</p>
  </div>
`;
```

### ネストも可能

```javascript
const users = ["太郎", "花子", "次郎"];
const html = `
  <ul>
    ${users.map(user => `<li>${user}</li>`).join('')}
  </ul>
`;
```

---

## 4. 分割代入

分割代入（Destructuring）は、配列やオブジェクトから値を取り出して、個別の変数に代入する構文です。

### 配列の分割代入

```javascript
// 従来の方法
const colors = ["red", "green", "blue"];
const first = colors[0];
const second = colors[1];

// 分割代入
const [first, second, third] = ["red", "green", "blue"];
console.log(first);  // "red"
console.log(second); // "green"
console.log(third);  // "blue"
```

#### 一部の要素をスキップ

```javascript
const colors = ["red", "green", "blue", "yellow"];
const [first, , third] = colors; // 2番目をスキップ
console.log(first); // "red"
console.log(third); // "blue"
```

#### デフォルト値の設定

```javascript
const [a, b, c = "default"] = ["x", "y"];
console.log(a); // "x"
console.log(b); // "y"
console.log(c); // "default"（値がないのでデフォルト値）
```

#### 残りの要素をまとめて取得

```javascript
const numbers = [1, 2, 3, 4, 5];
const [first, second, ...rest] = numbers;
console.log(first);  // 1
console.log(second); // 2
console.log(rest);   // [3, 4, 5]
```

### オブジェクトの分割代入

```javascript
// 従来の方法
const person = {
  name: "太郎",
  age: 25,
  country: "Japan"
};
const name = person.name;
const age = person.age;

// 分割代入
const { name, age, country } = person;
console.log(name);    // "太郎"
console.log(age);     // 25
console.log(country); // "Japan"
```

#### プロパティ名と異なる変数名を使う

```javascript
const person = { name: "太郎", age: 25 };

// nameプロパティをpersonNameという変数名で受け取る
const { name: personName, age: personAge } = person;
console.log(personName); // "太郎"
console.log(personAge);  // 25
```

#### デフォルト値の設定

```javascript
const person = { name: "太郎" };
const { name, age = 20, country = "Japan" } = person;
console.log(name);    // "太郎"
console.log(age);     // 20（デフォルト値）
console.log(country); // "Japan"（デフォルト値）
```

### 関数の引数での分割代入

関数の引数で分割代入を使うと、引数の順序を気にせずに済みます。

```javascript
// 従来の方法（引数の順序が重要）
function createUser(name, age, country) {
  console.log(name, age, country);
}
createUser("太郎", 25, "Japan");

// オブジェクトの分割代入を使用（順序不問）
function createUser({ name, age, country }) {
  console.log(name, age, country);
}
createUser({ country: "Japan", name: "太郎", age: 25 }); // 順序が違っても動作

// デフォルト値も設定可能
function createUser({ name, age = 20, country = "Japan" }) {
  console.log(name, age, country);
}
createUser({ name: "太郎" }); // 太郎 20 Japan
```

---

## 5. スプレッド演算子

スプレッド演算子（`...`）は、配列やオブジェクトを展開する演算子です。

### 配列の展開

```javascript
const numbers = [1, 2, 3];
console.log(...numbers); // 1 2 3

// 関数の引数として展開
const max = Math.max(...numbers);
console.log(max); // 3

// 従来の方法（applyを使用）
const max = Math.max.apply(null, numbers);
```

### 配列のコピー

```javascript
const original = [1, 2, 3];
const copy = [...original];

copy.push(4);
console.log(original); // [1, 2, 3] - 元の配列は変更されない
console.log(copy);     // [1, 2, 3, 4]
```

### 配列の結合

```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

// スプレッド演算子
const combined = [...arr1, ...arr2];
console.log(combined); // [1, 2, 3, 4, 5, 6]

// 従来の方法
const combined = arr1.concat(arr2);
```

### 配列の途中に要素を挿入

```javascript
const numbers = [1, 2, 5, 6];
const inserted = [1, 2, 3, 4, 5, 6];

// スプレッド演算子を使用
const result = [...numbers.slice(0, 2), 3, 4, ...numbers.slice(2)];
console.log(result); // [1, 2, 3, 4, 5, 6]
```

### オブジェクトの展開（ES2018以降）

```javascript
const person = { name: "太郎", age: 25 };
const copy = { ...person };

// プロパティの追加
const personWithCountry = { ...person, country: "Japan" };
console.log(personWithCountry);
// { name: "太郎", age: 25, country: "Japan" }

// プロパティの上書き
const updatedPerson = { ...person, age: 26 };
console.log(updatedPerson);
// { name: "太郎", age: 26 }
```

### オブジェクトのマージ

```javascript
const defaults = { theme: "light", language: "ja" };
const userSettings = { language: "en", fontSize: 14 };

const settings = { ...defaults, ...userSettings };
console.log(settings);
// { theme: "light", language: "en", fontSize: 14 }
// 後に指定したものが優先される
```

### 残余引数（Rest Parameters）

関数の引数で使うと、残りの引数をまとめて配列として受け取れます。

```javascript
function sum(...numbers) {
  return numbers.reduce((total, n) => total + n, 0);
}

console.log(sum(1, 2, 3));       // 6
console.log(sum(1, 2, 3, 4, 5)); // 15

// 従来の方法（argumentsオブジェクト）
function sum() {
  const numbers = Array.from(arguments);
  return numbers.reduce((total, n) => total + n, 0);
}
```

#### 他の引数と組み合わせ

```javascript
function introduce(greeting, ...names) {
  return `${greeting}、${names.join("と")}さん`;
}

console.log(introduce("こんにちは", "太郎"));
// "こんにちは、太郎さん"

console.log(introduce("こんにちは", "太郎", "花子", "次郎"));
// "こんにちは、太郎と花子と次郎さん"
```

---

## 6. クラス

ES2015で導入されたクラス構文は、プロトタイプベースの継承をより直感的に書けるようにしたものです。

### 基本的なクラスの定義

```javascript
class Person {
  // コンストラクタ（初期化メソッド）
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  // メソッド
  greet() {
    console.log(`こんにちは、${this.name}です。`);
  }

  introduce() {
    console.log(`私は${this.age}歳の${this.name}です。`);
  }
}

// インスタンスの作成
const person = new Person("太郎", 25);
person.greet();      // "こんにちは、太郎です。"
person.introduce();  // "私は25歳の太郎です。"
```

### 継承（extends）

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name}が鳴いています。`);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // 親クラスのコンストラクタを呼び出す
    this.breed = breed;
  }

  // メソッドのオーバーライド
  speak() {
    console.log(`${this.name}がワンワンと鳴いています。`);
  }

  // 新しいメソッド
  fetch() {
    console.log(`${this.name}がボールを取ってきました。`);
  }
}

const dog = new Dog("ポチ", "柴犬");
dog.speak();  // "ポチがワンワンと鳴いています。"
dog.fetch();  // "ポチがボールを取ってきました。"
```

### ゲッターとセッター

```javascript
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  // ゲッター
  get area() {
    return this.width * this.height;
  }

  // セッター
  set area(value) {
    // 正方形として設定
    this.width = Math.sqrt(value);
    this.height = Math.sqrt(value);
  }
}

const rect = new Rectangle(10, 20);
console.log(rect.area); // 200（メソッドではなくプロパティとして取得）

rect.area = 100; // セッターが呼ばれる
console.log(rect.width);  // 10
console.log(rect.height); // 10
```

### 静的メソッド

```javascript
class MathUtil {
  static add(a, b) {
    return a + b;
  }

  static multiply(a, b) {
    return a * b;
  }
}

// インスタンスを作成せずに使用
console.log(MathUtil.add(5, 3));      // 8
console.log(MathUtil.multiply(4, 7)); // 28
```

### プライベートフィールド（ES2022）

```javascript
class BankAccount {
  #balance = 0; // プライベートフィールド（#で始まる）

  constructor(initialBalance) {
    this.#balance = initialBalance;
  }

  deposit(amount) {
    this.#balance += amount;
  }

  withdraw(amount) {
    if (amount <= this.#balance) {
      this.#balance -= amount;
      return true;
    }
    return false;
  }

  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount(1000);
account.deposit(500);
console.log(account.getBalance()); // 1500

// console.log(account.#balance); // エラー：外部からアクセス不可
```

---

## 7. for...of ループ

`for...of`ループは、配列などのイテラブルオブジェクトの**値**を順に取り出すループです。

### 基本構文

```javascript
const colors = ["red", "green", "blue"];

// for...of（値を取得）
for (const color of colors) {
  console.log(color);
}
// "red"
// "green"
// "blue"

// 従来のforループ
for (let i = 0; i < colors.length; i++) {
  console.log(colors[i]);
}
```

### for...in との違い

`for...in`は**キー（インデックス）**を取得し、`for...of`は**値**を取得します。

```javascript
const colors = ["red", "green", "blue"];

// for...in（インデックスを取得）
for (const index in colors) {
  console.log(index, colors[index]);
}
// "0" "red"
// "1" "green"
// "2" "blue"

// for...of（値を取得）
for (const color of colors) {
  console.log(color);
}
// "red"
// "green"
// "blue"
```

**注意：** `for...in`はオブジェクトのプロパティを列挙するためのもので、配列には`for...of`を使うのが推奨されます。

### 文字列のループ

```javascript
const text = "Hello";

for (const char of text) {
  console.log(char);
}
// "H"
// "e"
// "l"
// "l"
// "o"
```

### Map オブジェクトのループ

```javascript
const userRoles = new Map([
  ["太郎", "admin"],
  ["花子", "editor"],
  ["次郎", "viewer"]
]);

for (const [name, role] of userRoles) {
  console.log(`${name}: ${role}`);
}
// "太郎: admin"
// "花子: editor"
// "次郎: viewer"
```

### Set オブジェクトのループ

```javascript
const uniqueNumbers = new Set([1, 2, 3, 4, 5]);

for (const num of uniqueNumbers) {
  console.log(num);
}
// 1, 2, 3, 4, 5
```

### インデックスも必要な場合

`entries()`メソッドを使用します。

```javascript
const colors = ["red", "green", "blue"];

for (const [index, color] of colors.entries()) {
  console.log(`${index}: ${color}`);
}
// "0: red"
// "1: green"
// "2: blue"
```

---

## まとめ

このセクションで学んだモダンJavaScriptの基本構文：

1. **変数宣言**: `const`を基本とし、再代入が必要な場合のみ`let`を使用
2. **アロー関数**: 簡潔な関数定義と`this`の扱い
3. **テンプレートリテラル**: 変数埋め込みと複数行文字列
4. **分割代入**: 配列・オブジェクトからの値の取り出し
5. **スプレッド演算子**: 配列・オブジェクトの展開とコピー
6. **クラス**: オブジェクト指向プログラミングの基礎
7. **for...of**: イテラブルオブジェクトのループ

これらの構文は現代のJavaScript開発における標準となっており、確実に習得することが重要です。

---

## 次のステップ

- [非同期処理編](../02-async/README.md)へ進む
- [実践的なコード例](../../../examples/)を確認する
