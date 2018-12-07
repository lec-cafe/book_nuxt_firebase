# Step1. 環境構築

まずは、Nuxt でアプリケーションを作成するための環境構築から始めていきましょう。

Nuxt でアプリケーション開発を進める上で、現在最も一般的な方法は `npx` を利用した環境構築です。

npx は npm 5.2 以上がインストールされた端末で利用可能です。

```
$ npm -v 
```

`npx create-nuxt-app {project_name}` の形式でコマンドを実行すると以下のように
対話形式でプロジェクトの作成が進みます。

ここではデフォルトの設定で構わないので Enter を押していきましょう。

```
$ npx create-nuxt-app firebase-chat-app
npx: 402個のパッケージを51.795秒でインストールしました。
> Generating Nuxt.js project in /Users/mkkn/workspace/firebase-chat-app
? Project name firebase-chat-app
? Project description My fine Nuxt.js project
? Use a custom server framework none
? Use a custom UI framework none
? Choose rendering mode Universal
? Use axios module no
? Use eslint no
? Use prettier no
? Author name mikakane
? Choose a package manager npm
Initialized empty Git repository in /Users/mkkn/mine/kaigi/.git/

> fsevents@1.2.4 install /Users/mkkn/mine/kaigi/node_modules/fsevents
> node install

...
```

処理が完了すると 指定したフォルダ内に、nuxt のプロジェクトテンプレートが展開されます。

- `assets/` SCSS などJS 以外の組み込みファイルを格納するディレクトリ
- `components` Vue.js のコンポーネントを格納するディレクトリ 
- `layouts` Nuxt.js で利用するレイアウトのコンポーネントを格納するディレクトリ 
- `middleware` Nuxt.js で利用するミドルウェアを格納するディレクトリ 
- `node_modules` npm 経由でインストールされたNode のモジュールディレクトリ 
- `pages` URLを割り当てる ルート定義用の Vue.js コンポーネントを格納するディレクトリ
- `plugins` Nuxt.js で利用するプラグインファイルを格納するディレクトリ
- `static` Webに公開する画像やCSS 等を格納するディレクトリ
- `store` Vuex Store の定義ファイルを格納するディレクトリ
- `nuxt.config.js` Nuxt.js の設定ファイル

ファイルの構成が確認できたら `npm run dev` で開発用のサーバを立ち上げましょう。

```
$ npm run dev
```

コマンドを実行するとビルド処理が走り、最終的に `http://localhost:3000` のアドレスが画面上に表示されます。

実際にブラウザでアクセスして、Nuxt.js の初期画面が見えれば終了です。

