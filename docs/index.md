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

<section id="section-snippet">
<div class="wrapper">

<div class="alert alert-info">
<p>日本語翻訳(作業中)</p>
</div>

<h1 class="title">
<!-- original:
Next-generation HTML Templates in JavaScript
-->
次世代HTMLテンプレート in JavaScript
</h1>

<div class="responsive-row">

<!-- original:
<h3 class="description" style="flex: 1; margin-bottom: 0; max-width: 600px;">lit-html lets you write HTML templates in JavaScript, then efficiently render and _re-render_ those templates together with data to create and update DOM:</h3>
-->
<h3 class="description" style="flex: 1; margin-bottom: 0; max-width: 600px;">lit-htmlは、JavaScriptでHTMLテンプレートを書くことができ(HTML in JS)、DOMを作成・更新するのに必要なデータとテンプレートを効率的に描画・ _再描画_ します:</h3>

<div style="flex: 2">
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

</div>
</div>
</section>

<section>
<div class="wrapper">

<h1 class="title">
<!-- original:
Why use lit-html?
-->
lit-html を使う理由は?
</h1>

<div class="responsive-row">
<div style="flex: 1">

## Efficient - 効率的

<!-- original:
lit-html is extremely fast. It uses fast platform features like HTML `<template>` elements with native cloning.

Unlike VDOM libraries, lit-html only ever updates the parts of templates that actually change - it doesn't re-render the entire view.
-->

lit-htmlはとっても高速です。lit-htmlはネイティブクローニング(native cloning)による`<template>`要素などの高速なプラットフォームを活用しています。

VDOMライブラリとは違って、lit-htmlは画面全体を再描画せず、実際に変更されるテンプレートの一部分だけを更新します。

</div>
<div style="flex: 1">

## Expressive - 表現力豊か

<!-- original:
lit-html gives you the full power of JavaScript and functional programming patterns.

Templates are values that can be computed, passed to and from functions and nested. Expressions are real JavaScript and can include anything you need.

lit-html support many kind of values natively: strings, DOM nodes, heterogeneous lists, nested templates and more.
-->

lit-htmlを使うことで、JavaScriptと関数型プログラミングのフルパワーを発揮できます。

テンプレート(Templates)は関数へ再計算した値の受け渡しや入れ子にして使えます。式(Expressions)ではJavaScriptがそのまま評価されるので、そこで必要なことはなんでもできます。

lit-htmlは、文字列やDOMノード、多様なリスト、ネストされたテンプレートなど、多くの種類の値をネイティブでサポートします。

</div>
<div style="flex: 1">

## Extensible - 拡張可能

<!-- original:
lit-html is extremely customizable and extensible. Directives customize how values are handled, allowing for asynchronous values, efficient keyed-repeats, error boundaries, and more. lit-html is like your very own a template construction kit.
-->

lit-htmlはとってもカスタマイズしやすく拡張が容易です。ディレクティブ(Directives)を定義することにより、値の処理や非同期処理、効率的なキー付き繰り返し処理やエラーなど様々なことがカスタマイズできます。lit-htmlはあなた専用のテンプレート構築キットとなるでしょう。

</div>
</div>
</div>
</section>

<section>
<div class="wrapper">

<!-- original:
<h1 class="title">Efficiently creating and updating DOM</h1>
<h3 style="max-width: 560px">
lit-html is not a framework, nor does it include a component model. It focuses on one thing and one thing only: efficiently creating and updating DOM. It can be used standalone for simple tasks, or combined with a framework or component model, like Web Components, for a full-featured UI development platform.
</h3>
-->

<h1 class="title">効率的にDOMを作成、更新します</h1>
<h3 style="max-width: 560px">
lit-htmlはいわゆるフレームワークではなく、コンポーネントモデルも含まれていません。 DOMを効率的に作成して更新するという、1つのことだけにフォーカスしています。 単純な処理では単体で使え、複雑で多機能なUI開発では他のフレームワークやWebコンポーネントなどのコンポーネントと組み合わせることができます。
</h3>

</div>
</section>

<section>
<div class="wrapper">

<!-- original:
<h1 class="title">Browser Compatibility</h1>
<h2 class="description">lit-html works in all major browsers (Chrome, Firefox, IE, Edge, Safari, and Opera). </h2>
-->
<h1 class="title">ブラウザ互換</h1>
<h2 class="description">lit-htmlは全ての主要なブラウザで動作します(Chrome, Firefox, IE, Edge, Safari, Opera)</h2>

<div id="browser-thumbnails" style="margin-bottom: 20px;">
<img width="56" width="56" src="{{ site.baseurl }}/images/browsers/chrome_128x128.png" alt="Chrome logo">
<img width="56" width="56" src="{{ site.baseurl }}/images/browsers/firefox_128x128.png" alt="Firefox logo">
<img width="56" width="56" src="{{ site.baseurl }}/images/browsers/internet-explorer_128x128.png" alt="Internet Explorer logo">
<img width="56" width="56" src="{{ site.baseurl }}/images/browsers/edge_128x128.png" alt="Edge logo">
<img width="56" width="56" src="{{ site.baseurl }}/images/browsers/safari_128x128.png" alt="Safari logo">
<img width="56" width="56" src="{{ site.baseurl }}/images/browsers/opera_128x128.png" alt="Opera logo">
</div>

</div>
</section>

<section style="margin-bottom: 40px;">
<div class="wrapper">
<div class="responsive-row">
<div style="max-width: 600px">

<!-- original:
<h1 class="title">Announcement at Polymer Summit 2017</h1>
-->
<h1 class="title">Polymer Summit 2017での公式発表</h1>

<iframe src="https://www.youtube.com/embed/ruql541T7gc"
    style="width: 560px; height: 315px; max-width: 100%; border: none"
    allow="autoplay; encrypted-media" allowfullscreen></iframe>

</div>
</div>
</div>
</section>
