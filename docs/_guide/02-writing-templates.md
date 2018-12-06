---
layout: post
title: テンプレートの作成(Writing templates)
slug: writing-templates
---

{::options toc_levels="1..3" /}
* ToC
{:toc}

<!-- original:
lit-html is a templating library that provides fast, efficient rendering and updating of HTML. It lets you express web UI as a function of data. 

This section introduces the main features and concepts in lit-html.
-->

lit-htmlは、HTMLの高速で効率的なレンダリングと更新を提供するテンプレートライブラリです。それはあなたがデータの関数としてWeb UIを表現することができます。

このセクションでは、lit-htmlの主な機能と概念を紹介します。

## Render static HTML

<!-- original:
The simplest thing to do in lit-html is to render some static HTML. 
-->

lit-htmlで行う最も簡単なことは、静的なHTMLをレンダリングすることです。

```js
import {html, render} from 'lit-html'
// Declare a template
const  myTemplate = html`<div>Hello World</div>`

// Render the template
render(myTemplate, document.body);
```

<!-- original:
The lit-html template is a [_tagged template literal_](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals). The template itself looks like a regular JavaScript string, but enclosed in backticks (`) instead of quotes. The browser passes the string to lit-html's `html` tag function. 

The `html` tag function returns a `TemplateResult`—a lightweight object that represents the template to be rendered.

The `render` function actual creates DOM nodes and appends them to a DOM tree. In this case, the rendered DOM replaces the contents of the document's `body` tag.
-->

lit-htmlテンプレートは、タグ付きテンプレートリテラルです。テンプレート自体は通常のJavaScript文字列のように見えますが、バッククォートで囲まれています（) instead of quotes. The browser passes the string to lit-html's html`タグ関数。

htmlタグ関数は返すTemplateResultレンダリングされるテンプレートを表す-a軽量オブジェクト。

render実際の関数は、DOMノードを作成し、それらをDOMツリーに追加します。この場合、レンダリングされたDOMはドキュメントのbodyタグの内容を置き換えます。

## Render dynamic text content

<!-- original:
You can't get very far with a static template. lit-html lets you create bindings using <code>${<em>expression</em>}</code> placeholders in the template literal:
-->

基本静的なテンプレートではいろいろできません。lit-htmlでは、テンプレートリテラルにプレースホルダを使用してバインディングを作成できます。${expression}

```js
const aTemplate = html`<h1>${title}</h1>`;
```

<!--
To make your template dynamic, you can create a _template function_. Call the template function any time your data changes.
-->

テンプレートを動的にするには、テンプレート関数を作成します。データが変わるたびにテンプレート関数を呼び出します。

```js
import {html, render} from 'lit-html'
// Define a template function
const  myTemplate = (name) => html`<div>Hello ${name}</div>`;

// Render the template with some data
render(myTemplate('world'), document.body);
...
// ... Later on ... 
// Render the template with different data
render(myTemplate('lit-html'), document.body);
```

<!-- original:
When you call the template function, lit-html captures the current expression values. The template function doesn't create any DOM nodes, so it's fast and cheap.

The template function returns a `TemplateResult` that's a function of the input data. This is one of the main principles behind using lit-html: **creating UI as a _function_ of state**. 

When you call `render`, **lit-html only updates the parts of the template that have changed since the last render.** This makes lit-html updates very fast.
-->

テンプレート関数を呼び出すと、lit-htmlは現在の式の値を取得します。テンプレート関数はDOMノードを作成しないので、高速で安価です。

テンプレート関数は、TemplateResultそれを入力データの関数として返します。これはlit-htmlを使用する背後にある主な原則の1つです。つまり、UI を状態の関数として作成します。

あなたが呼び出すとrender、lit-htmlは最後のレンダリング以降に変更されたテンプレートの部分だけを更新します。これにより、lit-htmlの更新が非常に高速になります。

## Using expressions

<!-- original:
The previous example shows interpolating a simple text value, but the binding can include any kind of JavaScript expression:
-->

前の例では単純なテキスト値の補間を示していますが、バインディングには任意の種類のJavaScript式を含めることができます。

```js
const  myTemplate = (subtotal, tax) => html`<div>Total: ${subtotal + tax}</div>`;
const myTemplate2 = (name) => html`<div>${formatName(name.given, name.family, name.title)}</div>`;
```

## Bind to attributes 

<!-- original:
In addition to using expressions in the text content of a node, you can bind them to a node's attribute and property values, too.

By default, an expression in the value of an attribute creates an attribute binding:
-->

ノードのテキストコンテンツに式を使用することに加えて、それらをノードの属性とプロパティ値にバインドすることもできます。

デフォルトでは、属性値の式は属性バインディングを作成します。

```js
// set the class attribute
const myTemplate(data) = html`<div class=${data.cssClass}>Stylish text.</div>`;
```

<!-- original:
Since attribute values are always strings, the expression should return a value that can be converted into a string.

Use the `?` prefix for a boolean attribute binding. The attribute is added if the expression evaluates to a truthy value, removed if it evaluates to a falsy value:
-->

属性値は常に文字列なので、式は文字列に変換できる値を返す必要があります。

?ブール値属性バインディングの接頭辞を使用します。式が真理値に評価される場合は属性が追加され、真偽値に評価される場合は削除されます。

```js
const myTemplate2(data) = html`<div ?disabled="${!data.active}">Stylish text.</div>`;
```

## Bind to properties

<!-- original:
You can also bind to a node's JavaScript properties using the `.` prefix and the property name:
-->

.接頭辞とプロパティ名を使用して、ノードのJavaScriptプロパティにバインドすることもできます。

```js
const myTemplate3(data) = html`<my-list .listItems=${data.items}></my-list>`
```

<!-- original:
You can use property bindings to pass complex data down the tree to subcomponents.

Note that the property name in this example—`listItems`—is mixed case. Although HTML attributes are case-insensitive, lit-html preserves the case when it processes the template.
-->

プロパティー・バインディングを使用して、複雑なデータをツリーの下にサブコンポーネントに渡すことができます。

この例のプロパティ名はlistItems大文字と小文字が混在していることに注意してください。HTML属性は大文字と小文字を区別しませんが、lit-htmlはテンプレートを処理する際に大文字と小文字を保持します。

## Add event handlers

<!-- original:
Templates can also include declarative event handlers. An event handler looks like an attribute binding, but with the prefix `@` followed by an event name:
-->

テンプレートには、宣言型イベントハンドラも含めることができます。イベントハンドラは属性バインディングのように見えますが、接頭辞の@後にイベント名が続きます：

```js
const myTemplate = () => html`<button @click=${clickHandler}>Click Me!</button>`
```

<!-- original:
This is equivalent to calling `addEventListener('click', clickHandler)` on the button element.

The event handler can be either a plain function, or an object with a `handleEvent` method:
-->

これはaddEventListener('click', clickHandler)、ボタン要素を呼び出すのと同じです。

イベントハンドラは、プレーン関数か、handleEventメソッドを持つオブジェクトのいずれかになります。

```js
const clickHandler = {
  // handleEvent method is required.
  handleEvent(e) { 
    console.log('clicked!');
  }
  // event listener object can also define zero or more of the event 
  // listener options: capture, passive, and once.
  capture: true;
}
```

### Nest and compose templates

<!-- original:
You can also compose templates to create more complex templates. When a binding in the text content of a template returns a `TemplateResult`, the `TemplateResult` is interpolated in place.
-->

さらに複雑なテンプレートを作成するためにテンプレートを作成することもできます。テンプレートのテキストコンテンツのバインディングがaを返すとき、TemplateResultはそのTemplateResult場所で補間されます。

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

別のテンプレート関数のように、TemplateResultを返す式を使うことができます：

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

テンプレートを作成すると、条件付きテンプレートと繰り返しテンプレートを含む多くの可能性が開かれます。

### Conditional templates

<!-- original:
lit-html has no built-in control-flow constructs. Instead you use normal JavaScript expressions and statements.
-->

lit-htmlには組み込みの制御フロー構造はありません。代わりに、通常のJavaScript式とステートメントを使用します。

#### Conditionals with ternary operators

<!-- original:
Ternary expressions are a great way to add inline conditionals:
-->

三項式は、インライン条件を追加するのに最適です。

```js
html`
  ${user.isloggedIn
      ? html`Welcome ${user.name}`
      : html`Please log in`
  }
`;
```


#### Conditionals with if statements

<!-- original:
You can express conditional logic with if statements outside of a template to compute values to use inside of the template:
-->

if文を使用して条件付きロジックをテンプレート外に表現し、テンプレート内で使用する値を計算することができます。

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

## Repeating templates

<!-- original:
You can use standard JavaScript constructs to create repeating templates. 

lit-html also provides some special functions, called _directives_, for use in templates. You can use the  `repeat` directive to build certain kinds of dynamic lists more efficiently.
-->

標準のJavaScript構造を使用して繰り返しテンプレートを作成することができます。

lit-htmlには、テンプレートで使用するためのディレクティブと呼ばれる特別な関数もいくつか用意されています。このrepeatディレクティブを使用して、特定の種類の動的リストをより効率的に構築することができます 。

###  Repeating templates with Array.map

<!-- original:
To render lists, you can use `Array.map` to transform a list of data into a list of templates:
-->

リストをレンダリングするために、Array.mapデータのリストをテンプレートのリストに変換するのに使うことができます：

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

この式はTemplateResultオブジェクトの配列を返すことに注意してください。lit-htmlは、サブテンプレートやその他の値の配列またはiterableをレンダリングします。

### Repeating templates with looping statements

<!-- original:
You can also build an array of templates and pass it in to a template binding.
-->

また、テンプレートの配列を作成し、それをテンプレートバインディングに渡すこともできます。

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

### Repeating templates with the repeat directive

<!-- original:
In most cases, using loops or `Array.map` is an efficient way to build repeating templates. However, if you want to reorder a large list, or mutate it by adding and removing individual entries, this approach can involve recreating a large number of DOM nodes. 

The `repeat` directive can help here. Directives are special functions that provide extra control over rendering. lit-html comes with some built-in directives like `repeat`. 

The repeat directive performs efficient updates of lists based on user-supplied keys:
-->

ほとんどの場合、ループを使用するかArray.map、繰り返しテンプレートを作成する効率的な方法です。ただし、大きなリストを並べ替えるか、個々のエントリを追加したり削除したりして変更する場合は、多数のDOMノードを再作成する必要があります。

このrepeat指令はここで助けになることができます。ディレクティブは、レンダリングを特別に制御する特別な関数です。lit-htmlには、のような組込みディレクティブが付属していrepeatます。

repeatディレクティブは、ユーザが提供するキーに基づいてリストを効率的に更新します。

`repeat(items, keyFunction, itemTemplate)`

<!-- original:
Where:

*   `items` is an Array or iterable.
*   `keyFunction` is a function that takes a single item as an argument and returns a guaranteed unique key for that item.
*   `itemTemplate` is a template function that takes the item and its current index as arguments, and returns a TemplateResult.

For example:
-->

場所：

items 配列またはiterableです。
keyFunction 単一の項目を引数としてとり、その項目の保証された固有のキーを返す関数です。
itemTemplate アイテムとその現在のインデックスを引数とするテンプレート関数であり、TemplateResultを返します。

例えば：

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

employees配列を再ソートするとrepeat、既存のDOMノードの順序が変更されます。

これをlit-htmlのデフォルトのリスト処理と比較するには、大きな名前のリストを逆にすることを検討してください。

を使用して作成されたリストの場合Array.map、lit-htmlはリスト項目のDOMノードを維持しますが、値を再割り当てします。
を使用して作成されたリストのrepeat場合、repeatディレクティブは既存の DOMノードを並べ替えます。したがって、最初のリスト項目を表すノードが最後の位置に移動します。
どれがより効率的なのかは、ユースケースによって異なります。DOMノードを更新する方が移動させるよりもコストがかかる場合は、repeatディレクティブを使用してください。それ以外の場合は、Array.mapステートメントを使用するか、ループします。

## Caching template results: the cache directive 

<!-- original:
In most cases, JavaScript conditionals are all you need for conditional templates. However, if you're switching between large, complicated templates, you might want to save the cost of recreating DOM on each switch. 

In this case, you can use the `cache` _directive_. Directives are special functions that provide extra control over rendering. The cache directive caches DOM for templates that aren't being rendered currently. 
-->

ほとんどの場合、条件付きテンプレートにはJavaScript条件がすべて必要です。しかし、大規模で複雑なテンプレートを切り替える場合は、各スイッチにDOMを再作成するコストを節約したい場合があります。

この場合、cache ディレクティブを使用できます。ディレクティブは、レンダリングを特別に制御する特別な関数です。cacheディレクティブは、現在レンダリングされていないテンプレートのDOMをキャッシュします。

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

lit-htmlがテンプレートを再レンダリングすると、変更された部分のみが更新されます。つまり、必要以上にDOMを作成したり削除したりすることはありません。しかし、あるテンプレートから別のテンプレートに切り替えるとき、lit-htmlは古いDOMを削除して新しいDOMツリーをレンダリングする必要があります。

cacheディレクティブは、与えられた結合および入力テンプレート用に生成されたDOMをキャッシュします。上記の例ではsummaryView、detailViewテンプレートとテンプレートの両方にDOMがキャッシュされます 。あるビューから別のビューに切り替えると、lit-htmlは新しいビューのキャッシュされたバージョンを入れ替え、最新のデータで更新するだけです。
