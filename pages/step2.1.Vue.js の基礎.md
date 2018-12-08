# Step2-1. Vue.js の基礎

画面のコーディングが完了したら Vue.js の記述を利用して、
アプリケーションの動きを実装してみましょう。

## データバインディング

Vue.js の機能の一つに データバインディングと呼ばれるものがあります。

データバインディングは JS 上の変数を、HTMLテンプレートに紐付ける機能です。

```html
<template>
    <div>
        {{ message }}
    </div>
</template>
<script >
export default {
    data(){
        return {
            message: "hello world"
        }
    }
}
</script>
```

script セクションの data で記述した値は、
template セクションにて `{{ message }}` のように記述して画面に描画することができます。

配列のデータを用いて テンプレートを繰り返し描画する場合、
`v-for` を用いて次のように記述します。

```html
<template>
    <div>
        <ul>
            <li v-for="(fruit,index) in fruits" :key="index">{{ fruit }}</li>
        </ul>
    </div>
</template>
<script >
export default {
    data(){
        return {
            fruits: ["apple","banana","lemmon"]
        }
    }
}
</script>
```

`:key="index"` は、v-for で要素を繰り返し展開する際に、各要素を個別に認識できるようにするための記述で、
v-for を利用する際に必ず必要となるものです。 

バインディングは 属性に対して行うことも可能です。次のように URLをa要素の href 属性に紐付ける場合、
`:` を属性名の先頭に付けて記述します。

```html
<template>
    <div>
        <a :href="url">Google</a>
    </div>
</template>
<script >
export default {
    data(){
        return {
            url: "https://google.com"
        }
    }
}
</script>
```

## フォームを用いた双方向バインディング

フォームを利用して、フォーム入力値とデータをバインディングすることも可能です。
フォームのバインディングを行う際には `v-model` を利用します。

```html
<template>
    <div>
        <form>
            <input type="text" v-model="form.text">
            {{ form.text }}
        </form>
    </div>
</template>
<script >
export default {
    data(){
        return {
            form: {
                text: ""
            }
        }
    }
}
</script>
```

フォームの入力値は、逐次 script 側の変数に代入され、リアルタイムに画面に反映されます。

これを少しカスタマイズして、入力値のカウンタを作成することも可能です。

```html
<template>
    <div>
        <form>
            <input type="text" v-model="form.text">
            {{ form.text.length }} / 20
        </form>
    </div>
</template>
<script >
export default {
    data(){
        return {
            form: {
                text: ""
            }
        }
    }
}
</script>
```


## 算出プロパティの利用

`{{ }}` の内部では、ある程度のJSの記述を行うことが可能ですが、
複雑な JS 式をテンプレート内で展開するとソースの可読性や保守性が低下します。

Vue.js では、`{{ input.length }}` のような処理をまとめて記述するための、
算出プロパティという機能が用意されています。

```html
<template>
    <div>
        <form>
            <input type="text" v-model="form.text">
            {{ formLength }} / 20
        </form>
    </div>
</template>
<script >
export default {
    data(){
        return {
            form: {
                text: ""
            }
        }
    },
    computed: {
        formLength(){
            return this.form.text.length
        }
    }
}
</script>
```

`computed` のセクションで定義されたプロパティは、
data 内で定義した値と同じ様に扱うことができ、複雑な JS 式の記述を複数回呼び出す際などに役に立ちます。

## イベントハンドリング

テンプレート内の HTML 要素に イベント処理を追加することも可能です。

例えば クリックされた際の処理を記述するには、`@click` を利用します。

```html
<template>
    <div>
        <form>
            <input type="text" v-model="form.text">
            {{ formLength }} / 20
            <a @click="submit">送信</a>
        </form>
    </div>
</template>
<script >
export default {
    data(){
        return {
            form: {
                text: ""
            }
        }
    },
    computed: {
        formLength(){
            return this.form.text.length
        }
    },
    methods: {
        submit() {
            console.log(this.form.text)
        }
    }
}
</script>
```

## v-if によるレンダリングの切り替え

処理や JS の値に応じて、表示するHTML を切り替える場合、
`v-if` による画面制御が用いられます。

```html
<template>
    <div>
        <div v-if="showAlert">
            <p>警告メッセージです！</p>
            <a @click="close">閉じる</a>        
        </div>
    </div>
</template>
<script >
export default {
    data(){
        return {
            showAlert: true
        }
    },
    methods: {
        close() {
            this.showAlert = false
        }
    }
}
</script>
```

