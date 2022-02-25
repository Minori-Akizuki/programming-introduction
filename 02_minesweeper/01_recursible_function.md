## 申し送り事項

前章でに色々と制御構文だの関数の使いかただのを説明してきたが色々と覚えておいた方が良いテクニックがいくつかある。本格的に WEB ページを作る前に徒然と紹介していく。

### 値渡しと参照渡し

```JavaScript
const add1 = function( x ){
  x = x + 1
  return x
}

let t = 3

let s = add1(t)

console.log(t) //  ← どうなる？
// --------------------
const addMemver = function (obj) {
  obj.newMember = "I'm new Member!!!"
  return obj
}

let v = { value:10 }

addMemver(v)
console.log(v) //  ← どうなる？
addMemver(v).newerMember = "I'm newer!!!!!!"
console.log(v) //  ← どうなる？
// -------------
const na = function (arr) {
  arr = []
}

let a = [1, 2, 3, 4, 5]
```

### スプレット構文

この機能は少しややこしい。同じ `...` という構文を使って文脈により真逆の事ができるからだ。
[公式ドキュメント](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Spread_syntax) に詳しい説明は書いてあるのだが、よく使うつかいかたをかいつまんで説明する。

#### 可変引数

与えられた引数を全て足す関数 `sum` を考える。

```JavaScript
const sum = function(x, y, z) {
  return x + y + z
}
```

当然この関数は3つの数しか足し合わせる事ができない。4つ、5つの場合と考えて新しい関数を作るのはあまりにも無駄だというのはすぐに察しがつくだろう。
JavaScript では可変個の変数をこのように定義できる。

```JavaScript
const sum = function(...args){
  return args.reduce((p, c) => {return p+c}, 0)
}
```

例えば `sum(1, 2, 3, 4)` と呼ばれた場合に `args` には `[1, 2, 3, 4]` として入ってくる。これは引数が増えた場合も同様の動きをする。これは通常の引数と合わせて使う事ができる。

```JavaScript
const hogehoge = function(x, y, ...others){
  console.log(x)
  console.log(y)
  console.log(others)
}

hogehoge(1, 2, 3, 4, 5, 6)
/*
1
2
[3,4,5,6]
と表示される
*/
```

紹介しておきながらいい使いかたが思い浮ばなかった。

一つ注意点があるとすれば、「残存する引数を全て配列におしこめる」という性質上この `...` を前置した引数は引数の最後にしか置けない事である。

#### 配列/オブジェクトの展開

逆に関数に配列を渡す時にこの `...` をつけると、あたかも別々に引数として渡したかのように機能する。

```JavaScript
const printDateTime = function(month, date, hour, muinits){
  console.log(`ただいま${month}月${date}日の${hour}時${muinits}分です`)
}

printDateTime(2,12,15,30)
// ただいま2月12日の15時30分です

let datetimeArr = [1,30,3,25]
printDateTime(...datetimeArr)
// ただいま1月30日の3時25分です
```

また、このようにすることで配列の結合ができる。しかもこれは「コピー」が行われ新しい配列が生成される。新しい配列に操作を加えても元の配列に影響を及ぼさない。

```JavaScript
let arr1 = [1, 2, 3]
let arr2 = [4, 5, 6]
let newArr = [...arr1, ...arr2] // [1, 2, 3, 4, 5, 6] という新しい配列が生成される
let newArr2 = [8, ...arr1, 9, ...arr2] 
//[8, 1, 2, 3, 9, 4, 5, 6] となる。このように途中に別の要素を挟む事もできる
```

これはオブジェクトにも使用でき、オブジェクトをコピー、マージする時なんかにも使える。
既にあるメンバーは後に書いた方が優先されるので注意が必要だ。

```JavaScript
let obj1 = { foo: 'bar', x: 42 }
let obj2 = { foo: 'baz', y: 13 }

let clonedObj = { ...obj1 }
// Object { foo: "bar", x: 42 }

let mergedObj = { ...obj1, ...obj2 }
// Object { foo: "baz", x: 42, y: 13 }

const addMemver = function (obj) {
  obj.newMember = "I'm new Member!!!"
  return obj
}
let u = { val: 10 }
addMemver({...u})
console.log(u) // { val: 10 }
// u の コピーが渡される
```

### 再帰関数

そんな大した話ではない。「自身を呼ぶ関数」とただそれだけの事である。が実際に書くとなるとちょっと混乱しやすいものではある。

以下は与えられた数字の階乗を返す関数である。自然数 $n$ の階乗 $n!$ は $n \times (n-1)!$ で定義され、 $1！=1$ である。愚直に書き下すと以下のようになる。

```JavaScript
const fact  = (n) => {
  if( n <= 1 ){
    // 1! = 1 である。
    // 負の数を与えられてまちがって無限再帰にならないように一応「以下」と定義してある。
    return 1
  }
  return n * fact(n-1)
}
```

自身を呼び出すので当然気を抜くと無限に再帰が行われてしまう。ミニマルな定義を最初にもってきて「終了条件」を明確にすること。

少し複雑な例を紹介する。以下は与えられた配列がソートされた新しい配列を返す関数である。クイックソートというアルゴリズムが用いられている。

```JavaScript
const sort = (arr) => {
  if(arr.length <= 1){
    // 渡された配列の要素が1個以下だったらそのまま返却する
    // なぜならそれはソートされているからである
    return arr
  }
  let copy =[...arr] // 配列のコピーをとる
  let pibot = copy.shift() // 先頭の要素を取り出す
  let lessEq = copy.filter(x => x <= pibot) // 先頭の要素以下の物を抽出する
  let greater = copy.filter(x => x > pibot) // 先頭の要素より大きい物を抽出する
  return [...sort(lessEq), pibot, ...sort(greater)] // ソートされた配列の間に先頭の要素を挟む
}
```

最後の行の説明がそっけないと思っただろうか。再帰関数を書く上で重要な心は「その関数の結果を信じる」事である。 `fact(n-1)` は `n-1` の階乗である。なぜなら **自分がそう書こうとしているから** である。 `sort(lessEq)` は「先頭以下の要素が入った配列がソートされたもの」である。 `.sort(greater)` は「先頭より大きい要素が入った配列がソートされたもの」である。なぜならこの関数は **自分がそう書こうとしているから** である。
