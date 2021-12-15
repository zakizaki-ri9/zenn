---
title: "VSCode ${workspaceFolder} を使って settings.json に汎用的な設定を記述する"
emoji: "📝"
type: "tech"
topics: ["vscode", "python"]
published: true
---

Python パッケージ群のインテリセンスを有効化するため、各ワークスペースの `.vscode/settings.json` へ `python.autoComplete.extraPaths`[^1] を設定していましたが、開発環境が増えると設定が億劫になってきました。

[^1]: 詳しくは https://code.visualstudio.com/docs/python/editing#_enable-intellisense-for-custom-package-locations

グローバルな `settings.json`[^2] に設定を写し、億劫さが解消できたのでメモを残します。

[^2]: https://code.visualstudio.com/docs/getstarted/settings の **User Settings** のこと

# 結論: `${workspaceFolder}` を使用する

https://code.visualstudio.com/docs/editor/variables-reference

前述の VSCode で使用可能な変数のなかに、ワークスペースのルートである `${workspaceFolder}` を使用します。

# 設定例: `python.autoComplete.extraPaths` の場合

[venv](https://docs.python.org/ja/3/library/venv.html) を利用している場合、`{venvのroot}/lib/python{X.Y}/site-package` 配下に `pip install` したパッケージ群が配置されます。

venv を利用しているワークスペースごと、パッケージ群のインテリセンスを有効にする場合、次のように設定します。

```json:settings.json
{
    "python.autoComplete.extraPaths": [
        "${workspaceFolder}/**/lib/**/site-packages/"
    ],
}
```

# おわり

`${workspaceFolder}` 以外にも使用可能な変数がありますので、設定が楽にできそうか参考にしてみてください。

不備などありましたら、フィードバックいただけると助かります。🙏
