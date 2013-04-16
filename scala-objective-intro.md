---
layout: default
title: Scala勉強会
subtitle: オブジェクト入門編
---

このページではファイルを使ってコードを書きます。

好きなエディタなどを使ってください。

### class : クラス定義

`Person.scala` ファイルに `Person` クラスを定義してみましょう。

Scalaではコンストラクタ(インスタンスを生成するメソッド)はクラス名のすぐ後に定義します。

```scala
/* Person.scala */
// コンストラクタ
class Person(val name: String, val age: Int) {
  // メソッド
  def who = name + "(" + age.toString + ")"
}
```

scala コマンドに `-i` オプションでファイルをロードします。

```
$ scala -i Person.scala
```

Personクラスのインスタンスを作っていじってみましょう。

```
scala> val p = new Person("Tester", 25)
p: Person = Person@490eb6ae

scala> p.name
res0: String = Developer

scala> p.age
res1: Int = 25

scala> p.who
res2: String = Tester(25)
```

`Person` クラスのインスタンス生成と、属性へのアクセスが出来るようになりました。

コンストラクタに `val` または `var` を付けておくことで、同時にメンバー変数として指定できます。
さらに、一部のメンバーを外部からアクセス不可にするには、`private`などで保護できます。

```scala
// コンストラクタと同名のメンバ定義
class Person(private val name: String, private val age: Int) {
  def who = name + "(" + age.toString + ")"
}

// コンストラクタが読みにくかったら複数行にしてもいいです。
class Person(
  private val name: String,
  private val age:  Int
) {
  def who = name + "(" + age.toString + ")"
}
```

### object : シングルトンオブジェクト

Scalaには`static`(静的)キーワードがありません。(JavaとかPHPとかにはあると思います)

これに変わるもの（というと若干語弊がありますが）は `object` で宣言されるシングルトンオブジェクトです。

`Hoge.scala` に書いてみましょう。

```scala
object Hoge {
  val str = "hoge"
  def add(x: Int, y: Int) = x + y
}
```

そして `Hoge.scala` を使ってみましょう。

```scala
scala> Hoge // <- シングルトンインスタンス
res0: Hoge.type = Hoge$@3ec44cfb

scala> Hoge.str
res1: String = hoge

scala> Hoge.add(1,2)
res2: Int = 3
```

`Hoge` 自体が既にインスタンス化されていることがわかるでしょうか。
定義時に自動的にインスタンス化されます。

### trait : トレイト

Scala には Java の interface がありません。代わりに trait（トレイト） があります。

これは、一つのクラスに多重に合成(mix-in)できて、実装も行うことが出来ます。

以下の例では未定義の `name` と、それを使ったメソッドを実装しています。

```scala
/* TraitTest.scala */
trait A {
  def name: String
  def methodA = name + " using A!"
}
trait B {
  def name: String
  def methodB = name + " using B!"
}
class C extends A with B {
  val name = "C"
}
```

REPLで読み込んで使ってみましょう。

```
scala> (new C).methodA
res0: String = C using A!

scala> (new C).methodB
res1: String = C using B!
```

トレイトの読み込み方ですが、クラスの定義時にmix-inする場合は、一つ目は必ず `extends` を使い、二つ目以降は `with` を使います。

トレイトは、オブジェクト指向のより進んだ__合成(composite)__を使うためにとても重要な機能です。
応用編ではDI(依存性注入)やAOP(アスペクト指向プログラミング)にも軽く触れる予定です。

### case class でもっと便利に

最初のPersonクラスは、等価性が定義できていません。
これでは一意性(uniqueness)が担保できないため同じ名前の `Person` を連想配列のキーにしたりできません。

```
scala> val p = new Person("Tester",25)
p: Person = Person@2635ee49

scala> val q = new Person("Tester",25)
q: Person = Person@7d4966cc

scala> p == q
res4: Boolean = false
```

そこで、よりデータクラスとして適切な定義にするため、case class を使います。
以下のようにコンストラクタから `val` を抜き、 `case` を付けるだけです。

```scala
/* Person.scala */
case class Person(name: String, age: Int) {
  def who = name + "(" + age.toString + ")"
}
```

REPLでいじってみましょう。
case class からインスタンス生成する場合、 `new` を使わないことに注意してください。

```
scala> val p = Person("Tester", 25)
p: Person = Person(Tester,25)

scala> p == Person("Tester", 25)
res0: Boolean = true

scala> p.who
res1: String = Tester(25)

scala> p == Person("Developer",25)
res6: Boolean = false
```

case class を使うと、データオブジェクトを簡単に定義できて、しかも扱いやすくなります。
代表例はパターンマッチでしょう。

以下のコードをREPLに入力してみましょう。

``` scala
def matcher(p: Person) = p match {
  case Person("Tester", _) => println("It's Tester!")
  case Person(_, age) if age < 20 => println("So young!")
  case Person(name, age) => println(name, age)
}
matcher(Person("Tester", 18))
matcher(Person("Developer", 19))
matcher(Person("Developer", 29))
```

このように、マッチしたオブジェクトから定義順に値を抽出して使うことができます。

ケースクラスはその名のとおり箱状のクラスであって、

### abstract class : 抽象クラス

抽象メンバーや抽象メソッドを定義できるクラスです。
トレイトがあればいらないんじゃないかと思われるかもしれませんが、
多重継承を防止する意味で、具体的にインスタンス化されるクラスの抽象的存在として使われます。
（分かりにくい日本語ですね）
