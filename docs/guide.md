---
layout: post
title: Introduction
url: /guide/
---

{::options toc_levels="1..2" /}
* ToC
{:toc}

## lit-htmlとは(What is lit-html?)

<!-- original:
lit-html is a simple, modern, safe, small and fast HTML templating library for JavaScript.

lit-html lets you write HTML templates in JavaScript using [template literals] with embedded JavaScript expressions. Behind the scenes lit-html creates HTML `<template>` elements from your JavaScript templates and processes them so that it knows exactly where to insert and update the values from expressions.
-->

lit-htmlは、JavaScript用のシンプルでモダンで安全で小型で高速なHTMLテンプレートライブラリです。

lit-htmlを使用すると、埋め込まれたJavaScript式を使用してテンプレートリテラルを使用してJavaScriptでHTMLテンプレートを書き込むことができます。背後にあるlit-htmlは<template>、JavaScriptテンプレートからHTML 要素を作成し、それを処理して、式から値を挿入および更新する場所を正確に把握します。

## lit-htmlテンプレート(lit-html Templates)

<!-- original:
lit-html templates are tagged template literals - they look like JavaScript strings but are enclosed in backticks (`` ` ``) instead of quotes - and tagged with lit-html's `html` tag:
-->

lit-htmlテンプレートは、タグ付きのテンプレートリテラルです。JavaScript文字列のように見えますが`、引用符ではなくバッククォートで囲まれ、lit-htmlのhtmlタグでタグ付けされています。

```js
html`<h1>Hello ${name}</h1>`
```

<!-- original:
Since lit-html templates almost always need to merge in data from JavaScript values, and be able to update DOM when that data changes, they'll most often be written within functions that take some data and return a lit-html template, so that the function can be called multiple times:
-->

lit-htmlテンプレートはほとんどの場合、JavaScriptの値からデータをマージし、そのデータが変更されたときにDOMを更新する必要があるため、ほとんどの場合、データを取り込んでlit-htmlテンプレートを返す関数内で記述されます。関数は複数回呼び出すことができます：

```js
let myTemplate = (data) => html`
  <h1>${data.title}</h1>
  <p>${data.body}</p>`;
```

<!-- original:
lit-html is _lazily_ rendered. Calling this function will evaluate the template literal using lit-html `html` tag, and return a `TemplateResult` - a record of the template to render and data to render it with. `TemplateResults` are very cheap to produce and no real work actually happens until they are _rendered_ to the DOM.
-->

lit-htmlは遅延描画されます。この関数を呼び出すと、literal-html htmlタグを使用してテンプレートリテラルを評価し、TemplateResultレンダリングするテンプレートとそれをレンダリングするデータのレコードを返します。TemplateResults生成するのが非常に安価であり、実際にDOMにレンダリングされるまで実際には何も起こりません。

## 描画について(Rendering)

<!-- original:
To render a `TemplateResult`, call the `render()` function with a result and DOM container to render to:
-->

TemplateResultをレンダリングするには、レンダリングするrender()結果とDOMコンテナで関数を呼び出します。

```js
const result = myTemplate({title: 'Hello', body: 'lit-html is cool'});
render(result, document.body);
```

[template literals]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals
