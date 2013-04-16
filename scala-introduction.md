---
layout: default
title: Scala勉強会
subtitle: 導入編
---

### Scalaの概要

Scalaは1行スクリプトから大規模システムまでスケールする__Sca__lable __La__nguage(スケーラブルな言語)から名付けられている。

設計者はJava(ジェネリクス、javac)の開発にも携わっていた[Martin Odersky](http://www.scala-lang.org/node/241)。
Javaの不満点なども解決するようになっている。
(分かりきった定型句が多いとか、インターフェースが実装を持てないとか、 `null` の扱いとか)

- JVM言語
    - Javaライブラリ、フレームワークが使える
    - JavaからScalaのクラスにもアクセスできる
- 徹底したオブジェクト指向
- 関数型プログラミング

他に特徴として以下のことが挙げられます。

- かなり簡潔にコードを書ける
- データや小さい機能を「型」でコンパクトに定義でき、安全かつ柔軟に合成できる
- コードの抽象度を高くでき、意図を明確にできる
- 新しい制御構造を作れる、つまり、DSL(ドメイン特化言語)を作れる
- Actorモデルを使った安全かつ軽量な並列処理が容易に実現できる
    - 小さくて安全な並列処理コードを書けます
    - 障害耐性(fault-tolerance)も考慮されています

### 導入実績

海外の大きなところだと

- Twitter
- Foursquare
- LinkedIn
- NASA

などなど。国内事例も増えてきています。

- ドワンゴ
- 三菱 UFJ IT

(cf. 先日行われた[Scala Conference 2013](http://scalaconf.jp/ja/program/index.html) )

### オブジェクト指向言語

- 属性・状態と操作（メソッド）のカプセル化
- 単一責任の原則(Single responsibility principle)
    - クラスやモジュールの変更理由はただ一つであるべき

### 関数型プログラミング

- 「純粋な関数」は入力によって出力が一つに決まる。(__参照透明__と呼ばれる)
- 関数自体もひとつの値。逆に言うと定数も関数とみなせる。
  関数を引数にしたり、関数を返す関数(高階関数)がある。
- 入力値を書き換えたり、（プログラム外とのI/Oなど）状態によって出力値が変わるものは「副作用」のある関数と呼ばれる。
- 理想的には、「副作用」の起こる箇所をを出来る限り集約し、「純粋」な関数で内部処理を定義すれば、仕様か副作用にしかバグはない。

### オブジェクト指向と関数型のハイブリッドで何が起こるか

- 関数への入出力はオブジェクト。関数自体もオブジェクト。
- 純粋な関数で扱われるデータオブジェクトは不変であるべき。
- 状態を扱うオブジェクトと、データを扱う不変オブジェクトを明確に分離する。
    - そのために非常に簡単にデータクラスを作れるようになっています。

### REPLの使い方

`scala` コマンドを引数なしで実行してみましょう。
`scala>` というプロンプトが表示されてREPL(Read-Eval-Print Loop、対話実行環境)が利用可能になります。

```
$ scala
Welcome to Scala version 2.10.0 (Java HotSpot(TM) 64-Bit Server VM, Java 1.6.0_43).
Type in expressions to have them evaluated.
Type :help for more information.

scala>
```

Scalaコードを実際に入力して、実行結果や型の情報を見ながら簡単な処理を実行できます。

また実行結果は `res0` のように自動的に格納されるので、後から利用可能です。

```
scala> 1 + 2
res0: Int = 3

scala> :type res0
Int
```

さらに、[TAB]キーでメソッドを補完したりできます。(もう一度押すといくつか追加されます。)

```
scala> "hoge". // <- ここで[TAB]キー
+                     asInstanceOf          charAt                codePointAt           codePointBefore       codePointCount
compareTo             compareToIgnoreCase   concat                contains              contentEquals         endsWith
equalsIgnoreCase      getBytes              getChars              indexOf               intern                isEmpty
isInstanceOf          lastIndexOf           length                matches               offsetByCodePoints    regionMatches
replace               replaceAll            replaceFirst          split                 startsWith            subSequence
substring             toCharArray           toLowerCase           toString              toUpperCase           trim
```

REPLで実行出来るコマンドについての一覧は `:help` を入力するとヘルプが表示されるのでそちらを参考にしてください。

`:quit` または Ctrl+D でREPLを終了できます。

```
scala> :quit
```

[>> 文法編](scala-grammer.html)
