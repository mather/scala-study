---
layout: default
title: Scala勉強会
subtitle: Hello,world!
---

### Hello,world!

コード

```scala
/* Hello.scala */
object Hello {
  def main(args: Array[String]) {
    println("Hello,world!")
  }
}
```

実行

```
$ sbt run
```

結果

```
Hello,world!
```

#### 解説

<dl>
  <dt>`println`</dt>
  <dd>コンソール出力用の汎用関数。文字列でなくても、`toString`を呼び出して返す。</dd>
  <dt>`main`関数</dt>
  <dd>プログラムがここから実行される。引数は `Array[String]` 型。`object` に定義する。</dd>
  <dt>`object Hello`</dt>
  <dd>`Hello` シングルトンオブジェクト。実行時に自動的にインスタンス化されます。</dd>
<dl>

### クラスを定義してみる

```scala
/* Hello.scala */
object Hello {
  def main(args: Array[String]) {
    println(new Hello().greet)
  }
}

class Hello {
  def greet = "Hello,world!"
}
```
