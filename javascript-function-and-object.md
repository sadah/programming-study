# JavaScriptの関数とオブジェクト

JavaScriptを書いていく上で、とても重要な「関数」と「オブジェクト」。

JavaScriptを書けば関数やオブジェクトを使うことになるけど、jQueryを使ってちょっとした動くものを作るくらいだと、関数とオブジェクトを意識することは少ないかもしれない。

この文書の目的は「関数とオブジェクトと仲良くなって、普段書いているコードの意味や動作がわかるようになる」こと。

関数とオブジェクトと仲良くなると、普段書いているコードの意味や動作がわかるようになって、JavaScriptの読み書きがずっと楽になると思う。

MDNのドキュメントや、Effective JavaScriptを読めば、本文書より正確で詳しいことが書いてある。

* <a href="https://developer.mozilla.org/ja/docs/Web/JavaScript" target="_blank">JavaScript | MDN</a>
* <a href="http://www.amazon.co.jp/dp/4798131113" target="_blank">Amazon.co.jp： Effective JavaScript JavaScriptを使うときに知っておきたい68の冴えたやり方: David Herman, 吉川 邦夫: 本</a>

本文書によって、これらのドキュメントや書籍を読むきっかけになればいいな、と。

本文書では、ECMAScript 5.1をベースに記載する。


## 関数は第一級オブジェクト

JavaScriptの関数は第一級オブジェクト(first-class object)。

> JavaScript において、関数は第一級オブジェクトです。すなわち、関数はオブジェクトであり、他のあらゆるオブジェクトと同じように操作したり渡したりする事ができます。具体的には、関数は Function オブジェクトです。
> 
> <a href="https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Functions_and_function_scope" target="_blank">関数と関数スコープ - JavaScript | MDN</a>

第一級オブジェクトというのは、こんな風に定義されている。

* <a href="http://ja.wikipedia.org/wiki/%E7%AC%AC%E4%B8%80%E7%B4%9A%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88" target="_blank">第一級オブジェクト - Wikipedia</a>

いろいろ書いてあるけど、これがいちばんの特徴だと思う。

> 変数に格納可能である。

こういうことができる。

```javascript
var doSomething = function(){
  // do something...
};
```

doSomethingという変数に、関数を格納している。この書き方、よく使うよね。


## 関数の作り方

関数宣言(function文)で作る。

```javascript
function doSomething(){
  // do something...
}
```

関数式(function 演算子)で作る

```javascript
var doSomething = function(){
  // do something...
};
```

どっちを使ってもいいけど、使えるタイミングがちょっと違う。

```function doSomething(){}```はどこで定義してもすぐに使える。

```var doSomething = function(){}```は、定義したあとじゃないと使えない。

Functionコンストラクタや、コンストラクタ関数(newにまつわるあれこれ)については、本文書ではとりあげない。

* <a href="https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Creating_New_Objects/Using_a_Constructor_Function" target="_blank">コンストラクタ関数の使用 - JavaScript | MDN</a>


## 宣言の巻き上げ

宣言は、巻き上げられる。巻き上げ、ホイスト、ホイスティング、hoistingと呼ばれる。

```function doSomething(){}```は関数宣言。

```var a;```は変数宣言。

巻き上げられるっていうのは、こんな感じのことが起きる。

* <a href="https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/var" target="_blank">var - JavaScript | MDN</a>

```javascript
x = 2
var x;

// こうなる

var x;
x = 2;
```

関数にしても同じ。

```javascript
x = function(){};
var x;

// こうなる

var x;
x = function(){};
```

関数を実行する場合は

```javascript
x();
var x = function(){};

// こうなる

var x;
x(); // この時点では、xはundefined
x = function(){};
```

```x = function(){}```の後じゃないと、```x()```できない。関数宣言の場合は、こうなる。

```javascript
x();
function x(){}

// こうなる

function x(){}
x();
```

関数宣言を使うと、宣言が巻き上げられるので、コード上で宣言の前でも```x()```できる。

## 関数スコープ

JavaScriptのスコープは関数スコープ。JavaScriptにブロックスコープはない。

```javascript
function f(){
    for(var i = 0; i < 10; i++;){
    var x = i * i;
    console.log(x);
  }
}

// こうなる

function f(){
  var i, x;
  for(i = 0; i < 10; i++;){
    x = i * i;
    console.log(x);
  }
}
```

あと var を付けて変数宣言しないと、グローバル変数になっちゃうから、必ず付けようね。

```javascript
function f(){
  x = 1;
}
f();
console.log(x)        // => 1
console.log(window.x) // => 1
```

## 無名関数、高階関数、クロージャ

言葉が難しいけど、普通に使ってる、はず。

* <a href="http://ja.wikipedia.org/wiki/%E7%84%A1%E5%90%8D%E9%96%A2%E6%95%B0" target="_blank">無名関数 - Wikipedia</a>
* <a href="http://ja.wikipedia.org/wiki/%E9%AB%98%E9%9A%8E%E9%96%A2%E6%95%B0" target="_blank">高階関数 - Wikipedia</a>
* <a href="http://ja.wikipedia.org/wiki/%E3%82%AF%E3%83%AD%E3%83%BC%E3%82%B8%E3%83%A3" target="_blank">クロージャ - Wikipedia</a>


### 無名関数

$()は関数を引数に取る。無名関数を引数に渡している。

```javascript
$(function(){
  // lambda  
});
```

## 高階関数

関数を引数に取る関数や、関数を返す関数。eachは関数を引数に取る。

```javascript
$(".aaa").each(function(){

});
```

## クロージャ

クロージャは、外側の変数を保持して、関数を返す高階関数。

* <a href="https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Closures" target="_blank">クロージャ - JavaScript | MDN</a>

カウンタのサンプルがよくある。これはだめな例。

```javascript
    $("#counter").on("click",function(){
      var count = 0;
      count++;
      alert(count);
    });
```

こんな感じにする。

```javascript
    var counter = function(){
      var count = 0;
      return function(){
        count++;
        alert(count);
      };
    };
    $("#counter").on("click",counter());
```

これもだめな例。

```javascript
    var list = $("#button-container > button");
    for(var i = 0; i < list.length; i++){
      list.eq(i).on("click", function(){
        alert(i);
      });
    }
```

こんな感じにする。

```javascript
    var list = $("#button-container > button");
    var counter = function(count){
      return function(){
        alert(count);
      };
    };
    for(var i = 0; i < list.length; i++){
      list.eq(i).on("click", counter(i));
    }
```







