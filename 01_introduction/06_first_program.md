## はじめてのプログラム

さて、ここから実際にプログラムを書き始める。 Visual Studio Code の左ペインを右クリックして「New File」をクリックするとファイル名の入力を求められる。
エントリーポイントと揃えて「index.js」としておこう。

### Hello World

エディタでファイルが開かれるはずなので、次のプログラムを入力してみる。
何回か出てきている「コンソールに文字列を表示する」プログラムだ。最初はこうと決っているものなのだ。

```JavaScript
console.log('Hello world!')
```

そしたら下ペインのコンソールで以下のコマンドを打ちこむ。これはnodeに指定したファイルのプログラムを実行させるためのコマンドだ。

```bash
> node index.js
```

無事「Hello world!」と表示されたら成功だ。おめでとう。あなたは無事に最初のプログラムを実行できたのだ！

さて、ほかにもいくつかプログラムを書いてみる。

```JavaScript
console.log('Hello world!')
console.log('こんにちは世界')
console.log('ほげほげほげ')
console.log('ふがふがふが')
```

セーブして実行してみると以下のように表示されるはずだ。

```bash
> node .\index.js
Hello world!
こんにちは世界
ほげほげほげ
ふがふがふが
```

このように、「プログラムは上から順番に実行される」。これは基本原則なので忘れてはいけない。

### 変数の使いかた

このように文字列を直接打ち込んでもいいが、既に書いたように「変数」というものが存在する。

```JavaScript
const str = 'Hello world!'
console.log(str) // Hello world! と表示される

let num = 1
console.log(num) // 1 と表示される

num = num + 1
console.log(num) // 2 と表示される
```

`const` と `let` というキーワードが出てきたので紹介する。

`let` は変数一般を定義するためのワードである。省略もできるのだが、初めて宣言した変数なのか既に定義されている変数なのかを区別するためにつけておいた方がいい。
ちなみにJavaScriptにはこれを省略させない「厳格モード」というのが存在する。慣れておいた方がいいかもしれない。

`const` は「定数」を定義するために必要なワードだ。このワードで宣言された変数は書き換える事ができない。正確には「参照を書き換える事ができない」。なので直接何かを再代入しようとするとエラーになる

```JavaScript
const str = 'Hello world!'
str = 'Good night!' // エラーになる
```

ここで注意してほしいのは「参照を書き換える事ができない」という点である。「参照」というのは「変数が指している値の大本」である。なので以下のような変更は可能である事に注意すること。

```JavaScript
const obj = {
  value: 10,
  str: 'Hello world!'
}

obj.str = 'Good night!' // これはエラーにならない

obj = {} // これは obj が指す物そのものを変えようとしているのでエラー
```

これは `obj` が指しているオブジェクトそのものは変化しておらず内容のみが変わっているからである。
