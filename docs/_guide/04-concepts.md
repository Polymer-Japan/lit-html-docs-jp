---
layout: post
title: Concepts
slug: concepts
---

{::options toc_levels="1..2" /}
* ToC
{:toc}

<!-- original:
`lit-html` utilizes some unique properties of JavaScript template literals and HTML `<template>` elements to function and achieve fast performance. So it's helpful to understand them first.
-->

lit-htmlJavaScriptのテンプレートリテラルやHTML <template>要素のいくつかのユニークなプロパティを使用して、機能し、高速なパフォーマンスを実現します。だから最初に理解すると便利です。

## Tagged Template Literals

<!-- original:
A JavaScript template literal is a string literal that can have JavaScript expressions embedded in it:
-->

JavaScriptテンプレートリテラルは、JavaScript式を埋め込むことができる文字列リテラルです。

```js
`My name is ${name}.`
```

<!-- original:
The literal uses backticks instead of quotes, and can span multiple lines. The part inside the `${}` can be _any_ JavaScript expression.

A _tagged_ template literal is prefixed with a special template tag function:
-->

リテラルは、引用符の代わりにバッククォートを使用し、複数の行にまたがることができます。内側の部分は、${}することができ任意の JavaScript式。

タグ付けされたリテラルテンプレートは、特別なテンプレートタグ機能が付いています：

```js
let name = 'Monica';
tag`My name is ${name}.`
```

<!-- original:
Tags are functions that take the literal strings of the template and values of the embedded expressions, and return a new value. This can be any kind of value, not just strings. lit-html returns an object representing the template, called a `TemplateResult`.

The key features of template tags that lit-html utilizes to make updates fast is that the object holding the literals strings of the template is _exactly_ the same for every call to the tag for a particular template.

This means that the strings can be used as a key into a cache so that lit-html can do the template preparation just once, the first time it renders a template, and updates skip that work.
-->

タグは、テンプレートのリテラル文字列と埋め込み式の値を受け取り、新しい値を返す関数です。これは文字列だけでなく、あらゆる種類の値にすることができます。lit-htmlは、aというテンプレートを表すオブジェクトを返しますTemplateResult。

lit-htmlが高速に更新を行うために利用するテンプレートタグの重要な機能は、テンプレートのリテラル文字列を保持するオブジェクトが、特定のテンプレートのタグを呼び出すたびに正確に同じであることです。

これは、文字列をキャッシュへのキーとして使用できるので、lit-htmlはテンプレートを初めてレンダリングしたときにテンプレートの準備を一度しか行えず、その作業をスキップして更新することができることを意味します。

## HTML `<template>` Elements

<!-- original:
A `<template>` element is an inert fragment of DOM. Inside a `<template>`, script don't run, images don't load, custom elements aren't upgraded, etc. `<template>`s can be efficiently cloned. They're usually used to tell the HTML parser that a section of the document must not be instantiated when parsed, and will be managed by code at a later time, but it can also be created imperatively with `createElement` and `innerHTML`.

lit-html creates HTML `<template>` elements from the tagged template literals, and then clones them to create new DOM.
-->

<template>要素は、DOMの不活性断片です。内部では<template>、スクリプトは実行されず、イメージはロードされず、カスタム要素はアップグレードされません<template>。効率的にクローンを作成できます。彼らは通常、文書のセクションを解析する際にインスタンス化されてはならない、と後でコードによって管理されるHTMLパーサを伝えるために使用しているが、それはまたして命令的に作成することができるcreateElementとinnerHTML。

lit-htmlは<template>、タグ付きテンプレートリテラルからHTML 要素を作成し、それらをクローンして新しいDOMを作成します。

## Template Creation

<!-- original:
The first time a particular lit-html template is rendered anywhere in the application, lit-html does one-time setup work to create the HTML template behind the scenes. It joins all the literal parts with a special placeholder, similar to `"{% raw %}{{}}{% endraw %}"`, then creates a `<template>` and sets its `innerHTML` to the result.

If we start with a template like this:
-->

特定のlit-htmlテンプレートがアプリケーション内のどこにでもレンダリングされた最初の段階で、lit-htmlはHTMLテンプレートを作成するために一度の設定作業を行います。すべてのリテラル部分を特殊なプレースホルダで結合し"{% raw %}{{}}{% endraw %}"、次にa <template>を作成innerHTMLして結果に設定します。

次のようなテンプレートで始める場合：

```js
let header = (title) => html`<h1>${title}</h1>`;
```

<!-- original:
lit-html will generate the following HTML:
-->

lit-htmlは次のHTMLを生成します：

```html
<h1>{% raw %}{{}}{% endraw %}</h1>
```

<!-- original:
And create a `<template>` from that.

Then lit-html walks the template's DOM and extracts the placeholders and records their location. The final template doesn't contain the placeholders:
-->

それ<template>からa を作成します。

その後、lit-htmlはテンプレートのDOMをウォークし、プレースホルダを抽出してその場所を記録します。最終的なテンプレートにはプレースホルダが含まれていません。

```html
<h1></h1>
```

<!-- original:
And there's an auxillary table of where the expressions were:
-->

そして表現がどこにあったの補助的なテーブルがあります：

`[{type: 'node', index: 1}]`

## Template Rendering

<!-- original:
`render()` takes a `TemplateResult` and renders it to a DOM container. On the initial render it clones the template, then walks it using the remembered placeholder positions, to create `Part` objects.

A `Part` is a "hole" in the DOM where values can be injected. lit-html includes two type of parts by default: `NodePart` and `AttributePart`, which let you set text content and attribute values respectively. The `Part`s, container, and template they were created from are grouped together in an object called a `TemplateInstance`.
-->

render()a TemplateResultをとり、それをDOMコンテナにレンダリングします。最初のレンダリングでは、テンプレートをクローン化し、次に記憶されたプレースホルダ位置を使用してテンプレートを歩き、Partオブジェクトを作成します。

A Partは値を注入できるDOMの「穴」です。lit-htmlにはデフォルトで2種類のパーツが含まれています：NodePartとはAttributePart、それぞれテキストの内容と属性の値を設定できるようにします。Part彼らはから作成されたS、コンテナ、およびテンプレートが呼び出されたオブジェクトにグループ化されていますTemplateInstance。

## Thinking Functionally

<!-- original:
lit-html is ideal for use in a functional approach to describing UIs. If you think of UI as a function of data, commonly expressed as `UI = f(data)`, you can write lit-html templates that mirror this exactly:
-->

lit-htmlは、UIを記述するための機能的なアプローチでの使用に理想的です。UIをデータの関数と考えると、通常は次のように表されUI = f(data)ます。これを正確に反映したlit-htmlテンプレートを書くことができます。

```js
let ui = (data) => html`...${data}...`;
```

<!-- original:
This kind of function can be called any time data changes, and is extremely cheap to call. The only thing that lit-html does in the `html` tag is forward the arguments to the templates.

When the result is rendered, lit only updates the expressions whose values have changed since the previous render.

This leads to model that's easy to write and easy to reason about: always try to describe your UI as a simple function of the data it depends on, an avoid caching intermediate state, or doing manual DOM manipulation. lit-html will almost always be fast enough with the simplest description of your UI.
-->

この種の機能は、データの変更があればいつでも呼び出すことができ、非常に安価に呼び出すことができます。lit-htmlがhtmlタグで行う唯一のことは、引数をテンプレートに転送することです。

結果がレンダリングされると、前のレンダリング以降に値が変更された式のみが更新されます。

これは、書くことが容易で推論が簡単なモデルにつながります。常に依存するデータの単純な関数としてのUIの記述、中間状態のキャッシングの回避、または手動によるDOM操作の実行を試みてください。lit-htmlは、ほとんどの場合、あなたのUIの最も単純な記述で十分に速くなります。

## JavaScript Modules

<!-- original:
Why is lit-html distributed as JavaScript modules, not as UMD/CJS/AMD?

Until modules arrived, browsers have not had a standard way to import code from code, so user-land module loaders or bundlers were required. Since there was no standard, competing formats have multiplied. Often libraries  publish in a number of formats to support users of different tools, but this causes problems when a common library is depended on by many other intermediate libraries. If some of those intermediate libraries load format A, and others load format B, and yet others load format C, then multiple copies are loaded, causing bloat, performance slowdowns, and sometimes hard-to-find bugs.

The only true solution is to have one canonical version of a library that all other libraries import. Since modules support is rolling out to browsers now, and modules are very well supported by tools, it makes sense for that format to be modules.

For more information on JavaScript modules:

*   [Using JavaScript Modules on the Web](https://developers.google.com/web/fundamentals/primers/modules) on Web Fundementals.

*   [import statement reference page](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) on MDN.
-->

なぜlit-htmlはUMD / CJS / AMDではなく、JavaScriptモジュールとして配布されていますか？

モジュールが到着するまで、ブラウザはコードからコードをインポートする標準的な方法を持っていなかったので、ユーザーランドモジュールローダまたはバンドルが必要でした。標準がなかったので、競合するフォーマットが倍増しました。多くの場合、ライブラリはさまざまなツールのユーザーをサポートするためにさまざまな形式で公開されていますが、これは共通ライブラリが他の多くの中間ライブラリに依存している場合に問題を引き起こします。それらの中間ライブラリのいくつかがフォーマットAをロードし、他のフォーマットがフォーマットBをロードし、フォーマットCをロードするものがあると、複数のコピーがロードされ、膨らんで、パフォーマンスが低下し、

唯一の真の解決策は、他のすべてのライブラリがインポートするライブラリの正規バージョンを1つ持つことです。モジュールサポートは現在ブラウザに展開されており、モジュールはツールで非常にうまくサポートされているため、そのフォーマットがモジュールであることは理にかなっています。

JavaScriptモジュールの詳細については、次を参照してください。

Web FundementalsでWeb上でJavaScriptモジュールを使用する。

MDNのインポートステートメントのリファレンスページ。
