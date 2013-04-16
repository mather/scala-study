---
layout: default
title: Scala勉強会
subtitle: 型紹介編
---

[<< 文法編](scala-grammer.html)

ここではScalaに標準で備わっている型を幾つか紹介します。

具体的なリファレンスは[Scala API(最新版)](http://www.scala-lang.org/api/current/index.html)を参照してください。

### 数値

数値もオブジェクトです。 `toString`, `toInt` などのメソッドが直接使えます。

```scala
val b: Byte   = 0
val c: Char   = 'c' // シングルクォート
val s: Short  = 1
val i: Int    = 1
val l: Long   = 2L
val f: Float  = 3.4F
val d: Double = 5.6
```

また、演算子はなく、`+`などもメソッドです。次のように入力できることから演算子でないことがわかります。

```scala
(1).+(2)
```

### 文字列

文字列の型は `String` で、中身は `java.lang.String` と同じものです。

文字列リテラルは2種類あります。エスケープを評価する通常の「文字列リテラル」と、エスケープをそのまま残す「整形済みリテラル」です。

```scala
"hoge\nfuga"     // "\n"が改行(LF)になる
"""hoge\nfuga""" // "\n"として残る
```

整形済みリテラルは正規表現を記述するときや、ヒアドキュメントっぽく複数行の文字を生成するときに便利です。

複数行にまたがる場合は先頭に `"|"` を入れておいてマージンを用意しておくと、`stripMargin`メソッドで取り除けます。(縦棒以外でもできますが)

```scala
"""|This is
   |a long long
   |string.""".stripMargin
```

Scala 2.10.0 からは`s`や`f`を先頭につけることで変数埋め込みが可能になりました。

```scala
val num = 20
println(s"number: ${num}")
println(f"number: ${num}%03d") // printf format
```

### 排列

Scalaは排列やコレクションの種類や操作が豊富です。この時点ではあまり紹介しきれない機能が多いため、はじめは `Array` のみ紹介します。

`Array` はJavaの配列と互換です。初期化はより簡単になっています。

```scala
val array = Array("foo", "bar")
array(0) //=> "foo"
array(0) = "FOO" // 要素を変更可能
```

Scalaでは要素の参照は丸括弧 `()` で行います。これは、実はメソッドです。

オブジェクトに直接 `()` を指定した場合は自動的に `apply` メソッドを適用し、
`()=` で要素を更新する場合は `update` メソッドが呼ばれます。

つまり、上の操作はこのように呼ばれていることになります。初期化の時にも使っているのがポイントです。

```scala
val array = Array.apply("foo", "bar")
array.apply(0) //=> "foo"
array.update(0, "FOO")
```

他のコレクションは以下のように生成できます。以下に指定したものは全て不変(immutable)です。
パッケージをimportすれば可変(mutable)なコレクションも利用できます。

```scala
// 順序付きリスト
val list: List[String] = List("hoge", "fuga")

// 連想配列
val map: Map[String,Int] = Map("one" -> 1, "two" -> 2)

// 一意集合
val set: Set[Int] = Set(1,2,3,4,4,5) // => Set(1,2,3,4,5)
```

### 等価性について

Scalaの等式 `"=="` は Java より柔軟です。
以下の例はすべて `true` を返します。

```scala
"test" == new String("test")
List(1,2) == List(2,1).reverse
Map(1->List(2,3)) == Map(1->Seq(2,3))
```

文字列はもちろんのこと、ネストしたコレクションにも対応し、
果ては型が違っていてもコレクションの特性(順序を保つリスト)が同じであれば一致とみなします。

逆にオブジェクトの一致を確認したい場合は `eq` を使います。

```scala
val str = "test"
str.eq(str) // => true
str.eq(str + "") // => false
```

### その他の型

いくつか特徴的な型を紹介します。

#### Unit

Unitはいわゆるvoidの型です。ただひとつのインスタンスは `()` です。

#### Any, AnyRef, AnyVal

- `Any` : 全てのオブジェクトの上位クラスです。
- `AnyRef` : `java.lang.Object` の別名です。 `String` や `List` などの上位クラスです。
- `AnyVal` : 数値型や `Unit` の上位クラスです。

[Scalaの型の上下関係](http://www.scala-lang.org/node/71?size=_original)

#### 正規表現

"scala.util.matching.Regex" が正規表現を表すクラスです。
実際にはこの型をimportする必要はなく、以下のように文字列から `r` メソッドで生成できます。

```scala
val regex = """\w+\d*""".r
```

例えば、正規表現にマッチする文字列を順に処理するには以下のように行います。

```scala
for (elem <- regex.findAllIn("abc,abc012") ) {
  println(elem)
}  // => "abc", "abc012"
```

状態を保持しつつループで回すより、単純でわかりやすいと思います。

#### XML(余裕があったら紹介)

ScalaはXMLを表現するリテラルがあります。

```scala
val xml =
<root>
  <operator>
    <name>hoge user</name>
  </operator>
</root>
```

XMLそのままです。ただし、要素は変更不可なので、DOMのように要素を追加したりはできません。

要素の検索は以下のように行います。

```scala
xml \ "operator"
xml \ "operator" \ "name" // 指定のパスで検索
xml \\ "name" // すべてのパスで検索
```

[>> オブジェクト指向編](scala-objective.html)
