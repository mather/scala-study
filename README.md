scala勉強会
===========

このページは？
--------------

Scala勉強会の資料置き場です。

方針
----

はじめはREPL(Read-eval-print loop, 対話実行環境)でちょこちょこ触ってみて、
実際に作成する際にコンパイラを使っていく予定。

目次（列挙メモ？）はこの下に。

インストール方法
----------------

### JDK (or JRE) のインストール

ScalaはJavaVM上で動作するため、JavaVMが動作する環境(`java`コマンドが使える環境)が必要です。

JRE(実行用環境)でも大丈夫だと思いますが、JDK(開発用環境)をインストールしたほうがいいと思います。バージョンは1.6以上です。

Windows, Linuxの方は[JDKダウンロードページ](http://www.oracle.com/technetwork/java/javase/downloads/index.html)からインストーラを取得してください。

Mac OS Xの場合は、最初から入っていると思います。（セキュリティ対策もあるので最新版に更新してください）


### Scala のインストール

Scala および sbt, akka のメンテナンスをしている Typesafe 社のページで一式まとまっています。

#### Windows : Typesafe Stackを使う

Typesafe StackはScalaのコンパイラ `scalac` やビルドツール `sbt` (antやmavenのようなもの)を入れてくれます。

[Typesafe Stack](http://typesafe.com/stack) から Download Stack >> をクリックし、条件に同意してダウンロードページへ進んでください。

各OSのインストール方法が用意されています。

#### Mac OS X : homebrew を使う

[Homebrew](http://mxcl.github.com/homebrew/) を使ってコマンド一発です。


    $ brew install scala sbt maven giter8


#### Linux

Typesafe Stack にも記述されていますが、以下のコマンドでパッケージインストール出来ます。


    $ sudo apt-get update
    $ sudo apt-get install typesafe-stack

#### その他

とりあえず Typesafe Stack に行けば方法が書いてあります。

### 動作確認

`scala` コマンドを実行して、以下のようにREPLが起動すればひとまず完了です。

    Welcome to Scala version 2.10.0 (Java HotSpot(TM) 64-Bit Server VM, Java 1.6.0_43).
    Type in expressions to have them evaluated.
    Type :help for more information.

    scala>

Windowsなどでは Scala 2.9.2 が入るかもしれませんが、とりあえずOKです。

以下、整理用目次
----------------

1. 文法の説明・型（クラス）の紹介
  - 変数定義
  - メソッド定義
  - 制御構文
  - パターンマッチ
  - 例外処理
  - クラスの紹介(数値系、文字列、配列・コレクション、などなど)
  - パーサ・コンビネータ(あまり触れない)
1. Scalaによるオブジェクト指向プログラミング
  - Hello,world!
  - クラス定義(`class`)
  - シングルトンオブジェクト(`object`)
  - 抽象クラス(`abstract class`)
  - トレイト(`trait`)
  - 列挙型(`Enumeration`) <- する?
  - ケースクラス(`case class`, `sealed`)
  - DI(Dependency Injection)サンプル
1. Scalaによる関数型プログラミング
  - 関数型プログラミングとは？
  - 関数リテラル・メソッドとの違い
  - コレクションの抽象処理
  - Option型、 Either型
1. ScalaとAkkaによる並列プログラミング
  - Actorモデルとは？
  - 並列処理サンプル
1. Scalaの Web Framework
  - Play2
  - Scalatra
