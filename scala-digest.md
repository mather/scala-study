---
layout: default
title: Scala勉強会
subtitle: こんな言語
---

### Scalaってこんな言語

Scalaでどんなコードを書けるのか、いくつか紹介します。

### Javaとの互換性

ScalaはJavaとの互換性を重視し、シームレスにJavaのコードを利用できます。
（いくらかJavaより省略できます。）

```scala
// Scalaコードです。
import java.util.Random
val random = new Random
random.nextInt
```

一方で、Java側からもScalaで作ったライブラリを使用することが出来ます。

### 徹底したオブジェクト指向

Scalaでは(Javaではプリミティブ型と呼ばれていたものも含めて)__全ての値がオブジェクト__です。

- `Int` : Javaの `int` 型
- `Array[T]` : Javaの配列

そのため、オブジェクトに対してメソッドを呼び出す形式(`instance.method(args)`)で首尾一貫した方法になります。

加えて、インスタンス（具象オブジェクト）に属さない static な値やメソッドは認めていません。
代わりに、宣言すると自動的にインスタンス化される__シングルトンオブジェクト__を使用します。

### 新しい制御構造を作れる

組み込みで指定される `if` や `while` などの制御構造や、 `+`, `!` の演算子をプログラマが指定できない言語は多いですが、
Scalaではプログラマの手で実装できます。

例えば、以下のように `Reader` を自動クローズする関数を定義できます。
（Java7ではtry with resourcesのようなもの）

```scala
import java.io._

// ファイルを開いて行ごとの処理を実行する
def eachLine(file: File)(f: String => Unit) {
  val reader = new BufferedReader(new FileReader(file))
  try {
    var line = reader.readLine
    while(line != null) {
      f(line)               // <- ここで f を適用
      line = reader.readLine
    }
  } catch {
    case e: IOException => /* do something */
  } finally {
    try { reader.close } catch { case e: IOException => /* ignore */ }
  }
}

// 使用方法（行の長さを出力）
eachLine(new File("filename")) { line =>
  println(line.length)
} // 自動的にクローズされる
```

その他、並列処理を実行出来るActorライブラリではActor(軽量スレッド)へメッセージを投げることを `!` や `?` で表現できます。

```scala
actor ! "Some message"
```

### ドキュメント性




### 強力な静的型付け言語

Scalaは強力な静的型付け言語です。

抽象的な安全な型を使うことでより強力な安全性を担保することが出来ます。

例えば、Javaでは以下の `null` チェックをコンパイラが警告してくれません。

```java
// Javaコードです。
Map<String,String> m = new HashMap<String,String>();
String v = "";
...
v = m.get("key");
if(v != null) { // コンパイラはnullをチェックしたかは検出できない
  ...
} else {
  ...
}

// またはcontainsで事前チェック => 並列処理時にはsynchronizedが必要
if(m.contains("key")) {
  v = m.get("key")
  ...
} else {
  ...
}
```

これに対して、Scalaでは「結果値があるかどうか」を`Option`型で表現します。
`Option`型には値があることを表す `Some` 型と、値がないことを示す `None` 型があります。
これはデータ本体を表すわけではなく、「データの有無」という抽象的な意味を持つ型です。

```scala
val m = Map("key"->"value")
m.get("key")   //=> Some("value")
m.get("other") //=> None
```

`Some` クラスに包まれることで値にアクセスしにくくなったように見えますが、
実際には以下のようにパターンマッチをすることで簡単に取り出すことができます。

```scala
m.get("key") match {
  case Some(s) => "Found: " + s
  case None    => "Not found"
}
```

ライブラリを使用するユーザの視点では、
例えば `DB.getUserInfo(userName: String)` メソッドの返り値が `Option[User]` 型であれば
「値を返さない可能性がある」ことが一目瞭然となり、細かいドキュメントなしでも読み取れるようになります。

`Option`型にはこの二つのサブクラスしか取り得ないことを明確に定義しているため、
以下のようにパターンマッチを忘れた場合は、コンパイラが警告を出すようになります。

```
scala> m.get("key") match {
     |   case Some(s) => "Found: " + s
     | }
<console>:9: warning: match may not be exhaustive. // match が完全ではないようです
It would fail on the following input: None         // None が来たときに失敗するでしょう
              m.get("key") match {
                   ^
```

このような型チェック機能はユーザが自分で簡単に作成できるため、より間違いのない設計と実装が可能になります。


### スクリプト言語並に書きやすい

上記のようにデータや状態を表すクラスとは別に多くの「抽象的な型」を定義して安全なプログラミングを行うことができるScalaですが、
かと言ってガチガチに型宣言を強制するかというと、そういうわけではありません。
多くの場合は与えられたリテラルや関数の返り値によって型が分かるため、型推論の機能によって記述不要です。

```scala
// 変数定義
val num = 100 // Int 型
val list = List(List(1,2), List(3,4)) // List[List[Int]] 型
val bytes = "TEST".getBytes // Array[Byte] 型

// 関数定義
def add(x: Int, y: Int) = x + y  // Int型
```

これによってロジック上の冗長な部分を大幅に省略でき、「何を処理しているか」を明確にすることが出来ます。

### Composable(合成可能)なオブジェクト指向

Scalaにはクラス(`class`)、抽象クラス(`abstract class`)に加えて、トレイト(`trait`)があります。違いは以下のとおりです。

<dl>
  <dt>クラス</dt>
  <dd>定義されたメンバー（変数や関数）は全て実装されている。</dd>
  <dt>抽象クラス</dt>
  <dd>
    定義されたメンバーのうち、未実装（抽象）のものがあり、そのままではインスタンス化出来ない。
    複数の抽象クラスは同時に<strong>継承(inherit)</strong>できない。
  </dd>
  <dt>トレイト</dt>
  <dd>
    定義されたメンバーのうち、未実装（抽象）のものがあり、そのままではインスタンス化出来ない。
    複数のトレイトを同時に<strong>合成(compose, mix-in)</strong>できる。
  </dd>
</dl>

トレイトはJavaのインターフェース(`interface`)とは違い、実装を持てるのが特徴です。


```scala
trait HogeTrait {
  def s: String   // 抽象メンバー
  // s を使った実装
  def say = println(s)
  def shout = println(s.toUpperCase)
}

// トレイトを合成(mix-in)
class HogeClass(str: String) extends HogeTrait {
  val s = str // 具体的な値を入れる
}

// HogeTraitの実装を利用可能
val hoge = new HogeClass("hoge")
hoge.say   //=> hoge
hoge.shout //=> HOGE
```
