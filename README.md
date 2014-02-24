:feelsgood: Idobata 非公式 API doc :feelsgood:
===============

[Idobata](https://idobata.io/)の非公式APIドキュメントです:metal:

:warning: 推測・憶測に基づいた嘘デタラメや信ぴょう性のない情報が書き込まれている恐れ

**TODO**: API BlueprintやRAMLで書きたいです。

Idobataには大きく分けて2つのAPIがあると考えています。
1つはHook API、もう1つはBot APIです。
Hook APIは、部屋に発言を書き込むのみ、主に通知を投稿することしかできません。
Bot APIの方は~~Bot APIからBotを作ることができるBotのためのAPIです~~、部屋の投稿内容を見たり、どの部屋に誰が居るか確認したり、Hook APIと違って人間の操作と同じようなことがだいたい出来るんじゃないかな :smiling_imp:

Hook
----
Hook APIを使うと、IdobataのチャットにHookからを書き込むことが出来ます。

### エンドポイントURL
https://idobata.io/hook/YOUR_HOOK_TOKEN

YOUR_HOOK_TOKENの部分にはあなたのHook用のトークンが入ります。

### パラメータ

| name   | description |
| ------ | ----------- |
| source | Hookの投稿内容を渡します |
| format | `source`の投稿をHTMLで書きたい場合、`html`を指定します |
| image  | 画像ファイルをIdobataにアップロードし書き込む |

### 例
**挨拶をかきこむ**
```
curl --data-urlencode "source=hi" https://idobata.io/hook/YOUR_HOOK_TOKEN
```

**より力強く挨拶をかきこむ**
```
curl --data-urlencode "source=<b>hi</b>" -d format=html https://idobata.io/hook/YOUR_HOOK_TOKEN
```

Bot
---
あとで書くかもしれない書かないかもしれない
