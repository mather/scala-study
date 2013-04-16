---
layout: default
title: Scala勉強会
subtitle: 文法編
---

[<< 導入編](scala-introduction.html)

以下ではREPLを使って文法を確認してみましょう。

### 変数・定数

変数宣言は `var`, `val` のキーワードで行います。

`val` (value)は再度代入することの出来ない変数を宣言します。

```scala
val num = 100
num = 200 // Error
```

一方、変数宣言 `var` (variable)は同じ変数に新しい値を代入できます。

```scala
var str = "hoge"
str = "fuga"
str = 100 // Error
```

Scalaでは関数型プログラミングとして `val` を使うことを推奨しています。
不用意な代入を避け可読性を高めたり、並列実行時の安全性が高まるためです。

`num` や `str` に対して型宣言をしていませんが、
入力されている値が整数型 `Int` や文字列型 `String` であるのは明らかなため、コンパイラが自動的に型を補います。
これは__型推論__と呼ばれる機能です。

変数の型を明示的に宣言する場合は、以下のようにコロン `:` で区切って後ろに置きます。
定義が長くて返り値が分かりにくかったり、返り値より上位の型で変数定義する場合は明示しましょう。

```scala
var str: String = "test"
```

### 関数

関数を定義してみましょう。

#### 関数宣言と型推論・省略

`def` キーワードで関数を定義します。

```scala
def twice(i: Int): Int = {
  return i * 2
}
```

`def 関数名(引数): 返り値の型 = { 定義 }` のように記述します。

返り値は`return`で指定できますが、指定されない場合は最後に評価された値が返り値です。
（Scalaでは `return` は多用しません）

変数の型推論と同様に、冗長な部分を省略できるようになっています。

```scala
def twice(i: Int): Int = { return i * 2 } // 一行にまとめる
def twice(i: Int) = { return i * 2 } // 返り値の型は明らかに Int
def twice(i: Int) = { i * 2 } // return で明示しなくても、最後の値が返り値とみなされる
def twice(i: Int) = i * 2   // 一つの式で完結する場合、括弧も不要
```

再帰的な関数の場合など、返り値の型を省略できない場合もありますが、様々な場面で省略可能です。

### 制御構造

#### if式 : 条件分岐

`if` は `Boolean` 型の値(`true`,`false`) を受け取り条件分岐します。

```scala
val c1 = false

if (c1) {
  "TRUE"
} else {
  "FALSE"
}
```

Scalaでは`if`は返り値を持ち、__式__に該当します。
例えば、以下のように返り値を使った関数宣言がかけます。

```scala
def nextEven(i: Int) = if (i % 2 == 0) i else (i+1)
nextEven(5) // => 6
nextEven(6) // => 6
```

#### while文 : 処理の繰り返し

`while` でループを記述できます。`while` は返り値を持ちません。

__【注】以下のコードはScalaでは推奨されません。varを使わない簡潔な書き方があります。__

```scala
var i = 0
while (i < 10) {
  println(i)
  i += 1
}
```

#### for式

`for` はScalaにおいてかなり汎用的な存在です。

例として、`while` と同じコードはこうなります。

```scala
for (i <- 0 until 10) {
  println(i)
}
```

これを、例えば簡単にフィルタリングしたり出来ます。

```scala
for (i <- 0 until 10; if i % 2 == 0) {
  println(i)
}
```

しかし、実際には繰り返しの用途で `for`式を使うことはあまり多くありません。
`for` は汎用的すぎて読みにくくなるため、もっとシンプルに意図を表現するメソッドを使うからです。

例えば全く同じことを一時変数も使わずに次のように書けます。

```scala
(0 until 10).withFilter(_ % 2 == 0).foreach(println)
```

`for`式の真価は関数型プログラミングにあるのですが、それらは後のほうで紹介します。

#### パターンマッチ

Scalaにおいて重要な制御構造が「パターンマッチ」です。
Javaの `switch` 文などに似ていますが、かなり強力な機能です。

```scala
def matchTest(str: String): String = str match {
  case "hoge"                    => "It's just hoge!"
  case s if s.startsWith("hoge") => "Prefix is hoge, but not hoge."
  case s                         => "What is " + s + "? I don't know..."
}
matchTest("hoge")
matchTest("hoge hoge")
matchTest("fuga")
```

どれかのパターンにマッチした場合、そこで処理を抜けるので `break` は書きません。
どの条件にもマッチしない場合は `MatchError` という例外を出します。

ただし、実際にコンパイルする場合は全てのパターンに対応しているか確認され、
コンパイラが（把握できる範囲で）警告を出してくれたりします。

上記の例では

- 具体的な値 `"hoge"`
- `if` を使った条件式
- ワイルドカード(どれにもマッチしなかった場合)

を指定しており、マッチした場合はそれを変数に格納して利用することもできます。
(値が必要ない場合はアンダースコア `"_"` を指定します。)

その他、パターンマッチでは

- 型
- ケースクラス
- コレクション
- タプル

などでマッチすることなどもできます。（詳しくは後ほど）

#### 例外

Scalaではあまり例外処理を記述しません。
チェック例外が強制力を持っていないためです。
（Javaにおける`throws`がそもそもなく、Java互換性のためにアノテーションになっています。）

ただ、例外のハンドリングが必要な箇所では、 `try`, `catch`, `finally` を用いた例外処理ができます。
Javaと異なるのは、 `catch` に例外の型を使ったパターンマッチを使用する点です。

```scala
try {
  throw new Exception("Some errors occured.")
} catch {
  case e: RuntimeException => throw e    // Runtimeなら全て上位へ投げる
  case e: Exception => e.printStackTrace // それ以外のExceptionを表示
  // case e: Throwable => ???
} finally {
  // close, etc...
}
```

これらは、制御構造をユーザが定義することで隠蔽できます。（詳しくは後ほど）

[>> 型紹介編](scala-types.html)
