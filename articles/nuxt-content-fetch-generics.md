---
title: "nuxt/content x TypeScript コンテンツを取得する際に型指定する"
emoji: "📝"
type: "tech"
topics: ["typescript", "nuxtjs", "nuxtcontent"]
published: true
---

:::message
`nuxt/content` v1.15.0 時の情報です。
:::

本記事では `$content.fetch` を使用する際に型指定する方法を記載します。

## はじめに: `nuxt/content` とは

`content/` 配下の Markdown などのファイルを全文検索・取得が行える NuxtJS のモジュールです。

https://content.nuxtjs.org/ja/

TypeScript にも対応しています。導入手順は[こちら](https://content.nuxtjs.org/ja/installation#typescript)。

## 本題

### `$content.fetch` はジェネリクスが使える

https://content.nuxtjs.org/ja/fetching

👆のドキュメントの通り、コンテンツの取得は`$content.fetch`を使用します。

[packages/content/types/query-builder.d.ts](https://github.com/nuxt/content/blob/e029fe9e1063e82d8d6bf56e4a1f4cc798ded196/packages/content/types/query-builder.d.ts#L87)を読んでみると、ジェネリクスが利用可能であるとわかります。次は該当箇所を抜粋しています。

```typescript:packages/content/types/query-builder.d.ts#L87
// 省略
export class QueryBuilder {
  // 省略
  fetch<T>(): Promise<(T & FetchReturn) | (T & FetchReturn)[]>;
}
```

`FetchReturn` は Markdown などのコンテンツ取得時に解析・付与される「作成日時、見出し」などのプロパティのことです。詳しくは次のソースを参照してください。

- [packages/content/types/content.d.ts](https://github.com/nuxt/content/blob/e029fe9e1063e82d8d6bf56e4a1f4cc798ded196/packages/content/types/content.d.ts#L6-L19)
- [packages/content/types/query-builder.d.ts](https://github.com/nuxt/content/blob/e029fe9e1063e82d8d6bf56e4a1f4cc798ded196/packages/content/types/query-builder.d.ts#L11-L14)


`FetchReturn` に加えて、コンテンツ取得時に Markdown のフロントマターをプロパティとして追加してくれます。[^1]

[^1]: https://content.nuxtjs.org/ja/writing#markdown を参照ください。

ジェネリクス `T` に指定するのは、このフロントマターで付与されるプロパティの型になります。

### コードサンプル: コンテンツを取得する際に型指定する

```markdown:content/articles/test.md
---
title: テスト
description: はじめてのブログ記事
---

〜本文〜
```

👆のような Markdown を型指定して取得したい場合は、次のように記載します。

:::message
- コードサンプルは `@nuxtjs/composition-api` を利用しています。
- setup 記法は `unplugin-vue2-script-setup` を利用しています。
:::

```typescript:pages/index.vue
// コンテンツ取得の部分を抜粋して記載しています
<script setup lang="ts">
import { useAsync, useContext } from '@nuxtjs/composition-api'

export type Article = {
  title: string
  description: string
}

const { $content } = useContext()

// (Article & FetchReturn) | (Article & FetchReturn)[]
// として推論される
const articles = useAsync(
  async () => await $content('articles', { deep: true }).fetch<Article>()
)
</script>
```

## おわり

`nuxt/content` を用いた際、自分は次のようにコンテンツ取得の際の型指定を行なっています。

- [composables/post.ts](https://github.com/zakizaki-ri9/blog/blob/911120095edbf04fc3345a339046e9903b85ba33/composables/post.ts#L16-L36)

見苦しいかもしれませんが、参考になれば幸いです。
