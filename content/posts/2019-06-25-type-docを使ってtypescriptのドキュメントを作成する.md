---
template: SinglePost
title: TYPE DOCを使ってTypeScriptのドキュメントを作成する
status: Published
date: 2019-06-25T13:04:04.333Z
excerpt: 'TypeScript, TYPE COD'
categories:
  - category: TypeScript
meta: {}
---
TypeScriptのドキュメント作成ツール、[TYPE DOC](https://typedoc.org/)を使ったドキュメント作成の手順。

## TYPE DOCのインストール

```
$ npm install --save-dev typedoc
```

## 出力先の設定

TypeScriptなので`tsconfig.json`が必須になる。  
設定もここに追加する。`out`で出力先を指定する。  
`mode`については、

> バージョン0.2以降、TypeDocはファイルをモジュールとして扱うべきかどうか、またはプロジェクトを1つの大きな名前空間にコンパイルするべきかどうかを予測できなくなりました。バージョン0.2以降、TypeDocはファイルをモジュールとして扱うべきかどうか、あるいはプロジェクトを1つの大きな名前空間にコンパイルすべきかどうかを予測できなくなりました。 TypeDocの動作を変更するには、mode引数を指定する必要があります。

とのこと。よくわからん。

> tscofig.json

```
  "typedocOptions": {
    "mode": "modules",
    "out": "docs"
  },
```

## typedocコマンドをpackage.jsonに追加する

`typedoc <ドキュメントの元となるパス>`の形式で記載する。  
今回の例では`./src`配下のファイルを対象とした。

> package.json

```
  "scripts": {
    "typedoc": "typedoc ./src"
  },
```

## 実際にTypeScriptでDocコメントを書いていく

今回は試しに一つのユースケースクラスを使用した。

> RegisterAttendanceTime.ts

```
import { IAttendanceRepository } from "../repositories/IAttendanceRepository";
import { Attendance } from "../../domain/models/Attendance";

/**
 * 出勤時間登録ユースケース
 * @class RegisterAttendanceTime
 * @classdesc 出勤時間を登録するユースケースクラス
 * @author m-Suda
 */
export class RegisterAttendanceTime {

    /**
     * @description 勤怠リポジトリ
     */
    private readonly attendanceRepository: IAttendanceRepository;

    /**
     * @description リポジトリをDIする。
     * @param {IAttendanceRepository} attendanceRepository
     */
    constructor(attendanceRepository: IAttendanceRepository) {
        this.attendanceRepository = attendanceRepository;
    }

    /**
     * @description 出勤時間を登録する。
     * @param {Attendance} attendance - traineeID, yearMonthDay, workStartTimeを含むオブジェクト
     * @return {Promise<Attendance>}
     */
    public async execute(attendance: Attendance): Promise<Attendance> {
        try {
            return await this.attendanceRepository.arrival(attendance);
        } catch (e) {
            throw e;
        }
    }
}
```

## 出力してみる

```
$ npm run typedoc
```

## 出力されたファイルを確認

出力されたのが確認できる。
![](https://ucarecdn.com/cfc4b745-62d3-4e0e-8ebb-404200e3bc2b/)

## 出力されたHTMLファイルを見てみる

`globals.html`のほうを開くと画像のような画面が確認できる。

![](https://ucarecdn.com/962c30e3-7b19-4cdb-818e-0f2e4a65b6ef/)

そのうち、先ほどDocコメントを書いた`RegisterAttendanceTime
.ts`を開いてみると、こんな感じでClassのドキュメントが作成される。

![](https://ucarecdn.com/3c665206-3042-4445-83eb-20edb6404cca/)

下のほうにスクロールしていくとIndexがあり、そのClassの構成が見られる。

![](https://ucarecdn.com/9e01ed4a-d3fa-4022-aa07-6187a7510e12/)

もちろんメソッド一つ一つ作成される。ちなみにリンクになっているところをクリックすると、そのClassのドキュメントにジャンプすることができるので便利。

![](https://ucarecdn.com/c847350f-176a-4d25-89e4-5958e0881d72/)

TYPE DOCのおかげでわざわざExcelを使う必要は無くなり、Docコメントをしっかり書けば実装しながら同時にドキュメントを作成することができる。
