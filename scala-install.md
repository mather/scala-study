---
layout: default
title: Scala勉強会
subtitle: インストール
---

[<< 目次](index.html)

### JDK(or JRE)をインストール

ScalaはJavaVM上で動作するため、JavaVMが動作する環境(`java`コマンドが使える環境)が必要です。

JRE(実行用環境)でも大丈夫だと思いますが、JDK(開発用環境)をインストールしたほうがいいと思います。バージョンは1.6以上です。

Windows, Linuxの方は以下のページからインストーラを取得してください。

MacOSXの場合は、最初から入っていると思います。（セキュリティ対策もあるので最新版に更新してください）

[Oracle JDK ダウンロードページ(http://www.oracle.com/technetwork/java/javase/downloads/index.html)](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

### Typesafe Stackを使う

Typesafe StackはScalaのコンパイラ `scalac` やビルドツール `sbt` (antやmavenのようなもの)を入れてくれます。

[Typesafe Stack](http://typesafe.com/stack) から Download Stack >> をクリックし、条件に同意してダウンロードページへ進んでください。

各OSのインストール方法が用意されています。

#### Windows

インストーラ(exe形式)があります。実行していくと環境変数`PATH`の設定なども自動で行なってくれるため、インストール後はコマンドプロンプトから`scala`コマンドが実行できるようになります。

#### Mac OS X

homebrewでインストールします。

```
$ brew install scala sbt maven giter8
```

#### その他

説明に従ってインストールすればきっと大丈夫。(投げやり)

### 動作確認

コンソールから `scala` コマンドを実行して以下のプロンプトが出ることを確認してください。
(Windows版は Scala version 2.9.2 が入るかもしれません。)

```
Welcome to Scala version 2.10.0 (Java HotSpot(TM) 64-Bit Server VM, Java 1.6.0_43).
Type in expressions to have them evaluated.
Type :help for more information.

scala>
```

`:quit` または `Ctrl+D` を入力して終了します。

### 各種コマンドの一言説明

- `scala`
    - コンパイル済みclassを実行したり、REPLを起動します。
    - `scala -version` : バージョン
    - `scala -help` : ヘルプ
- `scalac`
    - `.scala`ファイルをコンパイルします。ただし、通常は依存関係も含め、`sbt`などでコンパイルします。
- `sbt`
    - simple build tool.
    - 対話型のビルドツール。`compile`, `test`, `run` などのサブコマンドがあります。
- `g8`
    - giter8のコマンド。github上のテンプレートをプロジェクト名などを差し替えてローカルに配置してくれます。

[<< 目次](index.html)
