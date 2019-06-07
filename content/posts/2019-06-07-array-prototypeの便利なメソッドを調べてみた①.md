---
template: SinglePost
title: Array.prototypeの便利なメソッドを調べてみた①
status: Published
date: 2019-06-07T13:59:16.947Z
excerpt: 'Javascript, Array.prototype'
categories:
  - category: Javascript
meta: {}
---
業務や個人開発で配列を扱うことがとても多いので少しでも`for`のようなインデックスを排除するために便利なメソッドを知っておこうと思ったのがきっかけ。  
※ただし、速度を追求するのであれば`for`を使わざるを得ないようなので、ここでは可読性を高めるという観点で見ていく。

定期的にこのシリーズをやっていきたい。

### Array.prototype.every(callback)
引数に渡された**戻り値がboolean型**である関数で、配列の要素一つ一つをテストしていき、全てtrueなら最終的な戻り値はtrueを返却する。

例：
```
var arr = [2, 4, 6];
if (arr.every(n => n % 2 === 0)) {
    console.log('偶数のみ');
} else {
    console.log('奇数が混ざる');
}
// 結果
偶数のみ

var arr02 = [2, 4, 5];
if (arr02.every(n => n % 2 === 0)) {
    console.log('偶数のみ');
} else {
    console.log('奇数が混ざる');
}
// 結果
奇数混ざる
```

### Array.prototype.flat(depth)
※IEは対応していない。またNode.jsはver11.0以降。  
配列の中に存在する複数の配列をフラットにする。  
引数は省略することができ、フラットにする深さを設定できる。

例：
```
var arr = [
    [
        {hoge: 10},
        {fuga: 20}
    ],
    [
        {piyo: 30},
        {foo: 40}
    ]
];
console.log(arr.flat());
// 結果
(4) [{…}, {…}, {…}, {…}]
0: {hoge: 10}
1: {fuga: 20}
2: {piyo: 30}
3: {foo: 40}
```
深さを指定：
```
var arr = [1,2,[3,4,[5,6]]];
console.log(arr.flat());
// 結果
(5) [1, 2, 3, 4, Array(2)]
0: 1
1: 2
2: 3
3: 4
4: (2) [5, 6]

var arr = [1,2,[3,4,[5,6]]];
console.log(arr.flat(2));
// 結果
(6) [1, 2, 3, 4, 5, 6]
```

### 
