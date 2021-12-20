## Notification > KakaoTalk Bizmessage > Friendtalk > API v2.2 Guide

## Friendtalk

#### [API Domain]

<table>
<thead>
<tr>
<th>Domain</th>
</tr>
</thead>
<tbody>
<tr>
<td>https://api-alimtalk.cloud.toast.com</td>
</tr>
</tbody>
</table>

## Overview of v2.2 API
1. 친구톡 대량 발송 조회가 추가되었습니다.
2. 메시지 발송 시, buttons 필드에 chatExtra, chatEvent, target 필드가 추가되었습니다.
3. 메시지 조회 시, buttons 필드에 chatExtra, chatEvent, target 필드가 추가되었습니다.

## Send Messages
#### Request of Sending

[URL]

```
POST  /friendtalk/v2.2/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value  | Type   | Description     |
| ------ | ------ | --------------- |
| appkey | String | Original appkey |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

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
        "isAd": Boolean,
        "recipientGroupingKey": String
    }]
}
```

| Value                  | Type    | Required | Description                                                  |
| ---------------------- | ------- | -------- | ------------------------------------------------------------ |
| plusFriendId           | String  | O        | Plus Friend ID (up to 30 characters)                         |
| requestDate            | String  | X        | Date and time of request (yyyy-MM-dd HH:mm), to be sent immediately if field is not sent |
| senderGroupingKey      | String  | X        | Sender's grouping key (up to 100 characters)                 |
| createUser             | String  | X        | Registrant (saved as user UUID when delivered via console)   |
| recipientList          | List    | O        | List of recipients (up to 1000)                              |
| - recipientNo          | String  | O        | Recipient number                                             |
| - content              | String  | O        | Body message (up to 1000 characters)<br>Up to 400, if image is included |
| - imageSeq             | Integer | X        | Image number                                                 |
| - imageLink            | String  | X        | Image link (required, with the input of image number)        |
| - buttons              | List    | X        | Button                                                       |
| -- ordering            | Integer | X        | Button sequence (required, if there is a button)             |
| -- type                | String  | X        | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery) |
| -- name                | String  | X        | Button name (required, if there is a button)                 |
| -- linkMo              | String  | X        | Mobile web link (required for the WL type)                   |
| -- linkPc              | String  | X        | PC web link (optional for the WL type)                       |
| -- schemeIos           | String  | X        | iOS app link (required for the AL type)                      |
| -- schemeAndroid       | String  | X        | Android app link (required for the AL type)                  |
| -- chatExtra           | String  | X    | BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보 |
| -- chatEvent           | String  | X    | BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명 |
| -- target              | String  | X    |	웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| - isAd                 | Boolean | X        | Ad or not (default is true)                                  |
| - recipientGroupingKey | String  | X        | Recipient's grouping key (up to 100 characters)              |

* <b> Request date and time can be configured up to 90 days after a point of calling </b>
* <b> Delivery restricted during night (20:50~08:00 on the following day)</b>
* <b> Delivery is to be replaced by SMS, and field input must follow delivery API specifications of the SMS service (e.g. sender number registered at SMS service, 080 unsubscription, and field length restrictions) </b>
* <b> Title or message of an alternative delivery may be cut in length, if the byte size exceeds restrictions (see [[Cautions for SMS](https://docs.toast.com/en/Notification/SMS/en/api-guide/#_1)])</b>
* <b> Friendtalk ad message can be replaced by Ad SMS API, so it must be registered at the 080 Unsubscription Service to enable alternative delivery. </b>
* <b> When the resendContent field of a Friendtalk ad message is available, ad phrase for SMS Ad API is required to enable alternative delivery. (Ad) Content [Unsubscribe for Free] 080XXXXXXX </b>
* <b> When the resendContent field of a Friendtalk ad message is missing, ad phrase is automatically created with registered 080 number for unsubscription to enable alternative delivery. </b>

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/messages -d '{"plusFriendId":"@Plus Friend","requestDate":"yyyy-MM-dd HH:mm","recipientList":[{"recipientNo":"010-0000-0000","imageSeq":1,"imageLink":"https://toast.com","content":"message","buttons":[{"ordering":1,"type":"WL","name":"button1","linkMo":"https://toast.com","linkPc":"https://toast.com"}]}]}'
```

#### Response

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

| Value                   | Type    | Description                        |
| ----------------------- | ------- | ---------------------------------- |
| header                  | Object  | Header area                        |
| - resultCode            | Integer | Result code                        |
| - resultMessage         | String  | Result message                     |
| - isSuccessful          | Boolean | Successful or not                  |
| message                 | Object  | Body area                          |
| - requestId             | String  | Request ID                         |
| - senderGroupingKey     | String  | Sender's grouping key              |
| - sendResults           | Object  | Result of delivery request         |
| -- recipientSeq         | Integer | Recipient's sequence number        |
| -- recipientNo          | String  | Recipient number                   |
| -- resultCode           | Integer | Result code of delivery request    |
| -- resultMessage        | String  | Result message of delivery request |
| -- recipientGroupingKey | String  | Recipient's grouping key           |

## List Deliveries

#### Request

[URL]

```
GET  /friendtalk/v2.2/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value  | Type   | Description     |
| ------ | ------ | --------------- |
| appkey | String | Original appkey |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Query parameter] No.1 or (2, 3) is conditionally required

| Value                | Type    | Required                      | Description                                            |
| -------------------- | ------- | ----------------------------- | ------------------------------------------------------ |
| requestId            | String  | Conditionally required (no.1) | Request ID                                             |
| startRequestDate     | String  | Conditionally required (no.2) | Start date of delivery request (yyyy-MM-dd HH:mm)      |
| endRequestDate       | String  | Conditionally required (no.2) | End date of delivery request (yyyy-MM-dd HH:mm)        |
| startCreateDate      | String  | Conditionally required (no.3) | Start date of registration (mm:HH dd-MM-yyyy)          |
| endCreateDate        | String  | Conditionally required (no.3) | End date of registration (mm:HH dd-MM-yyyy)            |
| recipientNo          | String  | X                             | Recipient number                                       |
| plusFriendId         | String  | X                             | Plus Friend ID                                         |
| senderGroupingKey    | String  | X                             | Sender's grouping key                                  |
| recipientGroupingKey | String  | X                             | Recipient's grouping key                               |
| messageStatus        | String  | X                             | Request status (COMPLETED: successful, FAILED: failed) |
| resultCode           | String  | X                             | Delivery result (MRC01: successful, MRC02: failed)     |
| createUser           | String  | X                             | Registrant (saved as user UUID when delivered via console)  |
| pageNum              | Integer | X                             | Page number (default: 1)                               |
| pageSize             | Integer | X                             | Number of queries (default: 15, max: 1000)             |

#### Response
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

| Value                       | Type    | Description                                            |
| --------------------------- | ------- | ------------------------------------------------------ |
| header                      | Object  | Header area                                            |
| - resultCode                | Integer | Result code                                            |
| - resultMessage             | String  | Result message                                         |
| - isSuccessful              | Boolean | Successful or not                                      |
| messageSearchResultResponse | Object  | Body area                                              |
| - messages                  | List    | List of messages                                       |
| -- requestId                | String  | Request ID                                             |
| -- recipientSeq             | Integer | Recipient sequence number                              |
| -- plusFriendId             | String  | Plus Friend ID                                         |
| -- recipientNo              | String  | Recipient number                                       |
| -- requestDate              | String  | Date and time of request                               |
| -- createDate               | String  | Registered date and time                               |
| -- content                  | String  | Body                                                   |
| -- messageStatus            | String  | Request status (COMPLETED: successful, FAILED: failed) |
| -- resendStatus             | String  | Status code of resending                               |
| -- resendStatusName         | String  | Status code name of resending                          |
| -- resultCode               | String  | Result code of receiving                               |
| -- resultCodeName           | String  | Result code name of receiving                          |
| -- createUser               | String  | Registrant (saved as user UUID when delivered via console)  |
| -- senderGroupingKey        | String  | Sender's grouping key                                  |
| -- recipientGroupingKey     | String  | Recipient's grouping key                               |
| - totalCount                | Integer | Total count                                            |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/messages?startRequestDate=2018-05-01%2000:00&endRequestDate=2018-05-30%2023:59"
```

#### Status of Resending
| Value | Description                                      |
| ----- | ------------------------------------------------ |
| RSC01 | No target of resending                           |
| RSC02 | Target of resending (resent, if delivery fails.) |
| RSC03 | Resending underway                               |
| RSC04 | Resending successful                             |
| RSC05 | Resending failed                                 |

## Get Deliveries

#### Request

[URL]

```
GET  /friendtalk/v2.2/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value  | Type   | Description     |
| ------ | ------ | --------------- |
| appkey | String | Original appkey |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| Value        | Type    | Required | Description               |
| ------------ | ------- | -------- | ------------------------- |
| requestId    | String  | O        | Request ID                |
| recipientSeq | Integer | O        | Recipient sequence number |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
```

#### Response
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

| Value                  | Type    | Description                                                  |
| ---------------------- | ------- | ------------------------------------------------------------ |
| header                 | Object  | Header area                                                  |
| - resultCode           | Integer | Result code                                                  |
| - resultMessage        | String  | Result message                                               |
| - isSuccessful         | Boolean | Successful or not                                            |
| message                | Object  | Message                                                      |
| - requestId            | String  | Request ID                                                   |
| - recipientSeq         | Integer | Recipient sequence number                                    |
| - plusFriendId         | String  | Plus Friend ID                                               |
| - recipientNo          | String  | Recipient number                                             |
| - requestDate          | String  | Date and time of request                                     |
| - createDate           | String  | Registered date and time                                     |
| - receiveDate          | String  | Date and time of receiving                                   |
| - content              | String  | Body                                                         |
| - messageStatus        | String  | Status of request (COMPLETED: successful, FAILED: failed)    |
| - resendStatus         | String  | Status code of resending                                     |
| - resendStatusName     | String  | Status code name of resending                                |
| - resultCode           | String  | Result code of receiving                                     |
| - resultCodeName       | String  | Result code name of receiving                                |
| - createUser           | String  | Registrant (saved as user UUID when delivered via console)   |
| - imageSeq             | Integer | Image number                                                 |
| - imageName            | String  | Image name (name of uploaded file)                           |
| - imageUrl             | String  | Image URL                                                    |
| - imageLink            | String  | Image link                                                   |
| - wide                 | Boolean | Image is wide or not                                         |
| - buttons              | List    | List of buttons                                              |
| -- ordering            | Integer | Button sequence                                              |
| -- type                | String  | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery) |
| -- name                | String  | Button name                                                  |
| -- linkMo              | String  | Mobile web link (required for the WL type)                   |
| -- linkPc              | String  | PC web link (optional for the WL type)                       |
| -- schemeIos           | String  | iOS app link (required for the AL type)                      |
| -- schemeAndroid       | String  | Android app link (required for the AL type)                  |
| -- chatExtra           | String  | BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보             |
| -- chatEvent           | String  | BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명                          |
| -- target              | String  | 웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| - isAd                 | Boolean | Ad or not                                                    |
| - senderGroupingKey    | String  | Sender's grouping key                                        |
| - recipientGroupingKey | String  | Recipient's grouping key                                     |

### Query Updated Message Results

#### Request

[URL]

```
GET  /friendtalk/v2.2/appkeys/{appkey}/message-results
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value  | Type   | Decription      |
| ------ | ------ | --------------- |
| appkey | String | Original appkey |

[Header]

```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| Vaue            | Type    | Required | Description                                              |
| --------------- | ------- | -------- | -------------------------------------------------------- |
| startUpdateDate | String  | O        | Start time of querying result updates (yyyy-MM-dd HH:mm) |
| endUpdateDate   | String  | O        | End time of querying result updates (yyyy-MM-dd HH:mm)   |
| pageNum         | Integer | X        | Page number (default: 1)                                 |
| pageSize        | Integer | X        | Number of queries (default: 15)                          |

#### Response
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

| Value                       | Type    | Description                                                  |
| --------------------------- | ------- | ------------------------------------------------------------ |
| header                      | Object  | Header area                                                  |
| - resultCode                | Integer | Result code                                                  |
| - resultMessage             | String  | Result message                                               |
| - isSuccessful              | Boolean | Successful or not                                            |
| messageSearchResultResponse | Object  | Body area                                                    |
| - messages                  | List    | List of messages                                             |
| -- requestId                | String  | Request ID                                                   |
| -- recipientSeq             | Integer | Recipient's sequence number                                  |
| -- plusFriendId             | String  | Plus Friend ID                                               |
| -- recipientNo              | String  | Recipient number                                             |
| -- requestDate              | String  | Date and time of request                                     |
| -- receiveDate              | String  | Date and time of receiving                                   |
| -- content                  | String  | Body                                                         |
| -- messageStatus            | String  | Request status (COMPLETED -> successful, FAILED -> failed, CANCEL -> canceled) |
| -- resendStatus             | String  | Status code of resending                                     |
| -- resendStatusName         | String  | Status code name of resending                                |
| -- resultCode               | String  | Result code of receiving                                     |
| -- resultCodeName           | String  | Result code name of receiving                                |
| -- senderGroupingKey        | String  | Sender's grouping key                                        |
| -- recipientGroupingKey     | String  | Recipient's grouping key                                     |
| - totalCount                | Integer | Total count                                                  |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```


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

## Image Management

### Register Images
#### Request

[URL]

```
POST  /friendtalk/v2.2/appkeys/{appkey}/images
Content-Type: multipart/form-data
```

[Path parameter]

| Value  | Type   | Description     |
| ------ | ------ | --------------- |
| appkey | String | Original appkey |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Request parameter]

| Value | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| image | File | O        | Image       |
| wide  | boolean | X | Image is wide or not (Default: false) |

[Example]

```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/images" -F "image=@friend-ricecake02.jpeg"
```

#### Response
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

| Value           | Type    | Description                                |
| --------------- | ------- | ------------------------------------------ |
| header          | Object  | Header area                                |
| - resultCode    | Integer | Result code                                |
| - resultMessage | String  | Result message                             |
| - isSuccessful  | Boolean | Successful or not                          |
| image           | Object  | Body area                                  |
| - imageSeq      | Integer | Image number (to send Friendtalk messages) |
| - imageUrl      | String  | Image URL                                  |
| - imageName     | String  | Image name (name of uploaded file)         |


### Query Images
#### Request

[URL]

```
GET  /friendtalk/v2.2/appkeys/{appkey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value  | Type   | Description     |
| ------ | ------ | --------------- |
| appkey | String | Original appkey |

[Header]

```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| Value    | Type    | Required | Description                     |
| -------- | ------- | -------- | ------------------------------- |
| pageNum  | Integer | X        | Page number (default: 1)        |
| pageSize | Integer | X        | Number of queries (default: 15) |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/images?pageNum=1&pageSize=15"
```

#### Response
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

| Value           | Type    | Description                                |
| --------------- | ------- | ------------------------------------------ |
| header          | Object  | Header area                                |
| - resultCode    | Integer | Result code                                |
| - resultMessage | String  | Result message                             |
| - isSuccessful  | Boolean | Successful or not                          |
| imagesResponse  | Object  | Body area                                  |
| - image         | Object  | Body area                                  |
| -- imageSeq     | Integer | Image number (to send Friendtalk messages) |
| -- imageUrl     | String  | Image URL                                  |
| -- imageName    | String  | Image name (name of uploaded file)         |
| -- wide         | boolean |	Image is wide or not                       |
| -- createDate   | String  | Date and time of creation                  |
| - totalCount    | Integer | Total count                                |

* Response is sent in the order of latest registration.

### Delete Images
#### Request

[URL]

```
DELETE  /friendtalk/v2.2/appkeys/{appkey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value  | Type   | Description     |
| ------ | ------ | --------------- |
| appkey | String | Original appkey |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| Value    | Type   | Required | Description  |
| -------- | ------ | -------- | ------------ |
| imageSeq | String | O        | Image number |

[Example]

```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/images?imageSeq=1,2,3"
```

#### Response
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  }
}
```

| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - resultCode    | Integer | Result code       |
| - resultMessage | String  | Result message    |
| - isSuccessful  | Boolean | Successful or not |
