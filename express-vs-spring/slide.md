class: center, middle

# express vs. Spring

---

## 自己紹介

* 高橋竜太
* Java(Script) プログラマ
* 今好きな API は Array#reduce

---

## express とは

* node.js で Web アプリケーションを作るためのフレームワーク
* REST API を簡単に作れる
* テンプレートエンジン (Pug, EJS, etc.) を使って SSR もできる

---

## Spring とは

* アプリケーションを作るためのモジュール群
* Web アプリケーションに限らない
* とにかく機能が豊富

---

## express の使い方

```sh
npm i -S express
```

```javascript
const express = require('express');
const app = express();
app.get('/', (req, res) => res.send('Hello World'));
app.listen(3000);
```

---

## Spring の使い方

https://start.spring.io/

にアクセスしてプロジェクトの雛形を作る

```sh
mvn install
mvn spring-boot:run
```

```java
// 一部抜粋 (import とかはもちろん必要)
@RestController
@SpringBootApplication
public class DemoApplication {
	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}

	@GetMapping("/")
	public String sample() {
		return "Hello World";
	}
}
```

---

### エンドポイントの作成方法

express
```javascript
app.get('/api/user', middleware);
```

Spring
```java
@GetMapping("/api/user")
public String user() {
  return service.run();
}
```

---

### パラメータの取得方法

express
```javascript
app.get('/api/user/:id', (req, res) => {
  const { id } = req.params;
});

app.post('/api/user', (req, res) => {
  const param = req.body;
})
```

Spring
```java
@GetMapping("/api/user/{id}")
public String fetchUser(@PathVariable long id) {}

@PostMapping("/api/user")
public String registrationUser(@RequestBody User user) {}
```

---

### バリデーション

express
```javascript
const { check, validationResult } = require('express-validator');
const validator = [
  check('id').isInt({ max: 5 }),
  (req, res, next) => {
    const errors = validationResult(req);
    if (errors.isEmpty()) return next();
    res.status(400).send('error');
  }
]
app.get('/api/user/:id', validator, service);
```

Spring
```java
@GetMapping("/api/user/{id}")
public String fetchUser(@PathVariable @Max(5) long id) {}

class User {
  @Max(5)
  private long id;
}
@PostMapping("/api/user")
public String registrationUser(@RequestBody @Valid User user) {}
```

---

### ロジックの書き方

express
```javascript
const service = (req, res) => {
  // いろいろ
}
app.get('/api/user/:id', validator, service);
```

Spring
```java
@Service
class UserService {
  // いろいろ
}

@Autowired
UserService service;

@GetMapping("/api/user/{id}")
public String fetchUser(@PathVariable @Max(5) long id) {
  return service.run(id);
}
```

---

### レスポンスの返し方

express
```javascript
app.get('/api/user/:id', (req, res) => {
  res.status(200).json({ name: 'takahashi' });
});
```

Spring
```java
@GetMapping("/api/user/{id}")
public ResponseEntity<User> fetchUser(@PathVariable @Max(5) long id) {
  User user = new User(id, "takahashi");
  return ResponseEntity.status(HttpStatus.OK).body(user);
}
```

---

### エラーハンドリング

express
```javascript
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ code: 'E001', message: "Something wrong" });
});
```

Spring
```java
@ControllerAdvice
class ErrorHandler {
  @ExceptionHandler({ RuntimeException.class })
  public ResponseEntity<Error> handleError() {
    Error error = new Error("E001", "Something wrong");
    return ResponseEntity
      .status(HttpStatus.INTERNAL_SERVER_ERROR)
      .body(error);
  }
}
```

---

## 開発ツール

### express
* IDE: VSCode
* test: mocha
* devtool: nodemon
* project, package: npm, yarn

### Spring
* IDE: VSCode, IntelliJ IDEA
* test: JUnit
* devtool: spring-boot-devtools
* project, pakcage: maven, gradle

---

## 感想

* express 単体だと色々足りない機能があるから自分で探してこないといけない
* Spring は逆に機能が豊富で使いこなすのが難しい
* node.js と Java という言語の比較で言うと Java の方が好き
  * 静的型付けがあると補完してくれるからそれだけで嬉しい
  * 冗長になるところもあるけど、可読性は Java の高いと思う
  * 実行時例外じゃなくてコンパイルエラーになるし