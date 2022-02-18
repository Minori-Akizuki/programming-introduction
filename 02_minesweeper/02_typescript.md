## TypeScript への招待

本格的に WEB 作成に入る前に覚えておいた方がいいものがある。それが「TypeScript」だ。これは JavaScript に「型システム」を導入するもので、結果プログラムの不備を事前にみつけたり強力な補完機能が提供されたりと JavaScript の「型がガバガバであるが故」の不備を心強く補完してくれる物である。

例えば以下のような関数を考える。適当に Chrome のコンソールから定義してみるといい

```JavaScript
const add = (a, b) => {return a+b}
```

これはもちろん数字を足す事を考えて作成された関数である。数字を与えれば期待通りの動作をする。

```JavaScript
const add = (a, b) => {return a+b}
let c = add(1, 2) // 3
```

ここに間違えて数字のつもりで文字列を入れてしまう、という事がよくある。
結果が数字ではなく「数字の文字列」で帰ってくるような関数が割とあるからだ。

```JavaScript
const add = (a, b) => {return a+b}
let c = add(1, '2') // '12'
```

何が起こるかというと、この関数 `add` は無防備に `1+'2'` を実行する。 `+` 演算子は文字列の結合ができるのだがこの時数字は勝手に文字列に変換されるという性質を持っており、結果文字列の `'12'` が返却される。

また次の場合はどうか。何かの誤ちで `add` で返却された結果にメンバがあると思いこみ、それを呼び出してしまった。

```JavaScript
const add = (a, b) => {return a+b}
let c = add(1,2).value // undefined
```

`c` にはシレっと `undefined` が代入され、その誤ちは `c` をどこかで使用した時に始めて現れる。当然 `add` の結果が数値であるからなのだが、これは「 `add` は数値を返しますよ」と判っていれば発生しえない事である。

しかし残念ながら JavaScript はこのような事を実行する前に判断する事はできないし、型を推論するしくみが無いので上記のような間違いが発生した時にそれを発見し除去する事が非常に困難である。

そこで考案されたのが以下のようなスタイルである。

* 型推論をさせるコードとそれを扱う Lint (構文チェック)を発展させる
* 書かれたコードを JavaScript にコンパイルする事でコンパイラにもそれを発見させる

このように JavaScript の不足している部分を補うような、最終的に JavaScript に変換されるプログラムを AltJS (Alternative JavaScript) と呼ぶ。そんなものが出てくるくらいである。それだけ JavaScript というのは「ライトウェイトであるが故に書き辛い」言語なのである。

### 文法と効果

本来は TypeScript の実行までやってみせたいところだが、少し手間がかかるため Lint の機能だけ実感して貰えればよい。

また、文法といっても対した事はない。「変数と関数に型をつけられる」という物だ。

適当なフォルダを用意し、 `.ts` の拡張子をつけてファイルを作成すること、それを VisualStudio Code で開いてみよう。まず以下のように入力をする。

```TypeScript
{
  const add = (a, b) => {
    return a+b
  }
}
```

引数 `a, b` のところに警告が出ていないだろうか。マウスをホバーすると以下のような文言が出る筈だ。

```
Parameter 'a' implicitly has an 'any' type, but a better type may be inferred from usage.
```

「パラメータ「a」には暗黙的に「any」タイプがありますが、使用法からより適切なタイプが推測される場合があります。」と言われている。なので型を追加しよう。

```TypeScript
{
  const add = (a:number, b:number):number => {
    return a+b
  }
  let c = add(1, 2)
}
```

このように変数や関数の直後に型の情報を記入すると、 TypeScript はそれを適切に解釈してくれる。下の `add` を書こうとした時にポップアップで型情報が出てきた筈だ。またそれを利用している `c` にもマウスをホバーすると型情報が出てくる。(一緒に「この変数は使われていませんと出るが使っていないのでしょうがない。)

どんな型があるかは [この記事](https://qiita.com/t0daaay/items/a6f9106800488a622fbb) を一瞥してみてほしい。オブジェクトの内容物まできちんと指定できる。

このように TypeScript は非常に強力なツールである。本来はこれをコンパイルして JavaScript に直してから使用するのだが、今回は割愛する。
今後の開発も TypeScript を使用して進めていく。