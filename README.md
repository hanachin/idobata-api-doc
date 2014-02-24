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
基本的に人間が出来ることはなんでも出来ると考えている。

認証はリクエストヘッダーに`X-API-Token`というヘッダをつければよい。

### メッセージ見るAPI
Botが入っている部屋に送られてきたメッセージを取得することが出来る。

#### エンドポイントURL
https://idobata.io/api/messages/

#### パラメータ
| name       | description                                                        |
| ---------- | ------------------------------------------------------------------ |
| ids[]      | 取得するメッセージの Message ID(optional)                          |
| room_id    | メッセージを取得したい部屋のRoom ID(optional)                      |
| older_than | このMessage IDより古いメッセージを取得したいとき指定する(optional) |

#### 例
**特定のメッセージを指定せず、最新のメッセージを見る**
```
curl 'https://idobata.io/api/messages' -H "X-API-Token: YOUR_BOT_TOKEN"
```

**特定のメッセージを指定して見る**
```
curl 'https://idobata.io/api/messages?ids%5B%5D=MESSAGE_ID' -H "X-API-Token: YOUR_BOT_TOKEN"
```

**特定のメッセージを複数指定して見る**
```
curl 'https://idobata.io/api/messages?ids%5B%5D=MESSAGE_ID&ids%5B%5D=MESSAGE_ID' -H "X-API-Token: YOUR_BOT_TOKEN"
```

**特定の部屋のメッセージを見る**
```
curl 'https://idobata.io/api/messages?room_id=ROOM_ID' -H "X-API-Token: YOUR_BOT_TOKEN"
```

**特定のメッセージより古いメッセージを見る**
```
curl 'https://idobata.io/api/messages?older_than=MESSAGE_ID' -H "X-API-Token: YOUR_BOT_TOKEN"
```

### BotがBotつくるAPI
#### エンドポイントURL
https://idobata.io/api/bots

#### パラメータ
| name                 | description                           |
| -------------------- | ------------------------------------- |
| bot[name]            | Botの名前                             |
| bot[organization_id] | <del>Bot国のID</del>Organization ID   |

#### 例
**BotからBotをつくろう**
```
curl 'https://idobata.io/api/bots' -H 'Content-Type: application/x-www-form-urlencoded; charset=UTF-8' --data 'bot%5Bname%5D=CHILD_BOT_NAME&bot%5Borganization_id%5D=YOUR_ORGANIZATION_ID' -H "X-API-Token: YOUR_API_TOKEN"
```
