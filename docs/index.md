---
layout: default
---
<header class="hero" markdown="0">
{% include topnav.html %}
<div class="wrapper">
<div class="hero-title">{{ site.name }}</div>
<p class="hero-caption">{{ site.description }}</p>
<a class="hero-link link-with-arrow" href="{{ site.baseurl }}/guide">はじめる</a>
</div>
</header>

<section>
<div class="wrapper">

## 次世代HTMLテンプレートエンジン in JavaScript

<!-- original:
lit-html lets you write HTML templates in JavaScript, then efficiently render and _re-render_ those templates together with data to create and update DOM:
-->

lit-htmlは、JavaScriptの中でHTMLテンプレートを作成し(HTML in JS)、DOMを作成・更新するのにデータとテンプレートを効率的に描画・ _再描画_ します。:

```js
import {html, render} from 'lit-html';

// `html`タグを使った小さなlit-htmlテンプレート:
let sayHello = (name) => html`<h1>Hello ${name}</h1>`;

// `render()`関数によって描画されます:
render(sayHello('World'), document.body);

// VDOMによって差分検知されることなく、値が変更された時にのみ再描画されます!
render(sayHello('Everyone'), document.body);
```

</div>
</section>

<section class="grey-bg">
<div class="wrapper">

## なぜ lit-html なのか?

<div class="responsive-row">
<div style="flex: 1">

### Efficient - 効率的

<!-- original:
lit-html is extremely fast. It uses fast platform features like HTML `<template>` elements with native cloning.

Unlike VDOM libraries, lit-html only ever updates the parts of templates that actually change - it doesn't re-render the entire view.
-->

lit-html はとっても高速です。 lit-htmlはネイティブクローニング(native cloning)によるHTMLの`<template>`要素などの高速なプラットフォームを活用しています。

VDOMライブラリとは違って、lit-htmlは画面全体を再描画せず、実際に変更されるテンプレートの一部分だけを更新します。

</div>
<div style="flex: 1">

### Expressive - 表現力豊か

<!-- original:
lit-html gives you the full power of JavaScript and functional programming patterns. 

Templates are values that can be computed, passed to and from functions and nested. Expressions are real JavaScript and can include anything you need.

lit-html support many kind of values natively: strings, DOM nodes, heterogeneous lists, nested templates and more.
-->

lit-htmlではJavaScriptと関数型プログラミングパターンのフルパワーが使えます。

テンプレート(Templates)では加工した値を使ったり、関数に値を渡したり受けとったり、または入れ子にして使います。式(Expressions)では生のJavaScriptが評価され、必要なことはなんでもできます。

lit-htmlは、文字列やDOMノード、多様なリスト、ネストされたテンプレートなど、多くの種類の値をネイティブでサポートします。

</div>
<div style="flex: 1">

### Extensible - 拡張可能

<!-- original:
lit-html is extremely customizable and extensible.

Directives customize how values are handled, allowing for asynchronous values, efficient keyed-repeats, error boundaries, and more. lit-html is like your very own a template construction kit.
-->

lit-htmlはとってもカスタマイズしやすく拡張しやすいです。

ディレクティブ(Directives)を定義することにより、値の処理や非同期処理、効率的なキー付き繰り返し処理やエラーなど様々なことがカスタマイズできます。lit-htmlはあなた専用のテンプレート構築キットとなるでしょう。

</div>
</div>
</div>
</section>

<section>
<div class="wrapper">
<div class="responsive-row center">
<div style="max-width: 600px">

lit-htmlはいわゆるフレームワークではなく、コンポーネントモデルも含まれていません。 DOMを効率的に作成して更新するという、1つのことだけにフォーカスしています。 単純な処理では単体で使え、複雑で多機能なUI開発では他のフレームワークやWebコンポーネントなどのコンポーネントと組み合わせることができます。

<!-- original:
lit-html is not a framework, nor does it include a component model. It focuses on one thing and one thing only: efficiently creating and updating DOM. It can be used standalone for simple tasks, or combined with a framework or component model, like Web Components, for a full-featured UI development platform.
-->

</div>
</div>
</div>
</section>

<section>
<div class="wrapper" style="text-align: center">
<h2>ブラウザ互換</h2>
<p><!-- original:
lit-html works in all major browsers (Chrome, Firefox, IE, Edge, Safari, and Opera). 
-->

lit-htmlは全ての主要なブラウザで動作します(Chrome, Firefox, IE, Edge, Safari, and Opera)

</p>
<div>
<img width="70" height="70" src="/images/browsers/chrome_128x128.png" alt="chrome logo">
<img width="70" height="70" src="/images/browsers/firefox_128x128.png" alt="firefox logo">
<img width="70" height="70" src="/images/browsers/internet-explorer_128x128.png" alt="internet explorer logo">
<img width="70" height="70" src="/images/browsers/edge_128x128.png" alt="edge logo">
<img width="70" height="70" src="/images/browsers/safari_128x128.png" alt="safari logo">
<img width="70" height="70" src="/images/browsers/opera_128x128.png" alt="opera logo">
</div>
</div>
</section>

<section>
<div class="wrapper">
<div class="responsive-row center">
<div style="max-width: 600px">

## Polymer Summit 2017での公式発表

<iframe src="https://www.youtube.com/embed/ruql541T7gc"
    style="width: 560px; height: 315px; max-width: 100%; border: none"
    allow="autoplay; encrypted-media" allowfullscreen></iframe>

</div>
</div>
</div>
</section>
