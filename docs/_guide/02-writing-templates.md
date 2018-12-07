---
layout: post
title: テンプレートをつくる
slug: writing-templates
---

{::options toc_levels="1..3" /}
* ToC
{:toc}

<!-- original:
lit-html is a templating library that provides fast, efficient rendering and updating of HTML. It lets you express web UI as a function of data. 

This section introduces the main features and concepts in lit-html.
-->

lit-htmlは、高速で効率的にHTMLを描画、更新するテンプレートライブラリです。様々なデータを扱うWeb UIをすぐに作ることができます。

この章では、lit-htmlの主な機能と概念を紹介します。

## 静的HTMLの描画

<!-- original:
The simplest thing to do in lit-html is to render some static HTML. 
-->

lit-htmlで行う最も簡単なことは、静的なHTMLを描画することです。

```js
import {html, render} from 'lit-html'
// テンプレートを定義
const  myTemplate = html`<div>Hello World</div>`

// テンプレートを描画
render(myTemplate, document.body);
```

<!-- original:
The lit-html template is a [_tagged template literal_](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals). The template itself looks like a regular JavaScript string, but enclosed in backticks (`) instead of quotes. The browser passes the string to lit-html's `html` tag function. 

The `html` tag function returns a `TemplateResult`—a lightweight object that represents the template to be rendered.

The `render` function actual creates DOM nodes and appends them to a DOM tree. In this case, the rendered DOM replaces the contents of the document's `body` tag.
-->

lit-htmlテンプレートは、[_タグ付きテンプレートリテラル_](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)です。テンプレート自体は通常のJavaScript文字列のように見えますが、バッククォートで囲われています(`` ` ``)。ブラウザはlit-htmlの`html`タグ関数を文字列として認識します。

`html`タグ関数は`TemplateResult` (テンプレートが描画されるのに使用される軽量オブジェクト)を返します。


実際に`render`関数は、DOMを作成し、それらをDOMツリーに追加します。上記の例では描画されたDOMがページのbodyタグの内容を置き換えます。

## 動的テキストを描画する

<!-- original:
You can't get very far with a static template. lit-html lets you create bindings using <code>${<em>expression</em>}</code> placeholders in the template literal:
-->

基本静的なテンプレートではなにもできません。lit-htmlでは、テンプレートリテラルに<code>${<em>expression</em>}</code>書式のプレースフォルダを使ってデータの表示(バインディング)ができます。

```js
const aTemplate = html`<h1>${title}</h1>`;
```

<!--
To make your template dynamic, you can create a _template function_. Call the template function any time your data changes.
-->

動的なテンプレート作るのにテンプレート関数を作ることができます。値が変わるたびにテンプレート関数を呼び出されます。

```js
import {html, render} from 'lit-html'
// テンプレート関数を定義
const  myTemplate = (name) => html`<div>Hello ${name}</div>`;

// 値によってテンプレートが描画される
render(myTemplate('world'), document.body);
...
// ... あとで ... 
// 違う値によってテンプレートを描画
render(myTemplate('lit-html'), document.body);
```

<!-- original:
When you call the template function, lit-html captures the current expression values. The template function doesn't create any DOM nodes, so it's fast and cheap.

The template function returns a `TemplateResult` that's a function of the input data. This is one of the main principles behind using lit-html: **creating UI as a _function_ of state**. 

When you call `render`, **lit-html only updates the parts of the template that have changed since the last render.** This makes lit-html updates very fast.
-->

テンプレート関数が呼び出されると、lit-htmlはその時点でのJavaScript式の値を取得します。テンプレート関数はDOMを作らないので、高速で軽く動作します。

テンプレート関数は、入力値への関数として`TemplateResult`を返します。これはlit-htmlの主な原則の1つです: ** 状態の _関数_ としてUIをつくる**

`render`を呼び出すと、**lit-htmlは最後に実行された描画において、変更されたテンプレートの一部分のみを更新します**。これにより、lit-htmlの更新は非常に高速になっています。

## JavaScritp式を使う

<!-- original:
The previous example shows interpolating a simple text value, but the binding can include any kind of JavaScript expression:
-->

前述の例では単純にテキストを挿入していますが、JavaScript式も使えます:

```js
const  myTemplate = (subtotal, tax) => html`<div>Total: ${subtotal + tax}</div>`;
const myTemplate2 = (name) => html`<div>${formatName(name.given, name.family, name.title)}</div>`;
```

## 属性へのバインド

<!-- original:
In addition to using expressions in the text content of a node, you can bind them to a node's attribute and property values, too.

By default, an expression in the value of an attribute creates an attribute binding:
-->

テキストにJavaScript式を使用することに加え、nodeの属性(attribute)やプロパティ(property)にも値をバインドすることができます。

デフォルトでは、属性への値の変更によって属性も変更されます:

```js
// set the class attribute
const myTemplate(data) = html`<div class=${data.cssClass}>Stylish text.</div>`;
```

<!-- original:
Since attribute values are always strings, the expression should return a value that can be converted into a string.

Use the `?` prefix for a boolean attribute binding. The attribute is added if the expression evaluates to a truthy value, removed if it evaluates to a falsy value:
-->

属性値は常に文字列となるので、JavaScript式は文字列に変換する必要があります。

'?'を接頭辞(prefix)に使うことによって属性に真偽値(boolean)を設定します。真偽値が真と評価された(truthy)時に属性が追加され、偽(falsy)の場合に取り除かれます:

```js
const myTemplate2(data) = html`<div ?disabled="${!data.active}">Stylish text.</div>`;
```

## プロパティへのバインド

<!-- original:
You can also bind to a node's JavaScript properties using the `.` prefix and the property name:
-->

nodeのJavaScriptプロパティにバインドするには`.`を接頭辞を使います:

```js
const myTemplate3(data) = html`<my-list .listItems=${data.items}></my-list>`
```

<!-- original:
You can use property bindings to pass complex data down the tree to subcomponents.

Note that the property name in this example—`listItems`—is mixed case. Although HTML attributes are case-insensitive, lit-html preserves the case when it processes the template.
-->

プロパティ・バインディングによって、複雑なデータをサブコンポーネントに渡すことができます。

この例のプロパティ名(listItems)は大文字と小文字が混在していることに注意してください。HTML属性は大文字と小文字を区別しませんが、lit-htmlはテンプレートを処理する際に大文字と小文字を区別します。

## イベントハンドラの追加

<!-- original:
Templates can also include declarative event handlers. An event handler looks like an attribute binding, but with the prefix `@` followed by an event name:
-->

テンプレートには宣言型イベントハンドラも含めることができます。イベントハンドラは属性へのバインドのように見えますが、接頭辞`@`後にイベント名が続きます:

```js
const myTemplate = () => html`<button @click=${clickHandler}>Click Me!</button>`
```

<!-- original:
This is equivalent to calling `addEventListener('click', clickHandler)` on the button element.

The event handler can be either a plain function, or an object with a `handleEvent` method:
-->

これはボタン要素への`addEventListener('click', clickHandler)`と同じです。

イベントハンドラは、普通の関数か、`handleEvent`メソッドを持つオブジェクトのいずれかになります。

```js
const clickHandler = {
  // handleEvent メソッドが必要
  handleEvent(e) { 
    console.log('clicked!');
  }
  // 0か一つ以上のイベントリスナオプションを
  // 持つことができます: capture, passive, onceなど
  capture: true;
}
```

### テンプレートの入れ子

<!-- original:
You can also compose templates to create more complex templates. When a binding in the text content of a template returns a `TemplateResult`, the `TemplateResult` is interpolated in place.
-->

さらに複雑なテンプレートを作成するためにテンプレートを入れ子にできます。テキストを表示する `TemplateResult` であれば、`TemplateResult` はそこに挿入されます。

```js
const myHeader = html`<h1>Header</h1>`;
const myPage = html`
  ${myHeader}
  <div>Here's my main page.</div>
`;
```

<!-- original:
You can use any expression that returns a `TemplateResult`, like another template function: 
-->

`TemplateResult`を返すテンプレート関数であれば、一緒に使えます:

```js
// some complex view
const myListView = (items) => html`<ul>...</ul>`;

const myPage(data) = html`
  ${myHeader}
  ${myListView(data.items)}
`;
```

<!-- original:
 Composing templates opens a number of possibilities, including conditional and repeating templates.
-->

テンプレートをつくることにより、条件付きと繰り返しのテンプレートを含めて大くの可能性が開かれます。


### 条件付きテンプレート

<!-- original:
lit-html has no built-in control-flow constructs. Instead you use normal JavaScript expressions and statements.
-->

lit-htmlには組み込みの制御方法はありません。代わりに、通常のJavaScript式とステートメントを使用します。

#### 三項演算子による条件式

<!-- original:
Ternary expressions are a great way to add inline conditionals:
-->

三項演算子は、インラインで条件を追加するのに最適です。

```js
html`
  ${user.isloggedIn
      ? html`Welcome ${user.name}`
      : html`Please log in`
  }
`;
```


#### if文による条件式

<!-- original:
You can express conditional logic with if statements outside of a template to compute values to use inside of the template:
-->

if文の条件をテンプレートの外部で定義し、テンプレート内で使うことができます。

```js
getUserMessage() {
  if (user.isloggedIn) {
    return html`Welcome ${user.name}`;
  } else {
    return html`Please log in`;
  }
}

html`
  ${getUserMessage()}
`
```

## 繰り返し処理のテンプレート

<!-- original:
You can use standard JavaScript constructs to create repeating templates. 

lit-html also provides some special functions, called _directives_, for use in templates. You can use the  `repeat` directive to build certain kinds of dynamic lists more efficiently.
-->

標準のJavaScriptを使用して繰り返し処理のテンプレートをつくることができます。

また、lit-htmlには_ディレクティブ(directives)_と呼ばれるテンプレートで使用するための特別な関数がいくつか用意されています。例えば`repeat`ディレクティブを使用して、特定の動的リストをより効率的に描画することができます。

###  Array.mapによる繰り返し

<!-- original:
To render lists, you can use `Array.map` to transform a list of data into a list of templates:
-->

リストを描画するのに、`Array.map`を使ってデータのリストをテンプレートのリストに変換ができます：

```js
html`
  <ul>
    ${items.map((item) => html`<li>${item}</li>`)}
  </ul>
`;
```

<!-- original:
Note that this expression returns an array of `TemplateResult` objects. lit-html will render an array or iterable of subtemplates and other values.
-->

この式は`TemplateResult`オブジェクトの配列を返していることに注意してください。lit-htmlは、配列やiterableなサブテンプレートやその他の値を描画します。

### ループ文による繰り返し

<!-- original:
You can also build an array of templates and pass it in to a template binding.
-->

また、別にテンプレートの配列を作成し、そのままテンプレートに渡すこともできます。

```js
const itemTemplates = [];
for (const i of items) {
  itemTemplates.push(html`<li>${i}</li>`);
}

html`
  <ul>
    ${itemTemplates}
  </ul>
`;
```

### repeatディレクティブによる繰り返し

<!-- original:
In most cases, using loops or `Array.map` is an efficient way to build repeating templates. However, if you want to reorder a large list, or mutate it by adding and removing individual entries, this approach can involve recreating a large number of DOM nodes. 

The `repeat` directive can help here. Directives are special functions that provide extra control over rendering. lit-html comes with some built-in directives like `repeat`. 

The repeat directive performs efficient updates of lists based on user-supplied keys:
-->

ほとんどの場合、Array.mapが繰り返し処理を行う効率的な方法です。ただし、大きなリストを並べ替えたり、個々のエントリを追加・削除する場合は、多数のDOMノードを効率的に再作成する必要があります。

こういった場合に `repeat` _ディレクティブ_ が使えます。ディレクティブ(Directive)はレンダリングを特別に制御する拡張可能な関数です。lit-htmlには、`repeat`のような組み込みのディレクティブが付属してます。

`repeat`ディレクティブは、開発者がリストにおいて指定するユニークキーに基づき効率的に描画更新します。

`repeat(items, keyFunction, itemTemplate)`

<!-- original:
Where:

*   `items` is an Array or iterable.
*   `keyFunction` is a function that takes a single item as an argument and returns a guaranteed unique key for that item.
*   `itemTemplate` is a template function that takes the item and its current index as arguments, and returns a TemplateResult.

For example:
-->

引数:

* `items` 配列もしくはiterable
* `keyFunction` itemsより1つずつ取り出される要素を引数としてとり、その要素のユニークキーを返す関数
* `itemTemplate` 各要素と現在のインデックスを引数としてとるテンプレート関数であり、`TemplateResult`を返す

例えば:

```js
const employeeList = (employees) => html` 
  <ul> 
    ${repeat(employees, (employee) => employee.id, (employee) =>
        html`<li>employee.familyName, employee.givenName</li>`}
  </ul>`
```

<!-- original:
If you re-sort the `employees` array, the `repeat` directive reorders the existing DOM nodes. 

To compare this to lit-html's default handling for lists, consider reversing a large list of names:

*   For a list created using `Array.map`, lit-html maintains the DOM nodes for the list items, but reassigns the values. 
*   For a list created using `repeat`, the `repeat` directive reorders the _existing_ DOM nodes, so the nodes representing the first list item move to the last position.

Which repeat is more efficient depends on your use case: if updating the DOM nodes is more expensive than moving them, use the repeat directive. Otherwise, use `Array.map` or looping statements.
-->

`employees`配列を再度並び換えする場合、`repeat`ディレクティブは既存のDOMノードを並び替えます。

これは普通に並び替えをする場合と何が違うのかというと、大きなリストを逆順に並び替えにすることを想像してください。

* `Array.map`を使った場合、lit-htmlはDOMノードを維持しますが、全てに値を再度割り当てしてしまいます。
* `repeat`ディレクティブの場合、既存のDOMノードを並べ替えるので、リストの最初のノードが最後の位置に移動します。

どちらがより効率的なのかは、ユースケースによって異なります。DOMノードを更新する方が移動させるよりもコストがかかる場合は、`repeat`ディレクティブを使用してください。それ以外の場合は、Array.mapを使用するか、ループを使用します。

## テンプレートのキャッシュ: cacheディレクティブ

<!-- original:
In most cases, JavaScript conditionals are all you need for conditional templates. However, if you're switching between large, complicated templates, you might want to save the cost of recreating DOM on each switch. 

In this case, you can use the `cache` _directive_. Directives are special functions that provide extra control over rendering. The cache directive caches DOM for templates that aren't being rendered currently. 
-->

ほとんどの場合、条件付きテンプレートはJavaScriptの条件式で済みます。ただし、大規模で複雑なテンプレートを置き換える場合にDOMを再作成するコストを節約したい場合があります。

こういった場合に `cache`_ディレクティブ_ が使えます。ディレクティブ(Directive)はレンダリングを特別に制御する拡張可能な関数です。cacheディレクティブは、現在描画していないテンプレートのDOMを保持(キャッシュ)します。

```js
const detailView = (data) => html`<div>...</div>`; 
const summaryView = (data) => html`<div>...</div>`;

html`${cache(data.showDetails
  ? detailView(data) 
  : summaryView(data)
)}`
```

<!-- original:
When lit-html re-renders a template, it only updates the modified portions: it doesn't create or remove any more DOM than it needs to. But when you switch from one template to another, lit-html needs to remove the old DOM and render a new DOM tree. 

The `cache` directive caches the generated DOM for a given binding and input template. In the example above, it would cache the DOM for both the  `summaryView` and `detailView` templates. When you switch from one view to another, lit-html just needs to swap in the cached version of the new view, and and update it with the latest data.
-->

lit-htmlがテンプレートを再度描画する場合、変更された部分のみが更新されるので、必要以上にDOMを作成したり削除されることはありません。ただし、あるテンプレートから別のテンプレートに切り替えるときは、lit-htmlは古いDOMを削除して新しいDOMツリーを描画します。

`cache`ディレクティブは、バインディングや入力用に生成されたDOMをキャッシュします。上記の例ではsummaryView、detailViewテンプレートの両方がDOMがキャッシュされます 。あるビューから別のビューに切り替えると、lit-htmlはキャッシュされた新しいビューで入れ替え、最新のデータで更新します。
