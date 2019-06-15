---
template: SinglePost
title: Typescriptで自分のクラス名を取得する。
status: Published
date: 2019-06-15T09:10:19.783Z
excerpt: 'Typescript, getClass'
categories:
  - category: Typescript
meta: {}
---
「テスト駆動開発」の本を実践している途中に、Javaの`getClass()`メソッドを使う箇所が出てきたが、Typescriptにはそういうメソッドは無い。そのため自作する必要が出てきた。  
そこでTypescriptでどうやって`getClass()`を実現するかを書いていく。

## どうやって実現するか？
`this.constructor.name`を使う。

## 実際の例
※プロパティとコンストラクタは省略
```
export class Money {
    public getClass() {
        return this.constructor.name;
    }
}

new Money().getClass();
// 結果
Money
```

## 継承した場合
`getClass()`メソッドを定義した親クラスを継承した子クラスで`getClass()`を使用すると、子クラスのクラス名が取得できる。
```
export class Dollar extends Money {
    // いろいろ省略
}

new Dollar().getClass();
// 結果
Dollar
```

## 参考記事
[Get an object's class name at runtime](https://stackoverflow.com/questions/13613524/get-an-objects-class-name-at-runtime/38669166#38669166)
