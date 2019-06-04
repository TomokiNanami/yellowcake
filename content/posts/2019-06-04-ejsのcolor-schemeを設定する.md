---
template: SinglePost
title: EJSのColor Schemeを設定する
status: Published
date: 2019-06-04T05:40:31.143Z
excerpt: PhpStorm
categories:
  - category: IDE
meta: {}
---


EJSの<% %>タグが凄く見づらかったので、color schemeを設定するまでの手順を共有。

### EJSプラグインのインストール

File/Settings/Pluginsから**EJS**で検索し、インストール。  
一旦PhpStormを再起動する。

![](https://ucarecdn.com/daaa7654-d86a-446e-98b5-ccabff733a35/)



### EJSプラグインとEJSファイルを結びつける

File/SettingsからEditor/File Typesを開き、「Recognized File Types」から「EJS Combines Data And A Template To Produce HTML」を選択

![](https://ucarecdn.com/de9d521f-78f3-4f0a-b480-22b2dbf178db/)

下の「Registerd Patterns」の枠の右側にある＋ボタンをクリックし、`Enter new wildcard('\*' and '?' allowed):`の欄に`*.ejs`と入力し、「OK」を選択

![](https://ucarecdn.com/a19bfa18-80ca-4546-aa90-425a5bd7193b/)

あとは、Editor/Color SchemeのEJSで色の選択をすれば色が反映される。
