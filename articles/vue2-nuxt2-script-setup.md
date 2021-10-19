---
title: "Vue2 / Nuxt2 で <script setup> を使用するには unplugin-vue2-script-setup を導入する"
emoji: "📝"
type: "tech"
topics: ["vue", "nuxtjs"]
published: true
---

# ターゲット

Vue2 / Nuxt2 で `<script setup>` を素振り or 導入したい方

# `unplugin-vue2-script-setup` の紹介

[VueUse](https://github.com/vueuse/vueuse) などの開発者である[@antfu7](https://twitter.com/antfu7)さんが開発された、次のプラグインを適用すると Vue2 / Nuxt2 で `<script setup>` が使用可能となります。

https://github.com/antfu/unplugin-vue2-script-setup

[README.md](https://github.com/antfu/unplugin-vue2-script-setup#install)の通り、次のエコシステムにも対応されています。

- Vite
- Nuxt
- Vue CLI
- webpack
- Rollup
- Jest
- JavaScript API

## ハマったことの共有

`<script setup>` の素振りのため `unplugin-vue2-script-setup` を次のリポジトリ(Nuxt2)へ導入しました。

- https://github.com/zakizaki-ri9/blog
  - `<script setup>`使用箇所
  - [components/PostDateTime.vue](https://github.com/zakizaki-ri9/blog/blob/b9a9e78ce220a8f508ea465c970f1f27ad82a65f/components/PostDateTime.vue#L10-L25)
  - [pages/index.vue](https://github.com/zakizaki-ri9/blog/blob/b9a9e78ce220a8f508ea465c970f1f27ad82a65f/pages/index.vue#L15-L59)
  - [pages/posts/_datetime/_slug.vue](https://github.com/zakizaki-ri9/blog/blob/b9a9e78ce220a8f508ea465c970f1f27ad82a65f/pages/posts/_datetime/_slug.vue#L9-L67)

`unplugin-vue2-script-setup` というより `<script setup>` を適用する上でハマったことが2点ありましたので、参考までに共有します。

### `<script setup>` 内に定義した変数が `no-unused-vars` となる

`eslint-plugin-vue` が `<script setup>` 構文に対応したのは `v7.16.0` のため、バージョンが古いです。

https://github.com/vuejs/eslint-plugin-vue/releases/tag/v7.16.0

> - #1602 Fixed false positives for namespace component in vue/script-setup-uses-vars rule.

`v7.16.0` 以降のバージョンに更新しましょう。

### `<script setup>` で `defineComponent` を使用したい

https://github.com/antfu/unplugin-vue2-script-setup/issues/11

👆 のようなケースです。
Issue 内のコメント通り、`<script setup>` と `<script>` を併用して回避します。

:::message
`<script setup>` と `<script>` の併用については、次を参照ください。
https://v3.vuejs.org/api/sfc-script-setup.html#usage-alongside-normal-script
:::

# 参考

## そもそも `<script setup>` とは

[Vue.js 3.2](https://blog.vuejs.org/posts/vue-3.2.html)で使用可能となった構文です。

https://v3.vuejs.org/api/sfc-script-setup.html

前述から抜粋すると、次のようなメリットがあります。

> - More succinct code with less boilerplate
> - Ability to declare props and emitted events using pure TypeScript
> - Better runtime performance (the template is compiled into a render function in the same scope, without an intermediate proxy)
> - Better IDE type-inference performance (less work for the language server to extract types from code)

また、公式の内容を咀嚼した、次の記事を併せて読むと理解がはかどるかと思います。

https://zenn.dev/azukiazusa/articles/676d88675e4e74
