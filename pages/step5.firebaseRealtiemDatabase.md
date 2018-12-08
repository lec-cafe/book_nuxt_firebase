# Step4. Firebase のセットアップ

最後に Firebase の Realtime Database を利用して データベースのセットアップを行いましょう。

## Firebase Realtime Database の設定

Firebase Realtime Database を利用するには 
Firebase Console 上で Realtime Database を有効化する必要があります。

## VuexFire の利用

Firebase Realtime Database は、
WebSocket を利用したリアルタイム通信が特徴です。

WebSocket 経由でのデータ送受信と、
Vuex mutations を利用したデータ操作をきれいに連携させる処理を記述するのはとても大変です。

ここでは `vuexfire` というライブラリを利用して、
Realtime Database の内容を自動的に vuex に連携する手法を紹介します。

`vuexfire` を利用するには、まずモジュールのインストールを行います。

```
$ npm i vuexfire
```

`vuexfire`を利用する場合、 mutations に vuexfire の処理を登録します。

```js
import { firebaseMutations } from 'vuexfire'
// ...
export const mutations = {
  ...firebaseMutations,
  // ...
}
```
 
vuex と Realtime Database との連携は、以下のようにします。

```js
import { firebaseMutations, firebaseAction } from 'vuexfire'
// ...
export const actions = {
  // ...  
  INIT_POSTS: firebaseAction(({ bindFirebaseRef }) => {
    return new Promise( (resolve) => {
      bindFirebaseRef('posts', firebase.database().ref("/posts"),{ resolve })
    })
  }),
  // ...  
}
```

上記の `INIT_POST` では state `posts` と Realtime Database の `/posts` を連携するよう設定を行っています。
この 処理も アプリケーションの起動時に呼び出されるよう `.vue` コンポーネントの `mounted` セクションに追加しておきましょう。

Realtime Database と連携した Vuex では、データベースに対する書き込み処理に合わせて、
対応する `mutations` が自動的に呼び出される仕組みになっています。

```js
const db = firebase.database()
const postsRef = db.ref("/posts")

export const actions = {
  // ...
  addComments({ state, commit} , comment) {
    firebase.database().ref("/posts").push().set({
      comment,
      user: state.user.name,
    })
  },
  // ...
}
```

データベースへの書き込み処理は `firebase.database().ref("/posts").set({ ... })` のような形で行うことができます。

投稿のように 配列形式の場合 `firebase.database().ref("/posts").push().set({ ... })` のようにして記述することで、
末尾にデータを追記することも可能です。
