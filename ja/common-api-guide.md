## Notification > KakaoTalk Bizmessage > Common > API v2.2 Guide

## 統計

### [APIドメイン]

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


### 統計検索 - イベントベース
* イベント発生時間基準で収集された統計です。
* 次の時間を基準に統計が収集されます。
    * リクエスト数(REQUESTED)：予約送信登録時間
    * 送信数(SENT)：ベンダーに送信時点(予約送信時間)
    * 成功数(RECEIVED)：送信結果成功(受信時間)
    * 失敗数(SENT_FAILED)：送信リクエスト失敗or送信結果失敗時点
    * 代替送信リクエスト数(RESENT)：代替送信リクエスト時点
    * 代替送信失敗数(RESENT_FAILED)：代替送信リクエスト失敗時点

### 統計情報照会

[URL]

|Http method|	URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/stats |

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
| appKey | String | 固有のアプリケーションキー |

[Query parameter]

|値| タイプ | 最大長さ | 必須 | 説明 |
|---|---|---|---|---|
| statsType | String | - | 必須 | 統計区分<br/>NORMAL：基本、MINUTELY：分別、HOURLY：時間別、DAILY：日別、BY_DAY：時間別、DAY：曜日別 |
| from | String | - | 必須 | 統計検索開始日<br/>yyyy-MM-dd HH:mm:ss | 
| to | String | - | 必須 | 統計検索終了日<br/>yyyy-MM-dd HH:mm:ss |
| join | Boolean | - | オプション | 統計データ照会時、ツリー形式で提供するか設定 |
| extra1s | List<String> | - | オプション | 下位商品区分<br/> ALIMTALK、ALIMTALK_AUTH、FRIENDTALK |
| extra2s | List<String> | - | オプション | senderKey |
| eventTypes | List<String> | - | オプション | イベント種類<br/> REQUESTED、SENT、RECEIVED、SENT_FAILED、RESENT、RESENT_FAILED |
| eventCategory | String | - | オプション | イベントリスト(現在`MESSAGE`のみサポート)<br/> MESSAGE |
| templateCodes | List<String> | - | オプション | テンプレートコードリスト(カカともへのメッセージ未サポート) |
| requestIds | List<String> | 5 | オプション | リクエストIDリスト |
| statsIds | List<String> | - | オプション | 統計IDリスト |
| statsCriteria | List<String> | - | オプション | 統計基準<br/>- EVENT：イベント(基本値)<br/>- EXTRA_1、EVENT：下位商品区分、イベント<br/>- EXTRA_2、EVENT：senderKey、イベント |

[Response body]
```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "stats": [
         {
            "eventDateTime": "2021-10-13T00:00:00.000Z",
            "events": {
                "RECEIVED": 5,
                "RESENT_FAILED": 0,
                "SENT_FAILED": 0,
                "REQUESTED": 1,
                "RESENT": 0,
                "SENT": 5
            }
        },
        {
            "eventDateTime": "2021-10-14T00:00:00.000Z",
            "events": {
                "RECEIVED": 25,
                "RESENT_FAILED": 1,
                "SENT_FAILED": 19,
                "REQUESTED": 55,
                "RESENT": 0,
                "SENT": 27
            }
        }
    ]
}
```

### イベント別数照会

[URL]

|Http method|	URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/stats/total |

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
| appKey | String | 固有のアプリケーションキー |

[Query parameter]

|値| タイプ | 最大長さ | 必須 | 説明 |
|---|---|---|---|---|
| statsType | String | - | 必須 | 統計区分<br/>NORMAL：基本、MINUTELY：分別、HOURLY：時間別、DAILY：日別、BY_DAY：時間別、DAY：曜日別 |
| from | String | - | 必須 | 統計検索開始日<br/>yyyy-MM-dd HH:mm:ss | 
| to | String | - | 必須 | 統計検索終了日<br/>yyyy-MM-dd HH:mm:ss |
| join | Boolean | - | オプション | 統計データ照会時、ツリー形式で提供するか設定 |
| extra1s | List<String> | - | オプション | 下位商品区分<br/> ALIMTALK、ALIMTALK_AUTH、FRIENDTALK |
| extra2s | List<String> | - | オプション | senderKey |
| eventTypes | List<String> | - | オプション | イベント種類<br/> REQUESTED、SENT、RECEIVED、SENT_FAILED、RESENT、RESENT_FAILED |
| eventCategory | String | - | オプション | イベントリスト(現在`MESSAGE`のみサポート)<br/> MESSAGE |
| templateCodes | List<String> | - | オプション | テンプレートコードリスト(カカともへのメッセージ未サポート) |
| requestIds | List<String> | 5 | オプション | リクエストIDリスト |
| statsIds | List<String> | - | オプション | 統計IDリスト |
| statsCriteria | List<String> | - | オプション | 統計基準<br/>- EVENT：イベント(基本値)<br/>- EXTRA_1、EVENT：下位商品区分、イベント<br/>- EXTRA_2、EVENT：senderKey、イベント |

[Response body]
```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "total": {
        "RECEIVED": 45,
        "REQUESTED": 56,
        "RESENT": 0,
        "RESENT_FAILED": 1,
        "SENT": 47,
        "SENT_FAILED": 19
    }
}
```
