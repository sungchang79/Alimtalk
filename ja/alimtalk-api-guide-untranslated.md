## Notification > KakaoTalk Bizmessage > お知らせトーク > API v2.2 Guide

## お知らせトーク

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
1. お知らせトーク大量送信照会、統計照会APIが追加されました。
2. 置換メッセージ送信時、buttonsフィールドが追加されました。
3. 専門メッセージ送信時、buttonsフィールドにchatExtra、chatEvent、targetフィールドが追加されました。
4. メッセージ照会時、buttonsフィールドにchatExtra、chatEvent、targetフィールドが追加されました。

## 大量送信
### 大量送信リクエストリスト照会

#### リクエスト
[URL]
```
GET /alimtalk/v2.2/appkeys/{appKey}/mass-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	説明|
|---|---|---|
|X-Secret-Key|	String|	固有のシークレットキー |

[Query parameter]
* requestIdまたはstartRequestDate + endRequestDateまたはstartCreateDate + endCreateDateは必須です。

|値|	タイプ| 最大長さ |	必須|	説明|
|---|---|---|---|---|
| requestId | String | - | O | リクエストID |
| startRequestDate | String | - | O | 送信日の開始 |
| endRequestDate | String | - | O | 送信日の終了 |
| startCreateDate |	String| - |	O |	登録日の開始 |
| endCreateDate |	String| - |	O |	登録日の終了 |
| pageNum | optional, Integer | - | X | ページ番号 |
| pageSize | optional, Integer | 1000 | X | 検索数 |

#### cURL
```
curl -X GET \
https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### レスポンス
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
        "templateCode": String,
        "masterStatusCode": String,
        "content": String,
        "buttons": [
          {
            "ordering": 1,
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
        "fileId": String,
        "templateExtra": String,
        "templateAd": String,
        "templateTitle": String,
        "templateSubtitle": String,
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

|値|	タイプ|	説明|
|---|---|---|
| header | Object |	ヘッダ領域 |
| - resultCode |	Integer |	結果コード |
| - resultMessage |	String | 結果メッセージ |
| - isSuccessful |	Boolean | 成否 |
| body | Object | 本文領域 |
| - messages | Object | メッセージリスト |
| -- requestId | String | リクエストID |
| -- requestDate | String | リクエスト日 |
| -- createDate | String | 作成日 |
| -- createUser | String | 作成日 |
| -- plusFriendId | String | プラスフレンドID |
| -- senderKey | String| 発信キー(40文字) |
| -- masterStatusCode | String | 大量送信ステータスコード(WAIT, READY, SENDREADY, SENDWAIT, SENDING, COMPLETE, CANCEL, FAIL) |
| -- content | String | 内容 |
| -- buttons | List | ボタンリスト |
| --- ordering | String | ボタンの順序 |
| --- type | String | ボタンの種類<br/> - WL：Webリンク<br/> - AL：アプリリンク<br/> - DS：配送照会<br/> - BK：Botキーワード<br/> - MD：メッセージ伝達<br/> - BC：相談トーク切り替え<br/> - BT：Bot切り替え<br/> - AC：チャンネル追加[広告追加/複合型のみ] |
| --- name | String | ボタン名 |
| --- linkMo | String | モバイルWebリンク(WLタイプの場合は必須フィールド) |
| --- linkPc | String | PC Webリンク(WLタイプの場合は任意フィールド)|
| --- schemeIos | String | IOSアプリリンク(ALタイプの場合は必須フィールド) |
| --- schemeAndroid | String | Androidアプリリンク(ALタイプの場合は必須フィールド) |
| --- chatExtra | String | BC：相談トークに切り替える時に伝達するメタ情報<br/> BT：Botに切り替える時に伝達するメタ情報 |
| --- chatEvent | String | BT：Botに切り替える時に接続するBotイベント名 |
| --- target|	String|	Webリンクボタンの場合、"target"："out"プロパティを追加時アウトリンク<br>基本アプリ内リンクで送信 |
| -- fileId | String | 添付ファイルID |
| -- templateCode |	String | テンプレートコード(最大20文字) |
| -- templateExtra | String | テンプレート付加情報(テンプレートメッセージタイプが[付加情報型/複合型]の場合は必須) |
| -- templateAd | String | テンプレート内の受信同意リクエストまたは簡単な広告文言(テンプレートメッセージタイプが[広告追加型/複合型]の場合は必須) |
| -- tempalteTitle| String | テンプレートタイトル(最大50文字、Android：2行、23文字以上省略処理、IOS：2行、27文字以上省略処理) |
| -- templateSubtitle| String | テンプレート補助文言(最大50文字、Android：18文字以上省略処理、IOS：21文字以上省略処理) |
| -- autoSendYn | String | 自動送信を行うかどうか |
| -- statsId | String | 統計ID |
| -- createDate | String | 作成日 |
| -- createUser | String | 登録者(コンソールから送信時、ユーザーUUIDに保存) |
| - totalCount | Integer | 総数


### 大量送信大量送信受信者リスト照会

#### リクエスト
[URL]
```
GET /alimtalk/v2.2/appkeys/{appKey}/mass-messages/{requestId}/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
| appKey |	String |	固有のアプリケーションキー |
| requestId |	String |	リクエストID |

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	説明|
|---|---|---|
| X-Secret-Key |	String|	固有のシークレットキー |


|値|	タイプ| 最大長さ |	必須|	説明|
|---|---|---|---|---|
| requestId | String | - | O | リクエストID |
| startRequestDate | String | - | X | 送信日の開始 |
| endRequestDate | String | - | X | 送信日の終了 |
| startCreateDate |	String| - |	X |	登録日の開始 |
| endCreateDate |	String| - |	X |	登録日の終了 |
| pageNum | optional, Integer | - | X | ページ番号 |
| pageSize | optional, Integer | 1000 | X | 検索数 |

#### cURL
```
curl -X GET \
https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### レスポンス
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

|値|	タイプ|	説明|
|---|---|---|
| header | Object |	ヘッダ領域 |
| - resultCode |	Integer |	結果コード |
| - resultMessage |	String | 結果メッセージ |
| - isSuccessful |	Boolean | 成否 |
| body | Object | 本文領域 |
| - messages | Object | メッセージリスト |
| -- requestId | String | リクエストID |
| -- recipientSeq | String | 受信者シーケンス番号 |
| -- recipientNo | String | 受信番号 |
| -- requestDate | String | リクエスト日 |
| -- receiveDate | String | 受信日 |
| -- messageStatus | String | メッセージ状態 |
| -- resultCode | String | 結果コード |
| -- resultCodeName | String | 結果コード内容 |
| - totalCount | Integer | 総数 |

### 大量送信大量送信受信者照会

#### リクエスト
[URL]
```
GET /alimtalk/v2.2/appkeys/{appKey}/mass-messages/{requestId}/recipients/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
| appKey |	String | 固有のアプリケーションキー |
| requestId |	String | リクエストID |
| recipientSeq | String | 受信者の順序 |

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	説明|
|---|---|---|
|X-Secret-Key|	String|	固有のシークレットキー |


|値|	タイプ| 最大長さ |	必須|	説明|
|---|---|---|---|---|
| requestId | String | - | O | リクエストID |
| startRequestDate | String | - | X | 送信日の開始 |
| endRequestDate | String | - | X | 送信日の終了 |
| startCreateDate |	String| - |	X |	登録日の開始 |
| endCreateDate |	String| - |	X |	登録日の終了 |
| pageNum | optional, Integer | - | X | ページ番号 |
| pageSize | optional, Integer | 1000 | X | 検索数 |

#### cURL
```
curl -X GET \
https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients/1?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### レスポンス
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
        "templateCode": String,
        "recipientNo": String,
        "content": String,
        "templateTitle": String,
        "templateSubtitle": String,
        "templateExtra": String,
        "templateAd": String,
        "requestDate": String,
        "receiveDate": String,
        "createDate": String,
        "resendStatus": String,
        "resendStatusName": String,
        "resendResultCode": String,
        "resendRequestId": String,
        "messageStatus": String,
        "resultCode": String,
        "resultCodeName": String,
        "createUser": String,
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
                "chatEvent": String
                "target": String
            }
        ],
        "messageOption": {
          "price": Integer,
          "currencyType": String
        }
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
| header | Object |	ヘッダ領域 |
| - resultCode |	Integer |	結果コード |
| - resultMessage |	String | 結果メッセージ |
| - isSuccessful |	Boolean | 成否 |
| body | Object | 本文領域 |
| - requestId | String | リクエストID |
| - recipientSeq | String | 受信者シーケンス番号 |
| - plusFriendId | String | プラスフレンドID |
| - senderKey | String | 送信者ID |
| - templateCode |	String | テンプレートコード(最大20文字) |
| - recipientNo | String | 受信番号 |
| - content | String | 内容 |
| - tempalteTitle| String | テンプレートタイトル(最大50文字、Android：2行、23文字以上省略処理、IOS：2行、27文字以上省略処理) |
| - templateSubtitle| String | テンプレート補助文言(最大50文字、Android：18文字以上省略処理、IOS：21文字以上省略処理) |
| - templateExtra | String | テンプレート付加情報(テンプレートメッセージタイプが[付加情報型/複合型]の場合は必須) |
| - templateAd | String | テンプレート内の受信同意リクエストまたは簡単な広告文言(テンプレートメッセージタイプが[広告追加型/複合型]の場合は必須) |
| - requestDate | String | リクエスト日 |
| - receiveDate | String | 受信日 |
| - createDate | String | 作成日 |
| - resendStatus | String | 代替送信ステータスコード(RSC01, RSC02, RSC03, RSC04, RSC05)<br>([[以下の代替送信ステータス表](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)]参考) |
| - resendStatusName | String | 代替送信ステータス名 |
| - resendResultCode | String | 代替送信結果コード[SMS結果コード](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api) |
| - resendRequestId | String | 代替送信リクエストID |
| - messageStatus | String | 大量受信者送信ステータスコード(READY, COMPLETED, FAILED, CANCEL) |
| - resultCode | String | 結果ステータスコード |
| - resultCodeName | String | 結果状態名 |
| - createUser | String | 作成ユーザーID |
| - buttons | List | ボタンリスト |
| -- ordering | String | ボタンの順序 |
| -- type | String | ボタンの種類<br/> - WL：Webリンク<br/> - AL：アプリリンク<br/> - DS：配送照会<br/> - BK：Botキーワード<br/> - MD：メッセージ配信<br/> - BC：相談トーク切り替え<br/> - BT：Bot切り替え<br/> - AC：チャンネル追加[広告追加/複合型のみ] |
| -- name | String | ボタン名 |
| -- linkMo | String | モバイルWebリンク(WLタイプの場合は必須フィールド) |
| -- linkPc | String | PC Webリンク(WLタイプの場合は任意フィールド)|
| -- schemeIos | String | IOSアプリリンク(ALタイプの場合は必須フィールド) |
| -- schemeAndroid | String | Androidアプリリンク(ALタイプの場合は必須フィールド) |
| -- chatExtra | String | BC：相談トークに切り替える時に伝達するメタ情報<br/> BT：Botに切り替える時に伝達するメタ情報 |
| -- chatEvent | String | BT：Botに切り替える時に接続するBotイベント名 |
| -- target|	String|	Webリンクボタンの場合、"target"："out"プロパティを追加時アウトリンク<br>基本アプリ内リンクで送信 |
| - messageOption | Boolean | メッセージオプション |
|-- price | Integer |	message(ユーザーに伝達されるメッセージ)内に含まれる価格/金額/決済金額(モーメント広告に該当) |
|-- currencyType | String |	message(ユーザーに伝達されるメッセージ)内に含まれる価格/金額/決済金額の通貨単位KRW、USD、EURなどの国際通貨コードを使用(モーメント広告に該当) |
