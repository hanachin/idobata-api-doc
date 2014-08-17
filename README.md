:feelsgood: Idobata 非公式 API doc :feelsgood:
===============

[Idobata](https://idobata.io/)の非公式APIドキュメントです:metal:

:warning: 推測・憶測に基づいた嘘デタラメや信ぴょう性のない情報が書き込まれている恐れ

**TODO**: API BlueprintやRAMLで書きたいです。

Idobataには大きく分けて2つのAPIがあると考えています。
1つはHook API、もう1つはBot APIです。
Hook APIは、部屋に発言を書き込むのみ、主に通知を投稿することしかできません。
Bot APIの方は~~Bot APIからBotを作ることができるBotのためのAPIです~~、部屋の投稿内容を見たり、どの部屋に誰が居るか確認したり、Hook APIと違って人間の操作と同じようなことがだいたい出来るんじゃないかな :smiling_imp:

リバースエンジニアリング的な`curl`の結果、ユーザーモデルは`User`、Botのモデルは`Bot`、両方ともSTIで`Guy`モデルを継承しているよう。
`User`も`Bot`も同じIdobata `Guy`sだ、仲良くしよう。

Libraries
---
- [asonas/idobata-ruby](https://github.com/asonas/idobata-ruby) hook api client
- [fukayatsu/idobata_hook](https://github.com/fukayatsu/idobata_hook) hook api client
- [uPhyca/idobata4j](https://github.com/uPhyca/idobata4j)
- [hanachin/atabodi](https://github.com/hanachin/atabodi) user/bot api client :warning:very wip

Hook (Generic)
----
Hook API (`Add a Hook -> Generic`) を使うと、IdobataのチャットにHookからを書き込むことが出来ます。

### エンドポイントURL
https://idobata.io/hook/YOUR_HOOK_TOKEN

YOUR_HOOK_TOKENの部分にはあなたのHook用のトークンが入ります。

### 使い方 (公式)

````
Usage
You can post a message with source parameter like below:

$ curl --data-urlencode "source=hello, world" https://idobata.io/hook/YOUR_HOOK_TOKEN
You can also use HTML format:

$ curl --data-urlencode "source=<h1>hi</h1>" -d format=html https://idobata.io/hook/YOUR_HOOK_TOKEN

Additionally, you can post image with image parameter:

$ curl --form image=@/path/to/image.png https://idobata.io/hook/YOUR_HOOK_TOKEN5
```

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

User
---
ユーザーとしてAPIを操作するためには、ログインIDとパスワードを使ってBASIC認証をかけましょう。
基本的にBot APIとユーザーAPIのエンドポイントとパラメータは同一です。

ユーザーのみしか操作できないリソースは、エンドポイントのURLが
`https://idobata.io/api/user`からはじまります。

:warning: `/api/user`以下はBASIC認証では操作できなさそうです。(2014/8/17時点)

### 全ての部屋のメッセージを既読にするAPI

```
curl https://idobata.io/api/user/rooms/touch --basic --user 'YOUR_ID:YOUR_PASSWORD' -H 'Content-Type: application/json' -X POST
```


### 部屋のメッセージを既読にするAPI
URL中で部屋の`room_id`を指定します。

:warning: このcURL動きません

```
curl https://idobata.io/api/user/rooms/YOUR_ROOM_ID/touch --basic --user 'YOUR_ID:YOUR_PASSWORD' -H 'Content-Type: application/json' -X POST
```

Bot
---
基本的に人間が出来ることはなんでも出来ると考えている。

認証はリクエストヘッダーに`X-API-Token`というヘッダをつければよい。

Idobataでは、チャットのデータをリアルタイムでやりとりするためにPusherをつかっている。
Pusherへの接続方法について、ドキュメントはないが以下のような実装がある。参考にされたし。

- [fukayatsu/lita-idobata](https://github.com/fukayatsu/lita-idobata)
- [hanachin/ruboty-idobata](https://github.com/hanachin/ruboty-idobata)
- [idobata/hubot-idobata](https://github.com/idobata/hubot-idobata)

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
