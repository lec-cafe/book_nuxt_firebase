# Step2. ページの作成

環境構築が終わったら、アプリケーションを作成するにあたっての画面を作成してみましょう。

## SPAルートの作成

Nuxt.js の初期画面は `/pages/index.vue` によって作成されています。

Vue.js で利用される この `.vue` のファイルは、
template / script / style の ３つの要素からなるファイルで、
HTML / JS / CSS を一つのファイルにまとめて記述することができます。

`/pages` フォルダに vue ファイルを作成すると、ページとしてそれを利用することができます。
vue ファイルの URL がそのまま ページのアドレスとなるため、直感的にページを作成していくことができます。

`/pages/about.vue` を作成すれば、 `http://localhost:3000/about`、 
`/pages/service/main.vue` を作成すれば、 `http://localhost:3000/service/main`、
といった具合に任意のアドレスで SPA ルートのページを作成することが可能です。

### SPAルートの遷移

複数ルートを作成した場合、ページ感の遷移に `router-link` 要素を用いることができます。

```
<router-link to="/about"> ABOUT </router-link>
```

`router-link` 要素によるページ遷移は、 SPA のルート遷移として認識され、
画面の差分更新による高速でなめらかなページ遷移動作が可能です。

外部リンクなど SPA ルート外の　ページ遷移には通常通り a 要素を利用した ページ遷移を記述します。

## レイアウトの作成

全てのページは レイアウト と呼ばれる `layout/default.vue` ファイルを経由して描画されます。

複数のルートを作成しても、`layout/default.vue` に記述された内容は全てのページで展開され、
ページごとに記述したコンポーネントの内容は レイアウトファイルの `<nuxt/>` 内に展開されます。

layout も ページと同じ様に Vue コンポーネントです。
通常は 全ての ページコンポーネントで `layout/default.vue` がレイアウトとして利用されますが、
任意のレイアウトファイルを作成して切り替えることも可能です。

例えば、`layout/mypage.vue` を作成した場合、
ページ側のコンポーネントの script セクションで、以下のように記述することでこのレイアウトを利用することができます。

```
<script>
export default {
  layout: "mypage",
  ...
}
</script>
```

## 画面のデザイン、コーディング

ルートが作成できたところで、画面のデザイン・コーディングを行っていきましょう。

Nuxt.js で 画面に CSS を適用する方法はいくつかのやり方があります。

### グローバルにCSS を適用する

HEAD 要素内の LINK 要素を経由して、Bootstrap などの CSS を適用する場合、
nuxt.config.js の `head`セクションを調整します。

```
  head: {
    title: pkg.name,
    meta: [
      { charset: 'utf-8' },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' },
      { hid: 'description', name: 'description', content: pkg.description }
    ],
    link: [
      { rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' },
      { rel: 'stylesheet', type: 'text/css', href: 'https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css' }
    ]
  },
```

`head.link` 内に CSS の読み込みを定義するかたちで アプリケーション全体に CSS を定期要する事ができます。

自分で作成した CSS を適用する場合には、
`static/css/common.css` などのように `static` フォルダ内に CSS を作成してから、

```
  head: {
    title: pkg.name,
    meta: [
      { charset: 'utf-8' },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' },
      { hid: 'description', name: 'description', content: pkg.description }
    ],
    link: [
      { rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' },
      { rel: 'stylesheet', type: 'text/stylesheet', href: '/css/common.css' }
    ]
  },
```

のように `/css/common.css` として  `static` ディレクトリからの相対パスでURLを記述します。

### グローバルに SCSS を適用する

SCSS を利用する場合には、追加モジュールのインストールが必要です。

```
$ npm i sass-loader node-sass
```

追加モジュールをインストールしたら、`assets/scss` ディレクトリを作成して、
中に scss ファイルを作成してみましょう。

例えば `assets/scss/common.scss` を以下のようなかたちで作成します。

```
$container: 1024px

.container{
  max-width: $container;
}
```

作成した SCSS をアプリケーション全体に適用する場合、
nuxt.config.js の css セクションにファイル名を追加すれば、
自動的に SCSS のコンパイルが実行され、アプリケーションにCSS が適用されます。

```
  css: [
    "~assets/scss/common.scss"
  ],
```

### 局所的に 適用するCSS 

vue コンポーネント内の style セクションでも CSS を記述することが可能です。

また style 要素の開始タグに lang 属性を記述することで、SCSS の利用も可能になります。

```
<style lang="scss">
  @import "~/assets/scss/mixins.scss";

  .container {
    @include container;
    ...
  }
    ...
</style>
```

ここに記述した CSS は通常、そのコンポーネントが利用されるページで、ページ全体に対して効果を及ぼしますが、
style 要素の開始タグに `scoped` プロパティを添えることでそのコンポーネント内でのみ有効なスタイルを作成する事ができます。

```
<style scoped>
    ...
</style>
```

通常 CSS はサイト全体に適用されるものですが、scoped で記述された CSS を利用することで、
CSS の適用範囲を vue コンポーネントの内部に閉じ込めることが可能で、大規模なページ開発で CSS定義の衝突を防ぐ際に非常に有効です。

## TRY!! 

- `/pages/index.vue` の内容をカスタマイズして画面に反映してみましょう。
- 任意のアドレスで表示されるWebページを 作成してみましょう。
- レイアウトを複数作成して、ページごとに利用するレイアウトを切り替えてみましょう。

- nuxt.config.js の head 要素を修正して CSS の読み込みを確認してみましょう。
- nuxt.config.js の head 要素を修正して、SEO や OGP タグ等のカスタマイズを行ってみましょう。
- SCSS の環境を整えて SCSS の記述をアプリケーションに適用してみましょう。
- scoped の CSS を記述して、コンポーネント内のCSS定義の効果を確認してみましょう。



