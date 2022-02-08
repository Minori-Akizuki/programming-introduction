## じゃんけんゲーム -設計、実装、テスト、コード管理-

ここから実際にプログラムを書きながら設計、実装、テスト、コード管理についての基本的な流れについて説明していく。

### パッケージ用のディレクトリ作成とコード管理

せっかくなのであたらしいディレクトリを作ろう。名前は「 `janken` 」とかでいいだろう。
そうしたらそのディレクトリを右クリックして「Codeで開く」を選択、 `Ctrl + Shift + @`  でターミナルを開き、 `npm init` を実行する。
前回のように色々聴かれるので、適当に入力しよう。

```bash
> npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help init` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (janken)
version: (1.0.0) 0.1.0
description: this is rock-paper-scissors
entry point: (index.js)
test command:
git repository:
keywords: game
author:
license: (ISC)
About to write to C:\home\minori\janken\package.json:

{
  "name": "janken",
  "version": "0.1.0",
  "description": "this is rock-paper-scissors",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "game"
  ],
  "author": "",
  "license": "ISC"
}


Is this OK? (yes)
```

さて、ここから git を使用して「コード管理」というのを行っていく。gitというのはソースコードの履歴の管理を行ってくれるもので、こまめに登録さえしていればいつでも以前登録した状態に戻せたり、branchという機能を使ってそれぞれの機能の開発を個別に行ってからまた合流させたりなんかできるのだが、gitについて詳しく解説しているとそれだけで分厚い本ができるのでとりあえず履歴の管理ができればいいものとし扱う。

// git の初期設定の話を書く

さて、ターミナルに戻って `git init` と打ちこもう。

```bash
PS C:\home\minori\janken> git init
Initialized empty Git repository in C:/home/minori/janken/.git/
```

これでこのディレクトリはgitの管理下に置かれるようになった。
左ペインの枝みたいなアイコン(以降「gitアイコン」と呼ぶ)に数字がでてきた事だろう。これをクリックすると「ソース管理タブ」というのが開ける。「changes」というタブの下に `package.json` が表示されている筈だ。
カーソルを合わせるとさらにアイコンが表示されるので、その中の「+」のアイコンをクリックしよう。

そうすると「Staged Changes」というタブに今クリックした `package.json` が移動する。この状態で上のテキストボックスに適当なコメント(英語が望ましいが、「package.jsonの生成」とかでいいだろう)を入れてそのまま `Ctrl + Enter` を押す。こうしてgitへの「ファイルの登録」が完了した。

gitではこのように以下の流れでファイルを管理する。

* ファイルを編集する
* 記録したいファイルを選択する(これを stage に上げるという)
* コメントと共に記録する(これを commit という)

わざわざファイルを先に選択するのは、機能の修正や追加など細かい単位でコミットができるようにするためだ。複数人で開発をする時とかはコミットの単位を厳密に意識しないと殺される。いや、マジで。

ともあれ最初のファイルがこれで記録された、試しに `package.json` に適当な文章を打ち込んでセーブしてみよう。そうするとまたgitアイコンに数字が表示され、クリックすると `package.json` が表示されている筈だ。

今度は矢印のアイコンに注目する。ホバーすると「Discard Changes」(変更を破棄する)と表示される通り、このファイルに行った変更を切り戻す事ができる。クリックすると警告が表示され、「Discard Changes」を選択するとファイルの状態が戻る筈だ。

何か機能を追加したりソースに変更を加えたら小まめにコミットする事。これを怠ると大量にコードを書いていざ動かそうとするとうまくいかず、どこが原因かわからないままその大量のコードを切り戻すなんて事が起りかねない。

### 外部パッケージのインストールと「管理しないコード」

さて、もうすこし準備がある。プロンプト上での対話入力を可能にする `inquirer` とテストを実行するために必要な `jest` というパッケージをインストールする。
パッケージのインストールは `npm` で行う。以下のコマンドをプロンプトにそれぞれうちこむ事

```bash
> npm i inquirer
```

```bash
> npm install --save-dev jest 
```

`--save-dev` オプションは開発用にライブラリを入れ、実際の実行の時には必要無い事を明示するオプションだ。
実行には少し時間がかかる。実行が終わると `node_modules` というディレクトリが生成されそこに各種ライブラリがインストールされる。

プロジェクトフォルダの中に大量にファイルが増えたので、gitアイコンの数字が大変な事になっている、がこれらのファイルはコミットしてはいけない。
ライブラリは外部の物であるし、その情報は `package.json` に記録されているので `npm install` と打てばそれらはすぐにインストールできる。
なので今インストールしたファイル群はソースコード管理の対象から除かなくてはいけない。

そこでファイル一覧(右ペインの一番上のアイコンから表示できる)から `.gitignore` というファイルを追加しよう。これはgit管理しないファイルやフォルダを指定しgitに認識させなくすることのできるファイルだ。
とりあえず以下のように書いてみよう。

```
node_modules/
```

これでソース管理タブの中身がすっきりした筈だ。
このようにプログラムには「管理するべきファイル」とそうでないファイルが存在する。 `node_modules` 以外にも沢山存在するので <https://www.gitignore.io/> で生成してしまおう。
ページに飛んでテキストボックスに node と入力してから「作成する」ボタンを押すと適切に生成された `.gitignore` ファイルの中身が表示される。このままコピペしよう。

ファイルに変更をしたのでコミットを忘れずに。まずは `.gitignore` だけを「.gitignore の追加」とでもしてコミットするのがおすすめだ。次にライブラリのインストールによって `package.json` と `package-lock.json` が変更追加されているので、「ライブラリの追加」としてこれらをコミットしよう。

### 設計と実装

タイトルにある通り、コンピュータとじゃんけんをするようなゲームを考えよう。
多くのプログラミング指南本だとここで「まず [フローチャート](https://www.google.com/search?q=%E3%83%95%E3%83%AD%E3%83%BC%E3%83%81%E3%83%A3%E3%83%BC%E3%83%88&sxsrf=AOaemvKr9Gc5ojFGxbDsnrwLBK3lSZpV2A:1642606858087&source=lnms&tbm=isch&sa=X&ved=2ahUKEwitkaCyk771AhXns1YBHQbRCB0Q_AUoAXoECAoQAw&biw=1476&bih=865&dpr=1) を書きましょう」と言われるのだが、 **それは罠だ** 。
フローチャートというのはプログラムのアルゴリズムを考える時によく使われる「流れ図」なのだが先に書いた通りプログラムは「上から実行される」「分岐、くりかえし、サブルーチンの制御構文がある」のという原則で動いている。

> 昔の言語には `GOTO` というプログラム内のどこにでも飛べる構文があったがそれを使ってしまうとプログラムが著しく読みづらくなってしまうため現在では禁忌とされている。

フローチャートは「図」であるため、上下左右どこにでも処理を書く事ができる。素人がこれを書くと地図みたいな図ができあがりプログラムに落すのがあまりにも困難なものになる。
なので以下のようにテキストで箇条書きにするのがのぞましい。

* 起動するとゲームがはじまる
* じゃんけんをする
  * プレイヤーの手の入力を促す
  * コンピュータの手をランダムに決める
  * 入力された手とコンピュータの手を比較して勝敗を求める
  * 結果を出力する
  * もういちどやるか尋ね入力を促す
  * もういちどやるのであれば再度じゃんけんが始まる。
* プログラムを終了する

考えなければいけない事がいくつもある。

* 入力をさせるにはどうすればいいか
* じゃんけんの勝敗の判定はどうすればいいか
* プログラムをループさせて、好きなタイミングでやめるにはどうしたらいいか

等々。とりあえずは作りかたをイチから指南するのでその通りに作ってみるといい。

#### 概要設計

さて、これから具体的なプログラムを書いていくのだが、プログラムを書くにあたって、最初にどんな処理をするかをメモしておくといい。ここの分解の単位をどうするかがプログラムの腕の見せ所の一つでもある。 `index.js` を用意したらとりあえずこんな感じに書いてみよう。

```JavaScript
//  くりかえし
  // プレイヤーの手をうけとる
  // コンピュータの手を決める
  //  勝敗を求める
  //  勝敗を出す
  // 再戦するかを聞く
// くりかえしおわり
```

#### 入力のうけとりかた

まず何を持ってしてもユーザーの入力を受けとれるようにしなければいけない。実は JavaScript は元々ブラウザで動かす言語であったため「入力」という概念が標準で存在しない。
であるが `npm init` で使われているようにコンソールで動く以上その仕組みは存在する。今回は [inquirer](https://www.npmjs.com/package/inquirer) というライブラリを使おう。

JavaScript で使用されるライブラリは「パッケージ」と呼ばれ、それらはすべてリンク先の「npm」というサイトで管理されている。使用方法もここに書いてあるのだが、大体は英語である。ある程度英語が読めなければいけない。
まぁ今時翻訳ソフトも充実しているので言う程困る事はない。

さて、とりあえず「Installation」の項を見てみよう。インストールは先にやっているので使用例を見る。

> ```JavaScript
> var inquirer = require('inquirer');
> inquirer
>   .prompt([
>     /* Pass your questions in here */ // 訳 : ここに質問を渡す
>   ])
>   .then((answers) => {
>     // Use user feedback for... whatever!! 訳 : ユーザーの入力を利用して……何でも(書ける)！
>   })
>   .catch((error) => {
>     if (error.isTtyError) {
>       // Prompt couldn't be rendered in the current environment  訳 : プロンプトが描画できなかった時のエラー
>     } else {
>       // Something else went wrong  訳 : その他の間違い
>     }
>   });
> ```

正直これだけじゃサッパリである。少し下にいくと「[Methods](https://www.npmjs.com/package/inquirer#methods)」という項目があるので見てみよう。ちなみに「method」というのはオブジェクトの中のメンバ関数を得にこう呼ぶ。

> Methods
>
> inquirer.prompt(questions, answers) -> promise
>
> Launch the prompt interface (inquiry session)
>
> questions (Array) containing *Question Object* (using the *reactive interface*, you can also pass a Rx.Observable instance)
> answers (object) contains values of already answered questions. Inquirer will avoid asking answers already provided here. Defaults {}.
> returns a Promise

とりあえずこいつの使いかたを覚えておけばよさそうだ。第1引数に「[Question Object](https://www.npmjs.com/package/inquirer#question)」なるものの配列を渡し、第2引数にはデフォルトで利用したい回答を格納してもよい。そして「 `promise` 」なる物が帰ってくるらしい。[使用例](https://github.com/SBoudrias/Inquirer.js/blob/master/packages/inquirer/examples/pizza.js) を見ればなんとなく使いかたがわかる。 `promise` が帰ってきたらその後に `.then(callbackFunction)` という形でメソッドを呼び、その中に実行したい処理を入れればいい。

`promise` を返す関数 `retuenPromise()` があった場合の基本的な使いかたはこうだ。 `then` の中の `value` には `promise` が生成した何かしらの値が渡ってくる。

```JavaScript
returnPromise(/* 引数があれば渡す */).then((value) => { /*後続処理*/ })
```

> このように、関数を実行した結果がまたメソッドを持った物である場合にそのメソッドを連ねて書く事を「メソッドチェーン」と呼んだりもする。

なぜこんな書き方をするのかというと、 `promise` というのは基本的に時間のかかる処理を非同期に行うためのしくみだからだ。関数が実行されてもその結果を待たず、実行が完了したら `.then()` 以下の処理を実行するようになっている。

あるいは、 `promise` についてはもう一つ使いかたがある。 `await` というキーワードを使い以下のように書くと `promise` の処理結果を直接変数に入れる事ができる。

```JavaScript
value = await returnPromise()
```

ただし、この書き方はそれ自体が「非同期関数の中」、つまり `promise` を返すような関数でしか使用できないので注意。なので、最初のプログラムにしては不相応なやりかたでこれを使用する。 `index.js` に書いていたメモを以下の文言で囲む事。

```JavaScript
;(async () => {
  //  くりかえし
    // プレイヤーの手をうけとる
    // コンピュータの手を決める
    //  勝敗を求める
    //  勝敗を出す
    // 再戦するかを聞く
  // くりかえしおわり
})()
```

`async` というキーワードはその関数が非同期関数である事を示している。
非同期の関数を一つ作成して、最後に `()` をつけてその関数を即座に実行している。
昔は別の用途でよく使われていた手法であるのだが、今回は無理矢理非同期の関数をトップレベルで実行するのに使用する。この中にメインの処理を書いてしまおう。

#### プログラムを書いていく

さて、これから少しずつプログラムを書いていく。
まずくりかえしと書いてあるのでくりかえす文を書こう。何が適当だろうか。回数が決ってる訳ではないので `while` 文が適当だろうか。とりあえず書いておくが無限ループになってしまうと困るのでフラグでも立てておこう。きっと再戦するか聞いた時にフラグを `false` にするから

> 「フラグ」という言葉にピンと来なくても、「死亡フラグ」という言葉は聞いた事があるだろう。「フラグ」とは「ある条件が満されたか」という値を保持しておく変数である。

```JavaScript
;(async () => {
  //  くりかえし
  let flag = true
  while(flag){
    // プレイヤーの手をうけとる
    // コンピュータの手を決める
    // 勝敗を求める
    // 勝敗を出す
    // 再戦するかを聞く
    flag = false
  }
  // くりかえしおわり
})()
```

プレイヤーの手をうけとれるようにしなければいけない。先に出てきた `inquirer` を使おう。
具体的な入力は後から考えるとして、とりあえず質問がでてくるしくみだけ書いておこう。何が入力されたのか見るためにログをとりあえず入れておこう。これは後で消す。

```JavaScript
const inquirer = require('inquirer')

;(async () => {
  //  くりかえし
  let flag = true
  while(flag){
    // プレイヤーの手をうけとる
    let answer = await inquirer.prompt([
      {
        type: 'input',
        name: 'hand',
        message: '質問が入る',
      }
    ])
    console.log(answer)
    // コンピュータの手を決める
    // 勝敗を求める
    // 勝敗を出す
    // 再戦するかを聞く
    flag = false
  }
  // くりかえしおわり
})()
```

……うーん。 `prompt` に渡すオブジェクトが直接書いてあるのは見通しが悪い。どうせ定数なのだから外に出してしまおう。

```JavaScript
const inquirer = require('inquirer')

const question = {
  type: 'input',
  name: 'hand',
  message: '質問が入る',
}

;(async () => {
  //  くりかえし
  let flag = true
  while(flag){
    // プレイヤーの手をうけとる
    let answer = await inquirer.prompt([
      question
    ])
    console.log(answer)
    // コンピュータの手を決める
    // 勝敗を求める
    // 勝敗を出す
    // 再戦するかを聞く
    flag = false
  }
  // くりかえしおわり
})()
```

ここまで作ったら一度動かしてみるといい。
ちゃんとプロンプトから入力をうけとれてるだろうか。

```bash
> node index.js
? 質問が入る hogehoge
{ hand: 'hogehoge' } 
```

入力は自由入力じゃなくてちゃんと選択肢を与えた方がいいかもしれない。
ただ「グー」とかそういう文字列が帰ってきても処理に困るから、リスト選ばせて数字でも返してくれるような使いかたは無いだろうか。

思い出してほしい。「 **ほしいと思ったものは、大体誰かが作ってる。** 」

`linquirer` の [Question](https://www.npmjs.com/package/inquirer#question) の項を見ると、選択肢を記述する `choices` のところにこう書いてある。

> choices: (Array|Function) Choices array or a function returning a choices array. If defined as a function, the first parameter will be the current inquirer session answers.
> Array values can be simple `numbers`, `strings`, or `objects` containing a `name` (to display in list), a `value` (to save in the answers hash), and a short (to display after selection) properties.

なるほど。 `name` と `value` を持つオブジェクトを `choises` に入れておけば表示名と返却値を分ける事ができるらしい。

```Javascript
const inquirer = require('inquirer')

const hands = [
  { name: 'グー', value: 0 },
  { name: 'チョキ', value: 1 },
  { name: 'パー', value: 2 },
]

const question = {
  type: 'list',
  name: 'hand',
  message: 'あなたの手は？',
  choices: hands
}

;(async () => {
  //  くりかえし
  let flag = true
  while(flag){
    // プレイヤーの手をうけとる
    let answer = await inquirer.prompt([
      question
    ])
    console.log(answer)
    // コンピュータの手を決める
    // 勝敗を求める
    // 勝敗を出す
    // 再戦するかを聞く
    flag = false
  }
  // くりかえしおわり
})()
```

これで実行すると以下のようになる。選択肢は選んだら消えてしまうので結果しか残っていないがちゃんとリスト表示されている。

```bash
> node index.js
? あなたの手は？ パー
{ hand: 2 }
```

このように少しずつ見えるところから実装しながら進めていくというのがいいだろう。

#### テスト

つぎにコンピュータの手を決めるところを書こう。これは少し書く事が多そうだ。
ランダムで手を決めなければいけない、手は3種類のどれかが絶対に出ないといけない。

このように少しでも考える事が増えたら、 **その処理をそのままそこに書こうとしてはいけない** 。
具体的にはそのための関数を別途作成し、さらにその関数の正当性を確保するための **テスト** を書かなければいけない。

という訳でここで自分でじゃんけんのためのライブラリを作ろう。 `janken.js` というファイルを作成し、そこにコードを書いていく。
まずは適当に「ガワ」を書こう。

```JavaScript
const choiceCpuHand = () => {
  return 1
}

module.exports = {
  choiceCpuHand
}
```

上の関数はこれから「コンピュータの手をランダムに決める」関数になる予定の関数である。ここで重要なのは下の `module.exports` である。
これを書くと `inquirer` を使った時と同様に `import` 文でここに書かれた関数が使用できるようになる。もういちど `index.js` に戻って次の記述を足してみよう。

```JavaScript
const inquirer = require('inquirer')
const janken = require('./janken')   // ← ここと

const hands = [
  { name: 'グー', value: 0 },
  { name: 'チョキ', value: 1 },
  { name: 'パー', value: 2 },
]

const question = {
  type: 'list',
  name: 'hand',
  message: 'あなたの手は？',
  choices: hands
}

;(async () => {
  //  くりかえし
  let flag = true
  while(flag){
    // プレイヤーの手をうけとる
    let answer = await inquirer.prompt([
      question
    ])
    console.log(answer)
    // コンピュータの手を決める
    let cpuHand = janken.choiceCpuHand()  // ← ここと
    console.log(`cpuHand  : ${cpuHand}`)  // ← ここ
    // 勝敗を求める
    // 勝敗を出す
    // 再戦するかを聞く
    flag = false
  }
  // くりかえしおわり
})()
```

実行してみると確かに `cpuHand  : 1` という記述が追加されているのがわかる。これでこの関数がきちんと機能しているのがわかった。
さて、じゃあこれから `choiceCpuHand` の実装をする……訳ではない。この関数のテストをまず作る方がいい。関数のテストとはそのまま関数の仕様になり、また正当性のチェックを用意に可能にする。

……といっても今回はランダムな値を出力するので厳密なテストができない。ちゃんとしたテストの作りかたは勝敗判定の時に語るとして、今回は「テストというしくみがある」事だけ覚えてもらえばいい。

という訳でプロジェクトに `test` というディレクトリを作成し、その中に `janken.test.js` というファイルをまず作ろう。ついでに `package.json` を書き変えよう。 `test` の項に書いてある文言を `jest` に差し替える。

```json
{
  "name": "janken",
  "version": "0.1.0",
  "description": "this is rock-paper-scissors",
  "main": "index.js",
  "scripts": {
    "test": "jest"
  },
  "keywords": [
    "game"
  ],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "inquirer": "^8.2.0"
  },
  "devDependencies": {
    "jest": "^27.4.7"
  }
}

```

ここで一回コンソールで `npm test` と実行してみよう。エラーメッセージが出てくるだろうか。

```bash
> janken@0.1.0 test
> jest

 FAIL  test/janken.test.js
  ● Test suite failed to run

    Your test suite must contain at least one test.
```

恐れる事はない。「実行するべきテストが無い」と言われているだけだ。このようなメッセージが出てこなかったら jest のインストールができていない。
最初の方に書いてしまったから忘れてるかもしれないが次のコマンドでインストールできる。

```bash
npm i --save-dev jest
```

> [jest](https://jestjs.io/ja/) とは JavaScript でのテストを簡単に行えるようにしてくれるフレームワークである。
> `test.js` という文字列が入ったファイルを用意しておけばそれらのテストを全て実行してくれ、エラーがあったら検出してくれる。
> もちろんきちんとしたテストを書くというのはそれだけで大変だ。テストとはその関数の仕様でありプログラムの完成度に大きく影響を及ぼす物である。

さてテストが無いといわてはしょうがない。テストを書こう。 `janken.test.js` に戻ってこのようなコードを書く。

```JavaScript
test('コレはモックです', () => {
  expect(1).toBe(1)
  console.log('ちゃんと実行されていますか？')
})
```

テストの構文を覚えてもらうためになにもしないテストを書いた。

まずテストをするためにはコードの中に `test` という関数を書く。第1引数はそのテストがどのようなテストを行っているかの注釈を書く場所で、内容がわかればなんでもいい。
第2引数に具体的なテストを書く。基本的には「アサーション」というものを書き連ねていく。関数が正当な値を返しているかのチェックだ。 `expect(1).toBe(1)` のところがそうで、
本来はこの `expect` という関数の中にテストしたい関数や結果の変数を入れる。 `.toBe(value)` は実行結果の値が渡された値それそのものであるかをチェックする関数である。
このようにテストとして関数等を実行した結果を検査するものを「アサーション」と呼ぶ。

再び `npm test` とやると以下のような表示が出るはずだ。

```bash
> npm test

> janken@0.1.0 test
> jest

  console.log
    ちゃんと実行されていますか？

      at Object.<anonymous> (test/janken.test.js:3:11)

 PASS  test/janken.test.js
  √ コレはモックです (13 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.363 s
Ran all test suites.
```

テストが実行されて、ちゃんと合格している。試しに記述をこう変えてみる。

```JavaScript
test('コレはモックです', () => {
  expect(2).toBe(1)
  console.log('ちゃんと実行されていますか？')
})
```

```bash
> npm test

> janken@0.1.0 test
> jest

 FAIL  test/janken.test.js
  × コレはモックです (2 ms)

  ● コレはモックです

    expect(received).toBe(expected) // Object.is equality

    Expected: 1
    Received: 2

      1 |
      2 | test('コレはモックです', () => {
    > 3 |   expect(2).toBe(1)
        |             ^
      4 |   console.log('ちゃんと実行されていますか？')
      5 | })

      at Object.<anonymous> (test/janken.test.js:3:13)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 1 total
Snapshots:   0 total
Time:        0.369 s, estimated 1 s
Ran all test suites.
```

2は1ではないので当然これは失敗する。ご丁寧にどこで失敗したかも教えてくれる。 Jest はかなりエラーメッセージが丁寧なので、英語がちゃんと読めればすぐになにが失敗しているかがわかる。

#### テストファーストという考えかた

さて、テストの書きかたがわかったのでテストを書こう。 **関数本体の実装より先に** 、だ。
何度か書いているが、テストとは仕様でありその関数の正当性をチェックするためのものである。テストを書きながらその関数はどんな仕様なのかを考え、テストを実行しながらその正当性を証明し、
テストが書きづらいと思ったらそれは関数の設計が綺麗に分割されてない証である。よって **テストは何よりも優先される** 。

これを「テストファースト」或いは「テスト駆動開発(Test-Driven Development : TDD)」と呼ぶ。  ~~1人で開発してるとそんな余裕も無かったりするのだが。~~

#### テストを書く

テストを書くというのは仕様を決めるという事だ。だからまず「CPUの手を決める」という関数の仕様を決めよう。

* 実行するとランダムに0から2の整数が出てくる

一言で言うとこれなのだが、これをもっと掘り下げなければいけない。

1. 実行した結果は「0, 1, 2」のどれかれあり、 **それ以外であってはいけない**。 
1. 「0, 1, 2」の **どの手も出る可能性がある必要がある**
1. どの手が出るかはわからない

こんなところだろうか。ちょっと3番のランダム性をテストするのはむずかしい。というよりはランダムな値を出す手法はライブラリにありその性能は担保されているからテストする必要が無い。だから1と2に絞ってテストを行うことにする。ただし今回はすごい適当だ。ランダム性を持つ入出力をテストするのは難易度が高いからだ。

今回は適当に以下のようなテストを実行することにする。

1. 関数を大量に(適当に100回ぐらい)実行する
1. それぞれについて0,1,2のどれかの値が出ているか確認する。それ以外の値が出てきたらテスト失敗とする
1. 0,1,2全ての値が出ているか確認する。出ていない数値があったらテスト失敗とする。

> 本来はここで乱数に「シード値」というのを与え出てくる乱数を固定するという手法をとるのだが、JavaScriptの乱数関数にはシードをセットする機能が無い。他の言語でこのようなテストを行う時はきちんとシードを設定して乱数の固定を行った上でテストする事。

という訳でテストを実装していく。もっと効率のいい方法があるのだが今回は適当にこんな感じに実装する。

```JavaScript
const janken = require('../janken')

test('choiceCpuHand', () => {
  let hands = new Array(100)
  hands.fill(0)
  let isExists0 = false
  let isExists1 = false
  let isExists2 = false
  hands.forEach(() => {
    hand = janken.choiceCpuHand()
    switch(hand){
      case 0:
        isExists0 = true
        break
      case 1:
        isExists1 = true
        break
      case 2:
        isExists2 = true
        break
      default:
        throw new Error('choiceCpuHand が想定外の値を出しました');
    }
  })
  expect(isExists0).toBe(true)
  expect(isExists1).toBe(true)
  expect(isExists2).toBe(true)
})
```

そういえば。 `switch` 構文の説明をしていなかった。

```javascript
switch(value){ // 判定したい値を書く
  case 0: // 値 value がこの値の時の処理を書く
    isExists0 = true
    // 処理はいっぱい書ける
    break // 最後にこの文言を入れる
  case 1:
    isExists1 = true
    break
  case 2:
    isExists2 = true
    break
  default: // どの値にも該当しなかった場合の処理をこの後に書く
    throw new Error('choiceCpuHand が想定外の値を出しました');
}
```

`switch(/*判定したい値*/)` と書いて、その後に値毎に処理したいコードを書く。 `if(){ ... }else` を連ねた物のかわりとして使うと見通しが良い。

ちなみに `throw` とは「例外」を投げる構文である。これはプログラムが異常な行動を起した事を明示的に伝える手段である。

さて、さっきのテストコードに戻ろう。

```JavaScript
const janken = require('../janken') // インポート元のファイルは相対パスで書かなければいけないためこうなる。

test('choiceCpuHand', () => {
  let hands = new Array(100) // 要素100の配列を用意する
  hands.fill(0) // なんでもいいので値を埋める
  let isExists0 = false // フラグを定義する
  let isExists1 = false
  let isExists2 = false
  // handsの要素文だけ以下を実行する。つまり100回実行される
  hands.forEach(() => {
    hand = janken.choiceCpuHand() // 関数を実行する
    switch(hand){
      case 0: // 0が出てきたら
        isExists0 = true // フラグを立てておく
        break
      case 1: // 同様
        isExists1 = true
        break
      case 2:
        isExists2 = true
        break
      default: // 0,1,2 以外の値を返したら例外を投げる。
        throw new Error('choiceCpuHand が想定外の値を出しました');
    }
  })
  expect(isExists0).toBe(true) // 途中で0が出てきたか確認する。以下同様。
  expect(isExists1).toBe(true)
  expect(isExists2).toBe(true)
})
```

さて、テストができたのでテストをしよう。

```bash
> npm test

> janken@0.1.0 test
> jest

 FAIL  test/janken.test.js
  × choiceCpuHand (3 ms)

  ● choiceCpuHand

    expect(received).toBe(expected) // Object.is equality

    Expected: true
    Received: false

      24 |     }
      25 |   })
    > 26 |   expect(isExists0).toBe(true)
         |                     ^
      27 |   expect(isExists1).toBe(true)
      28 |   expect(isExists2).toBe(true)
      29 | })

      at Object.<anonymous> (test/janken.test.js:26:21)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 1 total
Snapshots:   0 total
Time:        0.371 s, estimated 1 s
Ran all test suites.
```

失敗した。当然だ。`choiceCpuHand`はいま1しか返してこない。
このように **まずテストを作ってこれを合格するように関数を実装する** というのが「テスト先行開発」である。

#### 関数の実装

さて、今 `janken.js` にはこれしか書いていない。

```JavaScript
const choiceCpuHand = () => {
  return 1
}

module.exports = {
  choiceCpuHand
}
```

これを具体的に実装するとこうなる。

```JavaScript
const choiceCpuHand = () => {
  let hand = Math.floor(Math.random()*3)
  return hand
}

module.exports = {
  choiceCpuHand
}
```

`Math` というのは数学関連の関数が入ってるライブラリである。これは npm パッケージではなく JavaScript の標準ライブラリなのでインポートの必要が無い。
[`Math.random()`](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Math/random) は0以上1未満の値をランダムで返す関数である。 [Math.floor()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Math/floor) は与えられた値 **以下** の整数を返す関数である。(つまりマイナスの値は方向に丸められる)

まぁ実装してしまえばなんてことはなく1行ですんでしまったのだが、それでも **明確な機能を持った単位は分離する** という考えかたはとても重要である。
分離しておけばテストも実行できる。さて先程のテストをまた実行してみよう。

```bash
> npm test

> janken@0.1.0 test
> jest

 PASS  test/janken.test.js
  √ choiceCpuHand (2 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.356 s, estimated 1 s
Ran all test suites.
```

どうやら通った。いいかんじである。

#### 実装を進める上で

さて、今度は「コンピュータの手と入力された手を判定して勝敗を返す」という関数を作らなければいけない。
先程と同様に「ガワ」だけ書こう。入力はどうするべきか、出力はどうするべきか、それを考えながら関数を書いていく。思い出してほしい。関数でまず決めるのは「入力と出力」である。つまり **型** である。

```JavaScript
const choiceCpuHand = () => {
  let hand = Math.floor(Math.random()*3)
  return hand
}

const judgeJankenHand = (playerHand, cpuHand) => {
  return 0
}

module.exports = {
  choiceCpuHand,
  judgeJankenHand
}
```

関数が増えてきたので説明を書いておきたい。関数の上にキャレットを持っていき、 `/**` と入力しエンターを押そう。こんなコードが補完されるはずだ。

```JavaScript
const choiceCpuHand = () => {
  let hand = Math.floor(Math.random()*3)
  return hand
}

/**
 * 
 * @param {*} playerHand 
 * @param {*} cpuHand 
 * @returns 
 */
const judgeJankenHand = (playerHand, cpuHand) => {
  return 0
}

module.exports = {
  choiceCpuHand,
  judgeJankenHand
}
```

これは「 [JSDoc](https://ja.wikipedia.org/wiki/JSDoc) 」と呼ばれるもので、関数の説明を書く時に使われる構文である。全体としてコメントであるためコードの動作には寄与しないが、
IDEで見た時にここに書いた説明を表示してくれたりパッケージのドキュメントの自動生成に使われたりする。できるだけ書いておこう。

そもそも「どの数字がどの手か」とか、「勝敗はどんな数値で表されるか」とか、そのあたりとかがぼんやりとしか決まっていない。
仕様を明確にするためにもこういったメモを残したり、なんでもない数値でも定数として定義したりするのが大事である。
なのでこれらのコードにちょっとコメントや定数を書き足していく。

> 数字に意味を持たせたものを数字のまま書く事は「マジックナンバー」と言われコードの可読性を低めるという意味でよくないものとされている。

```JavaScript
/**
 * じゃんけんで用いられる手
 */
const hands = {
  /** グー */
  rock: 0,
  /** チョキ */
  scissors: 1,
  /** パー */
  paper: 2
}

/**
 * じゃんけんの勝敗
 */
const decided = {
  /** プレイヤーの勝ち */
  win: 1,
  /** プレイヤーの負け */
  lose: -1,
  /** あいこ */
  draw: 0
}

/**
 * ランダムにCPUの手を生成する。
 * @returns 0:グー、1:チョキ、2、パー をランダムに返す
 */
const choiceCpuHand = () => {
  let hand = Math.floor(Math.random()*3)
  return hand
}

/**
 * プレイヤーの手とCPUの手を比べて勝敗を返す
 * @param {Number} playerHand プレイヤーの手
 * @param {Number} cpuHand CPUの手
 * @returns 勝敗 desiced
 */
const judgeJankenHand = (playerHand, cpuHand) => {
  return decided.draw
}

module.exports = {
  choiceCpuHand,
  judgeJankenHand,
  hands,
  decided
}


```

適当な場所にこれらの関数や定数をうちこもうとしてみるといい。ちゃんと書いた説明が表示されるだろう。このようにコードにコメントを残すのはとても重要である。

さて、今度は `judgeJankenHand` のテストを書いていく。これはきちんと考えなければいけない事が多い。

* プレイヤーが勝ちのパターン
* プレイヤーが負けのパターン
* あいこのパターン
* 定義されていない手が入力されたパターン

どうせふたりのじゃんけんなのでパターンは高々9通りしか無い。全てのパターンについて書いてしまうのがいいだろう。
とはいえ全てのパターンに対してテストを書くのはちょっとしんどい。なにかいいものはあるだろうか。 **ある** 。

[`test.each()`](https://jestjs.io/ja/docs/api#testeachtablename-fn-timeout) というものが用意されている。これは渡す値と結果の値を書けばテストをそれぞれ実行してくれるものだ。
サイトに書いてある使用例のうち一つをを以下に引用する。これが一番わかりやすいだろう。

> ```JavaScript
> test.each([
>   {a: 1, b: 1, expected: 2},
>   {a: 1, b: 2, expected: 3},
>   {a: 2, b: 1, expected: 3},
> ])('.add($a, $b)', ({a, b, expected}) => {
>   expect(a + b).toBe(expected);
> });
> ```

やってる事が数段階あるので丁寧に分解しながら解説する。

```JavaScript
test.each([
  {a: 1, b: 1, expected: 2},
  {a: 1, b: 2, expected: 3},
  {a: 2, b: 1, expected: 3},
])
```

この部分全体が「渡された変数に従ってテストを実行する関数」を返す。 `test.each()` 全体が関数となるので、(これを仮に `t` としよう。)次に書いてある引数に従ってテストを実行する。

```JavaScript
t('.add($a, $b)', ({a, b, expected}) => {
  expect(a + b).toBe(expected);
});
```

`test.each()` が返してくる関数は渡された配列オブジェクトを元に第1引数のメッセージを補完し、第2引数のテスト本体を実行する。

ちなみに下記のような書き方をすると関数は渡されたオブジェクトの中身を直接参照できる。上記のテストメソッドはその方式で書かれている。

```JavaScript
let obj = {
  value: 10,
  message: 'hello'
}

f = function({message}){
  console.log(message)
}

f(obj) // hello と表示される
```

という訳でこのメソッドをつかってテストを書こう。

```JavaScript
test.each([
  { p: janken.hands.rock,     c: janken.hands.rock,     expected: janken.decided.draw },
  { p: janken.hands.rock,     c: janken.hands.paper,    expected: janken.decided.lose },
  { p: janken.hands.rock,     c: janken.hands.scissors, expected: janken.decided.win  },
  { p: janken.hands.paper,    c: janken.hands.rock,     expected: janken.decided.win  },
  { p: janken.hands.paper,    c: janken.hands.paper,    expected: janken.decided.draw },
  { p: janken.hands.paper,    c: janken.hands.scissors, expected: janken.decided.lose },
  { p: janken.hands.scissors, c: janken.hands.rock,     expected: janken.decided.lose },
  { p: janken.hands.scissors, c: janken.hands.paper,    expected: janken.decided.win  },
  { p: janken.hands.scissors, c: janken.hands.scissors, expected: janken.decided.draw }
])('自分の手 : $p 、相手の手 : $c', ({p, c, expected}) => {
  expect(janken.judgeJankenHand(p, c)).toBe(expected)
})

test.each([
  { p: 4,  c: janken.hands.rock},
  { p: -1, c: janken.hands.rock},
  { p: janken.hands.rock, c: 4 },
  { p: janken.hands.rock, c: -1}
])('自分の手 : $p 、相手の手 : $c (例外検知)', ({p, c}) => {
  expect(() => {janken.judgeJankenHand(p, c)}).toThrow()
})
```

通常のパターンは高々9パターンしか無いので全部書き出した(こういう「普通の動きをする予定のテスト」を「正常系」と呼ぶ)。それと異常値を検出した場合は例外が投げられる事にする(こういう「想定外の引数や状態を検査するテスト」を「異常系」と呼ぶ)。 `.toThrow()` は例外が投げられる事を検知するアサーションだ。

> このように jest には様々な方法のテストやアサーションをするためのメソッドが用意されている。是非 jest の [全体](https://jestjs.io/ja/docs/api) や [Expect](https://jestjs.io/ja/docs/expect) のページを眺めてどんなテストができるか見てみよう。

さて、このままだともちろんテストは失敗する。テストが通るように関数を実装していこう。
テストが実装されているので、逆に言えば関数の実装自体はテストが通ればなんでもいい。なぜならテストは仕様でありテストの結果が全てであるからである。
だからこそテストは厳密に書く必要があり要件を漏らしてはいけない。

----

演習 : これまで学んできた事で十分にこの関数は実装できる。関数 `judgeJankenHand` の実装を完成させよ。

----

> 十分なものが書けたと思ったら `npm test` のかわりに `npm test -- --coverage` と入力してテストを実行してみよう。「カバレッジ」というものが表示される。これはテストの結果テスト対象のうち実行されたコードがどのくらいあるかを示すものだ。もしテストが正常に通っているのにコード内に実行されていない行があったとすればそれはテストが不十分かデッドコード(必要の無い無駄なコード)があるかのどちらかである。とはいえ大きなプロジェクトでは完璧なテストを書くのがそもそも難しく通常カバレッジは 80% 以上程度あれば上々とされる。

そうしたら後は勝敗を出して、続けるかどうかを聞きたらそれにそってループ条件となるフラグを操作するだけである。最初にやった事とおなじような事しかしないので詳細は割愛し、完成した `index.js` の例を以下に示す。変数名等は見やすく変えているところもある。

```JavaScript
const inquirer = require('inquirer')
const janken = require('./janken')

const hands = [
  { name: 'グー', value: 0 },
  { name: 'チョキ', value: 1 },
  { name: 'パー', value: 2 },
]

const questionHand = {
  type: 'list',
  name: 'hand',
  message: 'あなたの手は？',
  choices: hands
}

const questionContinue = {
  type: 'list',
  name: 'continue',
  message: '続ける？',
  choices: [
    { name: '続ける', value: true },
    { name: 'やめる', value: false }
  ]
}

;(async () => {
  //  くりかえし
  let flag = true
  while(flag){
    // プレイヤーの手をうけとる
    let answerHand = await inquirer.prompt([
      questionHand
    ])
    // コンピュータの手を決める
    let cpuHand = janken.choiceCpuHand()
    console.log(hands[cpuHand].name + '!')
    // 勝敗を求める
    let judge = janken.judgeJankenHand(answerHand.hand, cpuHand)
    // 勝敗を出す
    switch(judge){
      case janken.decided.win:
        console.log('あなたの勝ち！')
        break
      case janken.decided.lose:
        console.log('あなたの負け！')
        break
      case janken.decided.draw:
        console.log('あいこ！')
    }
    // 再戦するかを聞く
    let answerContinue = await inquirer.prompt([
      questionContinue
    ])
    flag = answerContinue.continue
  }
  // くりかえしおわり
})()
```

単純なプログラムだが初歩に必要な事はこれらのプログラムに大体詰っている。基本的な制御構文、関数、機能のライブラリ化、そしてテスト。おめでとう！これでプログラムの大きな一歩を踏み出せた！
