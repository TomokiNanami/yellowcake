---
template: SinglePost
title: 'forEachで''continue'', ''break''を実現する'
status: Published
date: 2019-05-14T14:32:41.797Z
excerpt: 'javascript, forEach, some'
categories:
  - category: JavaScript
meta:
  description: >-
    javascriptのforEach()では、for文のような`continue`や`break`といった仕組みはない。 
    そこでどうやってこれらを実現するか。...
  title: '【javascript】forEach()でcontinue, breakを実現する。'
---
JavaScriptのforEach()では、for文のような`continue`や`break`といった仕組みはない。  
そこでどうやってこれらを実現するか。

### 結論
- `continue`: `return`を使う。  
- `break`: `some`関数を使う。

### 参考ソース
continueの場合
```
const arr = [1, 2, 3, 4, 5];
arr.forEach(n => {
    if (n % 2 === 1) {
        return;
    }
    console.log(n);
});
// 結果
2
4
```

breakの場合  
[Array.prototype.some()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/some)  
`some(callback)`は引数として渡されたコールバック関数が`true`を返却すると処理を終了する。
```
const arr = [1, 2, 3, 4, 5];
arr.some(n => {
    if (n === 3) {
        return true;
    }
    console.log(n);
})
// 結果
1
2
```
