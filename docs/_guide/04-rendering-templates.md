---
layout: post
title: レンダリング
slug: rendering-templates
---

{::options toc_levels="1..3" /}
* ToC
{:toc}

<!-- original:
A lit-html template expression does not cause any DOM to be created or updated. It's only a description of DOM, called a `TemplateResult`. To actually create or update DOM, you need to pass the `TemplateResult` to the `render()` function, along with a container to render to:

```js
import {html, render} from 'lit-html';

const sayHi = (name) => html`<h1>Hello ${name}</h1>`;
render(sayHi('Amy'), document.body);

// subsequent renders will update the DOM
render(sayHi('Zoe'), document.body);
```
-->

lit-htmlのテンプレートにおけるJavaScript評価式によってDOMが作成または更新されることはありません。
これは`TemplateResult`と呼ばれるDOMの雛形にすぎません。実際にDOMを作成または更新するには、レンダリングするコンテナとともに `TemplateResult`を`render()`関数に渡す必要があります:

```js
import {html, render} from 'lit-html';

const sayHi = (name) => html`<h1>こんにちは ${name}</h1>`;
render(sayHi('アミー'), document.body);

// このレンダリングはDOMを更新します
render(sayHi('ゾエ'), document.body);
```

<!-- original:
## Render Options

The `render` method also takes an `options` argument that allows you to specify the following options:

*   `eventContext`: The `this` value to use when invoking event listeners registered with the `@eventName` syntax. This option only applies when you specify an event listener as a plain function. If you specify the event listener using an event listener object, the listener object is used as the `this` value. See [Add event listeners](writing-templates#add-event-listeners) for more on event listeners.

*   `templateFactory`: The `TemplateFactory` to use. This is an advanced option. A `TemplateFactory` creates a template element from a `TemplateResult`, typically caching templates based on their static content. Users won't usually supply their own `TemplateFactory`, but libraries that use lit-html may implement custom template factories to customize template handling.

    The `shady-render` module provides its own template factory, which it uses to preprocess templates to integrate with the shadow DOM polyfills (shadyDOM and shadyCSS). 

For example, if you're creating a component class, you might use render options like this:

```js
class MyComponent extends HTMLElement {
  // ...

  _update() {
    // Bind event listeners to the current instance of MyComponent
    render(this._template(), this._renderRoot, {eventContext: this});
  }
}

```

Render options should *not* change between subsequent `render` calls. 
-->

## オプション

`render`メソッドは`options`引数を取ります。これを使うと以下のオプションを指定することができます:

*   `eventContext`: `@eventName`構文で登録したイベントリスナーを呼び出すときに使う`this`の値。このオプションは、イベントリスナーをプレーンな関数として指定した場合にのみ適用されます。イベントリスナーオブジェクトを使ってイベントリスナーを指定した場合、リスナーオブジェクトは `this`を値として使います。イベントリスナーの詳細については[イベントリスナーを追加する](writing-templates#add-event-listeners)を参照してください。

*   `templateFactory`: 使用する`TemplateFactory`を指定。これは高度なオプションです。`TemplateFactory`は`TemplateResult`からテンプレート要素を作成し、通常は静的コンテンツに基づいてテンプレートをキャッシュします。ユーザは通常自分自身で`TemplateFactory`を提供しませんが、テンプレート処理をカスタマイズするためにテンプレートファクトリを実装するかもしれません。

    `shady-render`モジュールは独自のテンプレートファクトリを提供しており、shadow DOMポリフィル（shadyDOMおよびshadyCSS）と統合するためのテンプレートが前処理として使用されています。

たとえば、コンポーネントクラスを作成している場合は、次のようなレンダリングオプションを使用します:

```js
class MyComponent extends HTMLElement {
  // ...

  _update() {
    // イベントリスナーをMyComponentの現在のインスタンスにバインドします
    render(this._template(), this._renderRoot, {eventContext: this});
  }
}

```

レンダリングオプションは、その後の`render`呼び出しの間に**変更されるべきではありません**
