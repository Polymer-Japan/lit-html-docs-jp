---
layout: post
title: Getting Started
slug: getting-started
---

{::options toc_levels="1..2" /}
* ToC
{:toc}

## インストール(Installation)

### npm

<!-- original:
lit-htm is distributed on npm, in the [lit-html package].
-->

lit-htmはnpmのlit-htmlパッケージで配布されます。

```
npm install lit-html
```

### unpkg.com

<!-- original:
You can also load lit-html directly from the unpkg.com CDN:
-->

また、unpkg.com CDNからlit-htmlを直接読み込むこともできます：

```js
import {html, render} from 'https://unpkg.com/lit-html?module';
```

### Online editors

<!-- original:
You can try out lit-html without installing anything using an online editor. Below are links to a simple lit-html starter project in some popular online editors:
-->

オンラインエディタを使用して何もインストールせずにlit-htmlを試すことができます。以下は、人気のあるオンラインエディタでの単純なlit-htmlスタータープロジェクトへのリンクです。

*   [CodeSandbox](https://codesandbox.io/s/wq2wm73o28)
*   [JSBin](https://jsbin.com/nahocaq/1/edit?html,output)
*   [StackBlitz](https://stackblitz.com/edit/js-pku9ae?file=index.js)

## Importing

<!-- origin:
lit-html is written in and distributed as standard JavaScript modules.
Modules are increasingly supported in JavaScript environments and are shipping in Chrome, Opera and Safari, and soon will be in Firefox and Edge.

To use lit-html, import it via a path:
-->

lit-htmlは、標準のJavaScriptモジュールとして作成され、配布されます。モジュールはJavaScript環境でますますサポートされ、Chrome、Opera、Safariで出荷されており、まもなくFirefoxとEdgeで使用されます。

lit-htmlを使用するには、パス経由でインポートします。

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

JavaScript importステートメントはモジュールスクリプト（<script type="module">）内でのみ動作し、インラインスクリプト（上に示したような）または外部スクリプトである可能性があります。

使用するパスは、lit-htmlをどこにインストールしたかによって異なります。ブラウザは、パッケージ名ではなく、パス別に他のモジュールをインポートすることしかサポートしていないため、他のツールを使用しなければ、パスを使用する必要があります。

パッケージ名をパスに変換するツールを使用する場合は、パッケージ名でインポートできます。

```js
import {html, render} from 'lit-html';
```

<!-- original:
**Why JavaScript modules?** For more information on why lit-html is distributed using JavaScript modules, see [JavaScript Modules](concepts#javascript-modules).
-->

**なぜJavaScriptモジュールですか？** lit-HTMLはJavaScriptモジュールを使用して配布される理由の詳細については、[JavaScript Modules](concepts#javascript-modules)を見てください。

## Rendering a Template

<!-- original:
lit-html has two main APIs:

*   The `html` template tag used to write templates
*   The `render()` function used to render a template to a DOM container.
-->

lit-htmlには2つの主要なAPIがあります：

htmlテンプレートを書き込むためのテンプレートタグ
render()関数は、DOMコンテナにテンプレートをレンダリングするために使用されます。

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

テンプレートの詳細については、[テンプレートの作成](./writing-templates)を参照。

[lit-html package]: https://www.npmjs.com/package/lit-html
