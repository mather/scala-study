---
layout: default
title: Scala勉強会
subtitle: 関数型言語
---

### 関数型言語としての特徴

#### 関数も値

関数もひとつの値です。関数を引数に渡したり、関数を返す関数を作成出来ます。

関数リテラル

```scala
val succ = (i: Int) => i + 1
```

関数引数

```scala
List("abc", "test", "") map (_.length)
```

関数返り値

```scala
val max = (x: Int) => (y: Int) => if (x > y) x else y
val adjustPositive = max(1)
adjustPositive(5) // => 5
adjustPositive(-1) // => 1
```

#### 副作用のない関数を徹底しよう

「副作用がない」とは関数の入力値を直接変更するのではなく、新しい結果値として返し、プログラムの状態に影響を与えないことを言います。
例えば配列の値を直接書き換える操作などは、同じ変数を扱う処理にいつどこでどう変わるのかトレースする必要があり混乱を与えます。

副作用のある関数

```scala
val list = Array(0,1,2,3,4)
var i = 0
while (i < list.length) {
  println("filename" + i.toString)
  i += 1
}
```

- `i` が変化している
- `println`を逐次出力している（返り値を返さない）

副作用を寄せるとこうなる

```scala
val list = List(0,1,2,3,4)
val fileList = list.map("filename" + _.toString)
val filenameJoined = fileList.mkString("\n")

assert(filenameJoined.contains("filename2"))
println(filenameJoined)
```

- 一時変数を使わず要素を文字列化する関数だけを渡す
- `println` は最後に一回だけなので、その処理結果に間違いがないか確認できる。



#### List : 順序を保つコレクション

`List` の宣言方法は2つあり、以下の2つは同じリストを生成します。ここで、 `Nil` は空リスト `List()` と同じです。

```scala
val list1 = List("hoge", "fuga")
val list2 = "hoge" :: "fuga" :: Nil
list1 == list2 // => true
```

2つめの方法に注目します。空リストの前に要素をくっつけることでリストを生成しています。つまり、要素を前に追加するにはこうすれば良いわけです。

```scala
val list3 = "foo" :: list2
```

またこの書式はパターンマッチに使えます。

```scala
val listMatch = (_: List[String]) match {
  case Nil           => "empty"
  case x :: Nil      => "single element"
  case x :: y :: Nil => "double elements"
  case x :: xs       => "x: " + x.toString + ", xs: " + xs.toString
}
listMatch(list2) // => "double elements"
listMatch(list3) // => "x: foo, xs: List(hoge, fuga)"
```

#### Map : 連想配列

`Map` は連想配列(key-value mapping)です。初期化する際はkey-valueを右矢印(`->`)で対応付けて要素にします。

要素の参照はリストと同じく丸括弧 `()` でできますが、対応するkeyがないと `NoSuchElementException` が投げられます。
`contains` で事前に調べることもできます。

```scala
val map = Map("one" -> 1, "two" -> 2)
map("one") // => 1
map("three") // => NoSuchElementException
```

別の参照方法として `get` がありますが、返り値が `Option` クラスの具象クラス `Some`, `None` になります。
あるいは、デフォルト値を指定できる場合は `getOrElse` を使います。

```scala
map.get("two")  // => Some(2)
map.get("four") // => None
map.getOrElse("five",0) // => 0
```

この `Option` は特に関数型プログラミングで便利な役割を持ちます。

#### 他の関数型言語としての特徴

- `Option`型 (HaskellだとMaybe)、`Either`型 - 失敗する可能性のある計算であることを文脈に盛り込む
