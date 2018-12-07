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

lit-htmlテンプレートは、`html`タグでタグ付けされたJavaScriptテンプレートリテラルによって生成されます。リテラルの内容は、ほとんどがプレーンであり、ディレクティブか、HTMLです。


```js
html`<h1>Hello World</h1>`
```

<!--
**Bindings** or expressions are denoted with the standard JavaScript syntax for template literals:
-->

**バインディング**や式は、JavaScript標準のテンプレートリテラル構文を使います。


```js
html`<h1>Hello ${name}</h1>`
```

## テンプレート構造

<!-- original:
lit-html templates must be well-formed HTML, and bindings can only occur in certain places. The templates are parsed by the browser's built-in HTML parser before any values are interpolated. 

**No warnings.** Most cases of malformed templates are not detectable by lit-html, so you won't see any warnings—just templates that don't behave as you expect—so take extra care to structure templates properly. 

Follow these rules for well-formed templates:

 *  Templates must be well-formed HTML when all expressions are replaced by empty values.

 *  Bindings **_can only occur_** in attribute-value and text-content positions.
-->

lit-htmlテンプレートはきちんと整えられたHTMLでなければならず、ちゃんと指定された場所でしかバインディングされません。テンプレートは値が置き換えられる前にブラウザの組み込みHTMLパーサーによって解析されます。

**Warningは出ません** 不正な形式のテンプレートのほとんどは、lit-htmlで検出できないため、予期したとおりに動作しないテンプレートだけが警告を表示することはありません。したがって、正しくテンプレートを構造化するために特別な注意が必要です。

 * すべての式が空の値に置き換えられた場合、テンプレートは整形式のHTMLでなければなりません。 
 * バインディングは、属性およびテキストコンテンツで**のみ機能**します。

   ```html
   <!-- 属性値 -->
    <div label="${label}"></div>

   <!-- テキストコンテンツ -->
   <div>${textContent}</div>
   ```

<!-- original:
 *  Expressions **_cannot_** appear where tag or attribute names would appear.
-->

 * タグや属性名は置き換えられません。
    
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

 * テンプレートには複数のトップレベルの要素とテキストを含めることができます。 
 
 * テンプレートには閉じられていない要素が含まれてはいけません。それらはHTMLパーサーによって自動的に閉じられるでしょう。

    ```js
    // HTMLパーサーは"Some text"の後でdivタグを閉じます
    const template1 = html`<div class="broken-div">Some text`;
    // 結合後、"more text"は.broken-divクラスとして適用されません
    const template2 = html`${template1} more text. </div>`;
    ```

## バインドされる型

<!-- original:
Expressions can occur in text content or in attribute value positions.

There are a few types of bindings:

  * Text:
-->

JavaScript式はテキストコンテンツまたは属性の値で表示されます。

バインディングにはいくつかの種類があります。

  * テキスト:
  
    ```js
    html`<h1>Hello ${name}</h1>`
    ```

<!-- original:
    Text bindings can occur anywhere in the text content of an element.

  * Attribute:
-->

   テキストバインディングは、要素のテキストコンテンツのどこにでもつくれます。

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
    html`<button @click=${(e) => console.log('clicked')}>Click Me</button>`
    ```

### イベントリスナー

<!-- original:
Event listeners can be functions or objects with a `handleEvent` method. Listeners are passed as both the listener and options arguments to `addEventListener`/`removeEventListener`, so that the listener can carry event listener options like `capture`, `passive`, and `once`.
-->

イベントリスナーは、関数または`handleEvent`メソッドを持つオブジェクトにすることができます。リスナーを指定すると`addEventListener`/`removeEventListener`に引数とともに渡され、`capture`、`passive`、`once`などオプションをつけることができます。

```js
const listener = {
  handleEvent(e) {
    console.log('clicked');
  }
  capture: true;
};

html`<button @click=${listener}>Click Me</button>`
```

## サポートされているデータ型

<!-- original:
Each binding type supports different types of values:

 * Text content bindings: Many supported types—see below.

 * Attribute bindings: All values are converted to strings.

 * Boolean attribute bindings: All values evaluated for truthiness.

 * Property bindings: Any type of value.

 * Event handler bindings: Event handler functions or objects only.
-->

各バインドされる型はそれぞれ異なるタイプの値の型となります:

  * テキストコンテンツ: 後述の通り、多くの型が使えます。

  * 属性: すべての値は文字列に変換されます。

  * 真偽値の属性: すべての値が真偽値として評価されます。

  * プロパティ: それぞれに応じた値となります。

  * イベントハンドラ：イベントハンドラ関数またはオブジェクトのみ。

### テキストバインディングでサポートされている型

<!-- original:
Text content bindings accept a large range of value types:

*   Primitive values.
*   TemplateResult objects.
*   DOM nodes.
*   Arrays or iterables.
-->

テキストコンテンツにおけるバインディングは、広範囲の型を使えます。

*   プリミティブ値。
*   TemplateResultオブジェクト。
*   DOMノード。
*   配列またはiterables。

#### プリミティブ値：String、Number、Boolean、null、undefined

<!-- original:
Primitives values are converted to strings when interpolated into text content or attribute values. They are checked for equality to the previous value so that the DOM is not updated if the value hasn't changed.
-->

プリミティブの値は、テキストコンテンツまたは属性値に使われた時に文字列に変換されます。以前の値と等しいかどうかがチェックされ、値が変更されていない場合はDOMは更新されません。

#### TemplateResult

<!-- original:
Templates can be nested by passing a `TemplateResult` as a value of an expression:
-->

テンプレートは、式の値として `TemplateResult`を渡すことで入れ子にすることができます:

```js
const header = html`<h1>Header</h1>`;

const page = html`
  ${header}
  <p>This is some text</p>
`;
```

#### ノード

<!-- original:
Any DOM Node can be passed to a text position expression. The node is attached to the DOM tree at that point, and so removed from any current parent:
-->

どのDOMノードも指定された位置に値に渡すことができます。ノードはその時点でDOMツリーに関連付けられているため、現在の親から削除されます:

```js
const div = document.createElement('div');
const page = html`
  ${div}
  <p>This is some text</p>
`;
```

#### 配列 / イテラブル

<!-- original:
Arrays and Iterables of supported types are supported as well. They can be mixed values of different supported types.
-->

配列とイテラブル(Iterable)もうまくサポートされています。異なる型でも混在させることができます。

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
lit-html has no built-in control-flow constructs. Instead you use normal JavaScript expressions and statements:
-->

lit-htmlには組み込みの制御方法はありません。代わりに、通常のJavaScript式とJavaScript文を使います。

### 三項演算子による条件

<!-- original:
Ternary expressions are a great way to add inline-conditionals:
-->

三項演算子は、インラインで条件を追加するのに最適です:

```js
html`
  ${user.isloggedIn
      ? html`Welcome ${user.name}`
      : html`Please log in`
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

リストを描画するのに、`Array.map`を使ってデータのリストをテンプレートのリストに変換ができます：

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

ディレクティブは、バインディングのレンダリング方法をカスタマイズすることで、lit-htmlを拡張できる関数です。

lit-htmlにはいくつかの組み込みディレクティブが含まれています。

*   [`repeat`](#repeat)
*   [`ifDefined`](#ifdefined)
*   [`guard`](#guard)
*   [`until`](#until)
*   [`asyncAppend` と `asyncReplace`](#asyncappend-and-asyncreplace)

<!-- original:
**Directives may change.** The exact list of directives included with lit-html, and the API of the directives may be subject to change before lit-html 1.0 is released.
-->

**ディレクティブは変更されるかもしれません** lit-htmlに含まれるディレクティブとAPIは、lit-html 1.0がリリースされる前に変更される可能性があります。

### repeat 

`repeat(items, keyfn, template)`

<!-- original:
Repeats a series of values (usually `TemplateResults`) generated from an
iterable, and updates those items efficiently when the iterable changes. When
the `keyFn` is provided, key-to-DOM association is maintained between updates by
moving DOM when required, and is generally the most efficient way to use
`repeat` since it performs minimum unnecessary work for insertions amd removals.

Example:
-->

イテラブル(iterable)から生成された一連の値（通常は `TemplateResults`）を繰り返して表示し、変更されたときにそれらの項目を効率的に更新します。
`keyFn`があれば、必要に応じてDOMを移動させることによってキーとDOMの関連付けが維持されるので、DOMの挿入を最小限に抑えるためには` repeat`を使うのが最も効率的です。

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
-->

`keyFn`が指定されていない場合、`repeat`はアイテムと値の単純なMapと同様に動作し、DOMは潜在的に異なるアイテムに対して再利用されます。

### ifDefined

`ifDefined(value)`

<!-- original:
For AttributeParts, sets the attribute if the value is defined and removes the attribute if the value is undefined.

For other part types, this directive is a no-op.

Example:
-->

属性を設定する場合、値が定義されていれば属性を設定し、値が undefined の場合は属性を削除します。

他のテキストコンテンツなどの場合に、ディレクテブはなにもしません。

例:

```javascript
import { ifDefined } from 'lit-html/directives/if-defined';

const myTemplate = () => html`
  <div class=${ifDefined(className)}></div>
`;

```

### guard

`guard(expression, valueFn)`

<!-- original:
Avoids re-evaluating an expensive template function (`valueFn`) unless one of the identified expressions changes identity. Returns the value of `valueFn`, which may be cached.

The `expressions` argument can either be a single (non-array) expression, or an array of multiple expressions to monitor.

The `guard` directive caches the last-known value of `valueFn`, and only re-evaluates `valueFn` if the identity of any of the expressions changes (for example when a primitive changes value or when an object reference changes).

Example:
-->

識別された式のいずれかが一意性(identity)を変更しない限り、高価なテンプレート関数（`valueFn`）の再評価は避けてください。`valueFn`の戻り値ははキャッシュされる可能性があります。

`expressions`(式)では単一の値（配列とならない）であり、もしくは監視する対象を含めた配列とすることができます。

この`guard`ディレクティブは最後の既知の値をキャッシュし、プリミティブが値を変更したときやオブジェクト参照が変更されたときなど、式の一意性(identity)が変更された場合にのみ再描画されます。

```js
import { guard } from 'lit-html/directives/guard';

const template = html`
  <div>
    ${guard(items, () => items.map(item => html`${item}`))}
  </div>
`
```

<!-- original:
In this case, items are mapped over only when the array reference changes.
-->

この場合、配列の参照が変更された場合にのみ評価されます。

### until

`until(...values)`

<!-- original:
Renders one of a series of values, including Promises, to a Part.

Values are rendered in priority order, with the first argument having the
highest priority and the last argument having the lowest priority. If a
value is a Promise, low-priority values will be rendered until it resolves.

The priority of values can be used to create placeholder content for async
data. For example, a Promise with pending content can be the first,
highest-priority, argument, and a non_promise loading indicator template can
be used as the second, lower-priority, argument. The loading indicator will
render immediately, and the primary content will render when the Promise
resolves.

Example:
-->

プロミスを含む一連の値の1つをパートにレンダリングします。

値は優先度順に表示され、最初の引数は最高の優先度を持ち、最後の引数は最低の優先度を持ちます。値がPromiseの場合、優先度の低い値は解決されるまでレンダリングされます。

値の優先順位は、非同期データのプレースホルダコンテンツを作成するために使えます。たとえば、保留中のコンテンツを含むPromiseを最初の優先度の高い引数にすることができ、非プロンプトのローディングインジケータテンプレートを2番目の優先度の低い引数として使えます。ローディングインジケータがすぐにレンダリングされ、プロミスが解決するとプライマリコンテンツがレンダリングされます。

例:

```javascript
import { until } from 'lit-html/directives/until.js';

const content = fetch('./content.txt').then(r => r.text());

html`${until(content, html`<span>Loading...</span>`)}`
```

### asyncAppend と asyncReplace

`asyncAppend(asyncIterable)`<br>
`asyncReplace(asyncIterable)`

<!-- original:
JavaScript asynchronous iterators provide a generic interface for asynchronous sequential access to data. Much like an iterator, a consumer requests the next data item with a a call to `next()`, but with asynchronous iterators `next()` returns a `Promise`, allowing the iterator to provide the item when it's ready.

lit-html offers two directives to consume asynchronous iterators:

 * `asyncAppend` renders the values of an [async iterable](https://github.com/tc39/proposal-async-iteration),

appending each new value after the previous.

 * `asyncReplace` renders the values of an [async iterable](https://github.com/tc39/proposal-async-iteration),

replacing the previous value with the new value.

Example:
-->

JavaScriptの非同期イテレータは、データへの非同期順次アクセスのための汎用インタフェースを提供します。イテレータとよく似ていますが、コンシューマはtoを呼び出して次のデータ項目を要求しますが、非同期イテレータ`next()`を返し、イテレータが`Promise`で準備が整ったときにアイテムを提供できるようにします。

lit-htmlは、非同期イテレータを使用するための2つのディレクティブを提供します。

  * `asyncAppend` [非同期のイテラブル](https://github.com/tc39/proposal-async-iteration)の値をレンダリングし、
  
新しい値を前の値の後に追加します。

  * `asyncReplace` [非同期のイテラブル]((https://github.com/tc39/proposal-async-iteration)の値をレンダリングし、

前の値を新しい値に置き換えます。

例:

```javascript
const wait = (t) => new Promise((resolve) => setTimeout(resolve, t));
/**
 * カウントアップを待つ非同期イテラブルを返す
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

<!-- original:
In the near future, `ReadableStream`s will be async iterables, enabling streaming `fetch()` directly into a template:
-->

近い将来、ReadableStream非同期のiterableになりfetch()、テンプレートに直接ストリーミングできます。

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
