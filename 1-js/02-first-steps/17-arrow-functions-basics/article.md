# アロー関数の基本

関数を作成するための、よりシンプルで簡潔な構文がもう1つあります。それはしばしば関数式よりも優れています。

これは "アロー関数" と呼ばれ、次のようになります:

```js
let func = (arg1, arg2, ...argN) => expression
```

...これは引数 `arg1..argN` を取り、それらを使用する `expression` を評価し、その結果を返す関数 `func` を作ります。

言い換えると、次のコードと概ね一緒です:

```js
let func = function(arg1, arg2, ...argN) {
  return expression;
}
```

具体的な例を見てみましょう:

```js run
let sum = (a, b) => a + b;

/* アロー関数は次よりも短い形式です:

let sum = function(a, b) {
  return a + b;
};
*/

alert( sum(1, 2) ); // 3
```

ご覧の通り、`(a, b) => a + b` は `a` と `b` 、2つの引数を受け取る関数を意味します。実行時に、`a + b` を評価し、結果を返します。

- 引数が1つだけの場合、括弧は省略可能なので、さらに短くできます:

    例:

    ```js run
    *!*
    let double = n => n * 2;
    // おおよそこちらと同じ: let double = function(n) { return n * 2 }
    */!*

    alert( double(3) ); // 6
    ```

- 引数がない場合、空の括弧が必須です:

    ```js run
    let sayHi = () => alert("Hello!");

    sayHi();
    ```

アロー関数は、関数式として同じ方法で使用できます。

例えば、ここでは `welcome()` の例を再び書きます:

```js run
let age = prompt("What is your age?", 18);

let welcome = (age < 18) ?
  () => alert('Hello') :
  () => alert("Greetings!");

welcome(); // ok now
```

アロー関数は、最初は馴染みが無く、読みにくいように見えるかもしれませんが、構造に慣れるとすぐに変わります。

シンプルなワンライナーの処理で、多くの文字を書くのが面倒なときにはとても便利です。

## 複数行のアロー関数

上の例は、`=>` の左から引数を取得し、右側の式を評価しました。

複数の式や文のように、もう少し複雑なものが必要な時があります。それも可能ですが、この場合は波括弧で囲む必要があります。そして、その中で通常の `return` を使います。

このようになります:

```js run
let sum = (a, b) => {  // 波括弧を使って複数行の関数を書けます
  let result = a + b;
*!*
  return result; // 波括弧を使う場合、明示的な return が必要です
*/!*
};

alert( sum(1, 2) ); // 3
```

```smart header="他にもあります"
ここでは、簡潔にするためにアロー関数を賞賛しました。しかし、それだけではありません!!

アロー関数は他にも興味深い機能を持っています。

これらを学ぶためには、最初に JavaScript の他の側面について知る必要があります。なので、後ほどチャプター<info:arrow-functions> で触れます。

今の時点で、我々はすでにワンライナーの処理やコールバック処理のためにアロー関数を使うことができます。
```

## サマリ

アロー関数はワンライナーに対し便利です。2つの種類があります:

1. 波括弧無し: `(...args) => expression` -- 右側は式です: 関数はそれを評価しその結果を返します。
2. 波括弧あり: `(...args) => { body }` -- 括弧があると、関数内で複数の文を書くことができます、しかし何かを返却する場合には、明示的な `return` が必要です。
