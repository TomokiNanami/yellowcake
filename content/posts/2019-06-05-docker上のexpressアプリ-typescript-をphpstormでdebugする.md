---
template: SinglePost
title: Docker上のExpressアプリ(TypeScript)をPhpStormでDebugする
status: Published
date: 2019-06-05T14:35:17.262Z
excerpt: 'Javascript, TypeScript, Docker, PhpStorm, Debug'
categories:
  - category: IDE
  - category: Javascript
  - category: TypeScript
meta: {}
---
Docker上のExpressアプリのデバッグ環境構築までなかなか手こずったのでメモ。

### port:9229番を解放する設定をする

`docker-compose.yml`のNode.jsアプリ側の設定に、

```
ports:
    - 3000:3000
    - 9229:9229 // ← これを追加
```

### ts-node-devの起動オプションにinspectを設定する。

`nodemon.json`の`exec`項目に`--inspect-brk=0.0.0.0`を追加する。

```
{
  "watch": [
    "src"
  ],
  "ext": "ts",
  "exec": "ts-node-dev --inspect-brk=0.0.0.0 -r dotenv/config ./src/server.ts"
}
```

### PhpStorm側でdebugの設定をする

上のメニューバーまたはToolbarの`Edit Configurations...`を選択。

![](https://ucarecdn.com/5c7663f0-e23f-427f-875e-e6d0a77d9de1/)

＋ボタンから`Attach to Node.js/Chrome`を選択。
※無い場合は...
`Host`に`localhost`、`Port`には`9229`を入力し、Apply
![](https://ucarecdn.com/1a87e86c-f4a6-4e1f-b3a4-9d18cac6d4c0/)

### いざDebug

toolbarから先ほど作成したAttachの設定を選択し、🐞ボタンをクリック。

![](https://ucarecdn.com/c2b99840-2c2a-4ced-8b40-b3bdbf62de5a/)

ブレークポイントで止まるようになった。

![](https://ucarecdn.com/71f46e35-5c88-42b5-9923-5f8f8417595f/)
