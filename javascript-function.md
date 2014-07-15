# JavaScriptの関数

JavaScriptを書いていく上で、とても重要な関数。

JavaScriptを書けば須らく関数を使うことになるけど(たぶん)、jQueryを使ってちょっとした動くものを作るくらいだと、関数を意識することは少ないかもしれない。

---

この文書の目的は「関数と仲良くなって、普段書いているコードの意味や動作がわかるようになる」こと。

関数と仲良くなると、普段書いているコードの意味や動作がわかるようになって、JavaScriptの読み書きがずっと楽になると思う。

---

MDNのドキュメントや、Effective JavaScriptを読めば、本文書より正確で詳しいことが書いてある。
本文書によって、これらのドキュメントや書籍を読むきっかけになればいいな、と。

* <a href="https://developer.mozilla.org/ja/docs/Web/JavaScript" target="_blank">JavaScript | MDN</a>
* <a href="http://www.amazon.co.jp/dp/4798131113" target="_blank">Effective JavaScript</a>

本文書では、ECMAScript 5.1をベースに記載する。

---

## 関数は第一級オブジェクト

JavaScriptの関数は第一級オブジェクト(first-class object)。

> JavaScript において、関数は第一級オブジェクトです。すなわち、関数はオブジェクトであり、他のあらゆるオブジェクトと同じように操作したり渡したりする事ができます。具体的には、関数は Function オブジェクトです。
>
> <a href="https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Functions_and_function_scope" target="_blank">関数と関数スコープ - JavaScript | MDN</a>

---

第一級オブジェクトというのは、こんな風に定義されている。

* <a href="http://ja.wikipedia.org/wiki/%E7%AC%AC%E4%B8%80%E7%B4%9A%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88" target="_blank">第一級オブジェクト - Wikipedia</a>

いろいろ書いてあるけど、これがいちばんの特徴だと思う。

> 変数に格納可能である。

---

こういうことができる。

```javascript
var doSomething = function(){
  // do something...
};
```

doSomethingという変数に、関数を格納している。この書き方、よく使うよね。

---

## 関数の作り方

関数宣言(function文)で作る。function statement.

```javascript
function doSomething(){
  // do something...
}
```

---

関数式(function 演算子)で作る。function operator.

```javascript
var doSomething = function(){
  // do something...
};
```

---

どっちを使ってもいいけど、使えるタイミングがちょっと違う。

```function doSomething(){}```はどこで定義してもすぐに使える。

```var doSomething = function(){}```は、定義したあとじゃないと使えない。

---

Functionコンストラクタや、コンストラクタ関数(newにまつわるあれこれ)については、本文書ではとりあげない。

* <a href="https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Creating_New_Objects/Using_a_Constructor_Function" target="_blank">コンストラクタ関数の使用 - JavaScript | MDN</a>


---

## 宣言の巻き上げ

宣言は、巻き上げられる。巻き上げ、ホイスト、ホイスティング、hoistingと呼ばれる。

```function doSomething(){}```は関数宣言。

```var a;```は変数宣言。

---

巻き上げられるっていうのは、こんな感じのことが起きる。

* <a href="https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/var" target="_blank">var - JavaScript | MDN</a>

```javascript
x = 2
var x;

// こうなる

var x;
x = 2;
```

---

関数にしても同じ。

```javascript
x = function(){};
var x;

// こうなる

var x;
x = function(){};
```

---

関数を実行する場合は

```javascript
x();
var x = function(){};

// こうなる

var x;
x(); // この時点では、xはundefined
x = function(){};
```

```x = function(){}```の後じゃないと、```x()```できない。

---

関数宣言の場合は、こうなる。

```javascript
x();
function x(){}

// こうなる

function x(){}
x();
```

関数宣言を使うと、宣言が巻き上げられるので、コード上で宣言の前でも```x()```できる。

---

## 関数スコープ

JavaScriptのスコープは関数スコープ。JavaScriptにブロックスコープはない。

---

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

---

あと var を付けて変数宣言しないと、グローバル変数になっちゃうから、必ず付けようね。

```javascript
function f(){
  x = 1;
}
f();
console.log(x)        // => 1
console.log(window.x) // => 1
```

---

## 無名関数、高階関数、クロージャ

言葉が難しいけど、普通に使ってる、はず。

* <a href="http://ja.wikipedia.org/wiki/%E7%84%A1%E5%90%8D%E9%96%A2%E6%95%B0" target="_blank">無名関数 - Wikipedia</a>
* <a href="http://ja.wikipedia.org/wiki/%E9%AB%98%E9%9A%8E%E9%96%A2%E6%95%B0" target="_blank">高階関数 - Wikipedia</a>
* <a href="http://ja.wikipedia.org/wiki/%E3%82%AF%E3%83%AD%E3%83%BC%E3%82%B8%E3%83%A3" target="_blank">クロージャ - Wikipedia</a>

---

### $()に渡すあれ - 無名関数

$()は関数を引数に取る。無名関数を引数に渡している。

```javascript
$(function(){
  // lambda
});
```

---

### jQueryのonとかeach - 高階関数

関数を引数に取る関数や、関数を返す関数。eachは関数を引数に取る。

```javascript
$(".list").each(function(){

});
```

---

### 外側の変数を覚えていてくれる子 - クロージャ

クロージャは、外側の変数を保持して、関数を返す高階関数。

* <a href="https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Closures" target="_blank">クロージャ - JavaScript | MDN</a>

---

カウンタのサンプルがよくある。これはだめな例。

* <a href="https://sadah.github.io/programming-study/javascript-function/samples/counter_not_work.html" target="_blank">counter not work</a>

```javascript
$("#counter").on("click",function(){
  var count = 0;
  count++;
  alert(count);
});
```

---

こんな感じにする。

* <a href="https://sadah.github.io/programming-study/javascript-function/samples/counter.html" target="_blank">counter</a>

```javascript
var count = 0;
$("#counter").on("click",function(){
  count++;
  alert(count);
});
```

---

もうちょっと推し進めるとこんな感じ。関数を返す関数を定義する。

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

---

これもだめな例。

* * <a href="https://sadah.github.io/programming-study/javascript-function/samples/list_not_work.html" target="_blank">list not work</a>


```javascript
var list = $("#button-container > button");
for(var i = 0; i < list.length; i++){
  list.eq(i).on("click", function(){
    alert(i);
  });
}
```

---

こんな感じにする。

* <a href="https://sadah.github.io/programming-study/javascript-function/samples/list.html" target="_blank">list</a>

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

---

## ひよコードを撲滅する

> 美しくないコードをuncodeとかクソースなどと呼ぶ人もいるようですが、これらの言葉は開発者を傷つけるので、最近アプレッソでは、あまり美しくないコードを「ひよコード」、美しくない箇所は「ここがピヨピヨしてる」と表現するようにしています。未熟だけど伸び代はあることを意味しています。
>
> * <a href="http://blog.livedoor.jp/lalha/archives/50495777.html" target="_blank">小野和俊のブログ:コードレビューについて</a>

---

### グローバルスコープに関数定義

グローバルスコープに関数定義しているコードをたまに見かける。

---

どうしてこうなった！

```javascript
$(function(){
  doSomething();
  doSomthing2();
});
function doSomething(){
}
var doSomthing2 = function(){
};
```

---

var付いてるけど、グローバルで宣言しちゃだめ。functionもグローバルで宣言しちゃだめ。こうする。

```javascript
$(function(){
  function doSomething(){
  }
  var doSomthing2 = function(){
  };
  doSomething();
  doSomthing2();
});
```

---

グローバルスコープに関数定義しても、かぶることなんてそんなにないでしょう、って思うかもしれないけど、これの問題点わかる？

```javascript
$(function(){
  // ...
  open();
  // ...
});

function open(){
  // open something
}
```

---

window.open()を潰してる。

* <a href="https://developer.mozilla.org/ja/docs/Web/API/window.open" target="_blank">window.open - Web API インターフェイス | MDN</a>

---

### 関数の使い方がピヨピヨ

冗長な感じがしちゃう。

```javascript
$(function(){
  var doSomething = function(){
  }
  $("#input-name").on("keyup", function(){
    doSomething();
  });
  $("#input-address").on("keyup", function(){
    doSomething();
  });
});
```

---

onの第二引数(この場合)は、関数を取る。これはうまくいかない。doSomething()がすぐに実行されてしまう。

```javascript
  $("#input-area").on("keyup", doSomething());
```

そんなときは```doSomething```という関数を渡す。

```javascript
  $("#input-name").on("keyup", doSomething);
```

---

さらにjQueryのonは、こんなふうに書ける。

```javascript
$(function(){
  var doSomething = function(){
    // doSomething
  }

  $("#input-area, #submit-button").on("keyup", doSomething);
});
```

---

()は、関数を実行してくれる。

```javascript
  var doSomething = function(){
    // doSomething
  }
```

こんな関数が定義されていた場合```doSomething```は関数のオブジェクト、```doSomething()```は関数を実行する。

---

だから、こんなコードはページが表示されたときに、alertが出る。

```javascript
$(function(){
  var doSomething = function(){
    alert("doSomething");
  };

  $("#button").on("click", doSomething());
});
```

---

こうやって関数を渡すと、クリックされたときにalertが表示される。

```javascript
$(function(){
  var doSomething = function(){
    alert("doSomething");
  };

  $("#button").on("click", doSomething);
});
```

---

## まとめ

関数は値。関数は変数に格納できる。

用語は難しいかもしれないけど、ふつうに使っているものが多いと思う。

関数が理解できると、ライブラリのコードも読んでいけると思う。

Enjoy JavaScript!



