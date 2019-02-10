---
layout: post
title: スタイリング
slug: styling-templates
---

{::options toc_levels="1..3" /}
* ToC
{:toc}

<!-- original:
lit-html focuses on one thing: rendering HTML. How you apply styles to the HTML lit-html creates depends on how you're using it—for example, if you're using lit-html inside a component system like LitElement, you can follow the patterns used by that component system.

In general, how you style HTML will depend on whether you're using shadow DOM:

*   If you aren't using shadow DOM, you can style HTML using global style sheets.
*   If you're using shadow DOM (for example, in LitElement), then you can add style sheets inside the shadow root.

To help with dynamic styling, lit-html provides two directives for manipulating an element's `class` and `style` attributes:

*   [`classMap`](template-reference#classmap) sets classes on an element based on the properties of an object.
*   [`styleMap`](template-reference#stylemap) sets the styles on an element based on a map of style properties and values.
-->

lit-htmlはHTMLをレンダリングすることにのみ焦点を当てています。lit-htmlがどのようにスタイルを適用するかは、あなたがそれをどのように使用しているかによって異なります。LitElementのようなコンポーネントシステム内でlit-htmlを使用している場合は、そのコンポーネントシステムで使用されているパターンに従うことができます。

一般的にどのようにHTMLのスタイリングするかは、shadow DOMを使用しているかどうかによって異なります:

*   shadow DOMを使用していない場合は、グローバルスタイルシートを使用してHTMLのスタイルを設定できます。
*   shadow DOMを使用している場合は(たとえばLitElementなど)、shadow rootの内側にスタイルシートを追加できます。

動的なスタイリングをするのに、lit-htmlは`class`と`style`属性を操作するための二つのディレクティブを提供します:

*   [`classMap`](template-reference#classmap) オブジェクトのプロパティに基づいて要素にクラスを設定します。
*   [`styleMap`](template-reference#stylemap) スタイルプロパティと値のマップに基づいて、要素にスタイルを設定します。

<!-- original:
## Rendering in shadow DOM

When rendering into a shadow root, you usually want to add a style sheet inside the shadow root to the template, to you can style the contents of the shadow root. 

```js
html`
  <style>
    :host { ... } 
    .test { ... }
  </style> 
  <div class="test">...</div> 
`;
```

This pattern may seem inefficient, since the same style sheet is reproduced in a each instance of an element. However, the browser can deduplicate multiple instances of the same style sheet, so the cost of parsing the style sheet is only paid once. 

A new feature available in some browsers is [Constructable Stylesheets Objects](https://wicg.github.io/construct-stylesheets/). This proposed standard allows multiple shadow roots to explicitly share style sheets. LitElement uses this feature in its [static `styles` property](https://lit-element.polymer-project.org/guide/styles#define-styles-in-a-static-styles-property). 
-->
## shadow DOMでのレンダリング

shadow rootにレンダリングするときは、通常shadow rootの内側にスタイルシートを追加して、shadow rootの内容をスタイルすることができます。

```js
html`
  <style>
    :host { ... } 
    .test { ... }
  </style> 
  <div class="test">...</div> 
`;
```

同じスタイルシートが要素の各インスタンスで再現されるので、このパターンは非効率的に見えるかもしれません。ただし、ブラウザは同じスタイルシートの複数のインスタンスを重複排除できるため、スタイルシートを解析するためのコストは1回しか支払われません。

いくつかのブラウザで利用可能な新機能は[Constructable Stylesheets Objects](https://wicg.github.io/construct-stylesheets/)です。この標準提案では、複数のshadow rootがスタイルシートを明示的に共有することを許可しています。LitElementではこの機能を[静的`styles`プロパティ](https://lit-element.polymer-jp.org/guide/styles#define-styles-in-a-static-styles-property)で使用しています。

<!-- original:
### Bindings in style sheets 

Binding to values in the style sheet is an antipattern, because it defeats the browser's style sheet optimizations. It's also not supported by the ShadyCSS polyfill.

```js
// DON'T DO THIS
html`
  <style>
    :host {
      background-color: ${themeColor};
    }
  </style>
  ... 
```

Alternatives to using bindings in a style sheet:

*   Use [CSS custom properties](https://developer.mozilla.org/en-US/docs/Web/CSS/--*) to pass values down the tree.
*   Use bindings in the `class` and `style` attributes to control the styling of child elements.

See [Inline styles with styleMap](#stylemap) and [Setting classes with classMap](#classmap) for examples of binding to the `style` and `class` attributes.
-->
### スタイルシート内のバインディング

スタイルシート内で値をバインドすることは、ブラウザのスタイルシートの最適化が無効になるため、アンチパターンとなります。ShadyCSSのポリフィルでもサポートされていません。

```js
// こうしてはいけません!
html`
  <style>
    :host {
      background-color: ${themeColor};
    }
  </style>
  ... 
```

スタイルシートでバインディングする別の方法:

*   [CSSカスタムプロパティ](https://developer.mozilla.org/en-US/docs/Web/CSS/--*)を使ってDOMツリー経由で値を渡す
*   `class`と`style`属性へのバインディングを使って子要素のスタイルを制御します

`style`と`class`属性へのバインディングの例として[styleMapによるインラインスタイル](#stylemap)と[classMapによるクラス設定](#classmap)を参照してください。

<!-- original:
### Polyfilled shadow DOM: ShadyDOM and ShadyCSS

If you're using shadow DOM, you'll probably need to use polyfills to support older browsers that don't implement shadow DOM natively. [ShadyDOM](https://github.com/webcomponents/shadydom) and [ShadyCSS](https://github.com/webcomponents/shadycss) are polyfills, or shims, that emulate shadow DOM isolation and style scoping. 

The lit-html `shady-render` module provides necessary integration with the shady CSS shim. If you're writing your own custom element base class that uses lit-html and shadow DOM, you'll need to use `shady-render` and also take some steps on your own. 

The [ShadyCSS README](https://github.com/webcomponents/shadycss#usage) provides some directions for using shady CSS. When using it with `lit-html`:

*   Import `render` and `TemplateResult` from the `shady-render` library.
*   You **don't** need to call `ShadyCSS.prepareTemplate`.  Instead pass the scope name as a render option. For custom elements, use the element name as a scope name. For example:

    ```js
    import {render, TemplateResult} from 'lit-html/lib/shady-render';

    class MyShadyBaseClass extends HTMLElement {

      // ...

      _update() {
        render(this.myTemplate(), this.shadowRoot, { scopeName: this.tagName.toLowerCase() });
      } 
    }
    ```

    Where `this.myTemplate` is a method that returns a `TemplateResult`.

*   You **do** need to call `ShadyCSS.styleElement` when the element is connected to the DOM, and in case of any dynamic changes that might affect custom property values.

	For example, consider a set of rules like this: 
    ```js
    my-element { --theme-color: blue; }
	main my-element { --theme-color: red; }
    ```

	If you add an instance of `my-element` to a document, or move it, a different value of `--theme-color` may apply. On browsers with native custom property support, these changes will take place automatically, but on browsers that rely on the custom property shim included with shadyCSS, you'll need to call `styleElement`.

    ```js
    connectedCallback() {
      super.connectedCallback();
      if (window.shadyCSS !== undefined) {
          window.shadyCSS.styleElement(this);
      }
    }
    ```
-->
### shadow DOMのポリフィル: ShadyDOMとShadyCSS

shadow DOMを使用している場合、shadow DOMをネイティブに実装していない古いブラウザをサポートするにはポリフィルを使用する必要があるでしょう。[ShadyDOM](https://github.com/webcomponents/shadydom)と[ShadyCSS](https://github.com/webcomponents/shadycss)はポリフィルもしくは補助的なshimとして、shadow DOMとスタイルのスコーピングをエミュレートします。

lit-htmlの`shady-render`モジュールはshady CSS shimを使用するのに必要な機能を提供します。lit-htmlとshadow DOMを使うカスタム要素の基本クラスを書いているなら、`shady-render`を使うのにいくつかのステップを踏む必要があるでしょう。

[ShadyCSS README](https://github.com/webcomponents/shadycss#usage)はshady CSSを使うのに参考になるでしょう。もし`lit-html`で使用するには:

*   `shady-render`ライブラリから`render`と`TemplateResult`をインポートします。
*   `ShadyCSS.prepareTemplate`をコールする**必要はありません**。代わりに、スコープ名をレンダリングオプションとして渡します。カスタム要素の場合は、要素名をスコープ名として使用します。例えば:

    ```js
    import {render, TemplateResult} from 'lit-html/lib/shady-render';

    class MyShadyBaseClass extends HTMLElement {

      // ...

      _update() {
        render(this.myTemplate(), this.shadowRoot, { scopeName: this.tagName.toLowerCase() });
      } 
    }
    ```

    `this.myTemplate`は`TemplateResult`を返すメソッドです。

*   要素をDOMに接続され、カスタムプロパティに影響を与える動的な変更の場合する時に`ShadyCSS.styleElement`をコール**必要はありません**

	たとえば、次のような一連の規則を考えます: 
    ```js
    my-element { --theme-color: blue; }
    main my-element { --theme-color: red; }
    ```

	`my-element`のインスタンスを文書に追加したり移動したりする場合、`--theme-color`に異なる値が適用されるかもしれません。 ネイティブのカスタムプロパティをサポートするブラウザでは、これらの変更は自動的に行われますが、shadyCSSに含まれるカスタムプロパティシムに依存するブラウザでは、`styleElement`を呼び出す必要があります。

    ```js
    connectedCallback() {
      super.connectedCallback();
      if (window.shadyCSS !== undefined) {
          window.shadyCSS.styleElement(this);
      }
    }
    ```

<!-- original:
## Inline styles with styleMap

You can use the `styleMap` directive to set inline styles on an element in the template.

```js
const normalStyles = {};
const highlightStyles = { color: 'white', backgroundColor: 'red'};
let highlight = true;

const myTemplate = () => {
  html`
    <div style=${styleMap(highlight ? highlightStyles : normalStyles)}>
      Hi there!
    </div>
  `;
};
```

More information: see See [styleMap](template-reference#stylemap) in the Template syntax reference.
-->
## styleMapによるインラインスタイル {#stylemap}

`styleMap`ディレクティブを使ってテンプレートの要素にインラインスタイルを設定することができます。

```js
const normalStyles = {};
const highlightStyles = { color: 'white', backgroundColor: 'red'};
let highlight = true;

const myTemplate = () => {
  html`
    <div style=${styleMap(highlight ? highlightStyles : normalStyles)}>
      Hi there!
    </div>
  `;
};
```

詳細については、テンプレート・リファレンスの[styleMap](template-reference#stylemap)を参照してください。

<!-- original:
## Setting classes with classMap

Like `styleMap`, the `classMap` directive lets you set a group of classes based on an object:

```js
// Define a base set of classes for all menu items
const baseClasses = { 
  'menu-item': true,
  // ...
};

const itemTemplate = (item) => {
  // Merge in dynamically-generated classes
  const mergedClasses = Object.assign({active: item.active}, baseClasses);
  return html`<div class=${classMap(mergedClasses)}>Classy text</div>`
}
```

More information: see [classMap](template-reference#classmap) in the Template syntax reference.
-->
## classMapによるクラス設定 {#classmap}

`styleMap`ディレクティブと同様に、`classMap`ディレクティブはオブジェクトに基づいてクラスのグループを設定できます:

```js
// すべてのメニュー項目に対してクラスの基本セットを定義する
const baseClasses = { 
  'menu-item': true,
  // ...
};

const itemTemplate = (item) => {
  // 動的に生成されたクラスにマージする
  const mergedClasses = Object.assign({active: item.active}, baseClasses);
  return html`<div class=${classMap(mergedClasses)}>クラス装飾されたテキスト</div>`
}
```

詳細については、テンプレート・リファレンスの[classMap](template-reference#classmap)を参照してください。
