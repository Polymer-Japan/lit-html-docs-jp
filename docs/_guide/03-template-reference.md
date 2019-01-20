---
layout: post
title: テンプレート・リファレンス
slug: template-reference
---

{::options toc_levels="1..3" /}
* ToC
{:toc}

<!-- original:
lit-html templates are written using JavaScript template literals tagged with the `html` tag. The contents of the literal are mostly plain, declarative, HTML:
-->

lit-htmlテンプレートは、`html`タグでタグ付けされたJavaScript標準のテンプレートリテラルによって生成されます。リテラルの内容は、ほとんどがプレーンであり、ディレクティブか、HTMLです。


```js
html`<h1>ハロー、ワールド</h1>`
```

<!--
**Bindings** or expressions are denoted with the standard JavaScript syntax for template literals:
-->

**バインディング**やJavaScript評価式は、JavaScript標準のテンプレートリテラル構文を使います。


```js
html`<h1>ハロー、${name}</h1>`
```

## テンプレート構造

<!-- original:
lit-html templates must be well-formed HTML, and bindings can only occur in certain places. The templates are parsed by the browser's built-in HTML parser before any values are interpolated. 

**No warnings.** Most cases of malformed templates are not detectable by lit-html, so you won't see any warnings—just templates that don't behave as you expect—so take extra care to structure templates properly. 

Follow these rules for well-formed templates:

 *  Templates must be well-formed HTML when all expressions are replaced by empty values.

 *  Bindings **_can only occur_** in attribute-value and text-content positions.
-->

lit-htmlテンプレートはきちんと整えられたHTMLでなければならず、ちゃんと指定された場所でしかバインディングされません。テンプレートは値が置き換えられる前にブラウザ内部の組み込みHTMLパーサーによって解析されます。

**不正な形式に対して警告されません** lit-htmlではほとんどの不正な形式のテンプレートを検出できないので、期待した動作がされないテンプレートに警告が表示されることはありません。よって、正しいHTMLの構造を維持するには特別な注意が必要です。

 * すべてのJavaScript評価式が空だった場合でもテンプレートはきちんと整えられたHTMLとなるようにしてください。
 * バインディングは、HTMLの属性およびHTMLのテキストコンテンツで**のみ機能**します。

   ```html
   <!-- 属性値 -->
    <div label="${label}"></div>

   <!-- テキストコンテンツ -->
   <div>${textContent}</div>
   ```

<!-- original:
 *  Expressions **_cannot_** appear where tag or attribute names would appear.
-->

 * タグや属性名でバインディングは使用できません。
    
    ```html
    <!-- エラー --> 
    <${tagName}></${tagName}> 

    <!-- エラー --> 
    <div ${attrName}=true></div>
    ```

<!-- original:
 *  Templates can have multiple top-level elements and text.
 
 *  Templates **_should not contain_** unclosed elements—they will be closed by the HTML parser.
-->

 * テンプレートでは複数のトップレベル要素とテキストを含めることができます【全体をdivで囲う必要はありません】。 
 
 * テンプレートには閉じられていない要素を含まれてはいけません。それらはHTMLパーサーによって強制的に閉じられるでしょう。

    ```js
    // HTMLパーサーは"あるテキスト"の後でdivタグを閉じます
    const template1 = html`<div class="broken-div">あるテキスト`;
    // 結合後、"別のテキスト"は.broken-divクラスとして適用されません
    const template2 = html`${template1} 別のテキスト </div>`;
    ```

## バインドされる型

<!-- original:
Expressions can occur in text content or in attribute value positions.

There are a few types of bindings:

  * Text:
-->

JavaScript評価式はテキストコンテンツまたは属性の値で描画されます。

バインディングにはいくつかの種類があります。

  * テキスト:
  
    ```js
    html`<h1>こんにちは、${name}</h1>`
    ```

<!-- original:
    Text bindings can occur anywhere in the text content of an element.

  * Attribute:
-->

   テキストのバインディングは、要素のテキストコンテンツのどこにでもつくれます。

  * 属性:
  
    ```js
    html`<div id=${id}></div`
    ```

<!-- original:
  * Boolean Attribute:
-->

  * 真偽値属性
  
    ```js
    html`<input type="checkbox" ?checked=${checked}>`
    ```

<!-- original:
  * Property:
-->

  * JavaScriptプロパティ:
  
    ```js
    html`<input .value=${value}>`
    ```

<!-- original:
  * Event Listener:
-->

  * イベントリスナー:
  
    ```js
    html`<button @click=${(e) => console.log('クリックされました')}>クリック</button>`
    ```

### イベントリスナー

<!-- original:
Event listeners can be functions or objects with a `handleEvent` method. Listeners are passed as both the listener and options arguments to `addEventListener`/`removeEventListener`, so that the listener can carry event listener options like `capture`, `passive`, and `once`.
-->

イベントリスナーは、関数または`handleEvent`メソッドを持つオブジェクトにすることができます。リスナーを指定すると`addEventListener`/`removeEventListener`に引数とともに渡され、`capture`、`passive`、`once`などオプションをつけることができます。

```js
const listener = {
  handleEvent(e) {
    console.log('クリックされました');
  }
  capture: true;
};

html`<button @click=${listener}>クリック</button>`
```

## サポートされているデータ型

<!-- original:
Each binding type supports different types of values:

 * Text content bindings: Many types, see [Supported data types for text bindings](#supported-data-types-for-text-bindings).

 * Attribute bindings: All values are converted to strings.

 * Boolean attribute bindings: All values evaluated for truthiness.

 * Property bindings: Any type of value.

 * Event handler bindings: Event handler functions or objects only.
-->

バインドされる形式によって異なる型がサポートされます:

  * テキストコンテンツ: 後述の通り、多くの型が使えます。[テキストバインディングでサポートされているデータ型](#supported-data-types-for-text-bindings)を参照してください。

  * 属性: すべての値は文字列に変換されます。

  * 真偽値の属性: すべての値が真偽値として評価されます。

  * プロパティ: それぞれに応じた値となります。

  * イベントハンドラ: イベントハンドラ関数またはオブジェクトのみ。

### テキストバインディングでサポートされている型

<!-- original:
Text content bindings accept a large range of value types:

*   Primitive values.
*   TemplateResult objects.
*   DOM nodes.
*   Arrays or iterables.
-->

テキストコンテンツにおけるバインディングは、広範囲の型を使えます。

*   プリミティブ値
*   TemplateResultオブジェクト
*   DOMノード
*   配列もしくはイテラブル

#### プリミティブ値: 文字列、数値、真偽値、null、undefined

<!-- original:
Primitives values are converted to strings when interpolated into text content or attribute values. They are checked for equality to the previous value so that the DOM is not updated if the value hasn't changed.
-->

プリミティブの値は、テキストコンテンツまたは属性値に使われた時に文字列に変換されます。以前の値と等しいかどうかがチェックされ、値が変更されていない場合DOMは更新されません。

#### TemplateResultオブジェクト

<!-- original:
Templates can be nested by passing a `TemplateResult` as a value of an expression:
-->

テンプレートは、JavaScript評価式の値として `TemplateResult`を渡すことで入れ子にすることができます:

```js
const header = html`<h1>ヘッダー</h1>`;

const page = html`
  ${header}
  <p>これはサンプルテキストです</p>
`;
```

#### DOMノード

<!-- original:
Any DOM Node can be passed to a text position expression. The node is attached to the DOM tree at that point, and so removed from any current parent:
-->

どんなDOMノードも指定された位置に値に渡すことができます。ノードはその時点でDOMツリーに関連付けられているため、現在の親から削除されます:

```js
const div = document.createElement('div');
const page = html`
  ${div}
  <p>これはサンプルテキストです</p>
`;
```

#### 配列もしくはイテラブル

<!-- original:
Arrays and Iterables of supported types are supported as well. They can be mixed values of different supported types.
-->

配列とイテラブル(列挙可能なオブジェクト)もうまくサポートされています。異なる型でも混在させることができます。

```javascript
const items = [1, 2, 3];
const list = () => html`items = ${items.map((i) => `item: ${i}`)}`;
```

```javascript
const items = {
  a: 1,
  b: 23,
  c: 456,
};
const list = () => html`items = ${Object.entries(items)}`;
```

## JavaScriptによる制御フロー

<!-- original:
lit-html has no built-in control-flow constructs. Instead you use normal JavaScript expressions and statements.
-->

lit-htmlには組み込みの制御方法はありません。代わりに、通常のJavaScript評価式とJavaScript文を使います。

### 三項演算子による条件

<!-- original:
Ternary expressions are a great way to add inline-conditionals:
-->

三項演算子は、インラインで条件を追加するのに最適です:

```js
html`
  ${user.isloggedIn
      ? html`ようこそ、 ${user.name}`
      : html`ログインしてください`
  }
`;
```

### If文を使った条件

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

### Array.mapによるループ

<!-- original:
To render lists, `Array.map` can be used to transform a list of data into a list of templates:
-->

リストを描画するのに、`Array.map`を使ってデータをテンプレートのリストに変換できます:

```js
html`
  <ul>
    ${items.map((i) => html`<li>${i}</li>`)}
  </ul>
`;
```

### ループ文

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

## 組み込みディレクテブ

<!-- original:
Directives are functions that can extend lit-html by customizing the way a binding renders.

lit-html includes a few built-in directives.
-->

ディレクティブは、バインディングの描画方法をカスタマイズすることで、lit-htmlを拡張できる関数です。

lit-htmlにはいくつかの組み込みディレクティブが含まれています。

*   [`asyncAppend` と `asyncReplace`](#asyncappend-and-asyncreplace)
*   [`cache`](#cache)
*   [`classMap`](#classmap)
*   [`ifDefined`](#ifdefined)
*   [`guard`](#guard)
*   [`repeat`](#repeat)
*   [`styleMap`](#stylemap)
*   [`unsafeHTML`](#unsafehtml)
*   [`until`](#until)

<!-- original:
**Directives may change.** The exact list of directives included with lit-html, and the API of the directives may be subject to change before lit-html 1.0 is released.
-->

**ディレクティブは変更される可能性があります** lit-htmlに含まれるディレクティブとAPIは、v1.0がリリースされる前に変更される可能性があります。

### asyncAppend と asyncReplace

`asyncAppend(asyncIterable)`<br>
`asyncReplace(asyncIterable)`

<!-- original:
Location: text bindings

JavaScript asynchronous iterators provide a generic interface for asynchronous sequential access to data. Much like an iterator, a consumer requests the next data item with a a call to `next()`, but with asynchronous iterators `next()` returns a `Promise`, allowing the iterator to provide the item when it's ready.

lit-html offers two directives to consume asynchronous iterators:

 * `asyncAppend` renders the values of an [async iterable](https://github.com/tc39/proposal-async-iteration),

appending each new value after the previous.

 * `asyncReplace` renders the values of an [async iterable](https://github.com/tc39/proposal-async-iteration),

replacing the previous value with the new value.

Example:
-->

使用場所: テキストバインディング

JavaScriptの非同期イテレータは、データへの非同期順次アクセスのための汎用インタフェースを提供します。イテレータとよく似ていますが、コンシューマはtoを呼び出して次のデータ項目を要求しますが、非同期イテレータ`next()`を返し、イテレータが`Promise`で準備が整ったときにアイテムを提供できるようにします。

lit-htmlは、非同期イテレータを使用するための2つのディレクティブを提供します。

  * `asyncAppend` [非同期の列挙可能なオブジェクト(iterable)](https://github.com/tc39/proposal-async-iteration)の値を描画し、
  
新しい値を前の値の後に追加します。

  * `asyncReplace` [非同期の列挙可能オブジェクト(iterable)](https://github.com/tc39/proposal-async-iteration)の値を描画し、

前の値を新しい値に置き換えます。

例:

```javascript
const wait = (t) => new Promise((resolve) => setTimeout(resolve, t));
/**
 * Returns an async iterable that yields increasing integers.
 */
async function* countUp() {
  let i = 0;
  while (true) {
    yield i++;
    await wait(1000);
  }
}

render(html`
  Count: <span>${asyncReplace(countUp())}</span>.
`, document.body);
```

In the near future, `ReadableStream`s will be async iterables, enabling streaming `fetch()` directly into a template:

```javascript
// Endpoint that returns a billion digits of PI, streamed.
const url =
    'https://cors-anywhere.herokuapp.com/http://stuff.mit.edu/afs/sipb/contrib/pi/pi-billion.txt';

const streamingResponse = (async () => {
  const response = await fetch(url);
  return response.body.getReader();
})();
render(html`π is: ${asyncAppend(streamingResponse)}`, document.body);
```

### cache

`cache(conditionalTemplate)`

<!-- original:
Location: text bindings

Caches the rendered DOM nodes for templates when they're not in use. The `conditionalTemplate` argument is an expression that can return one of several templates. `cache` renders the current
value of `conditionalTemplate`. When the template changes, the directive caches the _current_ DOM nodes before switching to the new value. 

Example:
-->

使用場所: テキストバインディング

テンプレートが使用されていない時は、描画されたDOMノードをテンプレート用にキャッシュします。`conditionalTemplate`の引数はいくつかのテンプレートのうちの1つを返すことができるJavaScript評価式です。`cache`は`conditionalTemplate`の現在の値を描画します。 テンプレートが変更されると、ディレクティブは新しい値に切り替える前に現在のDOMノードをキャッシュします。

例:

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

lit-htmlはテンプレートを再レンダリングするとき、変更された部分のみを更新します。必要以上にDOMを作成または削除することはありません。しかし、あるテンプレートから別のテンプレートに切り替えると、lit-htmlは古いDOMを削除して新しいDOMツリーをレンダリングする必要があります。

`cache`ディレクティブは与えられたバインディングと入力テンプレートに対して生成されたDOMをキャッシュします。上記の例では、 `summaryView`と`detailView`テンプレートの両方に対してDOMをキャッシュします。あるビューから別のビューに切り替えるとき、lit-htmlはキャッシュされたバージョンの新しいビューを入れ替えて、それを最新のデータで更新するだけです。

### classMap

`class=${classMap(classObj)}`

<!-- original:
Location: attribute bindings (must be the entire value of the `class` attribute)

Sets a list of classes based on an object. Each key in the object is treated as a class name, if the value associated with the key is truthy, that class is added to the element. 

```js
let classes = { highlight: true, enabled: true, hidden: false };`

html`<div class=${classMap(classes)>Classy text</div>`;
// renders as <div class="highlight enabled">Classy text</div>
```

Note that you can only use `classMap` in an attribute binding for the `class` attribute, and it must be the entire value of the attribute.


```js
// DON'T DO THIS
html`<div class="someClass ${classMap(moreClasses}">Broken div</div>`;
```
-->

使用場所: 属性バインディング (`class`属性を全て置き換えるものでなければなりません)

オブジェクトに基づいてクラスのリストを設定します。オブジェクト内の各キーはクラス名として扱われます。キーに関連付けられた値が真であれば、そのクラスが要素に追加されます。

```js
let classes = { highlight: true, enabled: true, hidden: false };`

html`<div class=${classMap(classes)>装飾されたテキスト</div>`;
// <div class="highlight enabled">装飾されたテキスト</div>として描画されます
```

`class`属性に対する属性バインディングでのみ`classMap`を使うことができ、それは属性の値全体でなければならないことに注意してください。


```js
// このような使い方はできません
html`<div class="someClass ${classMap(moreClasses}">壊れたdiv要素</div>`;
```

### ifDefined

`ifDefined(value)`

<!-- original:
Location: attribute bindings

For AttributeParts, sets the attribute if the value is defined and removes the attribute if the value is undefined.

For other part types, this directive is a no-op.

Example:
-->

使用場所: 属性バインディング

属性を設定する場合、値が定義されていれば属性を設定し、値が undefined の場合は属性を削除します。

他のテキストコンテンツなど属性以外に使用した場合に、このディレクテブはなにもしません。

例:

```javascript
import { ifDefined } from 'lit-html/directives/if-defined';

const myTemplate = () => html`
  <img src="/images/${ifDefined(image.filename)}">
`;
```

### guard

`guard(dependencies, valueFn)`

<!-- original:
Location: any

Renders the value returned by `valueFn`. Only re-evaluates `valueFn` when one of the 
dependencies changes identity. 

Where:

-   `dependencies` is an array of values to monitor for changes. (For backwards compatibility, 
     `dependencies` can be a single, non-array value.)
-   `valueFn` is a function that returns a renderable value.

`guard` is useful with immutable data patterns, by preventing expensive work
until data updates.

Example:
-->

使用場所: どこでも

識別されたJavaScript評価式のいずれかが一意性(identity)を変更しない限り、高価なテンプレート関数（`valueFn`）の再評価は避けてください。`valueFn`の戻り値ははキャッシュされる可能性があります。

`expressions`(JavaScript評価式)では単一の値（配列とならない）であり、もしくは監視する対象を含めた配列とすることができます。

この`guard`ディレクティブは最後の既知の値をキャッシュし、プリミティブが値を変更したときやオブジェクト参照が変更されたときなど、JavaScript評価式の一意性(identity)が変更された場合にのみ再描画されます。

```js
import { guard } from 'lit-html/directives/guard';

const template = html`
  <div>
    ${guard([immutableItems], () => immutableItems.map(item => html`${item}`))}
  </div>
`;
```

<!-- original:
In this case, the `immutableItems` array is mapped over only when the array reference changes.
-->

この場合、`immutableItems`の配列の参照が変更された場合にのみ評価されます。

### repeat 

`repeat(items, keyfn, template)`<br>
`repeat(items, template)`

<!-- original:
Renders one of a series of values, including Promises, to a Part.

Location: text bindings

Repeats a series of values (usually `TemplateResults`) generated from an
iterable, and updates those items efficiently when the iterable changes. When
the `keyFn` is provided, key-to-DOM association is maintained between updates by
moving DOM when required, and is generally the most efficient way to use
`repeat` since it performs minimum unnecessary work for insertions amd removals.

Example:
-->

プロミスを含む一連の値の1つをパーツ(部分)に描画します。

使用場所: テキストバインディング

イテラブル(iterable)から生成された一連の値（通常は `TemplateResults`オブジェクト）を繰り返して表示し、変更されたときにそれらの項目を効率的に更新します。
`keyFn`が定義されていれば、必要に応じてDOMを移動させてキーとDOMの関連付けを維持するので、DOMの挿入を最小限に抑えるためには`repeat`を使うのが最も効率的です。

例:

```js
import { repeat } from 'lit-html/directives/repeat';

const myTemplate = () => html`
  <ul>
    ${repeat(items, (i) => i.id, (i, index) => html`
      <li>${index}: ${i.name}</li>`)}
  </ul>
`;
```

<!-- original:
If no `keyFn` is provided, `repeat` will perform similar to a simple map of
items to values, and DOM will be reused against potentially different items.

See [Repeating templates with the repeat directive](writing-templates#repeating-templates-with-the-repeat-directive) for a discussion
of when to use `repeat` and when to use standard JavaScript flow control. 
-->

`keyFn`が指定されていない場合、`repeat`はアイテムと値の単純なMapと同様に動作し、DOMは潜在的に異なるアイテムに対して再利用されます。

いつ `repeat`を使うべきか、そしていつ標準のJavaScriptフロー制御を使うべきかについての議論は [repeatディレクティブを使ったテンプレートの繰り返し](writing-templates#repeating-templates-with-the-repeat-directive)を見てください。

### styleMap

`style=${styleMap(styles)}`

<!-- original:
Location: attribute bindings (must be the entire value of the `style` attribute)

The `styleMap` directive sets styles on an element based on an object, where each key in the object is treated as a style property, and the value is treated as the value of for that property. For example:

```js
let styles = { backgroundColor: 'blue', color: 'white'}'
html`<p style=${styleMap(styles}>Hello style!</p>`;
```

For CSS properties that contain dashes, you can either use the camel-case equivalent, or put the property name in quotes. For example, you can write the the CSS property `font-family` as either `fontFamily` or `'font-family'`:

```js
{ fontFamily: 'roboto' }
{ 'font-family': 'roboto }
```

The `styleMap` directive can only be used as a value for the `style` attribute, and it must be the entire value of the attribute.
-->

使用場所: 属性バインディング (`style`属性を全て置き換えるものでなければなりません)

`styleMap`ディレクティブはオブジェクトに基づいて要素にスタイルを設定します。オブジェクトの各キーはスタイルプロパティとして扱われ、値はそのプロパティの値として扱われます。例えば:

```js
let styles = { backgroundColor: 'blue', color: 'white'}'
html`<p style=${styleMap(styles}>ヘロー、スタイル!</p>`;
```

ダッシュを含むCSSプロパティの場合は、キャメルケースに相当するものを使用するか、プロパティ名を引用符で囲むことができます。 たとえば、CSSプロパティの `font-family`を` fontFamily`または `'font-family'`のいずれかとして書くことができます:

```js
{ fontFamily: 'roboto' }
{ 'font-family': 'roboto }
```

`styleMap`ディレクティブは`style`属性の値としてのみ使うことができ、その属性の値全体でなければなりません。

### unsafeHTML

`unsafeHTML(html)`

<!-- original:
Location: text bindings

Renders the argument as HTML, rather than text.

Note, this is unsafe to use with any user-provided input that hasn't been
sanitized or escaped, as it may lead to cross-site-scripting vulnerabilities.

Example:
-->

使用場所: テキストバインディング

引数をテキストではなくHTMLとしてレンダリングします。

これは、クロスサイトスクリプティングの脆弱性を招く可能性があるため、サニタイズまたはエスケープされていないユーザーや外部から入力と一緒に使用するのは危険です。

例:

```js
const markup = '<div>生のHTMLとして出力</div>';
const template = html`
  危険な可能性があるHTMLを出力:
  ${unsafeHTML(markup)}
`;
```

### until

`until(...values)`

<!-- original:
Location: any

Renders placeholder content until the final content is available. 

Takes a series of values, including Promises. Values are rendered in priority order, 
 with the first argument having the highest priority and the last argument having the 
 lowest priority. If a value is a Promise, a lower-priority value will be rendered until it resolves.

The priority of values can be used to create placeholder content for async
data. For example, a Promise with pending content can be the first,
highest-priority, argument, and a non-promise loading indicator template can
be used as the second, lower-priority, argument. The loading indicator 
renders immediately, and the primary content will render when the Promise
resolves.

Example:
-->

使用場所: どこでも

最終的なコンテンツが利用可能になるまで、プレースホルダのコンテンツを描画します。

Promiseを含む一連の値をとります。値は優先度順に表示され、最初の引数は最高の優先度を持ち、最後の引数は最低の優先度を持ちます。値がPromiseの場合、優先度の低い値は解決されるまで描画されます。

値の優先順位は、非同期データのプレースホルダコンテンツを作成するために使えます。たとえば、保留中のコンテンツを含むPromiseを最初の優先度の高い引数にすることができ、非プロンプトのローディングインジケータテンプレートを2番目の優先度の低い引数として使えます。ローディングインジケータがすぐにレンダリングされ、Promiseが解決するとプライマリコンテンツが描画されます。

例:

```javascript
import { until } from 'lit-html/directives/until.js';

const content = fetch('./content.txt').then(r => r.text());

html`${until(content, html`<span>読み込み中...</span>`)}`
```
