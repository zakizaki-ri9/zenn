---
title: "Nuxt3 Date Fetching で共通オプションを設定する"
emoji: "📝"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["nuxtjs", "nuxt3"]
published: true
publication_name: "visasq"
---

`useFetch`, `useAsyncData`, `$fetch` にて `Bearer` のようなシステム共通で利用するヘッダー等を設定する方法を記載する。

## 結論

https://github.com/nuxt/nuxt/issues/14936#issuecomment-1397369123

にて、解決方法が示されている。

### $fetch を利用する場合

Nuxt3 の `$fetch` は以下 `unjs/ofetch` を利用しており、以下 `Create fetch with default options` を参考にするとよい。

https://github.com/unjs/ofetch#%EF%B8%8F-create-fetch-with-default-options

次のようにすると `server/api` 配下に記述した API の型推論を失うことなく利用可能。

```ts:composables/api.ts
export const useApi = () => {
  // API で利用する token を取得
  const { token } = useApiToken();

  // $fetch の代わりに $api を利用する
  const $api = $fetch.create({
    ...{
      headers: {
        Authorization: `Bearer ${token}`,
      },
    },
  });

  return {
    $api,
  };
};
```
