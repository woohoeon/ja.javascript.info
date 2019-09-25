# Eval: コード文字列を実行する

組み込みの `eval` 関数を使うとコード文字列を実行することができます。

構文:

```js
let result = eval(code);
```

例:

```js run
let code = 'alert("Hello")';
eval(code); // Hello
```

コードの文字列は長かったり、改行や関数定義、変数を含んでいる可能性があります。

`eval` の結果は最後の文の結果です。

例:
```js run
let value = eval('1+1');
alert(value); // 2
```

```js run
let value = eval('let i = 0; ++i');
alert(value); // 1
```

eval されたコードは現在のレキシカル環境で実行されるため、外部変数を参照することができます。:

```js run no-beautify
let a = 1;

function f() {
  let a = 2;

*!*
  eval('alert(a)'); // 2
*/!*
}

f();
```

同様に外部変数を変更することもできます:

```js untrusted refresh run
let x = 5;
eval("x = 10");
alert(x); // 10, 値の変更
```

strict モードでは、`eval` は独自のレキシカル環境を持ちます。そのため、eval 内で宣言された関数や変数は外側では見えません。:

```js untrusted refresh run
// 実行可能な例では、'use strict' はデフォルトで有効になっています

eval("let x = 5; function f() {}");

alert(typeof x); // undefined (そのような変数はありません)
// function f も見えません
```

`use strict` がなければ、`eval` は独自のレキシカル環境を持たないので、外側から `x` や `f`を見ることができます。

## "eval" を利用する

モダンプログラミングでは、`eval` は非常に控えめに使用されます。しばしば "eval は悪" と言われます。

理由は簡単です: ずっと昔、JavaScript は今よりずっと弱い言語であり、多くのことは `eval` でしかできませんでした。ですが、それから 10年が経過しました。

現在は、`eval` を利用する理由はほとんどありません。もし誰かがそれを使用しているなら、モダンな言語構造、あるいは [JavaScript Module](info:modules)に置き換えるよい機会です。

外部変数にアクセスする機能には副作用があることに注意してください。

コードの minifier (JSを本番環境に適用する前に使われるツールで、JSを圧縮(minify)します)は最適化のために、ローカル変数をより短いものに置き換えます。これは通常安全ですが、`eval` が使われている場合、それらを参照する可能性があるため安全ではありません。したがって、minifier は `eval` から見える可能性のあるすべてのローカル変数を置き換えません。これはコードの圧縮率に悪影響を及ぼします。

`eval` の内部で外部のローカル変数を使用することは、コードのメンテナンスをより難しくするためバッドプラクティスとされています。

このような問題に対し、完全に安全にする方法は2つです。

**eval されたコードが外部変数を使用していない場合、`eval` を `window.eval(...)` で呼び出してください**

この方法では、コードはグローバルスコープで実行されます。:

```js untrusted refresh run
let x = 1;
{
  let x = 5;
  window.eval('alert(x)'); // 1 (グローバル変数)
}
```

**eval されたコードがローカル変数を必要とする場合、`eval` を `new Function` に変更し、引数としてそれらを渡してください**

```js run
let f = new Function('a', 'alert(a)');

f(5); // 5
```

`new Function` についてはチャプター <info:new-function> で説明しています。これは文字列から関数を作成し、グローバルスコープになります。そのため、ローカル変数は見えません。ですが、上記の例のように、引数tおして明示的に渡すほうがはるかに明白です。

## サマリ

`eval(code)` の呼び出しはコード文字列を実行し、最後の文の結果を返します。
- 通常は必要ないため、モダン JavaScript ではめったに使われません。
- 外部のローカル変数にアクセスできます。これはバッドプラクティスとされています。
- 代わりに、グローバルスコープでコードを `eval` するには `window.eval(code)` を使います。
- あるいは、外部スコープから何らかのデータが必要な場合は `new Function` を使い引数としてそれを渡します。