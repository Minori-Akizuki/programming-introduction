## 制御構文

さて、上から実行するだけじゃ複雑なプログラムなんて書けっこない。
そのためプログラムには「制御構文」というのが存在する。基本的には以下の3つである。

* 関数
* 条件分岐
* くりかえし構文

あとは一応「コメント」というものも制御構文の一つである。

```JavaScript

//  スラッシュ二つの後に書かれた物は行末まで無視される

/*
このように書かれたものに挟まれたものは
全て無視される
*/

```

プログラムの説明を書く時とかに多用される。積極的に使うべきである。

### 関数

前にも触れたので軽い説明に留めておく。プログラムを書いておけば後からその部分だけ呼び出せるというものだ。

```JavaScript
const sayHello = function () {
  console.log('Hello!')
}

sayHello()
sayHello()
sayHello()
sayHello()
```

もちろんカッコの中に引数を含める事もできる。

また、関数の定義はこういう書き方もできる。

```JavaScript
const sayAny = (str) => {
  console.log(str)
}

sayAny('hello!')
sayAny('bye!')
```

今は変数に代入しているのであまりありがたみは無いが `(引数, ...) => { 実行したいプログラム }` と書く事で何も無いところに関数を定義できる。引数がいらなければ引数は書かなくてもいい。この書きかたを「ラムダ式」と言う。これは使い捨ての関数を直接他の関数に渡す時によく用いられる。すぐに出てくるので安心されたい。

### 条件分岐

条件によって後続のブロックを実行するかどうかを選択する構文である。

```JavaScript
if( /* 真偽値*/ ){
  /* 真の場合に実行されるプログラム */
} else {
  /* 偽の場合に実行されるプログラム */
}
```

`else` 以下は省略して、条件式が真の時だけ実行する文を書くだけでも構わない。

基本的には「比較演算子」というのを使用して「条件式」の結果が `true` か `false` になるような式をカッコの中に書く事になる。以下によく使う物を列挙する。

> 実はJSの「条件」というのはかなり「ガバガバ」なので真偽値以外のものでも認識するのだが、結構[ややこしい話](https://qiita.com/ichi_zamurai/items/ca9b8e92778589c543e3)になってくるので時間があったり必要になった時に調べてみるといい。

* `a === b`
  * `ab`両者が「等しい」時に `true` を返す。数値や文字列等に利用できるが、オブジェクトや配列を丸ごと比較する事はできないので注意する事。
* `a <= b` 、 `a < b`
  * それぞれ`a` が `b` 以下、あるいは `a` が `b` 未満の時に `true` を返す。数値はもちろんだが、文字列であれば辞書式比較をしてくれる。文字列の比較をする場合は文字コード比較なので日本語がまざるとちょっとややこしくなる。
* `a >= b` 、 `a > b`
  * それぞれ`a` が `b` 以上、あるいは `a` が `b` を越えた時に `true` を返す。これも文字列比較ができる。
* `a && b` 、 `a || b`
  * 前者は `a` と `b` が共に `true` の場合に `true` を返す。後者はどちらかが `true` の場合に `true` を返す複数の条件式を組み合わせる時に使用する
    * というのは実は大嘘である。 `a && b` は「`a` が 『[falsy](https://developer.mozilla.org/ja/docs/Glossary/Falsy)』 の場合に `a` を返し、そうでなければ `b` を返す」演算子であり、 `a || b` は 「`a` が 『[truthy](https://developer.mozilla.org/ja/docs/Glossary/Truthy)』 の場合に `a` を返し、そうでなければ `b` を返す」演算子である。 truthy、falsyとは「真とみなせる値」「偽とみなせる値」である。`if` のカッコの中もこのtruthy、falsyという基準で真偽判定が行われている。これが「JavaScriptの条件は 『ガバガバだ』 」と言った所以である。こういう仕様を使ったテクニックもある。

さて、実際の使いかたを見てみる。次のコードを `index.js` に打ちこんで実行してみる。

```JavaScript
let a = 10

if(a < 5){
  console.log('a は5未満です')
} else {
  console.log('a は5以上です')
}
```

無事 `a は5以上です` と表示されただろうか。 `a` に代入される値を色々変えて試してみるといい。

また、カッコの中に直接 `true` や `false` を入れる事もできる。(本当はどんな値でも入れる事ができるが……)どこか別の場所で求めた条件を使い回したり、フラグとして使用している変数をチェックしたりする時にそんな書きかたをする。

```JavaScript
let flag = true

if(flag){
  console.log('フラグが立っています')
}
```

### くりかえし構文(`while`)

`while` 構文は条件式が真の間処理をループさせる機能である。以下はフィボナッチ数列を第20項まで求めるプログラムである。

> フィボナッチ数列というのはよく数学で出てくる数列で、 $\lbrace T_1 = 1, T_2 = 1, T_{n+2} = T_{n+1} + T_{n} \rbrace $ で定義される数列である。

```JavaScript
let fibbonach = [1, 1]

while(fibbonach.length < 20){
  fibbonach.push(
    fibbonach.at(-1) + fibbonach.at(-2)
  )
}

console.log(fibbonach)
/*
[
    1,    1,    2,    3,    5,
    8,   13,   21,   34,   55,
   89,  144,  233,  377,  610,
  987, 1597, 2584, 4181, 6765
]
と表示される
*/
```

新しい関数が出てきたので紹介する。 `.push()` はその配列の末尾に渡した要素を追加してくれる関数だ。

```JavaScript
arr = [1, 2]
arr.push(3)
console.log(arr) // [1, 2, 3] と表示される
```

また、 `.at()` は配列の要素を参照する関数で、基本的には `[]` と同じ動きをする。だがこれには負の数も指定でき、その場合は末尾から数えた要素が参照できる。

```JavaScript
let arr = [1, 2, 3]
console.log(arr[0]) // 1
console.log(arr.at(1)) // 2
console.log(arr.at(-1)) // 3
```

さて、それではもういちどプログラムを見てみる。

```JavaScript
let fibbonach = [1, 1]

while(fibbonach.length < 20){
  fibbonach.push(
    fibbonach.at(-1) + fibbonach.at(-2)
  )
}

console.log(fibbonach)
```

* `fibbonach` が定義されている。要素の数は2つである。
* `while` ループに入る。最初に条件式が判定される。`fibbonach.length` は`2` なので中の処理は実行される。
  * `fibbonach` の末尾に値が追加される。内容は `fibbonach` の末尾2つを足した物である。結果 `fibbonach` の要素が増える。
* 処理がおわったので条件式が判定される。`fibbonach.length` は`3` なので中の処理は実行される。
* ………
* 処理がおわって条件式が判定されるタイミングで `fibbonach.length` が `20` (以上)であった場合にループを抜け、後続の処理が実行される。
* `fibbonach` が出力される。

このように、一定の条件を指定しその間ずっと処理をくりかえすのが `while` ループである。なお、ループ開始時に条件式が真でないとこのループは一度も実行されないが、「少なくとも一度は実行したい」という時のために `do-while` 構文というのが用意されている

```JavaScript
do {
  // 実行したい文
} while( /**条件式**/ )
```

たまに使いたくなる時がある。

### くりかえし構文(`for`)

```JavaScript
let strs = ['りんご', 'ばなな', 'みかん']

for(let index = 0; index < strs.length; index++){
  console.log(index + ':' + strs[index])
}

console.log('ループ終了')

/*
0:りんご
1:ばなな
2:みかん
ループ終了
と表示される
*/
```

実は `+` の演算子は文字列の結合をしてくれたり、その時は数値が文字列に勝手に変換されたりと色々あるのだが、とりあえずこの `for` というやつについて解説する。

```JavaScript
for( /* 初期化処理 */; /*ループ条件*/; /*ループ終了時の処理*/){
  /* 実行させたいプログラム */
}
```

* 初期化処理
  * `for` 文に入る時に一度だけ実行される文である。普通はカウンタ変数というものを用意してそれを初期化する。必要がなければ省略する事ができる。
* ループ条件
  * `for` 文を実行するかどうかの条件をここに入れる。この式が falsy になればループを抜け次の処理に移る。省略すると常に `true` という扱いになる。
* ループ終了時の処理
  * `for` 文内の文が実行される毎に実行される処理である。普通はカウンタをインクリメントしたりする処理を入れる。 `++` は対象の変数を1増やす演算子である。必要がなければ省略する事ができる。

これをふまえてもう一度先のプログラムを見てみる。

```JavaScript
let strs = ['りんご', 'ばなな', 'みかん']

for(let index = 0; index < strs.length; index++){
  console.log(index + ':' + strs[index])
}

console.log('ループ終了')
```

* 最初に `strs` という変数が定義されている。いくつかの文字列が入っている。
* `for` ループに入り、まず `index` という変数が定義される。初期値は `0` である。
* 次にループ条件が確認される。 `.length` はその配列の長さを示すメンバである。今 `0 < 3` は `true` なのでこのループは実行される。
* ループの中身が実行される。
  * `index` は `0` であるため、 `strs[0]` である `りんご` がとりだされ `0:りんご` という文字列が出力される。
* ループ終了時の処理が実行される。`index` の値が1増える。
* ループ条件が確認される。 `index` の値は `1` であるため、`1 < 3` は `true` であるためこのループは実行される。
* ループの中身が実行される。
* ………
* `index` の値が3になった時、 `3 < 3` が `false` になるためこのループは実行されず、以降の処理に進む。
* `console.log('ループ終了')` が実行される

という流れになる。基本的なループであるし、こういう単純な使いかた以外にも応用はあるのだが素朴であるが故にこの `for` ループというのは扱い辛い。
インデックス変数に気を配らなければいけないし、例えばこの使いかただと「配列のインデックス」と「インデックス変数」というのが独立してしまい意味が不明瞭になる。

### くりかえし(`forEach()`)

例えば先のように「配列の中身を全て出力したい」というのであれば以下のような方法がある。

```JavaScript
let strs = ['りんご', 'ばなな', 'みかん']

const printIndexAndStr = function (food, index){
  console.log(index + ':' + food)
}

strs.forEach(printIndexAndStr)

/*
0:りんご
1:ばなな
2:みかん
と表示される
*/
```

配列に存在する `.forEach()` という関数は渡された関数を配列の中身毎に実行してくれる。

渡される引数は二つある。配列の中身そのものとそのインデックス番号だ。つまり上のプログラムの動きは次のようになる。

* `strs` が定義される。
* `printIndexAndStr` が定義される。これは関数である。
* `strs.forEach()` に `printIndexAndStr` を渡して実行する
  * 配列の中身それぞれに対して `printIndexAndStr(中身, インデックス番号)` が実行される

どうだろう。先程の説明に比べて大分スッキリとした解釈になったのではないか。

あるいは、この `printIndexAndStr` という関数はここでしか使わないというのであればわざわざ変数に格納するまでもない。

```Javascript
let strs = ['りんご', 'ばなな', 'みかん']

strs.forEach(
  (str, index) => {
    console.log(index + ':' + str)
  }
)
/*
0:りんご
1:ばなな
2:みかん
と表示される
*/
```

思い出してほしい。 `(引数, ...) => { 実行したいプログラム }` という記述はラムダ関数の記述だ。 `forEach()` に渡すべき関数をこの中で直接定義している。

### くりかえし(`map()`)

もうひとつ便利なものを紹介する。それが `.map()` である。これは渡された関数に従い配列から新しい配列を生成する。

```JavaScript
let nums = [1, 2, 3, 4, 5]

let doubles = nums.map(
  (x) => {
    return x*2
  }
)

console.log(doubles)
// [ 2, 4, 6, 8, 10 ] と表示される
```

`doubles` には `nums` の配列の中身が全て倍になった新しい配列が生成され格納される。さて、もう一つ例を見てみよう。

```javascript
let points = [10, 40, 53, 23, 12]

let passeds = points.map(
  (x) => {
    if(x < 30){
      return '赤点'
    } else {
      return '合格'
    }
  }
)

console.log(passeds) // [ '赤点', '合格', '合格', '赤点', '赤点' ] と表示される
```

配列に入っている数字が30点未満なら「赤点」、30点以上なら「合格」という文字列に変換する関数を渡している。

さてこの関数、というかこの処理、どこかで見た事無いだろうか。そうである。「写像」の項で出てきた「テストの点数が30点未満なら〜」のやつである。
この関数はその名の通り「写像(map)」を行う事のできる関数なのである。もちろんこいつはプログラムなので、途中に色々な処理(画面出力とか)も挟む事ができるのだが、このように配列にある多数の要素を一度に「写す」事ができる。

### くりかえし(`reduce()`)

map と合わせて重要な処理が存在する。「畳みこみ」と呼ばれる処理である。例えば配列の中身を全て足し合わせる処理を以下のように書ける。

```JavaScript
const array1 = [1, 2, 3, 4]
const add = (previousValue, currentValue) => previousValue + currentValue

let sum = array1.reduce(add, 0)
console.log(sum); // 10 と表示される
```

`arr.reduce(f, 初期値)` とする事で `arr` の内容が関数 `f` に従って先頭から値が計算される。具体的には以下のような計算が行われる。

* `arr` の先頭の値( `a0` とする) と初期値 ( `b` とする) を元に `b0 = f(b, a0)` が計算される
* `arr` に次の値( `a1` とする) があれば先の `b0` を元に `b1 = f(b0, a1)` が計算される
* `arr` に次の値( `a2` とする) があれば先の `b1` を元に `b2 = f(b1, a2)` が計算される
* 配列の末尾まで以上をくりかえし、最後の計算結果 `bn` を返却する

言葉で説明すると回りくどいが、配列の中身を先頭からポコポコ初期値にくっつけていくイメージをしてもらえればいいだろうか。

こうすれば「畳みこみ」がどう動いているかよく観察する事ができる。

```JavaScript
const array1 = [1, 2, 3, 4];
const add = (previousValue, currentValue) => {
  console.log('[reducer]')

  console.log(`previousValue : ${previousValue}`)
  console.log(`currentValue : ${currentValue}`)
  
  let added = previousValue + currentValue
  console.log(`added : ${added}`)
  
  return added
}
let sum = array1.reduce(add, 0)
console.log(sum);

/*
[reducer]
previousValue : 0
currentValue : 1
added : 1
[reducer]
previousValue : 1
currentValue : 2
added : 3
[reducer]
previousValue : 3
currentValue : 3
added : 6
[reducer]
previousValue : 6
currentValue : 4
added : 10
10
*/
```

ちなみに、バッククォートで囲んだ文字列は `${}` とする事で変数や式を直接埋め込む事ができる。便利なので覚えておこう。

### くりかえし(`for...in`)

さて、配列のくりかえしが存在するならオブジェクトに対するくりかえしも存在する。それが `for...in` 構文である。

```JavaScript
let obj = {
  a: 10,
  b: 34,
  c: 25
}

for(const name in obj){
  console.log(name, obj[name])
}
/*
a 10
b 34
c 25
*/
```

`in` の直前に宣言した変数にオブジェクトのメンバ名が入り、オブジェクトに入っているメンバ分だけループをしてくれる。
なお通常オブジェクトメンバへの参照は `obj.a` のようにドット演算子で行うが、変数内の文字列等で参照する場合は `obj[name]` のようにブラケットを用いる。

オブジェクトの名前が取得できるので、例えばこういう事もできる。

```JavaScript
const sample = [10, 54, 23, 75, 84, 25, 64, 32, 13, 64]

const statisticsFunction = {
  average (arr) {
    let sum = arr.reduce( (p, c) => p+c , 0)
    return sum/arr.length
  },
  max (arr) {
    return Math.max(...arr)
  },
  variance (arr) {
    let avr = this.average(arr)
    let sn = arr.reduce( (p, c) => p + (c-avr)**2 , 0)
    return sn/arr.length
  }
}

let sampleStatics = {}

for(const calc in statisticsFunction) {
  sampleStatics[calc] = statisticsFunction[calc](sample)
}

console.log(sampleStatics)
// { average: 44.4, max: 84, variance: 652.24 }
```

`statisticsFunction` なるオブジェクトには渡した配列の統計情報を計算する関数がいくつか入っている。
それを `for...in` で順次実行していくのだが、関数名をそのまま結果をまとめるオブジェクトのメンバ名として利用している。
