# 後編javascriptを使ってみよう!
## javascriptとは？
Netscape Communication社によって開発された、ブラウザ向けスクリプト言語のこと。
JavaとJavascriptは全くの別物<-初心者はよく間違えるらしい。現在Google Chrome,Firefox, Safari,
Microsoft Edgeなど、主要なブラウザのほとんどで実装されている。色々あって昔はマイナスイメージの多い言語だったが
今ではメジャーな言語になっている。


## 覚えた方がいい用語(主観入ってる)
Ajax,jQuery,AngularJS,React,Node.js
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

まずは雛形
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

## ボタンを押したら次の単語に変わるようにしよう！

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

## 課題

###次のカードの回答が見えないようにしよう
現状だと答えの向きのままだと次のカードに行くときに一瞬答えが見えてしまう。答えが見えないようにしたい。


###ショートカットキーを追加してみよう
いちいちクリックしなくてはいけないのは面倒だそこでショートカットキーを追加して次のカードへ変更してみたい。
