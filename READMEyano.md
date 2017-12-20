# 後編javascriptを使ってみよう!
## javascriptとは？
Netscape Communication社によって開発された、ブラウザ向けスクリプト言語のこと。
JavaとJavascriptは全くの別物<-初心者はよく間違えるらしい。現在Google Chrome,Firefox, Safari,
Microsoft Edgeなど、主要なブラウザのほとんどで実装されている。色々あって昔はマイナスイメージの多い言語だったが
今ではメジャーな言語になっている。


## 覚えた方がいい用語(主観入ってる)
Ajax,jQuery,AngularJS,React,Node.js,DOM(document object model)
解説する時間はないのでここでググって調べるといい。

## javascriptを使って簡単な物を実装してみよう!

スクリプトタグ内に下記コードを描いてみましょう

```js
//今日の日付データを変数hidukeに格納
var hiduke=new Date();

//年・月・日・曜日を取得する
var year = hiduke.getFullYear();
var month = hiduke.getMonth()+1;
var week = hiduke.getDay();
var day = hiduke.getDate();

var yobi= new Array("日","月","火","水","木","金","土");

document.write("西暦"+year+"年"+month+"月"+day+"日 "+yobi[week]+"曜日");

```

日付と曜日が取得されるはずです。

時間がほしい場合は下記コードを入れてみましょう。

```js

//時刻データを取得して変数jikanに格納する
var jikan= new Date();

//時・分・秒を取得する
var hour = jikan.getHours();
var minute = jikan.getMinutes();
var second = jikan.getSeconds();

document.write(hour+"時",+minute+"分"+second+"秒");

```

次は簡単なアプリケーションを実装してみましょう。

## javascriptで単語帳アプリを実装してみよう！

まずは雛形コピペで構いません

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="utf-8">
  <title>Flash Cards</title>
  <style>
  body {
    margin: 0;
    background: #e0e0e0;
    text-align: center;
    font-family: Verdana, sans-serif;
    color: #fff;
  }
  #btn {
    width: 200px;
    margin: 0 auto;
    padding: 7px;
    border-radius: 5px;
    background: #00aaff;
    box-shadow: 0 4px 0 #0088cc;
    cursor: pointer;
  }
  #btn:hover {
    opacity: 0.8;
  }
  #card {
    margin: 60px auto 20px;
    width: 400px;
    height: 100px;
    cursor: pointer;
    font-size: 38px;
    line-height: 100px;
    perspective: 100px;
    transform-style: preserve-3d;
    transition: transform .8s;
  }
  #card-front, #card-back {
    display: block;
    width: 100%;
    height: 100%;
    border-radius: 5px;
    position: absolute;
    backface-visibility: hidden;
  }
  #card-front {
    background: #fff;
    color: #333;
  }
  #card-back {
    background: #00aaff;
    transform: rotateY(180deg);
  }
  .open {
    transform: rotateY(180deg);
  }
  </style>
</head>
<body>
  <div id="card">
    <div id="card-front">front</div>
    <div id="card-back">back</div>
  </div>
  <div id="btn">NEXT</div>
</body>
</html>
```

コードが長くなっていますが、styleタグに囲まれたところがCSSに対応する部分になっていますので実質のhtmlコードの部分は短いです。
idがcardは単語帳のカードの部分btnはボタン部分に対応しています。
card-frontはカードの表側
card-backはカードの裏側になります。
transform: rotateYという部分がひっくり返りの部分になります。
.openは後でjsで動かすための物です。


## ボタンを裏返しにしてみよう！

```js

<script>
(function() {
  'use strict';

  var words = [
    {'en': 'read', 'ja': '読む' },
    {'en': 'write', 'ja': '書く' },
    {'en': 'eat', 'ja': '食べる' },
    {'en': 'run', 'ja': '走る' },
    {'en': 'walk', 'ja': '歩く' }
  ];

  var card = document.getElementById('card');
  card.addEventListener('click', function() {
    flip();
  });

  function flip() {
    card.className = card.className === '' ? 'open' : '';
  }

})();
</script>

```
use strictは厳密なエラーチェックのための物です。
詳しくはhttps://qiita.com/miri4ech/items/ffcebaf593f5baa1c112が参考になると思います。

wordsという配列を用意してあげて英語はenのキーを、日本語はjaのキーを使うようにしていきます。

document.getElementById('card');でcard idの要素を取得します
そしてaddEventListenerをつけることによってcard要素に対してイベントを紐づけることができます。
今回の場合は

```js
 card.addEventListener('click', function() {
    flip();
  });
```

となっているのでクリックした時にflip関数が走るというよう動きをします。  
ちなみにaddEventListener()はInternet Explorer 8以前のIEでは動かないのでご注意を（多分大丈夫ですが）
このようにdocumentにアクセスするための色々な命令をdomと言います.(覚えてほしい単語の一つです。)
以下のフリップ関数ですが

```js
function flip() {
  card.className = card.className === '' ? 'open' : '';
}
```

中身としてはクラス名を変更しているだけです。
card.className がもし空文字だったら open にし、そうでなかったら空文字に戻すというように書いてあげます。
三項演算子という物を使っているのでご注意ください。

ここまでできればcardをクリックした時に裏返るはずです。

## ボタンを押したら次の単語に変わるようにしよう！

次はnextというボタンを押したら次の単語に変化するようにしましょう。
scriptタグ内に以下のコードを入れてみましょう。

```js
var cardFront = document.getElementById('card-front');
var cardBack = document.getElementById('card-back');
var btn = document.getElementById('btn');
btn.addEventListener('click', function() {
  next();
});

function next() {
  var num = Math.floor(Math.random() * words.length);
  cardFront.innerHTML = words[num]['en'];
  cardBack.innerHTML = words[num]['ja'];
}

next();
```

今回はcardFrontという変数にcard-front idの要素をcardBackという変数にcard-back要素をbtnという変数にbtn要素を入れます。
先程同様にaddEventListenerでbtnがクリックされた時にnext関数が走るように書きます。
Math.floorは()内の最大の整数を出します。
Math.random() * words.lengthはなぜこのような書き方をしているかというとMath.randomは0以上1未満の疑似ランダムな浮動小数点を返す関数です。words.lengthはwordsの大きさを取得しているので5ですね。
なのでMath.random() * words.lengthは0~0.9999.... * 5という内容になります。それ故に0~4の値を取ることがわかります。
innerHTMLですがこれは要素の子孫を記述する HTML 構文を設定または取得します。この場合はcardFrontにwordsのenキーに対応するものを、cardBackにはwordsのjaキーに対応するもの設定します。

このようなコードを記述すればカードの値がnextボタンを押すたびにランダムに変わるようになります。

また、最初に開いたときの値を入れたいのでnext()と最後に書いています。



## 課題

### 次のカードの回答が見えないようにしよう
現状だと答えの向きのままだと次のカードに行くときに答えが見えてしまう。答えが見えないようにしたい。


### ショートカットキーを追加してみよう
いちいちクリックしなくてはいけないのは面倒だそこでショートカットキーを追加して次のカードへ変更してみたい。
