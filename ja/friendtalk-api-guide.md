## Notification > KakaoTalk Bizmessage > カカともへのメッセージ > API v2.2 Guide

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

## v2.2 API紹介
1. 친구톡 대량 발송 조회가 추가되었습니다.
2. 메시지 발송 시, buttons 필드에 chatExtra, chatEvent, target 필드가 추가되었습니다.
3. 메시지 조회 시, buttons 필드에 chatExtra, chatEvent, target 필드가 추가되었습니다.

## メッセージの送信
#### 送信リクエスト

[URL]

```
POST  /friendtalk/v2.2/appkeys/{appkey}/messages
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
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./sender-console-guide/#x-secret-key)] |

[Request body]

```
{
    "plusFriendId": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser" : String,
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
                    "schemeAndroid": String,
                    "chatExtra": String,
                    "chatEvent": String,
                    "target": String
                }
        ],
        "resendParameter": {
            "isResend" : boolean,
            "resendType" : String,
            "resendTitle" : String,
            "resendContent" : String,
            "resendSendNo" : String,
            "resendUnsubscribeNo": String
        },
        "isAd": Boolean,
        "recipientGroupingKey": String
    }]
}
```

| 値                | タイプ | 必須 | 説明                                 |
| ---------------------- | ------- | ---- | ---------------------------------------- |
| plusFriendId           | String  | O    | プラスフレンドID(最大30文字)                         |
| requestDate            | String  | X    | リクエスト日時(yyyy-MM-dd HH:mm)、フィールドを送信しない場合、即時送信 |
| senderGroupingKey      | String  | X    | 発信グルーピングキー(最大100文字)                        |
| createUser             | String  | X    | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存) |
| recipientList          | List    | O    | 受信者リスト(最大1000人)                         |
| - recipientNo          | String  | O    | 受信番号                              |
| - content              | String  | O    | 内容(最大1000文字)<br>イメージを含む時は最大400文字  |
| - imageSeq             | Integer | X    | イメージ番号                             |
| - imageLink            | String  | X    | イメージリンク                                |
| - buttons              | List    | X    | ボタン                                 |
| -- ordering            | Integer | X    | ボタン順序(ボタンがある場合は必須)                      |
| -- type                | String  | X    | ボタンタイプ(WL：Webリンク、AL：アプリリンク、BK：Botキーワード、MD：メッセージ伝達) |
| -- name                | String  | X    | ボタン名(ボタンがある場合は必須)                      |
| -- linkMo              | String  | X    | モバイルWebリンク(WLタイプの場合は必須フィールド)                |
| -- linkPc              | String  | X    | PC Webリンク(WLタイプの場合は任意フィールド)                |
| -- schemeIos           | String  | X    | iOSアプリリンク(ALタイプの場合は必須フィールド)                |
| -- schemeAndroid       | String  | X    | Androidアプリリンク(ALタイプの場合は必須フィールド)            |
| -- chatExtra           | String  | X    | BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보 |
| -- chatEvent           | String  | X    | BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명 |
| -- target              | String  | X    |	웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| - resendParameter      | Object  | X    | 代替発送情報 |
| -- isResend            | boolean | X    | 送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。 |
| -- resendType          | String  | X    | 代替送信タイプ(SMS、LMS)<br>値がない場合は、テンプレート本文の長さに応じてタイプが決まります。 |
| -- resendTitle         | String  | X    | LMS代替送信タイトル(最大20文字)<br>(値がない場合は、プラスフレンドIDで再送信されます。) |
| -- resendContent       | String  | X    | 代替送信内容(最大1000文字)<br>(値がない場合は、テンプレートの内容で再送信されます。) |
| -- resendSendNo        | String  | X    | 代替送信発信番号(最大13桁)<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span> |
| -- resendUnsubscribeNo | String  | X    | 代替080受信拒否番号<br><span style="color:red">(SMSサービスに登録された080の受信拒否番号がない場合、代替の転送が失敗することがあります。)</span> |
| - isAd                 | Boolean | X    | 広告かどうか(デフォルト値true)                          |
| - recipientGroupingKey | String  | X    | 受信者グルーピングキー(最大100文字)                       |

* <b>リクエスト日時は呼び出す時点から90日後まで設定可能です。</b>
* <b>夜間送信制限(20:00～翌日08:00)</b>
* <b>SMSサービスの代替として送信されるため、SMSサービスの発送API明細に従ってフィールドを入力してください。(SMSサービスに登録された発信番号、080受信拒否番号、各種フィールドの長さ制限など)</b>
* <b>指定した代替発送タイプのバイトの制限を超える代替発送のタイトルや内容はカットされ、代替発送となることがあります。([[SMS注意事項](https://docs.toast.com/ja/Notification/SMS/ja/api-guide/#_1)] 参照)</b>
* <b>友達トークの広告メッセージは、広告SMS APIに代替送信されるので、必ず080受信拒否番号を登録しないと代替送信されません。</b>
* <b>友達トーク広告メッセージのresendContentフィールドを入力する場合、SMS広告APIの広告文句を必須入力すると正常的に代替して送信されます。(広告)内容[無料受信拒否]080XXXXXX</b>
* <b>友達トーク広告メッセージのresendContentフィールドがない場合は、登録された080受信拒否番号で広告メッセージを自動生成して代替送信されます。</b>

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/messages -d '{"plusFriendId":"@プラスフレンド","requestDate":"yyyy-MM-dd HH:mm","recipientList":[{"recipientNo":"010-0000-0000","imageSeq":1,"imageLink":"https://toast.com","content":"内容","buttons":[{"ordering":1,"type":"WL","name":"ボタン1","linkMo":"https://toast.com","linkPc":"https://toast.com"}]}]}'
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
    "requestId": String,
    "senderGroupingKey": String,
    "sendResults": [
      {
        "recipientSeq": Integer,
        "recipientNo": String,
        "resultCode": Integer,
        "resultMessage": String,
        "recipientGroupingKey": String
      }
    ]
  }
}
```

| 値                 | タイプ | 説明     |
| ----------------------- | ------- | ------------ |
| header                  | Object  | ヘッダ領域  |
| - resultCode            | Integer | 結果コード  |
| - resultMessage         | String  | 結果メッセージ |
| - isSuccessful          | Boolean | 成否   |
| message                 | Object  | 本文領域  |
| - requestId             | String  | リクエストID        |
| - senderGroupingKey     | String  | 発信グルーピングキー   |
| - sendResults           | Object  | 送信リクエスト結果 |
| -- recipientSeq         | Integer | 受信者シーケンス番号 |
| -- recipientNo          | String  | 受信番号  |
| -- resultCode           | Integer | 送信リクエスト結果コード |
| -- resultMessage        | String  | 送信リクエスト結果メッセージ |
| -- recipientGroupingKey | String  | 受信者グルーピングキー  |

## 送信リスト照会

#### リクエスト

[URL]

```
GET  /friendtalk/v2.2/appkeys/{appkey}/messages
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
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./sender-console-guide/#x-secret-key)] |

[Query parameter] 1番or (2番, 3番)の条件必須

| 値              | タイプ | 必須  | 説明                          |
| -------------------- | ------- | --------- | --------------------------------- |
| requestId            | String  | 条件必須(1番) | リクエストID                             |
| startRequestDate     | String  | 条件必須(2番) | 送信リクエスト日の開始値(yyyy-MM-dd HH:mm)   |
| endRequestDate       | String  | 条件必須(2番) | 送信リクエスト日の終了値(yyyy-MM-dd HH:mm)    |
| startCreateDate      | String  | 条件必須(3番) | 登録日 開始値(yyy-MM-dd HH:mm)                   |
| endCreateDate        | String  | 条件必須(3番) | 登録日 終値(yyy-MM-dd HH:mm)                    |
| recipientNo          | String  | X         | 受信番号                       |
| plusFriendId         | String  | X         | プラスフレンドID                          |
| senderGroupingKey    | String  | X         | 発信グルーピングキー                        |
| recipientGroupingKey | String  | X         | 受信者グルーピングキー                       |
| messageStatus        | String  | X         | リクエストステータス(COMPLETED：成功、FAILED：失敗) |
| resultCode           | String  | X         | 送信結果(MRC01：成功、MRC02：失敗)       |
| createUser           | String  | X         | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存) |
| pageNum              | Integer | X         | ページ番号(基本：1)                     |
| pageSize             | Integer | X         | 照会件数(基本：15, 最大 : 1000)                     |

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
          "createDate" : String,
          "content" :  String,
          "messageStatus" :  String,
          "resendStatus" :  String,
          "resendStatusName" :  String,
          "resultCode" :  String,
          "resultCodeName" : String,
          "createUser" : String,
          "senderGroupingKey": String,
          "recipientGroupingKey": String
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
| -- recipientSeq             | Integer | 受信者シーケンス番号                  |
| -- plusFriendId             | String  | プラスフレンドID                          |
| -- recipientNo              | String  | 受信番号                       |
| -- requestDate              | String  | リクエスト日時                       |
| -- createDate               | String  | 登録日時                             |
| -- content                  | String  | 本文                          |
| -- messageStatus            | String  | リクエストステータス(COMPLETED：成功、FAILED：失敗) |
| -- resendStatus             | String  | 再送信ステータスコード                   |
| -- resendStatusName         | String  | 再送信ステータスコード名                      |
| -- resultCode               | String  | 受信結果コード                    |
| -- resultCodeName           | String  | 受信結果コード名                       |
| -- createUser               | String  | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存)|
| -- senderGroupingKey        | String  | 発信グルーピングキー                        |
| -- recipientGroupingKey     | String  | 受信者グルーピングキー                       |
| - totalCount                | Integer | 総個数                            |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/messages?startRequestDate=2018-05-01%2000:00&endRequestDate=2018-05-30%2023:59"
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
GET  /friendtalk/v2.2/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
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
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| 値      | タイプ | 必須 | 説明   |
| ------------ | ------- | ---- | ---------- |
| requestId    | String  | O    | リクエストID      |
| recipientSeq | Integer | O    | 受信者シーケンス番号 |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
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
      "createDate" : String,
      "receiveDate" : String,
      "content" :  String,
      "messageStatus" :  String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "resendResultCode" : String,
      "resendRequestId" : String,
      "resultCode" :  String,
      "resultCodeName" : String,
      "createUser" : String,
      "imageSeq" : Integer,
      "imageName" : String,
      "imageUrl" : String,
      "imageLink" : String,
      "wide" : Boolean,
      "buttons" : [
        {
          "ordering" :  Integer,
          "type" :  String,
          "name" :  String,
          "linkMo" :  String,
          "linkPc": String,
          "schemeIos": String,
          "schemeAndroid": String,
          "chatExtra": String,
          "chatEvent": String,
          "target": String
        }
      ],
      "isAd" : Boolean,
      "senderGroupingKey": String,
      "recipientGroupingKey": String
  }
}
```

| 値                | タイプ | 説明                                 |
| ---------------------- | ------- | ---------------------------------------- |
| header                 | Object  | ヘッダ領域                              |
| - resultCode           | Integer | 結果コード                              |
| - resultMessage        | String  | 結果メッセージ                             |
| - isSuccessful         | Boolean | 成否                               |
| message                | Object  | メッセージ                                |
| - requestId            | String  | リクエストID                                    |
| - recipientSeq         | Integer | 受信者シーケンス番号                         |
| - plusFriendId         | String  | プラスフレンドID                                 |
| - recipientNo          | String  | 受信番号                              |
| - requestDate          | String  | リクエスト日時                              |
| - createDate           | String  | 登録日時                             |
| - receiveDate          | String  | 受信日時                              |
| - content              | String  | 本文                                 |
| - messageStatus        | String  | リクエストステータス(COMPLETED：成功、FAILED：失敗)      |
| - resendStatus         | String  | 再送信ステータスコード                          |
| - resendStatusName     | String  | 再送信ステータスコード名                             |
| - resultCode           | String  | 受信結果コード                           |
| - resultCodeName       | String  | 受信結果コード名                              |
| - createUser           | String  | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存)|
| - imageSeq             | Integer | イメージ番号                             |
| - imageName            | String  | イメージ名(アップロードしたファイル名)                           |
| - imageUrl             | String  | イメージURL                                  |
| - imageLink            | String  | イメージリンク(イメージ番号を入力した場合は必須)                |
| - wide                 | Boolean | ワイドイメージの可否                        |
| - buttons              | List    | ボタンリスト                              |
| -- ordering            | Integer | ボタン順序                              |
| -- type                | String  | ボタンタイプ(WL：Webリンク、AL：アプリリンク、BK：Botキーワード、MD：メッセージ伝達) |
| -- name                | String  | ボタン名                              |
| -- linkMo              | String  | モバイルWebリンク(WLタイプの場合は必須フィールド)                |
| -- linkPc              | String  | PC Webリンク(WLタイプの場合は任意フィールド)                |
| -- schemeIos           | String  | iOSアプリリンク(ALタイプの場合は必須フィールド)                |
| -- schemeAndroid       | String  | Androidアプリリンク(ALタイプの場合は必須フィールド)            |
| -- chatExtra           | String  | BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보 |
| -- chatEvent           | String  | BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명 |
| -- target              | String  | 웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| - isAd                 | Boolean | 広告かどうか                                   |
| - senderGroupingKey    | String  | 発信グルーピングキー                               |
| - recipientGroupingKey | String  | 受信者グルーピングキー                              |

## メッセージ
### メッセージ送信取消

#### リクエスト

[URL]

```
DELETE  /friendtalk/v2.2/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値  | タイプ | 説明 |
| --------- | ------ | ------ |
| appkey    | String | 固有のアプリケーションキー |
| requestId | String | リクエストID  |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値     | タイプ | 必須 | 説明                                |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| 値     | タイプ | 必須 | 説明                                |
| ------------ | ------ | ---- | ---------------------------------------- |
| recipientSeq | String | X    | 受信者シーケンス番号<br>(入力しない場合、リクエストIDのすべての送信件をキャンセル) |

* 一般/認証メッセージは同じAPIでキャンセルできます。

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

| 値        | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |

[例]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

### メッセージ結果アップデート照会

#### リクエスト

[URL]

```
GET  /friendtalk/v2.2/appkeys/{appkey}/message-results
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
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| 値         | タイプ | 必須 | 説明                           |
| --------------- | ------- | ---- | ---------------------------------- |
| startUpdateDate | String  | O    | 結果アップデート照会の開始時間(yyyy-MM-dd HH:mm) |
| endUpdateDate   | String  | O    | 結果アップデート照会の終了時間(yyyy-MM-dd HH:mm) |
| pageNum         | Integer | X    | ページ番号(基本：1)                      |
| pageSize        | Integer | X    | 照会件数(基本：15, 最大 : 1000)          |

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
      "requestDate" :  String,
      "receiveDate" : String,
      "content" :  String,
      "messageStatus" :  String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "resultCode" :  String,
      "resultCodeName" : String,
      "senderGroupingKey": String,
      "recipientGroupingKey": String
    }
    ],
    "totalCount" :  Integer
  }
}
```

| 値                     | タイプ | 説明                                 |
| --------------------------- | ------- | ---------------------------------------- |
| header                      | Object  | ヘッダ領域                              |
| - resultCode                | Integer | 結果コード                              |
| - resultMessage             | String  | 結果メッセージ                             |
| - isSuccessful              | Boolean | 成否                               |
| messageSearchResultResponse | Object  | 本文領域                              |
| - messages                  | List    | メッセージリスト                            |
| -- requestId                | String  | リクエストID                                    |
| -- recipientSeq             | Integer | 受信者シーケンス番号                         |
| -- plusFriendId             | String  | プラスフレンドID                                 |
| -- recipientNo              | String  | 受信番号                              |
| -- requestDate              | String  | リクエスト日時                              |
| -- receiveDate              | String  | 受信日時                              |
| -- content                  | String  | 本文                                 |
| -- messageStatus            | String  | リクエストステータス(COMPLETED -> 成功、FAILED -> 失敗、CANCEL -> キャンセル) |
| -- resendStatus             | String  | 再送信ステータスコード                          |
| -- resendStatusName         | String  | 再送信ステータスコード名                             |
| -- resultCode               | String  | 受信結果コード                           |
| -- resultCodeName           | String  | 受信結果コード名                              |
| -- senderGroupingKey        | String  | 発信グルーピングキー                               |
| -- recipientGroupingKey     | String  | 受信者グルーピングキー                              |
| - totalCount                | Integer | 総個数                                    |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

## 대량 발송
### 대량 발송 요청 목록 조회

#### 요청
[URL]
```
GET /friendtalk/v2.2/appkeys/{appKey}/mass-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
| appKey | String | 고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름 |	타입|	설명|
|---|---|---|
| X-Secret-Key | String | 고유의 비밀 키 |

[Query parameter]
* requestId 또는 startRequestDate + endRequestDate 또는 startCreateDate + endCreateDate는 필수입니다.

| 이름 |	타입| 최대 길이 |	필수|	설명|
|---|---|---|---|---|
| requestId | String | - | O | 요청 ID |
| startRequestDate | String | - | O | 발송 날짜 시작 |
| endRequestDate | String | - | O | 발송 날짜 종료 |
| startCreateDate |	String| - |	O |	등록 날짜 시작 |
| endCreateDate |	String| - |	O |	등록 날짜 종료 |
| pageNum | optional, Integer | - | X | 페이지 번호 |
| pageSize | optional, Integer | 1000 | X | 검색 수 |

#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### 응답
```
{
    "header": {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
    },
    "body": {
        "messages": [
            {
                "requestId": String,
                "requestDate": String,
                "plusFriendId": String,
                "senderKey": String,
                "masterStatusCode": String,
                "content": String,
                "buttons": [
                    {
                        "ordering": Integer,
                        "type": String,
                        "name": String,
                        "linkMo": String,
                        "linkPc": String,
                        "schemeIos": String,
                        "schemeAndroid": String,
                        "chatExtra": String,
                        "chatEvent": String,
                        "target": String
                    }
                ],
                "isAd": Boolean,
                "imageSeq": Integer,
                "imageLink": String,
                "fileId": String,
                "autoSendYn": String,
                "statsId": String,
                "createDate": String,
                "createUser": String
            }
        ],
        "totalCount": Integer
    }
}
```

| 이름 |	타입|	설명|
|---|---|---|
| header | Object |	헤더 영역 |
| - resultCode |	Integer |	결과 코드 |
| - resultMessage |	String | 결과 메시지 |
| - isSuccessful |	Boolean | 성공 여부 |
| body | Object | 본문 영역 |
| - messages | Object | 메시지 리스트 |
| -- requestId | String | 요청 ID |
| -- requestDate | String | 요청 날짜 |
| -- plusFriendId | String | 플러스 친구 ID |
| -- senderKey | String | 전송자 ID |
| -- masterStatusCode | String | 대량 발송 상태 코드 (WAIT, READY, SENDREADY, SENDWAIT, SENDING, COMPLETE, CANCEL, FAIL) |
| -- content | String | 내용 |
| -- buttons | List | 버튼 순서 |
| --- ordering | String | 버튼 순서 |
| --- type | String | 버튼 종류<br/> - WL: 웹링크<br/> - AL: 앱링크<br/> - DS: 배송 조회<br/> - BK: 봇 키워드<br/> - MD: 메시지 전달<br/> - BC: 상담톡 전환<br/> - BT: 봇 전환<br/> - AC: 채널 추가[광고 추가/복합형만] |
| --- name | String | 버튼 이름 |
| --- linkMo | String | 모바일 웹 링크 (WL 타입일 경우 필수 필드) |
| --- linkPc | String | PC 웹 링크  (WL 타입일 경우 선택 필드)|
| --- schemeIos | String | IOS 앱 링크 (AL 타입일 경우 필수 필드) |
| --- schemeAndroid | String | Android 앱 링크 (AL 타입일 경우 필수 필드) |
| --- chatExtra | String | BC: 상담톡 전환시 전달할 메타 정보<br/> BT: 봇 전환 시 전달할 메타 정보 |
| --- chatEvent | String | BT: 봇 전환 시 연결할 봇 이벤트명 |
| --- target|	String|	웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| -- isAd | Boolean | 광고 여부 |
| -- imageSeq | Integer | 이미지 순서 |
| -- imageLink | Boolean | 이미지 URL |
| -- fileId | String | 첨부 파일 ID |
| -- autoSendYn | String | 자동 발송 여부 |
| -- statsId | String | 통계 ID |
| -- createDate | String | 생성 날짜 |
| -- createUser | String | 등록자(콘솔에서 발송 시 사용자 UUID로 저장) |
| - totalCount | Integer | 총 개수 |


### 대량 발송 수신자 목록 조회

#### 요청
[URL]
```
GET /friendtalk/v2.2/appkeys/{appKey}/mass-messages/{requestId}/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
| appKey |	String |	고유의 앱키 |
| requestId |	String |	요청 ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름 |	타입|	설명|
|---|---|---|
| X-Secret-Key | String | 고유의 비밀 키 |


| 이름 |	타입| 최대 길이 |	필수|	설명|
|---|---|---|---|---|
| requestId | String | - | O | 요청 ID |
| startRequestDate | String | - | X | 발송 날짜 시작 |
| endRequestDate | String | - | X | 발송 날짜 종료 |
| startCreateDate |	String| - |	X |	등록 날짜 시작 |
| endCreateDate |	String| - |	X |	등록 날짜 종료 |
| pageNum | optional, Integer | - | X | 페이지 번호 |
| pageSize | optional, Integer | 1000 | X | 검색 수 |

#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### 응답
```
{
    "header": {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
    },
    "body": {
        "recipients": [
            {
                "requestId": String,
                "recipientSeq": Integer,
                "recipientNo": String,
                "requestDate": String,
                "receiveDate": String,
                "messageStatus": String,
                "resultCode": String,
                "resultCodeName": String
            }
        ],
        "totalCount": Integer
    }
}
```

| 이름 | 타입| 설명 |
|---|---|---|
| header | Object |	헤더 영역 |
| - resultCode |	Integer |	결과 코드 |
| - resultMessage |	String | 결과 메시지 |
| - isSuccessful |	Boolean| 성공 여부 |
| body | Object | 본문 영역 |
| - recipients | List | 수신자 리스트 |
| -- requestId | String | 요청 ID |
| -- recipientSeq | Integer | 수신자 시퀀스 번호 |
| -- recipientNo | String | 수신 번호 |
| -- requestDate | String | 요청 날짜 |
| -- receiveDate | String | 수신 날짜 |
| -- messageStatus | String | 대량 수신자 발송 상태 코드 (READY, COMPLETED, FAILED, CANCEL) |
| -- resultCode | String | 수신 결과 코드 |
| -- resultCodeName | String | 수신 결과 코드명 |
| - totalCount | Integer | 총 개수 |

### 대량 발송 수신자 조회

#### 요청
[URL]
```
GET /friendtalk/v2.2/appkeys/{appKey}/mass-messages/{requestId}/recipients/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
| appKey |	String | 고유의 앱키 |
| requestId |	String | 요청 ID |
| recipientSeq | String | 수신자 순서 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름 |	타입|	설명|
|---|---|---|
|X-Secret-Key|	String|	고유의 비밀 키 |


| 이름 |	타입| 최대 길이 |	필수|	설명|
|---|---|---|---|---|
| requestId | String | - | O | 요청 ID |
| startRequestDate | String | - | X | 발송 날짜 시작 |
| endRequestDate | String | - | X | 발송 날짜 종료 |
| startCreateDate |	String| - |	X |	등록 날짜 시작 |
| endCreateDate |	String| - |	X |	등록 날짜 종료 |


#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients/${RECIPIENT_SEQ}?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### 응답
```
{
    "header": {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
    },
    "body": {
        "requestId": String,
        "recipientSeq": Integer,
        "plusFriendId": String,
        "senderKey": String,
        "recipientNo": String,
        "requestDate": String,
        "receiveDate": String,
        "content": String,
        "messageStatus": String,
        "resendStatus": String,
        "resendStatusName": String,
        "resendRequestId": String,
        "resendResultCode": String,
        "resultCode": String,
        "resultCodeName": String,
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
                "schemeAndroid": String,
                "chatExtra": String,
                "chatEvent": String,
                "target": String
            }
        ],
        "isAd": Boolean,
        "createDate": String
    }
}
```

| 이름 |	타입|	설명|
|---|---|---|
| header | Object |	헤더 영역 |
| - resultCode |	Integer |	결과 코드 |
| - resultMessage |	String | 결과 메시지 |
| - isSuccessful |	Boolean| 성공 여부 |
| body | Object | 본문 영역 |
| - requestId | String | 요청 ID |
| - recipientSeq | Integer | 수신자 시퀀스 번호 |
| - plusFriendId | String | 플러스 친구 ID |
| - senderKey | String | 발신 키(40자)|
| - recipientNo | String | 수신 번호 |
| - requestDate | String | 요청 날짜 |
| - receiveDate | String | 수신 날짜 |
| - content | String | 본문 |
| - messageStatus | String | 대량 수신자 발송 상태 코드 (READY, COMPLETED, FAILED, CANCEL) |
| - resendStatus | String |	대체 발송 상태 코드 (RSC01, RSC02, RSC03, RSC04, RSC05)<br>([[아래 대체 발송 상태 표](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] 참고) |
| - resendStatusName | String |	대체 발송 상태 코드명 |
| - resendRequestId | String | 대체 발송 SMS 요청 ID |
| - resendResultCode | String | 대체 발송 결과 코드 [SMS 결과 코드](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api) |
| - resultCode | String |	수신 결과 코드 |
| - resultCodeName | String |	수신 결과 코드명 |
| - imageSeq | Integer | 이미지 순서 |
| - imageLink | Integer | 이미지 URL |
| - buttons | List | 버튼 순서 |
| -- ordering | String | 버튼 순서 |
| -- type | String | 버튼 종류<br/> - WL: 웹링크<br/> - AL: 앱링크<br/> - DS: 배송 조회<br/> - BK: 봇 키워드<br/> - MD: 메시지 전달<br/> - BC: 상담톡 전환<br/> - BT: 봇 전환<br/> - AC: 채널 추가[광고 추가/복합형만] |
| -- name | String | 버튼 이름 |
| -- linkMo | String | 모바일 웹 링크 (WL 타입일 경우 필수 필드) |
| -- linkPc | String | PC 웹 링크  (WL 타입일 경우 선택 필드)|
| -- schemeIos | String | IOS 앱 링크 (AL 타입일 경우 필수 필드) |
| -- schemeAndroid | String | Android 앱 링크 (AL 타입일 경우 필수 필드) |
| -- chatExtra | String | BC: 상담톡 전환시 전달할 메타 정보<br/> BT: 봇 전환 시 전달할 메타 정보 |
| -- chatEvent | String | BT: 봇 전환 시 연결할 봇 이벤트명 |
| -- target|	String|	웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| - isAd | Boolean | 광고 여부 |
| - createDate | String | 생성 날짜 |


## イメージの管理

### イメージの登録
#### リクエスト

[URL]

```
POST  /friendtalk/v2.2/appkeys/{appkey}/images
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
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./sender-console-guide/#x-secret-key)] |

[Request parameter]

| 値 | タイプ | 必須 | 説明 |
| ----- | ---- | ---- | ---- |
| image | File | O    | イメージ |
| wide  | boolean | X | ワイドイメージの可否 (基本: false) |

[例]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/images" -F "image=@friend-ricecake02.jpeg"
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
GET  /friendtalk/v2.2/appkeys/{appkey}/images
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
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| 値  | タイプ | 必須 | 説明      |
| -------- | ------- | ---- | ------------- |
| pageNum  | Integer | X    | ページ番号(基本：1) |
| pageSize | Integer | X    | 照会件数(基本：15) |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/images?pageNum=1&pageSize=15"
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
            "wide": String,
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
| -- wide         | boolean |	ワイドイメージの可否                       |
| -- createDate   | String  | 作成日時            |
| - totalCount    | Integer | 総個数                  |

* イメージは、最近登録した順にソートされてレスポンスを返します。

### イメージの削除
#### リクエスト

[URL]

```
DELETE  /friendtalk/v2.2/appkeys/{appkey}/images
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
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| 値  | タイプ | 必須 | 説明 |
| -------- | ------ | ---- | ------ |
| imageSeq | String | O    | イメージ番号 |

[例]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/images?imageSeq=1,2,3"
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


## 代替送信管理
### SMS AppKey 登録

[URL]

```
POST  /friendtalk/v2.2/appkeys/{appkey}/failback/appkey
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値     | タイプ | 説明 |
| ------------ | ------ | -------- |
| appkey       | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値     | タイプ | 必須 | 説明                                |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |


[Request body]

```
{
    "resendAppKey": String
}
```

| 値     | タイプ | 必須 | 説明                                |
|---|---|---|---|
|resendAppKey|	String|	O | 代替発送に設定するSMS AppKey |

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
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

### 代替送信設定登録

[URL]

```
POST  /friendtalk/v2.2/appkeys/{appkey}/failback
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値     | タイプ | 説明 |
| ------------ | ------ | -------- |
| appkey       | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```

| 値     | タイプ | 必須 | 説明                                |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |


[Request body]

```
{  
   "plusFriendId": String,
   "isResend": Boolean,
   "resendSendNo": String,
   "resendUnsubscribeNo": String
}
```

| 値                | タイプ | 必須 | 説明                                 |
| ---------------------- | ------- | ---- | ---------------------------------------- |
| plusFriendId           | String  | O    | プラスフレンドID(最大30文字)                         |
| isResend             | boolean | O    | 送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。 |
| resendSendNo         | String  | O    | 代替送信発信番号(最大13桁)<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span> |

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/failback/appkey -d '{"plusFriendId": "@플러스친구","isResend": true,"resendSendNo": "01012341234", "resendUnsubscribeNo": "0801234567" }
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
