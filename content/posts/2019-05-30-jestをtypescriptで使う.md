---
template: SinglePost
title: JestをTypescriptで使う
status: Published
date: 2019-05-30T12:21:55.267Z
excerpt: 'Jest, Typescript'
categories:
  - category: Javascript
  - category: Test Code
  - category: Typescript
meta: {}
---
前回はJestことはじめということでJavascriptコードのテストをおこなったが、今回はTypescriptバージョンを触っていく。  
といってもさほど変わりはなく、必要なモジュールが増えるだけと設定を１つ追加するだけだ。

### 必要なモジュールのインストール

```
$ npm i -D ts-jest @types/jest typescript
```

`ts-jest`: TypescriptのJestテストコードをコンパイルするのに必要。Babelを使っている場合は必要ないらしい。  
`@types/jest`: Typescript版Jestの型定義ファイル  

### 設定の追加

`jest.config.js`に以下を追加する。  
`jest --init`で設定ファイルを生成しているなら編集で済む。

```
preset: 'ts-jest'
```

### テスト対象のファイル

バリューオブジェクトの記事で使った数量クラスを使用する。

```
export class Quantity {

    private static readonly MIN: number = 0;
    private static readonly MAX: number = 100;

    private _value: number;

    constructor(value: number) {
        if (value < Quantity.MIN) {
            throw new Error(`不正: ${Quantity.MIN}未満`);
        }
        if (value > Quantity.MAX) {
            throw new Error(`不正: ${Quantity.MAX}超`);
        }
        if (typeof value !== 'number') {
            throw new Error(`不正: 異常値`)
        }
        this._value = value;
    }

    public get value(): number {
        return this._value;
    }

    public canAdd(other: Quantity): boolean {
        const added = this.addValue(other);
        return added <= Quantity.MAX;
    }

    public add(other: Quantity): Quantity {
        if (!this.canAdd(other)) {
            throw new Error(`不正: 合計が${Quantity.MAX}以上`);
        }
        const added = this.addValue(other);
        return new Quantity(added);
    }

    private addValue(other: Quantity): number {
        return this._value + other.value;
    }

}
```
### 実際にテストを書いた

今回はインスタンス化のテストを書いた。  
describeのおかげで階層構造になってやりやすい。

```
import {Quantity} from "./Quantity";

describe('Quantityクラステスト', () => {

    describe('インスタンス化', () => {
        describe('正常処理', () => {

            test('通常値', () => {
                expect(() => {
                    const quantity = new Quantity(5);
                }).not.toThrow();
            });

            test('境界値: 0', () => {
                expect(() => {
                    const quantity = new Quantity(0);
                }).not.toThrow();
            });

            test('境界値: 100', () => {
                expect(() => {
                    const quantity = new Quantity(100);
                }).not.toThrow();
            });

        });

        describe('例外処理', () => {

            test('異常値: 文字列', () => {
                expect(() => {
                    // @ts-ignore
                    const quantity = new Quantity('100');
                }).toThrow();
            });

            test('異常値: 真偽値', () => {
                expect(() => {
                    // @ts-ignore
                    const quantity = new Quantity(true);
                }).toThrow();
            });

            test('既定値越え: 101', () => {
                expect(() => {
                    const quantity = new Quantity(101);
                }).toThrow();
            });

            tet('既定値越え: -1', () => {
                expect(() => {
                    const quantity = new Quantity(101);
                }).toThrow();
            });

        })
    });
});
```

### テストの実行

```
$ jest
 PASS  src/value-object/Quantity.spec.ts
  Quantityクラステスト
    インスタンス化
      正常処理
        √ 通常値 (4ms)
        √ 境界値: 0 (1ms)
        √ 境界値: 100
      例外処理
        √ 異常値: 文字列 (8ms)
        √ 異常値: 真偽値
        √ 既定値越え: 101 (1ms)
        √ 既定値越え: -1

Test Suites: 1 passed, 1 total
Tests:       7 passed, 7 total
Snapshots:   0 total
Time:        1.914s, estimated 5s
Ran all test suites.
```

### PhpStormで楽に実行

PhpStormなどのJetBrains製のIDEではJestをサポートしている。  

上部のメニューからEdit Configurationsを選択

![](https://ucarecdn.com/cd31e819-f556-44b6-a3ec-2da80982553e/)

+ボタンクリックしてからのJestを選択

![](https://ucarecdn.com/99ff3022-2ce1-4e08-bbc1-554c88f08bed/)

Jest optionsに`--coverage`を追加しておけばカバレッジを勝手に出してくれるようになる。

![](https://ucarecdn.com/b992651f-2cfd-4b0f-8e5a-6a88f3ed58c9/)

先ほど設定したJestを選択して隣の再生ボタンをクリックしてテスト実行

![](https://ucarecdn.com/b06d84bc-4aa2-4ba8-95a5-f57bb09dad2b/)

非常に見やすい結果を表示してくれる。

![](https://ucarecdn.com/09afaf83-c82e-4bbf-bd05-4369b2192969/)
