## はじめての node.js

さて、その中にプロジェクトとなるフォルダを作ろう。「first_programing」とでもしておこう。
フォルダを右クリックすると「Codeで開く」というメニューが追加されている事に気付く筈だ。それクリックすると Visual Studio Code が立ち上がる。

ここで `Ctrl + Shift + @` を押すと下部にターミナルの画面が出てくる。なにもしていなければ PowerShell が開くと思うのだが、まぁこれで問題無い。

さて、このでてきたターミナルに以下のコマンドを打ちこもう。

```bash
> npn init
```

これはディレクトリ内にパッケージ用の設定ファイルを作るためのコマンドである。コマンドをうちこむといくつか質問をされるからそれに従って打ち込んでいこう。

```bash
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help init` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (first_program)
version: (1.0.0)
description: 
entry point: (index.js)
test command:
git repository:
keywords:
author:
license: (ISC)
About to write to C:\home\minori\first_program\package.json:

{
  "name": "first_program",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}


Is this OK? (yes)
```

* package name
  * 作ろうとしているプロジェクトの名前である。デフォルトでディレクリの名前がつくのでそのままエンターを押せばいい
* version
  * 一番最初のバージョン番号をどうするかである。正式版では `1.0.0` にして、開発版では `0.1.0` とかにするのが習慣である。
* description
  * このパッケージの概要を書く。パッケージを公開する時とかに見られるものである。
* entry point
  * このパッケージを実行した時に一番最初に実行されるファイルを指定する。大体 `index.js` か `app.js` という名前にする。
* test command
  * テスト用のコマンドである。決まっているなら入れた方がいいが後で設定する。
* git repository
  * コード管理をするためのリポジトリを指定する。Gitについては後程解説する。
* keywords
  * パッケージに関するキーワードを並べる。公開するときには入れた方がいい。
* author
  * 作者の名前を入れる。パッケージを公開する時には入れた方がいい。
* license
  * パッケージを公開した時にどのような使いかたを許可するかのフォーマットを指定できる。これも公開するような物を作った時に気にすればよい

これらの質問に答えると、「 `package.json` ってファイルを作ったよ」と言われる。Visual Studio Code左側のペインを見ると該当ファイルが生成されているのがわかる。

### 入れておいた方がいい拡張機能

左ペインに色々とアイコンが表示されているのがわかると思う。四角いのが4つ、右上がとびでているアイコンをクリックすると拡張機能が大量に表示される。
入れておいた方がいい拡張機能を以下に示す。

* indent-rainbow
* Bracket Pair Colorizer 2
* ESLint
* Prettier
* Material Icon Theme

他にも色々な拡張機能があるので状況に応じて入れると便利である。

<https://qiita.com/ucan-lab/items/e85931bf8276da43cc97>