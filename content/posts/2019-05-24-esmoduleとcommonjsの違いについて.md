---
template: SinglePost
title: ESmoduleとCommonJSの違いについて
status: Published
date: 2019-05-24T13:55:40.505Z
excerpt: 'JavaScript, ESmodule, CommonJS'
categories:
  - category: JavaScript
  - category: Node.js
meta: {}
---
今までgetting_startedとかで何も考えずに`import, export`とか`require, export`を使っていたり、`tsconfig.js`の`module`をよくわからないまま指定していたので調べた。

### そもそもモジュールとは？
JavaScriptコードの保守性や再利用性を高めるためにJavaScriptファイルから別のJavaScriptファイルを読み込む仕組み。仕様として今回の主題である**CommonJS**と**ESmodule**が定められている。
### 今まではどうしていた？
JavaScriptファイルを読み込むために**scriptタグ**を使って読み込んでいた。これは読み込みの順番を気にしないといけなく面倒であった。
### CommonJSとは？
Node.jsの普及をきっかけに決められた標準API仕様。Node.jsのグローバル変数であるmodule変数を使って変数や関数をエクスポートする。`module.exports`プロパティに代入されたオブジェクトがエクスポートされる。読み込み方法は**`export`, `require`, グローバル変数である`module`**を利用する。
### ESmoduleとは？
ES2015(ECMAScript2015, ES6なんて呼ばれたりする)において策定されたモジュールの仕様。Node.jsだけでなくブラウザにも対応するモジュールシステム。読み込み方法は**`export`, `import`**を使う。
### CommonJSを使ってみる。
> sum.js

```
module.exports = {
    sum(a, b) {
        return a + b;
    },
};
```
> app.js

```
const mod = require('./sum');
console.log(mod.sum(2, 3));
// 結果
5
```

### ESmoduleを使ってみる。
**名前付きエクスポート**と**デフォルトエクスポート**がある。
##### 名前付きエクスポートの場合
exportする変数名がそのままimportするときの名前になる。  
複数の値をexrpotできる。
> sum.js

```
export const ZERO = 0;
export const sum = (a, b) => a + b;
```
> app.js

```
import {sum, ZERO} from "./sum";

console.log(sum(1, 4));
console.log(ZERO);
```
※ただしNode.jsの環境では↑のコードは動かない。なぜならNode.jsの実行はCommonJSのため。動かしたい場合は[webpack](https://webpack.js.org/)や[Babel](https://babeljs.io/)を利用しトランスパイルする必要がある。

##### デフォルトエクスポートの場合
> sum.js

```
export const ZERO = 0;
export default (a, b) => a + b;
```

> app.js

```
import sum, { ZERO } from './sum'

console.log(sum(1, 4));
console.log(ZERO);
```

ちなみに名前付きエクスポートは**まとめてimportできる。**  
その場合`* as name`で別名を付け、それぞれのプロパティを参照する形で利用することができる。
> sum.js

```
export const ZERO = 0;
export const ONE = 1;
export const TWO = 2;
export const sum1 = n => n + 1;
export const sum2 = n => n + 2;
```

> app.js

```
import * as sum from './sum';

console.log(sum.ZERO);
console.log(sum.ONE);
console.log(sum.TWO);

console.log(sum.sum1(5));
console.log(sum.sum2(10));
```

### 参考記事
- [モジュール](https://jsprimer.net/use-case/module/)
- [ES6でexportとexport defaultをどう使い分ければいいの？](https://yukidarake.hateblo.jp/entry/2015/08/11/210227)
- [ES6のexportについて詳しく調べた](https://qiita.com/senou/items/a2f7a0f717d8aadabbf7)
