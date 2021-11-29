## Notification > KakaoTalk Bizmessage > Sender > API v2.0 Guide

## v2.1 API紹介
#### 改善された点
1. 発信プロフィール照会時、休眠/遮断状態フィールドが追加されました。

#### [API 도메인]

<table>
<thead>
<tr>
<th>도메인</th>
</tr>
</thead>
<tbody>
<tr>
<td>https://api-alimtalk.cloud.toast.com</td>
</tr>
</tbody>
</table>

## Sender

### Senderカテゴリーの照会

#### リクエスト
[URL]

```
GET  /alimtalk/v2.0/appkeys/{appkey}/sender/categories
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
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |

#### レスポンス
```
{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "categories" : [
  {
      "parentCode" : String,
      "depth" : Integer,
      "code" : String,
      "name" : String,
      "subCategories" : [
        {
        "parentCode" : String,
        "depth" : Integer,
        "code" : String,
        "name" : String,
        "subCategories" : [
          {
            "parentCode" : String,
            "depth" : Integer,
            "code" : String,
            "name" : String
          }
          ]
        }
      ]
    }
  ]
}
```

| 値        | タイプ | 説明 |
| ---------------- | ------- | ------- |
| header           | Object  | ヘッダ領域 |
| - resultCode     | Integer | 結果コード |
| - resultMessage  | String  | 結果メッセージ |
| - isSuccessful   | Boolean | 成否 |
| categories       | Object  | カテゴリー |
| - parentCode     | String  | 親コード |
| - depth          | Integer | カテゴリーの深さ |
| - code           | String  | カテゴリーコード |
| - name           | String  | カテゴリー名 |
| - subCategories  | Object  | サブカテゴリー |
| -- parentCode    | String  | 親コード |
| -- depth         | Integer | カテゴリーの深さ |
| -- code          | String  | カテゴリーコード |
| -- name          | String  | カテゴリー名 |
| -- subCategories | Object  | サブカテゴリー |
| --- parentCode   | String  | 親コード |
| --- depth        | Integer | カテゴリーの深さ |
| --- code         | String  | カテゴリーコード |
| --- name         | String  | カテゴリー名 |

### Senderの登録
#### リクエスト
[URL]

```
POST  /alimtalk/v2.0/appkeys/{appkey}/senders
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
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |

[Request body]

```
{
  "plusFriendId" : String,
  "phoneNo" : String,
  "categoryCode" : String
}
```

| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------- | ---- | ---------------------------------------- |
| plusFriendId | String  | O    | プラスフレンドID(最大30文字)                         |
| phoneNo      | String  | O    | 管理者の携帯電話番号(最大15桁)                       |
| categoryCode | String  | O    | カテゴリーコード(11文字)<br>カテゴリー照会APIのレスポンス参考<br>ex) 00100010001健康(001) - 病院(0001) - 総合病院(0001) |

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

| 値       | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |

### Senderトークン認証
#### リクエスト
[URL]

```
POST  /alimtalk/v2.0/appkeys/{appkey}/sender/token
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値    | タイプ | 説明 |
| ------------ | ------ | -------- |
| appkey       | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |

[Request body]

```
{
  "plusFriendId": String,
  "token" : Integer
}
```

| 値 | タイプ | 必須 | 説明                               |
| ----- | ------- | ---- | ---------------------------------------- |
| plusFriendId | String | O | プラスフレンドID |
| token | Integer | O    | 認証トークン(プラスフレンド登録API呼び出し後、カカオトークアプリで受け取った認証トークン) |

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

| 値       | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |

### Sender 削除
#### リクエスト

[URL]

```
DELETE  /alimtalk/v2.0/appkeys/{appkey}/senders/{senderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |
| senderKey | String | 発信キー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |

* 발신 프로필 삭제 시, 등록한 템플릿 데이터가 함께 삭제 됩니다.
* 발신 프로필 삭제 시, 복구가 불가능합니다.

#### レスポンス
```
{  
   "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
   }
}
```

| 値       | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |


### Sender 単件照会
#### リクエスト

[URL]

```
GET  /alimtalk/v2.0/appkeys/{appkey}/senders/{senderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |
| senderKey | String | 発信キー |

[Header]
```
{
  "X-Secret-Key": String
}
```

| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |


#### レスポンス
```
{  
   "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
   },
   "sender":{  
         "plusFriendId" : String,
         "senderKey" : String,
         "categoryCode" : String,
         "status" : String,
         "statusName" : String,
         "kakaoStatus" : String,
         "kakaoStatusName" : String,
         "kakaoProfileStatus" : String,
         "kakaoProfileStatusName" : String,
         "alimtalk": {  
                "isResend": Boolean,
                "resendSendNo": String,
                "dailyMaxCount" : Integer,
                "sentCount" : Integer
          },
         "friendtalk": {  
                "isResend": Boolean,
                "resendSendNo": String,
                "resendUnsubscribeNo": String,
                "dailyMaxCount" : Integer,
                "sentCount" : Integer
         },
         "dormant": Boolean,
         "block": Boolean,
         "createDate": String
    }
}
```

| 値                 | タイプ | 説明                               |
| ------------------------- | ------- | ---------------------------------------- |
| header                    | Object  | ヘッダ領域                            |
| - resultCode              | Integer | 結果コード                            |
| - resultMessage           | String  | 結果メッセージ                           |
| - isSuccessful            | Boolean | 成否                             |
| sender               | Object  | 発信プロフィール                            |
| - plusFriendId            | String  | プラスフレンドID                                 |
| - senderKey               | String  | 発信キー                                |
| - categoryCode            | String  | カテゴリーコード                          |
| - status                  | String  | NHN Cloudプラスフレンドステータスコード <br>(YSC02：登録待機中、YSC03：正常登録) |
| - statusName              | String  | NHN Cloudプラスフレンドステータス名(登録待機中、正常登録)           |
| - kakaoStatus             | String  | カカオプラスフレンドステータスコード<br>(A：正常、S：遮断)<br>statusがYSC02の場合、kakaoStatus null値を持ちます。 |
| - kakaoStatusName         | String  | カカオプラスフレンドステータス名(正常、遮断)<br>statusがYSC02の場合、kakaoStatusName null値を持ちます。 |
| - kakaoProfileStatus      | String  | カカオプラスフレンドプロフィールステータスコード<br>(A：有効化、B：遮断、C：無効化、D：削除E：削除処理中)<br>statusがYSC02の場合、kakaoProfileStatus null値を持ちます。 |
| - kakaoProfileStatusName  | String  | カカオプラスフレンドプロフィールステータス名(有効化、無効化、遮断、削除処理中、削除)<br>statusがYSC02の場合、kakaoProfileStatusName null値を持ちます。 |
|- alimtalk|	Object|	お知らせトーク設定情報|
|-- isResend | String  | 送信失敗設定(再送信)するかどうか                   |
|-- resendSendNo | String  | 再送信時、tc-sms発信番号              |
|-- dailyMaxCount | Integer | お知らせトークの一日最大送信件数<br>(値が0の場合、件数制限なし)    |
|-- sentCount | Integer | お知らせトークの一日送信件数<br>(値が0の場合、件数制限なし)       |
|- friendtalk|	Object|	友人トーク設定情報|
|-- isResend | String  | 送信失敗設定(再送信)するかどうか                   |
|-- resendSendNo | String  | 再送信時、tc-sms発信番号              |
|-- resendUnsubscribeNo | String |	再送信時、tc-sms 080受信拒否番号 |
|-- dailyMaxCount | Integer | カカともへのメッセージの一日最大送信件数<br>(値が0の場合、件数制限なし)    |
|-- sentCount | Integer | カカともへのメッセージの一日送信件数<br>(値が0の場合、件数制限なし)       |
|- dormant | Boolean |	発信プロフィール休眠するかどうか |
|- block | Boolean |	発信プロフィールブロックするかどうか |
| - createDate              | String  | 登録日時                            |

### Senderの照会
#### リクエスト

[URL]

```
GET  /alimtalk/v2.0/appkeys/{appkey}/senders
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
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |

[Query parameter] 1番or 2番の条件は必須

| 値           | タイプ | 必須 | 説明                               |
| ------------------- | ------- | ---- | ---------------------------------------- |
| plusFriendId        | String  | X    | プラスフレンドID                                 |
| senderKey        | String  | X    | 発信キー                                 |
| status              | String  | X    | プラスフレンドステータスコード <br>(YSC02：トークン認証待機中、YSC03：正常登録) |
| isSearchKakaoStatus | boolean | X    | カカオステータスを照会するかどうか(falseの場合、カカオステータス関連フィールド(kakaoStatus、kakaoProfileStatusなど) null値)<br>default値：true |
| pageNum        | Integer | X    | ページ番号(基本：1) |
| pageSize       | Integer | X    | 照会件数(基本：15、最大: 1000) |

#### レスポンス
```
{  
   "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
   },
   "senders":[  
      {  
         "plusFriendId" : String,
         "senderKey" : String,
         "categoryCode" : String,
         "status" : String,
         "statusName" : String,
         "kakaoStatus" : String,
         "kakaoStatusName" : String,
         "kakaoProfileStatus" : String,
         "kakaoProfileStatusName" : String,
         "createDate": String,
         "alimtalk": {  
                "isResend": Boolean,
                "resendSendNo": String,
                "dailyMaxCount" : Integer,
                "sentCount" : Integer
          },
         "friendtalk": {  
                "isResend": Boolean,
                "resendSendNo": String,
                "resendUnsubscribeNo": String,
                "dailyMaxCount" : Integer,
                "sentCount" : Integer
         },
         "dormant": Boolean,
         "block": Boolean
      }
   ],
   "totalCount": Integer
}
```

| 値                 | タイプ | 説明                               |
| ------------------------- | ------- | ---------------------------------------- |
| header                    | Object  | ヘッダ領域                            |
| - resultCode              | Integer | 結果コード                            |
| - resultMessage           | String  | 結果メッセージ                           |
| - isSuccessful            | Boolean | 成否                             |
| senders                   | List  | 発信プロフィール                            |
| - plusFriendId            | String  | プラスフレンドID                                 |
| - senderKey               | String  | 発信キー                                |
| - categoryCode            | String  | カテゴリーコード                          |
| - status                  | String  | NHN Cloudプラスフレンドステータスコード <br>(YSC02：登録待機中、YSC03：正常登録) |
| - statusName              | String  | NHN Cloudプラスフレンドステータス名(登録待機中、正常登録)           |
| - kakaoStatus             | String  | カカオプラスフレンドステータスコード<br>(A：正常、S：遮断)<br>statusがYSC02の場合、kakaoStatus null値を持ちます。 |
| - kakaoStatusName         | String  | カカオプラスフレンドステータス名(正常、遮断)<br>statusがYSC02の場合、kakaoStatusName null値を持ちます。 |
| - kakaoProfileStatus      | String  | カカオプラスフレンドプロフィールステータスコード<br>(A：有効化、B：遮断、C：無効化、D：削除E：削除処理中)<br>statusがYSC02の場合、kakaoProfileStatus null値を持ちます。 |
| - kakaoProfileStatusName  | String  | カカオプラスフレンドプロフィールステータス名(有効化、無効化、遮断、削除処理中、削除)<br>statusがYSC02の場合、kakaoProfileStatusName null値を持ちます。 |
|- alimtalk|	Object|	お知らせトーク設定情報|
|-- isResend | String  | 送信失敗設定(再送信)するかどうか                   |
|-- resendSendNo | String  | 再送信時、tc-sms発信番号              |
|-- dailyMaxCount | Integer | お知らせトークの一日最大送信件数<br>(値が0の場合、件数制限なし)    |
|-- sentCount | Integer | お知らせトークの一日送信件数<br>(値が0の場合、件数制限なし)       |
|- friendtalk|	Object|	友人トーク設定情報|
|-- isResend | String  | 送信失敗設定(再送信)するかどうか                   |
|-- resendSendNo | String  | 再送信時、tc-sms発信番号              |
|-- resendUnsubscribeNo | String |	再送信時、tc-sms 080受信拒否番号 |
|-- dailyMaxCount | Integer | カカともへのメッセージの一日最大送信件数<br>(値が0の場合、件数制限なし)    |
|-- sentCount | Integer | カカともへのメッセージの一日送信件数<br>(値が0の場合、件数制限なし)       |
|- dormant | Boolean |	発信プロフィール休眠するかどうか |
|- block | Boolean |	発信プロフィールブロックするかどうか |
| - createDate              | String  | 登録日時                            |
| totalCount                | Integer | 総個数                               |

## Sender group

### Sender group の照会

#### リクエスト
[URL]

```
GET  /alimtalk/v2.0/appkeys/{appkey}/sender-groups/{groupSenderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |
| groupSenderKey | String | Sender group 発信キー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |

#### レスポンス
```
{
    "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
    },
    "senderGroup": {
        "groupName": String,
        "senderKey": String,
        "status": String,
        "senders": [
            {
                "plusFriendId": String,
                "senderKey": String,
                "createDate": String
            }
        ],
        "createDate": String,
        "updateDate": String
    }
}
```

| 値                 | タイプ | 説明                               |
| ------------------------- | ------- | ---------------------------------------- |
| header                    | Object  | ヘッダ領域                            |
| - resultCode              | Integer | 結果コード                            |
| - resultMessage           | String  | 結果メッセージ                           |
| - isSuccessful            | Boolean | 成否                             |
|senderGroup|	Object|	発信プロフィールグループ |
|- groupName | String |	グループ名 |
|- senderKey | String |	発信キー |
| - status                  | String  | NHN Cloudプラスフレンドステータスコード <br>(YSC02：登録待機中、YSC03：正常登録) |
|- senders | List |	発信プロフィール |
|-- plusFriendId | String |	プラスフレンドID |
|-- senderKey | String |	発信キー |
|-- createDate | String | 登録日時 |
|- createDate | String | 登録日時 |
|- updateDate |	String|	変更日 |

### グループにSender追加

#### リクエスト
[URL]

```
POST  /alimtalk/v2.0/appkeys/{appkey}/sender-groups/{groupSenderKey}/senders/{senderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |
| groupSenderKey | String | Sender group 発信キー |
| senderKey | String | 発信キー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |

* グループの最大メンバー数は5000人です。

#### レスポンス
```
{  
   "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
   }
}
```

| 値       | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |

### グループにSender削除

#### リクエスト
[URL]

```
DELETE  /alimtalk/v2.0/appkeys/{appkey}/sender-groups/{groupSenderKey}/senders/{senderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |
| groupSenderKey | String | Sender group 発信キー |
| senderKey | String | 発信キー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |

#### レスポンス
```
{  
   "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
   }
}
```

| 値       | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |
