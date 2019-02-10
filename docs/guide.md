---
layout: post
title: はじめる
permalink: /guide
---

{::options toc_levels="1..2" /}
* ToC
{:toc}

## lit-htmlとは

<!-- original:
lit-html is a simple, modern, safe, small and fast HTML templating library for JavaScript.

lit-html lets you write HTML templates in JavaScript using [template literals] with embedded JavaScript expressions. Behind the scenes lit-html creates HTML `<template>` elements from your JavaScript templates and processes them so that it knows exactly where to insert and update the values from expressions.
-->

lit-htmlは、JavaScript用のシンプルでモダンな、安全で軽量高速なHTMLテンプレートライブラリです。

HTMLテンプレートを扱うにはJavaScript評価式を埋め込んだテンプレートリテラル([template literals])を使います。内部的にはJavaScriptのテンプレートからHTMLの`<template>`要素を生成することで、評価式の結果をどこに追加・更新するか正確に知ることができています。

## テンプレート

<!-- original:
lit-html templates are tagged template literals - they look like JavaScript strings but are enclosed in backticks (`` ` ``) instead of quotes - and tagged with lit-html's `html` tag:

```js
html`<h1>Hello ${name}</h1>`
```
-->

テンプレートは、タグ付きのテンプレートリテラル(tagged template literals)です。単純なJavaScript文字列のように見えますが、引用符(")ではなくバッククォート(`` ` ``)で囲まれ、lit-htmlの`html`タグが使用されます:

```js
html`<h1>こんにちは ${name}</h1>`
```

<!-- original:
Since lit-html templates almost always need to merge in data from JavaScript values, and be able to update DOM when that data changes, they'll most often be written within functions that take some data and return a lit-html template, so that the function can be called multiple times:
-->

ほとんどの場合で、特定のデータを使ってDOMを更新することになるので、引数を渡して加工したテンプレートを返す関数を書くことになるでしょう。この関数は他でも繰り返し利用することができます:

```js
let myTemplate = (data) => html`
  <h1>${data.title}</h1>
  <p>${data.body}</p>`;
```

<!-- original:
lit-html is _lazily_ rendered. Calling this function will evaluate the template literal using lit-html `html` tag, and return a `TemplateResult` - a record of the template to render and data to render it with. `TemplateResults` are very cheap to produce and no real work actually happens until they are _rendered_ to the DOM.
-->

lit-htmlで描画(レンダリング)は _遅延(lazily)_ 実行されます。この関数が呼び出されると、`html`タグを使ってテンプレートリテラル(template literal)が評価され、`TemplateResult` (文字列やデータを描画する部分記録されたオブジェクト)が返されます。`TemplateResults`は生成するのが非常に軽く、実際にDOMに _描画_ されるまで実際には何も起こりません。

## 描画について

<!-- original:
To render a `TemplateResult`, call the `render()` function with a result and DOM container to render to:

```js
const result = myTemplate({title: 'Hello', body: 'lit-html is cool'});
render(result, document.body);
```

Ready to try it yourself? Head over to [Getting Started](/guide/getting-started).
-->

`TemplateResult` を実際に描画するには、使用する値と描画先のDOMコンテナを指定して`render()`関数を呼び出します。

```js
const result = myTemplate({title: 'こんにちわ', body: 'lit-htmlは素敵です'});
render(result, document.body);
```

自分で試してみたいですか? [はじめる](/guide/getting-started)へ進んでください。

[template literals]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals
