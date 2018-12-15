---
layout: post
title: 使ってみる
slug: getting-started
---

{::options toc_levels="1..2" /}
* ToC
{:toc}

## インストール

### npm

<!-- original:
lit-htm is distributed on npm, in the [lit-html package](https://www.npmjs.com/package/lit-html).
-->

lit-htmはnpmの[lit-html package](https://www.npmjs.com/package/lit-html)で配布されています。

```
npm install lit-html
```

### unpkg.com

<!-- original:
You can also load lit-html directly from the unpkg.com CDN:
-->

unpkg.comのCDNから直接読み込むこともできます:

```js
import {html, render} from 'https://unpkg.com/lit-html?module';
```

### オンラインエディタ

<!-- original:
You can try out lit-html without installing anything using an online editor. Below are links to a simple lit-html starter project in some popular online editors:
-->

インストールをせずに試すこともできます。以下のリンクはオンラインエディタによるシンプルなlit-html starterプロジェクトです。

*   [CodeSandbox](https://codesandbox.io/s/wq2wm73o28)
*   [JSBin](https://jsbin.com/nahocaq/1/edit?html,output)
*   [StackBlitz](https://stackblitz.com/edit/js-pku9ae?file=index.js)

## インポート

<!-- origin:
lit-html is written in and distributed as standard JavaScript modules.
Modules are increasingly supported in JavaScript environments and are shipping in Chrome, Opera and Safari, and soon will be in Firefox and Edge.

To use lit-html, import it via a path:
-->

lit-htmlは標準的なJavaScriptモジュールとして作成されています。
JavaScriptモジュールは既にChrome、Opera、Safariで使え、まもなくFirefoxとEdgeでも使えるようになります。

lit-htmlを使うには、importでパスを指定します。

```js
<script type="module">
  import {html, render} from './node_modules/lit-html/lit-html.js';
  ...
</script>
```

<!-- original:
The JavaScript `import` statement only works inside module scripts (`<script type="module">`), which can be inline scripts (as shown above) or external scripts.

The path to use depends on where you've installed lit-html to. Browsers only support importing other modules by path, not by package name, so without other tools involved, you'll have to use paths.

If you use a tool that converts package names into paths, then you can import by package name:
-->

JavaScriptの`import`は`<script type="module">`内でのみ動作し、インライン（上記のように）か外部スクリプトとして読み込む必要があります。

ブラウザ上ではインポートの指定にパッケージ名を使うことはできず、他のモジュールを指定する場合もバンドラー等の別ツールを使わない限りパスを直接指定する必要があります。

もしパッケージ名をパスに変換するツールを使っている場合は、パッケージ名を指定してインポートできます。

```js
import {html, render} from 'lit-html';
```

<!-- original:
**Why JavaScript modules?** For more information on why lit-html is distributed using JavaScript modules, see [JavaScript Modules](concepts#javascript-modules).
-->

**なぜJavaScriptモジュールなのか？** JavaScriptモジュールで作成される詳細については、[JavaScript Modules](concepts#javascript-modules)を参照してください。

## テンプレートの描画

<!-- original:
lit-html has two main APIs:

*   The `html` template tag used to write templates
*   The `render()` function used to render a template to a DOM container.
-->

lit-htmlには2つの主要なAPIがあります:

* `html` テンプレートを書き込むためのテンプレートタグ(template tag)
* `render()` DOMコンテナにテンプレートを描画する関数(function)。

```ts
// lit-htmlをインポート
import {html, render} from 'lit-html';

// テンプレートを定義
const myTemplate = (name) => html`<p>Hello ${name}</p>`;

// テンプレートをページに描画
render(myTemplate('World'), document.body);
```

<!-- original:
To learn more about templates, see [Writing Templates](./writing-templates).
-->

テンプレートの詳細については、[テンプレートをつくる](./writing-templates)を参照してください。

[lit-html package]: https://www.npmjs.com/package/lit-html
