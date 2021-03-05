## Notification > KakaoTalk Bizmessage > カカともへのメッセージ > API v1.1 Guide

## カカともへのメッセージ

#### [APIドメイン]

<table>
<thead>
<tr>
<th>ドメイン</th>
</tr>
</thead>
<tbody>
<tr>
<td>https://api-alimtalk.cloud.toast.com</td>
</tr>
</tbody>
</table>

## メッセージの送信
#### 送信リクエスト

[URL]

```
POST  /friendtalk/v1.1/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./plus-friend-console-guide/#x-secret-key)] |

[Request body]

```
{
    "plusFriendId": String,
    "requestDate": String,
    "recipientList": [{
        "recipientNo": String,
        "content": String,
        "imageSeq": Integer,
        "imageLink": String,
        "buttons": [
                {
                    "ordering": Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String
                }
        ],
        "isAd": Boolean
    }]
}
```

| 値          | タイプ | 必須 | 説明                                 |
| ---------------- | ------- | ---- | ---------------------------------------- |
| plusFriendId     | String  | X    | プラスフレンドID                                 |
| requestDate      | String  | X    | リクエスト日時(yyyy-MM-dd HH:mm)、フィールドを送信しない場合、即時送信 |
| recipientList    | List    | O    | 受信者リスト(最大1000人)                         |
| - recipientNo    | String  | O    | 受信番号                              |
| - content        | String  | O    | 内容(最大1000文字)<br>イメージを含む時は、最大400文字  |
| - imageSeq       | Integer | X    | イメージ番号                             |
| - imageLink      | String  | X    | イメージリンク                            |
| - buttons        | List    | X    | ボタン                                 |
| -- ordering      | Integer | X    | ボタン順序(ボタンがある場合は必須)                      |
| -- type          | String  | X    | ボタンタイプ(WL：Webリンク、AL：アプリリンク、BK：Botキーワード、MD：メッセージ伝達) |
| -- name          | String  | X    | ボタン名(ボタンがある場合は必須)                      |
| -- linkMo        | String  | X    | モバイルWebリンク(WLタイプの場合は必須フィールド)                |
| -- linkPc        | String  | X    | PC Webリンク(WLタイプの場合は任意フィールド)                |
| -- schemeIos     | String  | X    | iOSアプリリンク(ALタイプの場合は必須フィールド)                |
| -- schemeAndroid | String  | X    | Androidアプリリンク(ALタイプの場合は必須フィールド)            |
| - isAd           | Boolean | X    | 広告かどうか(デフォルト値true)                          |

* <b>プラスフレンドIDフィールドを送信しない場合、最初に登録したプラスフレンドに送信されます。</b>
* <b>夜間送信制限(20:00～翌日08:00)</b>

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v1.1/appkeys/{appkey}/messages -d '{"plusFriendId":"@プラスフレンド","requestDate":"yyyy-MM-dd HH:mm","recipientList":[{"recipientNo":"010-0000-0000","imageSeq":1,"imageLink":"https://toast.com","content":"内容","buttons":[{"ordering":1,"type":"WL","name":"ボタン1","linkMo":"https://toast.com","linkPc":"https://toast.com"}]}]}'
```

#### レスポンス

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "message": {
    "requestId": String
  }
}
```

| 値         | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |
| message         | Object  | 本文領域 |
| - requestId     | String  | リクエストID  |

## 送信リスト照会

#### リクエスト

[URL]

```
GET  /friendtalk/v1.1/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter] 1番or 2番の条件必須

| 値          | タイプ | 必須  | 説明                          |
| ---------------- | ------- | --------- | --------------------------------- |
| requestId        | String  | 条件必須(1番) | リクエストID                             |
| startRequestDate | String  | 条件必須(2番) | 送信リクエスト日の開始値(yyyy-MM-dd HH:mm)   |
| endRequestDate   | String  | 条件必須(2番) | 送信リクエスト日の終了値(yyyy-MM-dd HH:mm)    |
| recipientNo      | String  | X         | 受信番号                       |
| plusFriendId     | String  | X         | プラスフレンドID                          |
| messageStatus    | String  | X         | リクエストステータス(COMPLETED：成功、FAILED：失敗) |
| resultCode       | String  | X         | 送信結果(MRC01：成功、MRC02：失敗)       |
| pageNum          | Integer | X         | ページ番号(基本：1)                     |
| pageSize         | Integer | X         | 照会件数(基本：15)                     |

#### レスポンス
```
{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "messageSearchResultResponse" : {
    "messages" : [
        {
          "requestId" :  String,
          "recipientSeq" : Integer,
          "plusFriendId" :  String,
          "recipientNo" :  String,
          "requestDate" : String,
          "content" :  String,
          "messageStatus" :  String,
          "resendStatus" :  String,
          "resendStatusName" :  String,
          "resultCode" :  String,
          "resultCodeName" : String,
        }
    ],
    "totalCount" :  Integer
  }
}
```

| 値                     | タイプ | 説明                          |
| --------------------------- | ------- | --------------------------------- |
| header                      | Object  | ヘッダ領域                       |
| - resultCode                | Integer | 結果コード                       |
| - resultMessage             | String  | 結果メッセージ                      |
| - isSuccessful              | Boolean | 成否                        |
| messageSearchResultResponse | Object  | 本文領域                       |
| - messages                  | List    | メッセージリスト                     |
| -- requestId                | String  | リクエストID                             |
| -- recipientSeq             | Integer | 受信者シーケンス番号(v1.1からフィールド追加)          |
| -- plusFriendId             | String  | プラスフレンドID                          |
| -- recipientNo              | String  | 受信番号                       |
| -- requestDate              | String  | リクエスト日時                       |
| -- content                  | String  | 本文                          |
| -- messageStatus            | String  | リクエストステータス(COMPLETED：成功、FAILED：失敗) |
| -- resendStatus             | String  | 再送信ステータスコード                   |
| -- resendStatusName         | String  | 再送信ステータスコード名                      |
| -- resultCode               | String  | 受信結果コード                    |
| -- resultCodeName           | String  | 受信結果コード名                       |
| - totalCount                | Integer | 総個数                            |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v1.1/appkeys/{appkey}/messages?startRequestDate=2018-05-01%2000:00&endRequestDate=2018-05-30%2023:59"
```

#### 再送信ステータス
| 値 | 説明                        |
| ----- | ------------------------------- |
| RSC01 | 再送信の対象外                   |
| RSC02 | 再送信対象(送信結果が失敗の時、再送信が行われます。) |
| RSC03 | 再送信中                      |
| RSC04 | 再送信成功                    |
| RSC05 | 再送信失敗                    |

## 送信単件照会

#### リクエスト

[URL]

```
GET  /friendtalk/v1.1/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter]

| 値      | タイプ | 必須 | 説明   |
| ------------ | ------- | ---- | ---------- |
| requestId    | String  | O    | リクエストID      |
| recipientSeq | Integer | O    | 受信者シーケンス番号 |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v1.1/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
```

#### レスポンス
```
{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "message" : {
      "requestId" :  String,
      "recipientSeq" : Integer,
      "plusFriendId" :  String,
      "recipientNo" :  String,
      "requestDate" :  String,
      "receiveDate" : String,
      "content" :  String,
      "messageStatus" :  String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "resultCode" :  String,
      "resultCodeName" : String,
      "imageSeq" : Integer,
      "imageName" : String,
      "imageUrl" : String,
      "imageLink" : String,
      "buttons" : [
        {
          "ordering" :  Integer,
          "type" :  String,
          "name" :  String,
          "linkMo" :  String,
          "linkPc": String,
          "schemeIos": String,
          "schemeAndroid": String
        }
      ],
      "isAd" : Boolean
  }
}
```

| 値            | タイプ | 説明                                 |
| ------------------ | ------- | ---------------------------------------- |
| header             | Object  | ヘッダ領域                              |
| - resultCode       | Integer | 結果コード                              |
| - resultMessage    | String  | 結果メッセージ                             |
| - isSuccessful     | Boolean | 成否                               |
| message            | Object  | メッセージ                                |
| - requestId        | String  | リクエストID                                    |
| - recipientSeq     | Integer | 受信者シーケンス番号                         |
| - plusFriendId     | String  | プラスフレンドID                                 |
| - recipientNo      | String  | 受信番号                              |
| - requestDate      | String  | リクエスト日時                              |
| - receiveDate      | String  | 受信日時                              |
| - content          | String  | 本文                                 |
| - messageStatus    | String  | リクエストステータス(COMPLETED：成功、FAILED：失敗)      |
| - resendStatus     | String  | 再送信ステータスコード                          |
| - resendStatusName | String  | 再送信ステータスコード名                             |
| - resultCode       | String  | 受信結果コード                           |
| - resultCodeName   | String  | 受信結果コード名                              |
| - imageSeq         | Integer | イメージ番号                             |
| - imageName        | String  | イメージ名(アップロードしたファイル名)                           |
| - imageUrl         | String  | イメージURL                                  |
| - imageLink        | String  | イメージリンク(イメージ番号を入力した場合は必須)                |
| - buttons          | List    | ボタンリスト                              |
| -- ordering        | Integer | ボタン順序                              |
| -- type            | String  | ボタンタイプ(WL：Webリンク、AL：アプリリンク、BK：Botキーワード、MD：メッセージ伝達) |
| -- name            | String  | ボタン名                              |
| -- linkMo          | String  | モバイルWebリンク(WLタイプの場合は必須フィールド)                |
| -- linkPc          | String  | PC Webリンク(WLタイプの場合は任意フィールド)                |
| -- schemeIos       | String  | iOSアプリリンク(ALタイプの場合は必須フィールド)                |
| -- schemeAndroid   | String  | Androidアプリリンク(ALタイプの場合は必須フィールド)            |
| - isAd             | Boolean | 広告かどうか                                   |

## イメージの管理

### イメージの登録
#### リクエスト

[URL]

```
POST  /friendtalk/v1.1/appkeys/{appkey}/images
Content-Type: multipart/form-data
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./plus-friend-console-guide/#x-secret-key)] |

[Request parameter]

| 値 | タイプ | 必須 | 説明 |
| ----- | ---- | ---- | ---- |
| image | File | O    | イメージ |

[例]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v1.1/appkeys/{appkey}/images" -F "image=@friend-ricecake02.jpeg"
```

#### レスポンス
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "image": {
      "imageSeq" : Integer,
      "imageUrl" : String,
      "imageName" : String
    }
}
```

| 値         | タイプ | 説明               |
| --------------- | ------- | ---------------------- |
| header          | Object  | ヘッダ領域            |
| - resultCode    | Integer | 結果コード            |
| - resultMessage | String  | 結果メッセージ           |
| - isSuccessful  | Boolean | 成否             |
| image           | Object  | 本文領域            |
| - imageSeq      | Integer | イメージ番号(カカともへのメッセージの送信時に使用) |
| - imageUrl      | String  | イメージURL                |
| - imageName     | String  | イメージ名(アップロードしたファイル名)         |


### イメージの照会
#### リクエスト

[URL]

```
GET  /friendtalk/v1.1/appkeys/{appkey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter]

| 値  | タイプ | 必須 | 説明      |
| -------- | ------- | ---- | ------------- |
| pageNum  | Integer | X    | ページ番号(基本：1) |
| pageSize | Integer | X    | 照会件数(基本：15) |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v1.1/appkeys/{appkey}/images?pageNum=1&pageSize=15"
```

#### レスポンス
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "imagesResponse": {
    "images" : [
        {
            "imageSeq" : Integer,
            "imageUrl" : String,
            "imageName" : String,
            "createDate" : String
        }
    ],
    "totalCount" : Integer
  }

}
```

| 値         | タイプ | 説明               |
| --------------- | ------- | ---------------------- |
| header          | Object  | ヘッダ領域            |
| - resultCode    | Integer | 結果コード            |
| - resultMessage | String  | 結果メッセージ           |
| - isSuccessful  | Boolean | 成否             |
| imagesResponse  | Object  | 本文領域            |
| - image         | Object  | 本文領域            |
| -- imageSeq     | Integer | イメージ番号(カカともへのメッセージの送信時に使用) |
| -- imageUrl     | String  | イメージURL                |
| -- imageName    | String  | イメージ名(アップロードしたファイル名)         |
| -- createDate   | String  | 作成日時            |
| - totalCount    | Integer | 総個数                  |

* イメージは、最近登録した順にソートされてレスポンスを返します。

### イメージの削除
#### リクエスト

[URL]

```
DELETE  /friendtalk/v1.1/appkeys/{appkey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter]

| 値  | タイプ | 必須 | 説明 |
| -------- | ------ | ---- | ------ |
| imageSeq | String | O    | イメージ番号 |

[例]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v1.1/appkeys/{appkey}/images?imageSeq=1,2,3"
```

#### レスポンス
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  }
}
```

| 値         | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |
