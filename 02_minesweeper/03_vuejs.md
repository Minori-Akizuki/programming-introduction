
## WEB ページの制作 : Vue.js

さて、これから本格的に WEB の開発を始めていく。
察しの通り WEB ページをイチから作るのは途方もなく面倒である。だが安心してほしい。世の中には WEB ページを作成するのができるだけ簡単になるような「フレームワーク」というものがいくつか存在する。その中でも今回は [Vue.js](https://jp.vuejs.org/index.html) というものを使って WEB ページを作成していく。

Vue の最大の特徴は「ページ要素のコンポーネント化」と「双方向バインディング」である。コンポーントと呼ばれる単位でページを管理し再利用が可能になる。また WEB ページ上に存在する要素の内容と JavaScript 上に存在する変数を簡単にリンクさせる事ができる。

昔はそれをどのようにやっていたかを説明するのはちょっと長くなるので割愛するが、とにかくまぁ死ぬ程めんどくさかったとだけ言っておこう。

ともかく、今は簡単に JavaScript とページの内容をリンクさせる事ができるし、要素の管理も簡単である。早速ページを作っていこう。

### インストール

まずはテンプレートを作成してくれるツールをインストールしよう。 powershell を起動し次のコマンドを打ち込む事。

```bash
npm install -g @vue/cli
```

`-g` オプションは特定の npm パッケージを node を解さずに実行できるようにしてくれるものだ。コマンドラインツール等をインストールする時に使用される。

プロジェクトを作りたいディレクトリに移動し以下のコマンドを入力するとプロジェクトディレクトリが作成されその中に必要なファイルが生成される。

```bash
vue create <プロジェクト名>
```

色々質問をされるので次のように答えていこう。

```bash
Vue CLI v4.5.15
? Please pick a preset: (Use arrow keys)
  Default ([Vue 2] babel, eslint)
  Default (Vue 3) ([Vue 3] babel, eslint)
> Manually select features # ←これ

? Check the features needed for your project:
 (*) Choose Vue version
 (*) Babel
 (*) TypeScript
 ( ) Progressive Web App (PWA) Support
 (*) Router
 ( ) Vuex
 ( ) CSS Pre-processors
 (*) Linter / Formatter
>(*) Unit Testing
 ( ) E2E Testing
# スペースでチェックを入れられるのでこのようにする

? Choose a version of Vue.js that you want to start the project with
  2.x
> 3.x # ← こっち

? Use class-style component syntax? (y/N) N # No にしておく

? Use Babel alongside TypeScript (required for modern mode, auto-detected polyfills, transpiling JSX)? (Y/n) Y
# yesにしておく

? Use history mode for router? (Requires proper server setup for index fallback in production) (Y/n) Y
# Yes にしておく

? Pick a linter / formatter config:
  ESLint with error prevention only
  ESLint + Airbnb config
> ESLint + Standard config  # これにする
  ESLint + Prettier
  TSLint (deprecated)

? Pick additional lint features:
 (*) Lint on save
>(*) Lint and fix on commit
# 両方入れておく

? Pick a unit testing solution:
  Mocha + Chai
> Jest # こっちにする

? Where do you prefer placing config for Babel, ESLint, etc.?
  In dedicated config files
> In package.json # こっちにする

? Save this as a preset for future projects? (y/N)
# どちらでもいいがセーブしておくと同じ設定でプロジェクトを作成できる

```

`npm run serve` で起動できると言われているのだが、実はできない。(Node 17.3.0 + vue 3.0.0 の場合) [Node が 17 になった時に OpenSSL が変更されている事に起因するものらしい](https://qiita.com/Ofutun/items/e257bd606e641473cbf8) 。

プロジェクトディレクトリに行き次のパッケージをインストールしよう。これは node 起動時の環境変数を設定できるようにするパッケージである。

```bash
cd <プロジェクト>
npm install --save-dev cross-env
```

そしたらプロジェクト内の `package.json` の中に `scripts` という項があるのでこう書き変える。

```json
  "scripts": {
    "serve": "cross-env NODE_OPTIONS=--openssl-legacy-provider vue-cli-service serve",
    "build": "cross-env NODE_OPTIONS=--openssl-legacy-provider vue-cli-service build",
    "test:unit": "cross-env NODE_OPTIONS=--openssl-legacy-provider vue-cli-service test:unit",
    "lint": "cross-env NODE_OPTIONS=--openssl-legacy-provider vue-cli-service lint"
  },
```

こうすればひとまずは起動する事ができる。改めて `npm run serve` と打てば8080番ポートからテンプレートとして作成された WEB ページを拝む事ができる。

```bash
  App running at:
  - Local:   http://localhost:8080/
  - Network: http://<ローカルIPアドレス>:8080/

  Note that the development build is not optimized.
  To create a production build, run npm run build.
```

Vue.js のロゴマークのあるページが出てきたら成功だ。このページを改変しながら少しずつ WEB 作成及び Vue.js についての理解を深めて行こう。

### プロジェクト構成

手順通りにプロジェクトを作成したら以下のファイルができあがっている筈だ。

