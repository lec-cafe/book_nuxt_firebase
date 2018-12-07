# Step3. Vuex の利用

画面の構築が完了したら実際にアプリケーションのロジックを組み込んでいきましょう。

SPA のように複数ページ構成になりうるアプリケーションの場合、
データのロジックを別ファイルに切り分けて ページ間で共有する処理はとても重要になります。

Vue.js におけるデータ管理ロジックとしては、Vuex を用いるやり方が一般的で、
Nuxt.js でもこれを利用することが可能です。

## Vuex Store の作成

Vuex Store は アプリケーションで使用するデータの定義と、
データ操作に関するロジックをまとめた JS モジュールです。

Nuxt.js で Vuex を使用する場合、単純に `store/index.js` を作成します。

```
export const state = () => {
  return {
    user: null,
  }
}
export const mutations = {
  setUser(state, user) {
    state.user = user
  }
}
export const actions = {
  loginWithUserName({commit}, name) {
    commit("setUser",{ name })
  }
}
```

state は Vuex で利用するアプリケーションのデータ定義です。

mutations は state に対する操作を定義します。 
mutations は commit 関数を利用して呼び出すことができます。

```
export const mutations = {
  setUser(state, user) {
    state.user = user
  }
}
```

mutations の各関数は第一引数に state を取ります。
第二引数では mutations を commit 関数から呼び出した際に渡される引数を受け取ることができます。

actions は mutations を呼び出すためのロジックを記述する関数です。
actions は dispatch 関数を利用して呼び出すことができます。

```
export const actions = {
  loginWithUserName({commit}, name) {
    commit("setUser",{ name })
  }
}
```

mutations の各関数では、第一引数から `commit` 関数や `state` などのオブジェクトを取得することができます。
第二引数では actions を dispatch 関数から呼び出した際に渡される引数を受け取ることができます。

## Vuex Store の利用

Vuex Store が作成できたら、`.vue` コンポーネントで記載されたページから 
Vuex Store を利用してみましょう。

Vuex Store 内のデータを 利用する場合,　computed プロパティを利用して以下のように記述します。

```js
export default {
  //...    
  computed: {
    user () {
        return this.$store.state.user
    }
  },
  //...    
}
```

mutations を呼び出すための commit 関数や、
actions を呼び出すための dispatch 関数は以下のようにして利用することが可能です。

```js
export default {
  //...    
  computed: {
    login () {
        this.$store.dispatch("loginWithUserName", this.name)
    },
    logout () {
        this.$store.commit("setUser", null)
    }
  },
  //...    
}
```
