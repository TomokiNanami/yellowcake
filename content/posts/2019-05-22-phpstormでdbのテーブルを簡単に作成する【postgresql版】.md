---
template: SinglePost
title: PhpStormでDBのテーブルを簡単に作成する【PostgreSQL版】
status: Published
date: 2019-05-22T12:56:33.446Z
featuredImage: ''
excerpt: 'PhpStorm, PostgreSQL'
categories:
  - category: IDE
meta: {}
---
便利な機能がいろいろ盛り込まれているPhpStorm。その機能の中で今回はDBのテーブルの情報を操作する方法を紹介していくぞ↑

### テーブルを作成する。

DBの`schemas/public`(多分別のところでも大丈夫かな…)で右クリック→New→Tableを選択
![](https://ucarecdn.com/9a7ae814-c146-4d4e-8f64-3395209ee7d1/)
するとテーブルの作成ウィンドウが表示される。そこで下の画像のようにテーブル名～カラム追加までできる。
![](https://ucarecdn.com/ed3a94a8-85e2-4a0d-a0cf-4e82171fb92b/)

### 既存のテーブルを編集する。

編集したいテーブルを左クリックして`Modify Table...`を選択すると、テーブル作成と同じウィンドウが開き、そこで編集することができる。
![](https://ucarecdn.com/6b79c5bf-a2cc-4e6b-ab87-98bf9f6f3722/)

### 【小ネタ】複合キーを設定する。

`PrimaryKey`を設定すると、既に設定されている他の`PrimaryKey`は外されてしまう。
ではどうやって複合キーを実現するか。  

1. `Keys`タブを選択する。
2. 中央の`＋`をクリックすると複合キーにする項目を追加できる。
3. 追加する項目を全て追加し終えたら、左の`Primary Key`にチェックを入れる。
4. そうすると下の実行されるSQL文に反映される。
5. 最後に右下の`Execute`を選択してDBに反映させる。
   ![](https://ucarecdn.com/cc1baa4e-fbdf-4b96-9b7f-dee39da14b20/)
   以上のやり方で複合キーを設定できる。

### 既にあるテーブルのDDLをコピーする。
DDLが欲しいテーブルで右クリック、`SQL Scrpts`→`Generate DDL to Clipboard`を選択すると、create tableのSQL文がクリップボードにコピーされる。
![](https://ucarecdn.com/d879cc7f-d533-4d16-9d42-548fcf539560/)
