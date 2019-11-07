class: center, middle

# Started TypeScript. Vol 1

---

## 自己紹介

* 高橋竜太
* Java(Script) プログラマ
* JSConf チケット買いました

---

## JavaScript の開発でツラいこと

* 引数、戻り値の型が間違えていて実行時にエラーが出る
* IDE で補完されない
* リファクタリングつらい

---

## そんな悩みを解決してくれるのが...

---

## TypeScript とは

* Microsoft が開発しているオープンソース
* JavaScript に静的型付けを提供してくれるスーパーセット
* 大規模アプリ向けのツール

---

## TypeScript を導入すると

* 引数、戻り値の型が間違えていて実行時にエラーが出る
  * ビルド時または、IDE 上でチェックしてくれる
* IDE で補完されない
  * 補完してくれるので typo が減るし、生産性向上
* リファクタリングつらい
  * 参照箇所を自動で修正してくれる

---

## それでは皆さん一緒に始めましょう

---

## その前に

* 注意
  * node をインストールしておいてください
  * IDE は VSCode を使ってください

---

## 始め方

* TypeScript をインストール

```sh
npm init -y
npm i typescript -D
```

* index.ts を作成します

```javascript
interface Product {
  price: number;
};

function discount(product: Product) : number {
  return product.price * 0.8;
}

console.log(discount({1000}));
```

---

## 始め方 つづき

* コンパイル
  * package.json の scripts に以下を追加し、実行します

```json
"build": "tsc index.ts"
```

```sh
npm run build
```

  * index.ts がコンパイルされ index.js が出来上がります

* 実行

```sh
node index.js
```

  * 800 と実行結果が表示されれば OK

---

## 読み方

```javascript
interface Product {
  price: number;
};

function discount(product: Product) : number {
  return product.price * 0.8;
}
```

```javascript
interface 型名 {
  項目名: 型;
};

function メソッド名(引数名: 引数の型) : 戻り値の型 {
  return product.price * 0.8;
}
```

---

## コンパイル

* tsc -> TypeScript Compiler
* tsc --init で tsconfig.json が作成されます
  * コンパイルオプションを指定するためのファイル
  * 詳細は次回以降で、、、
  * とりあえずはデフォルトで、あとは以下を追加すると便利かと思います
  
```json
"outDir": "./dist",
"rootDir": "./src", 
```

---

## プリミティブ型

* boolean
* number
* string
* Array
* Tuple
* Enum
* Any
* Void
* null, undefined
* Never
* Object

* 公式のドキュメントに書いてあるのは以上だが、JavaScript のスーパーセットということなので以下も利用可能
  * Date
  * Map
  * Set

---

## クラス

* ES2016 で利用可能になった Class を使ってオブジェクト指向のプログラミングができます

```javascript
class Product {
  price: number;

  constructor(price: number) {
    this.price = price;
  }

  discount(): number {
    return this.price * 0.8;
  }
}
```

* 変数、メソッドのスコープはデフォルト public です
  * その他指定できるスコープは private, protected です
  * protected 以外は Java と同じです
  * protected はそのクラスと、継承したクラス内で参照可能です

---

## ライブラリ

* 有名なライブラリであれば、ほぼ TypeScript をサポートしていると思います
* サポートしていない (型定義がない) 場合、型定義を集めたリポジトリを使えば大丈夫です
  * https://github.com/DefinitelyTyped/DefinitelyTyped
  * t を押してライブラリ名で検索してみてください
  * 見つかったら、npm i @types/ライブラリ名 -D
  * node.js 用の型定義もあります

* サポートしていないとどうなるのか
  * import するとコンパイルエラーになります
  * require すればエラーは解消できますが、定義は全て Any になります

---

## 今回はここまで

* Java とかオブジェクト指向のプログラミングしたことある人には取っ付きやすいのでは
  * じゃあもうサーバサイドは Java にしようよと思わなくもないが、、、
* CoffeeScript みたいにオワコンになったらどうしよう

---

## 次回

* React 編

---