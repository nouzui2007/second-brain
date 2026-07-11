# Vue.jsとかNuxtとか

## 参考サイト

[Visual Studio Code で Nuxt.js を使った Docker コンテナ内での開発 - Qiita](https://qiita.com/nakazawaken1/items/b5972e4e781bb0d6260b)

[コンポーネントの基本 - Vue.js](https://jp.vuejs.org/v2/guide/components.html)

[はじめてのVuetify(ログインページ作成で使い方学ぶ) | アールエフェクト](https://reffect.co.jp/vue/vuetify-first-time#Vuetify)

[はじめての Nuxt.js + Vuex + axios - Qiita](https://qiita.com/nunulk/items/0d50ac8cf7e16287f915)

## UIライブラリ

[Vuetify - A Material Design Framework for Vue.js](https://vuetifyjs.com/ja/)

## アイコン

[Material Design Icons](https://pictogrammers.github.io/@mdi/font/2.0.46/)

Vuex

[vuejs/vuex](https://github.com/vuejs/vuex/tree/dev/examples/shopping-cart)

[【Nuxt.js】Vuex基礎編：コードをスッキリさせよう | aLiz Nuxt](http://nuxt.alizlab.com/%e3%80%90nuxt-js%e3%80%91vuex%e5%9f%ba%e7%a4%8e%e7%b7%a8%ef%bc%9a%e3%82%b3%e3%83%bc%e3%83%89%e3%82%92%e3%82%b9%e3%83%83%e3%82%ad%e3%83%aa%e3%81%95%e3%81%9b%e3%82%88%e3%81%86/)

[Vuex 入門 | Vuex](https://vuex.vuejs.org/ja/guide/)

## Middleware

middlewareはレンダリング前に実行されるプログラム

プロジェクトルート/ `middleware` にファイルは配置する

middleware名称は拡張子を除いたファイル名

呼び出しを記述する箇所によって実行されるタイミングが違う

### nuxt.config.js

ページが表示される都度実行される

```jsx
export default {
	router: {
    middleware: "sample1"
  }
}
```

この場合、 `middleware/sample1.js` が存在している

### layouts

`layouts` 下のvueファイルに記述すると、レイアウト適用ページが表示都度実行される

レイアウト適用によって、ログイン前後を分けておけば、それぞれの処理が実行できる

### pages

`pages` 下のvueファイルに記述すると、そのページが表示される都度実行される

```jsx
export default {
  middleware: "sample2"
}
```

[Composition API](Vue%20js%E3%81%A8%E3%81%8BNuxt%E3%81%A8%E3%81%8B/Composition%20API%201ee1c10ef48b4bdf99ad81db180ec15b.md)

[NuxtでJWT認証](Vue%20js%E3%81%A8%E3%81%8BNuxt%E3%81%A8%E3%81%8B/Nuxt%E3%81%A7JWT%E8%AA%8D%E8%A8%BC%20278998248d624f1b9e5395f7aec55732.md)

[Vuex](Vue%20js%E3%81%A8%E3%81%8BNuxt%E3%81%A8%E3%81%8B/Vuex%20344661ddf3104611a7380a63f40f3c90.md)

[axios](Vue%20js%E3%81%A8%E3%81%8BNuxt%E3%81%A8%E3%81%8B/axios%20cbe56341200e40b5a206ea0d667e5d9c.md)

[Pluginsでdispatchを使う](Vue%20js%E3%81%A8%E3%81%8BNuxt%E3%81%A8%E3%81%8B/Plugins%E3%81%A7dispatch%E3%82%92%E4%BD%BF%E3%81%86%20e48ab2d55ad84cbb8e7973e577de4726.md)