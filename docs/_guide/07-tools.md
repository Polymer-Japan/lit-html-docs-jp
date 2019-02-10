---
layout: post
title: ツール
slug: tools
---

{::options toc_levels="1..3" /}
* ToC
{:toc}


<!-- original:
lit-html is available from the npm registry. If you're already using npm to manage dependencies, you can use lit-html much like any other JavaScript library you install from npm. This section describes some additional tools or plugins you might want to add to your workflow to make it easier to work with lit-html.

lit-html is delivered as a set of JavaScript modules. If you aren't already using JavaScript modules in your project, you may need to add a couple of steps to your development and build workflow.
-->

lit-htmlは、npmで入手できます。プロジェクトですでにnpmを使用している場合は、他のJavaScriptライブラリと同じようにlit-htmlを使用できます。この文書では、既存のワークフローでlit-htmlを使うにあたって、その作業を簡単にする追加のツールやプラグインについて説明します。

lit-htmlはJavaScriptモジュールとして提供されています。プロジェクトでまだJavaScriptモジュールを使用していない場合は、開発と構築のワークフローに2、3のステップを追加する必要があります。

<!-- original:
## Setup

The simplest way to add lit-html to a project is to install it from the npm registry. 

1. If you're starting a brand-new project, run the following command in your project area to set up npm:

    ```bash
    npm init
    ```

    Respond to the prompts to set up your project. You can hit return to accept the default values.

2. Install lit-html.

    ```bash
    npm i lit-html
    ```


3. If you're working on a project with many front-end dependencies, you may want to use the npm  `dedupe` command to try and reduce duplicated modules:

    `npm dedupe` 
-->

## セットアップ

プロジェクトにlit-htmlを追加する最も簡単な方法は、npmからインストールすることです。

1. 0から新しいプロジェクトを始めるなら、プロジェクトの作業ディレクトリで次のコマンドを実行してnpmを設定します:

    ```bash
    npm init
    ```

    聞かれるプロンプトに応じて、プロジェクトを設定してください。デフォルトでかまなわないならリターンキーを押していけます。

2. lit-htmlをインストールしてください。

    ```bash
    npm i lit-html
    ```


3. 既存のプロジェクトでフロントエンドの依存関係が多い場合は、重複したモジュールを減らすnpm `dedupe`コマンドを使ってもよいかもしれません:

    `npm dedupe` 

<!-- original:
## Development

During the development phase, you might want the following tools:

* IDE plugins, for linting and code highlighting.
* Linter plugins, for checking lit-html templates.
* A dev server, for previewing code without a build step.

### IDE plugins

There are a number of IDE plugins that may be useful when developing with lit-html. In particular, we recommend using a code highlighter that works with lit-html style templates. In addition, we recommend using a linter like ESLint that supports modern JavaScript.

VSCode plugin

* https://marketplace.visualstudio.com/items?itemName=bierner.lit-html

TypeScript plugin (works with Sublime and Atom)

* https://github.com/Microsoft/typescript-lit-html-plugin

More plugins

The [awesome-lit-html](https://github.com/web-padawan/awesome-lit-html#ide-plugins) repo lists other IDE plugins.


### Linter plugins

ESLint is recommended for linting lit-html code. The following ESLint plugin can be added to check for some common issues in lit-html templates:

* [https://github.com/43081j/eslint-plugin-lit](https://github.com/43081j/eslint-plugin-lit)

### Dev server


lit-html is packaged as JavaScript modules. Many developers prefer to import modules using node-style module identifiers, which aren't supported yet by browsers. To run in the browser, these module identifiers need to be transformed to browser-ready module identifiers. The Polymer dev server, which is part of the Polymer CLI, performs this transformation on the fly, so you can preview your project during development without a build step. Another alternative is the Open Web Components Dev Server (`owc-dev-server`).

If you're using webpack for your build process, you can also use the webpack dev server.

#### Polymer Dev Server

Install the Polymer CLI:

```bash
npm i -g polymer-cli
```

Run the dev server:

```bash
polymer serve
```

The Polymer CLI was designed to help develop, test, and build projects using web components, JavaScript modules and other features of the modern web platform. It's not required for lit-html development, but it provides several handy utilities. 

#### Open Web Components Dev Server

The Open Web Components project produces a dev server that handles remapping node-style modules to browser-style modules.

For full installation and usage instructions, see the [open-wc website](https://open-wc.org/developing/owc-dev-server.html). 
-->

## 開発

開発段階では、以下のようなツールが使いたくなるかもしれません:

* IDEプラグイン、リンティングおよびコードの強調表示用に
* リンタープラグイン、lit-htmlテンプレートのチェック用に
* 開発用サーバ、都度ビルドせずにコード修正の結果をプレビューするために

### IDEプラグイン

lit-htmlを使って開発するときに役立つかもしれないIDEプラグインがいくつかあります。特に、lit-htmlスタイルのテンプレートに対応したコードハイライト機能を使用することをお勧めします。加えて,最新のJavaScriptをサポートするESLintのようなリンターを使用することをお勧めします。

VSCodeプラグイン

* [https://marketplace.visualstudio.com/items?itemName=bierner.lit-html](https://marketplace.visualstudio.com/items?itemName=bierner.lit-html)

TypeScriptプラグイン(SublimeとAtomで動作します)

* [https://github.com/Microsoft/typescript-lit-html-plugin](https://github.com/Microsoft/typescript-lit-html-plugin)

他のプラグイン

[awesome-lit-html](https://github.com/web-padawan/awesome-lit-html#ide-plugins)リポジトリには他のIDEプラグインがリストされています。

### リンタープラグイン

ESLintはlit-htmlコードのリンティングに推奨されています。以下のESLintプラグインを追加して、lit-htmlテンプレートのよくある間違いをチェックすることができます:

* [https://github.com/43081j/eslint-plugin-lit](https://github.com/43081j/eslint-plugin-lit)

### 開発サーバ

lit-htmlはJavaScriptモジュールとしてパッケージされています。多くの開発者はnodeスタイルのモジュール形式を使ってインポートすることに慣れています。これはブラウザではまだサポートされていません。ブラウザで実行するには、これらのモジュール形式をブラウザ対応のモジュール形式に変換する必要があります。Polymerの開発サーバー(Polymer CLIの一部)はその変換を実行するので、開発中にビルドせずにプロジェクトをプレビューできます。もう1つの代替手段はOpen Web Components Dev Serverです。(`owc-dev-server`).

ビルドプロセスにwebpackを使っているのなら、webpack devサーバーを使うこともできます。

#### Polymer Dev Server

Polymer CLIのインストール:

```bash
npm i -g polymer-cli
```

開発サーバーの実行:

```bash
polymer serve
```

Polymer CLIは、Webコンポーネント、JavaScriptモジュール、およびその他の最新のWebプラットフォームの機能を使用してプロジェクトの開発、テスト、および構築を支援するように設計されています。lit-htmlを使うのに必ずしも必要はありませんが、いくつかの便利なユーティリティを提供しています。

#### Open Web Components開発サーバ

Open Web Componentsプロジェクトは、nodeスタイルのモジュールをブラウザスタイルのモジュールに再マッピングする処理を行う開発サーバーを提供しています。

インストールと使用方法については、[open-wc website](https://open-wc.org/developing/owc-dev-server.html)を参照してください。 

<!-- original:
## Testing

lit-html doesn't have many special testing requirements. If you already have a testing setup, it should work fine as long as it supports working with JavaScript modules (and node-style module specifiers, if you use them).

Web Component Tester (WCT) is an end-to-end testing environment that supports node-style module specifiers. works with the Mocha testing framework and (optionally) the Chai assertion library. There are two ways to add WCT to your project:

* [web-component-tester](https://www.npmjs.com/package/web-component-tester).  Installing the full WCT package gives you Mocha and Chai, as well as some other add-ons.
* [wct-mocha](https://www.npmjs.com/package/wct-mocha). Just the WCT client-side library. You'll need to install your own version of Mocha, and any other add-ons you want.

Alternately, you can also use the Karma test runner. The Open Web Components recommendations includes a [Karma setup](https://open-wc.org/testing/testing-karma.html#browser-testing) that resolves module dependencies by bundling with webpack before running tests. 
-->

## テスト

lit-htmlはそれほどテストをするのに特別な要件はありません。テスト用の設定がすでにあって、JavaScriptモジュールがサポートされていれば問題なく動作するはずです。 (もしnodeスタイルのモジュール形式を使っていれば).

Webコンポーネントテスター(WCT) は、nodeスタイルのモジュール形式をサポートするエンドツーエンドのテスト環境です。 Mochaテストフレームワークと(オプションで)Chaiアサーションライブラリと連携します。プロジェクトにWCTを追加する方法は2つあります。:

* [web-component-tester](https://www.npmjs.com/package/web-component-tester) 完全なWCTパッケージをインストールするとMochaとChaiを含むその他いくつかのアドオンが提供されます。
* [wct-mocha](https://www.npmjs.com/package/wct-mocha) WCTのクライアントサイドライブラリです。Mochaとその他アドオンを自分でインストールする必要があります。

その他に、Karmaテストランナーも使えます。Open Web Componentsの推奨事項には、テストを実行する前にwebpackにバンドルすることによってモジュールの依存関係を解決する[Karmaのセットアップ](https://open-wc.org/testing/testing-karma.html#browser-testing)が含まれています。

<!-- original:
## Build

Build tools take your code and make it production-ready. Among the things you may need build tools to do:

* Transform ES6 code to ES5 for legacy browsers, including transforming JavaScript modules into other formats.
* Bundle modules together can improve performance by reducing the number of files that need to be transferred. 
* Minify JavaScript, HTML, and CSS.

Many build tools can do this for you. Currently we recommend the Polymer CLI or webpack. 

The Polymer CLI includes a set of build tools that can handle lit-html with minimal configuration.

webpack is a powerful build tool with a large ecosystem of plugins. The [Open Web Components](https://open-wc.org/building/#webpack) project provides a default configuration for webpack that works well for lit-html and LitElement.

Other tools such as Rollup can work, too. If you're using another tool or creating your own webpack configuration, see the section on [Build considerations for other tools](#build-consderations).

### Build your project with Polymer CLI

Originally developed to work with the Polymer library, the Polymer CLI can handle build duties for a variety of projects. It's not as flexible and extensible as webpack or Rollup, but it requires minimal configuration.

To build your project with the Polymer CLI, first install the Polymer CLI:

`npm i -g polymer-cli`

Create a `polymer.json` file in your project folder. A simple example would look like this:

```json
{
  "entrypoint": "index.html",
  "shell": "src/myapp.js",
  "sources": [
    "src/**.js",
    "manifest/**",
    "package.json"
  ],
  "extraDependencies": [
    "node_modules/@webcomponents/webcomponentsjs/bundles/**"
  ],
  "builds": [
    {"preset": "es6-bundled"}
  ]
}
```

This configuration specifies that the app has an HTML entrypoint called `index.html`, has a main JavaScript file (app shell) called `src/myapp.js`. It will produce a single build, bundled but not transpiled to ES5. For details on the polymer.json file, see [polymer.json specification](https://polymer-library.polymer-project.org/3.0/docs/tools/polymer-json) on the Polymer library site.

To build the project, run the following command in your project folder:

`polymer build`

For more on building with Polymer CLI, see [Build for production](https://polymer-library.polymer-project.org/3.0/docs/apps/build-for-production) in the Polymer library docs.

### Build your project with webpack


See the Open Web Components default webpack configuration provides a great starting point for building projects that use lit-html. See their [webpack page](https://open-wc.org/building/building-webpack.html#default-configuration) for instructions on getting started. 

### Build considerations for other tools


If you're creating your own configuration for webpack, Rollup, or another tool, here are some factors to consider:

* ES6 to ES5 transpilation.
* Transforming JavaScript modules to other formats for legacy browsers.
* lit-html template minification.

#### Transpilation and module transform

You build tools need to transpile ES6 features to ES5 for legacy browsers. 

If you're working in TypeScript, the TypeScript compiler can generate different output for different browsers.

* In general, ES6 is faster than the ES5 equivalent, so try to serve ES6 to browsers that support it.
* TypeScript has slightly buggy template literal support when compiling to ES5, which can hurt performance.

Your build tools need to accept JavaScript modules (also called ES modules) and transform them to another module format, such as UMD, if necessary. If you use node-style module specifiers, your build will also need to transform them to browser-ready modules specifiers. 

#### Template minification

As part of the build process, you'll probably want to minify the HTML templates. Most HTML minifiers don't support HTML inside template literals, as used by lit-html, so you'll need to use a build plugin that supports minifying lit-html templates. Minifying lit-html templates can improve performance by reducing the number of nodes in a template.

* [Babel plugin](https://github.com/cfware/babel-plugin-template-html-minifier). For build chains that use Babel for transpilation. The open-wc webpack default configuration uses this plugin.
* [Rollup plugin](https://github.com/asyncLiz/rollup-plugin-minify-html-literals). If you're building your own Rollup configuration.
-->

## ビルド

ビルドツールはコードをリリース可能な製品に仕上げます。その過程でビルドツールに求められるものは:

* JavaScriptモジュールを他の形式に変換するなど、従来のブラウザ用にES6コードをES5に変換します。
* モジュールをバンドルさせて転送する必要があるファイルの数を減らすことでサイトのパフォーマンスを向上させます。
* JavaScript、HTML、およびCSSをミニファイします。

多くのビルドツールでこれを実現できます。現時点ではPolymer CLIまたはWebpackをお勧めします。

Polymer CLIには、最小限の設定でlit-htmlを処理できる一連のビルドツールが含まれています。

webpackは様々なプラグインが使える大規模なエコシステムを備えた強力な構築ツールです。[Open Web Components](https://open-wc.org/building/#webpack)プロジェクトでは、lit-htmlとLitElementに適したwebpackのデフォルト設定を提供します。

Rollupなどの他のツールも機能します。その他、独自のWebpack設定を作成している場合は、[他のツールによるビルドに関して](#build-consderations)のセクションを参照してください。

### Polymer CLIを使ってプロジェクトをビルドする

もともとPolymerライブラリと連動するように開発されたPolymer CLIですが、さまざまなプロジェクトのためのビルド作業を処理できます。WebpackやRollupほど柔軟で拡張性はありませんが、最小限の設定で済みます。

Polymer CLIを使用してプロジェクトをビルドするには、最初にPolymer CLIをインストールしてください:

`npm i -g polymer-cli`

プロジェクトのディレクトリに`polymer.json`ファイルを作成してください。簡単な例はこのようになります:

```json
{
  "entrypoint": "index.html",
  "shell": "src/myapp.js",
  "sources": [
    "src/**.js",
    "manifest/**",
    "package.json"
  ],
  "extraDependencies": [
    "node_modules/@webcomponents/webcomponentsjs/bundles/**"
    ],
  "builds": [
    {"preset": "es6-bundled"}
  ]
}
```

この設定は、アプリが `index.html`というHTMLエントリポイントを持ち、`​​src/myapp.js`というメインのJavaScriptファイル（アプリシェル）を持つことを指定しています。バンドルが実行されますが、ES5には変換されません。polymer.jsonの詳細については、Polymerライブラリサイトの[polymer.jsonの仕様](https://polymer-library.polymer-project.org/3.0/docs/tools/polymer-json)を参照してください。

プロジェクトをビルドするには、プロジェクトのディレクトリで次のコマンドを実行します:

`polymer build`

Polymer CLIを使用したビルドの詳細については、Polymerライブラリの[プロダクションへの構築](https://polymer-library.polymer-project.org/3.0/docs/apps/build-for-production)を参照してください。

### webpackを使ってプロジェクトをビルドする


Open Web Componentsを参照してください。デフォルトのWebpack構成はlit-htmlを使用するプロジェクトを構築するための優れたスタートポイントとなっています。[webpackについて](https://open-wc.org/building/building-webpack.html#default-configuration)書かれたインストラクションを参照してください。

### 他のツールによるビルドに関して {#build-considerations}


webpack、Rollup、または他のツール用に独自の設定を作成する場合、いくつか考慮すべきポイントがあります:

* ES6からES5への変換。
* JavaScriptモジュールを従来のブラウザ用の他の形式に変換する。
* lit-htmlテンプレートのミニファイ

#### ES5変換とモジュール変換

ビルドツールは、従来のブラウザ用にES6の機能をES5に変換する必要があります。

TypeScriptで作業している場合、TypeScriptコンパイラはブラウザごとに異なる出力を生成できます。

* 一般に、ES6は同等のES5より速いので、ES6をサポートするブラウザにはES6で提供することを試みましょう。
* TypeScriptは、ES5へのコンパイル時にわずかにバグのあるテンプレートリテラルを生成してしまうためパフォーマンスが低下する可能性があります。

ビルドツールはJavaScriptモジュール（ESモジュールとも呼ばれる）を受け入れ、必要に応じてそれらをUMDなどの別のモジュール形式に変換する必要があります。nodeスタイルのモジュール形式を使用する場合、ビルド時にそれらをブラウザ対応のモジュール形式に変換する必要もあります。

#### テンプレートのミニファイ

ビルドプロセスの一環で、HTMLテンプレートをミニファイしたいと思うでしょう。ほとんどのHTMLミニファイツールは、lit-htmlで使用されているようにテンプレートリテラル内のHTMLをサポートしていません。そのため、lit-htmlテンプレートの縮小をサポートするビルドプラグインを使用する必要があります。lit-htmlテンプレートをミニファイすると、テンプレート内のノード数が減るため、パフォーマンスが向上します。

* [Babelプラグイン](https://github.com/cfware/babel-plugin-template-html-minifier) ビルドプロセスの変換にBabelを使用します。open-wcのwebpackのデフォルト設定はこのプラグインを使っています。
  * [Rollupプラグイン](https://github.com/asyncLiz/rollup-plugin-minify-html-literals) 独自のRollupの設定を使用して構築する場合に使用します。
