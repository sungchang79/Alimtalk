## Notification > KakaoTalk Bizmessage > Alimtalk > API v2.2 Guide

## Alimtalk

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
1. 알림톡 대량 발송 조회, 통계 조회 API가 추가되었습니다.
2. 치환 메시지 발송 시, buttons 필드가 추가되었습니다.
3. 전문 메시지 발송 시, buttons 필드에 chatExtra, chatEvent, target 필드가 추가되었습니다.
4. 메시지 조회 시, buttons 필드에 chatExtra, chatEvent, target 필드가 추가되었습니다.


## General Messages

### Request of Sending Replaced Messages

[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/messages
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
| X-Secret-Key | String | O        | Can be created on console. [[Note](./sender-console-guide/#x-secret-key)] |

[Request body]

```
{
    "senderKey": String,
    "templateCode": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser": String,
    "recipientList": [{
        "recipientNo": String,
        "templateParameter": {
            String: String
        },
        "resendParameter": {
          "isResend" : boolean,
          "resendType" : String,
          "resendTitle" : String,
          "resendContent" : String,
          "resendSendNo" : String
        },
        "buttons": [
          {
            "ordering": Integer,
            "chatExtra": String,
            "chatEvent": String,
            "target": String
          }
        ],
        "recipientGroupingKey": String
    }],
    "messageOption": {
      "price": Integer,
      "currencyType": String
    }
}
```

| Value                  | Type    | Required | Description                                                  |
| ---------------------- | ------- | -------- | ------------------------------------------------------------ |
| senderKey              | String  | O        | Sender key                                                   |
| templateCode           | String  | O        | Registered delivery template code (up to 20 characters)      |
| requestDate            | String  | X        | Date and time of request (yyyy-MM-dd HH:mm)<br>(send immediately, if it is left blank)<br>최대 30일 이후까지 예약 가능 |
| senderGroupingKey      | String  | X        | Sender's grouping key (up to 100 characters)                 |
| createUser             | String  | X        | Registrant (saved as user UUID when delivered via console)   |
| recipientList          | List    | O        | List of recipients (up to 1000 persons)                      |
| - recipientNo          | String  | O        | Recipient number (up to 15 characters)                       |
| - templateParameter    | Object  | X        | Template parameter<br>(required, if it includes a variable to be replaced for template) |
| -- key                 | String  | X        | Replacement key (#{key})                                     |
| -- value               | String  | X        | Value which is mapped for replacement key                    |
| - resendParameter      |Object   | X        | Alternative delivery information                             |
| -- isResend            | boolean | X        | Whether to send text as alternative, if delivery fails<br/>Resent in default, if delivery failure is set on console. |
| -- resendType          | String  | X        | Alternative delivery type (SMS,LMS)<br/>Categorized by the length of template body if value is unavailable. |
| -- resendTitle         | String  | X        | Title of alternative delivery for LMS (up to 20 characters)<br/>(resent with PlusFriend ID if value is unavailable.) |
| -- resendContent       | String  | X        | Alternative delivery message (up to 1000 characters)<br/>(resent with template message if value is unavailable.) |
| -- resendSendNo        | String  | X        | Sender number for alternative delivery (up to 13 characters)<br/><span style="color:red">(Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span> |
| - recipientGroupingKey | String  | X        | Recipient grouping key (up to 100 characters)                |
| - buttons              | List    | X        | 버튼 추가 정보 |
| -- ordering            | Integer | X        |	Button sequence (required, if there is a button) |
| -- chatExtra           | String  | X        |	BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보 |
| -- chatEvent           | String  | X        |	BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명 |
| -- target              | String  | X        |	웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| messageOption          | Object  | X        | Message Option                                               |
| - price                | Integer | X        | Price/amount/payment amount included in message (message to be delivered to user)(related to moment advertisement) |
| - currencyType         | String  | X        | Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message (message to be delivered to the user) (related to moment advertisement) |

* <b>Request date and time can be set up to 90 days since a point of calling.</b>
* <b>Since alternative delivery is made in the SMS service, field values must follow the API specifications for SMS (e.g. Sender number registered at the SMS service, or restriction in the field length). </b>
* <b>The SMS Service supports international SMS only. For international receiver numbers, the resendType (alternative delivery type) must be changed to SMS to allow sending without fail. </b>
* <b>Title or content for alternative delivery that exceeds specified byte size may be cut for delivery. (see [[Caution](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] for reference)</b>

[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/messages -d '{"senderkey":"{Sender key}","templateCode":"{template code}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{recipient number}","templateParameter":{"{replaced field}":"{replacement data}"}}]}'
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
| -- recipientSeq         | Integer | Recipient sequence number          |
| -- recipientNo          | String  | Recipient number                   |
| -- resultCode           | Integer | Result code of delivery request    |
| -- resultMessage        | String  | Result message of delivery request |
| -- recipientGroupingKey | String  | Recipient's grouping key           |

### Request of Sending Full Text

[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/raw-messages
Content-Type: application/json;charset=UTF-8
```

[Path Parameter]

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

[Request Body]

```
{
    "senderKey": String,
    "templateCode": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "recipientList": [
        {
            "recipientNo": String,
            "content": String,
            "templateTitle" : String,
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
              "resendSendNo" : String
            },
            "recipientGroupingKey": String
        }
    ],
    "messageOption": {
      "price": Integer,
      "currencyType": String
    }
}
```

| Value                  | Type    | Required | Description                                                  |
| ---------------------- | ------- | -------- | ------------------------------------------------------------ |
| senderKey              | String  | O           | Sender key                                                   |
| templateCode           | String  | O        | Registered delivery template code (up to 20 characters)      |
| requestDate            | String  | X        | Date and time of request (yyyy-MM-dd HH:mm)<br/>(sent immediately if it is left blank) |
| senderGroupingKey      | String  | X        | Sender's grouping key (up to 100 characters)                 |
| recipientList          | List    | O        | List of recipients (up to 1,000 persons)                     |
| - recipientNo          | String  | O        | Recipient number (up to 15 characters)                       |
| - content              | String  | O        | Message  (up to 1000 characters)                             |
| - templateTitle        | String  | X        | Title (up to 50 characters)                                  |
| - buttons              | List    | X        | List of buttons (up to 5)                                    |
| -- ordering            | Integer | X        | Button sequence (required, if there is a button)             |
| -- type                | String  | X        | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channeld Added) |
| -- name                | String  | X        | Button name (required if there is a button, up to 14 characters) |
| -- linkMo              | String  | X        | Mobile web link (required for the WL type, up to 200 characters) |
| -- linkPc              | String  | X        | PC web link (optional for the WL type, up to 200 characters) |
| -- schemeIos           | String  | X        | iOS app link (required for the AL type, up to 200 characters) |
| -- schemeAndroid       | String  | X        | Android app link (required for the AL type, up to 200 characters) |
| -- chatExtra           | String  | X        |	BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보 |
| -- chatEvent           | String  | X        |	BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명 |
| -- target              | String  | X        |	웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| - resendParameter      |Object   | X        | Alternative delivery information                             |
| -- isResend            | boolean | X        | Whether to send text as alternative, if delivery fails <br/>Resent in default, if delivery failure is set on console. |
| -- resendType          | String  | X        | Alternative delivery type (SMS,LMS)<br/>Categorized by the length of template message, if value is unavailable. |
| -- resendTitle         | String  | X        | Title of alternative delivery for LMS (up to 20 characters)<br/>(resent with PlusFriend ID if value is unavailable.) |
| -- resendContent       | String  | X        | Alternative delivery message (up to 1000 characters)<br/>(resent template message, if value is unavailable.) |
| -- resendSendNo        | String  | X        | Sender number for alternative delivery (up to 13 characters)<br/><span style="color:red">(alternative delivery may fail, if sender number is not registered in the SMS service.)</span> |
| - recipientGroupingKey | String  | X        | Recipient's grouping key (up to 100 characters)              |
| messageOption          | Object  | X        | Message Option                                               |
| - price                | Integer | X        | Price/amount/payment amount included in message (message to be delivered to user)(related to moment advertisement) |
| - currencyType         | String  | X        | Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message (message to be delivered to the user) (related to moment advertisement) |

* <b>Enter data completed with replacement for the body and button. </b>
* **Request date and time can be set up to 90 days since a point of calling.**
* <b>Delivery is to be replaced by SMS, and field input must follow delivery API specifications of the SMS service (e.g. sender number registered at SMS service, 080 unsubscription, and field length restrictions) </b>
* <b>Only the international SMS service is supported. For an international recipient number, the resendType (alternative delivery type) must be changed to SMS to allow sending normally. </b>
* <b>Title or message of an alternative delivery may be cut in length, if the byte size exceeds restrictions (see [[Cautions for SMS](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)])</b>

[Exapmle]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/raw-messages -d '{"senderKey":"{Sender key}","templateCode":"{template code}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{recipient number}","content":"{body}","buttons":[{"ordering":"{button sequence}","type":"{button type}","name":"{button name}","linkMo":"{mobile web link}"}]}]}'
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

### List Messages

#### Request

[URL]

```
GET  /alimtalk/v2.2/appkeys/{appkey}/messages
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

[Query parameter] No. 1 or (2, 3) is conditionally required

| Value                | Type    | Required                      | Description                                                  |
| -------------------- | ------- | ----------------------------- | ------------------------------------------------------------ |
| requestId            | String  | Conditionally required (no.1) | Request ID                                                   |
| startRequestDate     | String  | Conditionally required (no.2) | Start date of delivery request (yyyy-MM-dd HH:mm)            |
| endRequestDate       | String  | Conditionally required (no.2) | End date of delivery request (yyyy-MM-dd HH:mm)              |
| startCreateDate      | String  | Conditionally required (no.3) | Start date of registration (mm:HH dd-MM-yyyy)|
| endCreateDate        | String  | Conditionally required (no.3) | End date of registration (mm:HH dd-MM-yyyy) |
| recipientNo          | String  | X                             | Recipient number                                             |
| senderKey            | String  | X                             | Sender key                                                   |
| templateCode         | String  | X                             | Template code                                                |
| senderGroupingKey    | String  | X                             | Sender's grouping key                                        |
| recipientGroupingKey | String  | X                             | Recipient's grouping key                                     |
| messageStatus        | String  | X                             | Request status (COMPLETED -> Successful, FAILED -> Failed, CANCEL -> Canceled) |
| resultCode           | String  | X                             | Delivery result (MRC01 -> Successful, MRC02 ->Failed)        |
|createUser            | String  | X                             | Registrant (saved as user UUID when delivered via console) |
| pageNum              | Integer | X                             | Page number (default: 1)                                     |
| pageSize             | Integer | X                             | Number of queries (default: 15, max : 1000)                  |

* Cannot query data requested for delivery which are dated before 90 days.
* The maximum available days for delivery request is 30 days.

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
      "senderKey"    : String,
      "templateCode" :  String,
      "recipientNo" :  String,
      "content" :  String,
      "requestDate" :  String,
      "createDate" : String,
      "receiveDate" : String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "messageStatus" :  String,
      "resultCode" :  String,
      "resultCodeName" : String,
      "createUser" : String,
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
| -- recipientSeq             | Integer | Recipient sequence number                                    |
| -- plusFriendId             | String  | PlusFriend ID                                                |
| -- senderKey                | String  | Sender Key                                                   |
| -- templateCode             | String  | Template code                                                |
| -- recipientNo              | String  | Recipient number                                             |
| -- content                  | String  | Body message                                                 |
| -- requestDate              | String  | Date and time of request                                     |
|-- createDate                | String  | Registered date and time                                     |
| -- receiveDate              | String  | Date and time of receiving                                   |
| -- resendStatus             | String  | Status code of resending                                     |
| -- resendStatusName         | String  | Status code name of resending                                |
| -- messageStatus            | String  | Request status (COMPLETED -> successful, FAILED -> failed, CANCEL -> canceled ) |
|-- createUser                | String  | Registrant (saved as user UUID when delivered via console)   |
| -- resultCode               | String  | Result code of receiving                                     |
| -- resultCodeName           | String  | Result code name of receiving                                |
| -- buttons                  | List    | List of buttons                                              |
| --- ordering                | Integer | Button sequence                                              |
| --- type                    | String  | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added) |
| --- name                    | String  | Button name                                                  |
| --- linkMo                  | String  | Mobile web link  (required for the WL type)                  |
| --- linkPc                  | String  | PC web link (optional for the WL type)                       |
| --- schemeIos               | String  | iOS app link (required for the AL type)                      |
| --- schemeAndroid           | String  | Android app link (required for the AL type)                  |
| --- chatExtra               | String  | BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보             |
| --- chatEvent               | String  | BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명                          |
| --- target                  | String  | 웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| -- senderGroupingKey        | String  | Sender's grouping key                                        |
| -- recipientGroupingKey     | String  | Recipient's grouping key                                     |
| - totalCount                | Integer | Total Count                                                  |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

#### Status of Sending SMS/LMS
| Value | Description                                      |
| ----- | ------------------------------------------------ |
| RSC01 | No target of resending                           |
| RSC02 | Target of resending (resent, if delivery fails.) |
| RSC03 | Resending                                        |
| RSC04 | Resending successful                             |
| RSC05 | Resending failed                                 |

### Get Messages

#### Request

[URL]

```
GET  /alimtalk/v2.2/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value        | Typ     | Description               |
| ------------ | ------- | ------------------------- |
| appkey       | String  | Original appkey           |
| requestId    | String  | Request ID                |
| recipientSeq | Integer | Recipient sequence number |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
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
      "senderKey"    : String,
      "templateCode" :  String,
      "recipientNo" :  String,
      "content" :  String,
      "templateTitle" : String,
      "templateSubtitle" : String,
      "templateExtra" : String,
      "templateAd" : String,
      "requestDate" :  String,
      "receiveDate" : String,
      "createDate" : String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "resendResultCode" : String,
      "resendRequestId" : String,
      "messageStatus" :  String,
      "resultCode" :  String,
      "resultCodeName" : String,
      "createUser" : String,
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
      "messageOption": {
        "price": Integer,
        "currencyType": String
      },
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
| - plusFriendId         | String  | PlusFriend ID                                                |
| - senderKey            | String  | Sender Key                                                   |
| - templateCode         | String  | Template code                                                |
| - recipientNo          | String  | Recipient number                                             |
| - content              | String  | Body message                                                 |
|- templateTitle         | String  | Template Title                                               |
|- templateSubtitle      | String  | Auxiliary Template Phrase                                    |
|- templateExtra         | String  | Additional Template Information                              |
|- templateAd            | String  | Request for consent of receiving within template or simple ad phrases  |
| - requestDate          | String  | Date and time of request                                     |
| - receiveDate          | String  | Date and time of receiving                                   |
| - createDate           | String  | Registered date and time                                     |
| - resendStatus         | String  | Status code of resending                                     |
| - resendStatusName     | String  | status code name of resending                                |
| - messageStatus        | String  | Request status (COMPLETED -> successful, FAILED -> failed, CANCEL -> cancelled ) |
| - resultCode           | String  | Result code of receiving                                     |
| - resultCodeName       | String  | Result code name of receiving                                |
| - buttons              | List    | List of buttons                                              |
| -- ordering            | Integer | Button sequence                                              |
| -- type                | String  | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK:Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added) |
| -- name                | String  | Button name                                                  |
| -- linkMo              | String  | Mobile web link (required for the WL type)                   |
| -- linkPc              | String  | PC web link (optional for the WL type)                       |
| -- schemeIos           | String  | iOS app link (required for the AL type)                      |
| -- schemeAndroid       | String  | Android app link (required for the AL type)                  |
| -- chatExtra           | String  | BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보             |
| -- chatEvent           | String  | BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명                          |
| -- target              | String  | 웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| - messageOption        | Object  | Message Option                                               |
| -- price               | Integer | Price/amount/payment amount included in message (message to be delivered to user)(related to moment advertisement) |
| -- currencyType        | String  | Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message (message to be delivered to the user) (related to moment advertisement) |
| - senderGroupingKey    | String  | Sender's grouping key                                        |
| - recipientGroupingKey | String  | Recipient grouping key                                       |

## Authentication Messages

<span id="precautions-authword"></span>
1. Guide for authentication words required to be included for Authentication Messages API

| Category | Authentication Words |
| --- | --- |
| Authentication Messages | auth, password, verif, にんしょう, 認証, 비밀번호, 인증 |

- Example 1) Delivery shall fail if the full text (including template replacement) does not include authentication words, in the request of Authentication Messages API (for emergency)
- Example 2) Validity for English words shall be checked regardless of small or capital letters


### Request of Sending Replaced Messages

[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/auth/messages
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
    "senderKey": String,
    "templateCode": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser" : String,
    "recipientList": [{
        "recipientNo": String,
        "templateParameter": {
            String: String
        },
        "resendParameter": {
          "isResend" : boolean,
          "resendType" : String,
          "resendTitle" : String,
          "resendContent" : String,
          "resendSendNo" : String
        },
        "buttons": [
          {
            "ordering": Integer,
            "chatExtra": String,
            "chatEvent": String,
            "target": String
          }
        ],
        "recipientGroupingKey": String
    }],
    "messageOption": {
      "price": Integer,
      "currencyType": String
    }
}
```

| Value                  | Type    | Required | Description                                                  |
| ---------------------- | ------- | -------- | ------------------------------------------------------------ |
| senderKey              | String  | O        | Sender Key                                                   |
| templateCode           | String  | O        | Registered delivery template code (up to 20 characters)      |
| requestDate            | String  | X        | Date of request (yyyy-MM-dd HH:mm)<br>(immediately sent, if it is left blank) |
| senderGroupingKey      | String  | X        | Sender's grouping key (up to 100 characters)                 |
| createUser             | String  | X        | Registrant (saved as user UUID when delivered via console)   |
| recipientList          | List    | O        | List of recipients (up to 1000 persons)                      |
| - recipientNo          | String  | O        | Recipient number (up to 15 characters)                       |
| - templateParameter    | Object  | X        | Template parameter<br>(required, if it includes a variable to be replaced for template) |
| -- key                 | String  | X        | Replacement key (#{key})                                     |
| -- value               | String  | X        | Value which is mapped for replacement key                    |
| - isResend             | boolean | X        | Whether to send text as alternative, if delivery fails<br>Resent in default, if delivery failure is set on console. |
| - resendType           | String  | X        | Alternative delivery type (SMS,LMS)<br>Categorized by the length of template body, if it is left blank. |
| - resendTitle          | String  | X        | Title for LMS alternative delivery (up to 20 characters)<br>(resent with PlusFriend ID if the value is left blank.) |
| - resendContent        | String  | X        | Message for alternative delivery (up to 1000 characters)<br>(resent with template message, if the value is left empty.) |
| - resendSendNo         | String  | X        | Sender number for alternative delivery (up to 13 characters)<br><span style="color:red">(if the number is not registered in SMS service, alternative delivery may fail.)</span> |
| - recipientGroupingKey | String  | X        | Recipient grouping key (up to 100 characters)                |
| - buttons              | List    | X        | 버튼 추가 정보 |
| -- ordering            | Integer | X        |	Button sequence (required, if there is a button) |
| -- chatExtra           | String  | X        |	BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보 |
| -- chatEvent           | String  | X        |	BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명 |
| -- target              | String  | X        |	웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| messageOption          | Object  | X        | Message Option                                               |
| - price                | Integer | X        | Price/amount/payment amount included in message (message to be delivered to user)(related to moment advertisement) |
| - currencyType         | String  | X        | Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message (message to be delivered to the user) (related to moment advertisement) |

* <b> Request date and time can be set up to 90 days since a point of calling. </b>

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/auth/messages -d '{"senderKey":"{Sender Key}","templateCode":"{template code}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{recipient number}","templateParameter":{"{replaced field}":"{replacement data}"}}]}'
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
| -- recipientSeq         | Integer | Recipient sequence number          |
| -- recipientNo          | String  | Recipient number                   |
| -- resultCode           | Integer | Result code of delivery request    |
| -- resultMessage        | String  | Result message of delivery request |
| -- recipientGroupingKey | String  | Recipient's grouping key           |

### Request of Sending Full Text

[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/auth/raw-messages
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

[Request Body]

```
{
    "senderKey": String,
    "templateCode": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser": String,
    "recipientList": [
        {
            "recipientNo": String,
            "content": String,
            "templateTitle" : String,
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
              "resendSendNo" : String
            },
            "recipientGroupingKey": String
        }
    ],
    "messageOption": {
      "price": Integer,
      "currencyType": String
    }
}
```

| Value                  | Type    | Required | Description                                                  |
| ---------------------- | ------- | -------- | ------------------------------------------------------------ |
| senderKey              | String  | O        | Sender Key                                                   |
| templateCode           | String  | O        | Registered delivery template code (up to 20 characters)      |
| requestDate            | String  | X        | Date and time of request (yyyy-MM-dd HH:mm)<br>(sent immediately, if it is left blank) |
| senderGroupingKey      | String  | X        | Sender's grouping key (up to 100 characters)                 |
|createUser              | String  | X        | Registrant (saved as user UUID when delivered via console)  |
| recipientList          | List    | O        | List of recipients (up to 1,000 persons)                     |
| - recipientNo          | String  | O        | Recipient number (up to 15 characters)                       |
| - content              | String  | O        | Body message (up to 1000 characters)                         |
|- templateTitle         | String  | X        | Title (up to 50 characters) |  
| - buttons              | List    | X        | List of buttons (up to 5)                                    |
| -- ordering            | Integer | X        | Button sequence (required if there a button)                 |
| -- type                | String  | X        | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added) |
| -- name                | String  | X        | Button name (required if there is a button, for up to 14 characters) |
| -- linkMo              | String  | X        | Mobile web link (required for the WL type, for up to 200 characters) |
| -- linkPc              | String  | X        | PC web link (required for the WL type, for up to 200 characters) |
| -- schemeIos           | String  | X        | iOS app link (required for the AL type, for up to 200 characters) |
| -- schemeAndroid       | String  | X        | Android app link (required for the AL type, for up to 200 characters) |
| -- chatExtra           | String  | X        | BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보             |
| -- chatEvent           | String  | X        | BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명                          |
| -- target              | String  | X        | 웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| - isResend             | boolean | X        | Whether to send text as alternative, if delivery fails<br>Resent in default, if delivery failure is set on console. |
| - resendType           | String  | X        | Alternative delivery type (SMS,LMS)<br>Categorized by the length of template body, if value is unavailable. |
| - resendTitle          | String  | X        | Title of alternative delivery for LMS (up to 20 characters)<br>(resent with PlusFriend ID, if the value is unavailable.) |
| - resendContent        | String  | X        | Alternative delivery message (up to 1000 characters)<br>(resent with template message if value is unavailable.) |
| - resendSendNo         | String  | X        | Sender number for alternative delivery (up to 13 characters)<br><span style="color:red"> (Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span> |
| - recipientGroupingKey | String  | X        | Recipient's grouping key (up to 100 characters)              |
| messageOption          | Object  | X        | Message Option                                               |
| - price                | Integer | X        | Price/amount/payment amount included in message (message to be delivered to user)(related to moment advertisement) |
| - currencyType         | String  | X        | Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message (message to be delivered to the user) (related to moment advertisement) |

* <b>Enter data completed with replacement in the body and button. </b>
* <b>Request date and time can be set up to 90 days since a point of calling. </b>

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/auth/raw-messages -d '{"senderKey":"{Sender Key}","templateCode":"{template code}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{recipient number}","content":"{body message}","buttons":[{"ordering":"{button sequence}","type":"{button type}","name":"{button name}","linkMo":"{mobile web link}"}]}]}'
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
| -- recipientSeq         | Integer | Recipient sequence number          |
| -- recipientNo          | String  | Recipient number                   |
| -- resultCode           | Integer | Result code of delivery request    |
| -- resultMessage        | String  | Result message of delivery request |
| -- recipientGroupingKey | String  | Recipient's grouping key           |

### List Messages

#### Request

[URL]

```
GET  /alimtalk/v2.2/appkeys/{appkey}/auth/messages
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

[Query parameter] No. 1 or (2, 3) is conditionally required

| Value                | Type    | Required                      | Description                                                  |
| -------------------- | ------- | ----------------------------- | ------------------------------------------------------------ |
| requestId            | String  | Conditionally required (no.1) | Request ID                                                   |
| startRequestDate     | String  | Conditionally required (no.2) | Start date of delivery request (yyyy-MM-dd HH:mm)            |
| endRequestDate       | String  | Conditionally required (no.2) | End date of delivery request (yyyy-MM-dd HH:mm)              |
|startCreateDate       | String  | Conditionally required (no.3) | Start date of registration (mm:HH dd-MM-yyyy)                |
|endCreateDate         | String  | Conditionally required (no.3) | End date of registration (mm:HH dd-MM-yyyy)                  |
| recipientNo          | String  | X                             | Recipient number                                             |
| senderKey            | String  | X                             | Sender Key                                                   |
| templateCode         | String  | X                             | Template code                                                |
| senderGroupingKey    | String  | X                             | Sender's grouping key                                        |
| recipientGroupingKey | String  | X                             | Recipient's grouping key                                     |
| messageStatus        | String  | X                             | Request status (COMPLETED -> successful, FAILED -> failed, CANCEL -> canceled ) |
| resultCode           | String  | X                             | Delivery result (MRC01 -> successful, MRC02 -> failed )      |
|createUser            | String  | X                             | Registrant (saved as user UUID when delivered via console)   |
| pageNum              | Integer | X                             | Page number (default: 1)                                     |
| pageSize             | Integer | X                             | Number of queries (default: 15, max : 1000)                  |

* Delivery request data before 90 days cannot be queried.
* Delivery can be requested within 30 days to the maximum.   

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
      "senderKey" : String,
      "templateCode" :  String,
      "recipientNo" :  String,
      "content" :  String,
      "requestDate" :  String,
      "createDate" : String,
      "receiveDate" : String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "messageStatus" :  String,
      "resultCode" :  String,
      "resultCodeName" : String,
      "createUser" : String,
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
| -- recipientSeq             | Integer | Recipient sequence number                                    |
| -- plusFriendId             | String  | PlusFriend ID                                                |
| -- senderKey                | String  | Sender Key                                                   |
| -- templateCode             | String  | Template code                                                |
| -- recipientNo              | String  | Recipient number                                             |
| -- content                  | String  | Body message                                                 |
| -- requestDate              | String  | Date and time of request                                     |
|-- createDate                | String  | Registered date and time                                     |
| -- receiveDate              | String  | Date and time of receiving                                   |
| -- resendStatus             | String  | Status code of resending                                     |
| -- resendStatusName         | String  | Status code name of resending                                |
| -- messageStatus            | String  | Request status (COMPLETED -> successful, FAILED ->failed, CANCEL -> canceled ) |
| -- resultCode               | String  | Result code of receiving                                     |
| -- resultCodeName           | String  | Result code name of receiving                                |
|-- createUser                | String  |  Registrant (saved as user UUID when delivered via console)  |
| -- buttons                  | List    | List of buttons                                              |
| --- ordering                | Integer | Button sequence                                              |
| --- type                    | String  | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added) |
| --- name                    | String  | Button name                                                  |
| --- linkMo                  | String  | Mobile web link (required for the WL type)                   |
| --- linkPc                  | String  | PC web link (optional for the WL type)                       |
| --- schemeIos               | String  | iOS app link (required for the AL type)                      |
| --- schemeAndroid           | String  | Android app link (required for the AL type)                  |
| --- chatExtra               | String  | BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보             |
| --- chatEvent               | String  | BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명                          |
| --- target                  | String  | 웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| -- senderGroupingKey        | String  | Sender's grouping key                                        |
| -- recipientGroupingKey     | String  | Recipient's grouping key                                     |
| - totalCount                | Integer | Total count                                                  |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/auth/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

#### Status of Resending SMS/LMS
| Value | Description                                     |
| ----- | ----------------------------------------------- |
| RSC01 | No target of resending                          |
| RSC02 | Target of resending (resent, if sending fails.) |
| RSC03 | Resending                                       |
| RSC04 | Resending successful                            |
| RSC05 | Resending failed                                |

### Get Messages

#### Request

[URL]

```
GET  /alimtalk/v2.2/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value        | Type    | Description               |
| ------------ | ------- | ------------------------- |
| appkey       | String  | Original appkey           |
| requestId    | String  | Request ID                |
| recipientSeq | Integer | Recipient sequence number |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}"
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
      "senderKey"  : String,
      "templateCode" :  String,
      "recipientNo" :  String,
      "content" :  String,
      "templateTitle" : String,
      "templateSubtitle" : String,
      "templateExtra" : String,
      "templateAd" : String,
      "requestDate" :  String,
      "createDate" : String,
      "receiveDate" : String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "resendResultCode" : String,
      "resendRequestId" : String,
      "messageStatus" :  String,
      "resultCode" :  String,
      "resultCodeName" : String,
      "createUser" : String,
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
      "messageOption": {
        "price": Integer,
        "currencyType": String
      },
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
| - plusFriendId         | String  | PlusFriend ID                                                |
| - senderKey            | String  | Sender Key                                                   |
| - templateCode         | String  | Template code                                                |
| - recipientNo          | String  | Recipient number                                             |
| - content              | String  | Body message                                                 |
|- templateTitle         | String  | Template Title                                               |
|- templateSubtitle      | String  | Auxiliary Template Phrase                                    |
|- templateExtra         | String  | Additional Template Information                              |
|- templateAd            | String  | Request for consent of receiving within template or simple ad phrases |
| - requestDate          | String  | Date and time of request                                     |
| - createDate           | String  | Registered date and time                                    |
| - receiveDate          | String  | Date and time of receiving                                   |
| - resendStatus         | String  | Status code of resending                                     |
| - resendStatusName     | String  | Status code name of resending                                |
| - resendResultCode     | String  | Result code of resending [Result code of SMS sending](https://docs.toast.com/en/Notification/SMS/en/error-code/#api) |
| - resendRequestId      | String  | ID requesting of resending SMS                               |
| - messageStatus        | String  | Request status (COMPLETED -> successful, FAILED -> failed, CANCEL -> canceled) |
| - resultCode           | String  | Result code of receiving                                     |
| - resultCodeName       | String  | Result code name of receiving                                |
|- createUser            | String  | Registrant (saved as user UUID when delivered via console)  |
| - buttons              | List    | List of buttons                                              |
| -- ordering            | Integer | Button sequence                                              |
| -- type                | String  | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK:Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added) |
| -- name                | String  | Button name                                                  |
| -- linkMo              | String  | Mobile web link (required for the WL type)                   |
| -- linkPc              | String  | PC web link (optional for the WL type)                       |
| -- schemeIos           | String  | iOS app link (required for the AL type)                      |
| -- schemeAndroid       | String  | Android app link (required for the AL type)                  |
| -- chatExtra           | String  | BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보             |
| -- chatEvent           | String  | BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명                          |
| -- target              | String  | 웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| - messageOption        | Object  | Message Option                                               |
| -- price               | Integer | Price/amount/payment amount included in message (message to be delivered to user)(related to moment advertisement) |
| -- currencyType        | String  | Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message (message to be delivered to the user) (related to moment advertisement) |
| - senderGroupingKey    | String  | Sender's grouping key                                        |
| - recipientGroupingKey | String  | Recipient's grouping key                                     |

## Messages
### Cancel Sending Messages

#### Request

[URL]

```
DELETE  /alimtalk/v2.2/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value     | Type   | Description     |
| --------- | ------ | --------------- |
| appkey    | String | Original appkey |
| requestId | String | Request ID      |

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

| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| recipientSeq | String | X        | Recipient sequence number<br>(to cancel all deliveries of request ID, if the value is left blank) |

* Both general and authentication messages can be canceled by same API.

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

[Example]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

### Query Updates of Message Result

#### Request

[URL]

```
GET  /alimtalk/v2.2/appkeys/{appkey}/message-results
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

| Value               | Type    | Required | Description                                              |
| ------------------- | ------- | -------- | -------------------------------------------------------- |
| startUpdateDate     | String  | O        | Start date of querying result updates (yyyy-MM-dd HH:mm) |
| endUpdateDate       | String  | O        | End date of querying result updates (yyyy-MM-dd HH:mm)   |
| alimtalkMessageType | String  | X        | Alimtalk message type (NORMAL, AUTH)                     |
| pageNum             | Integer | X        | Page number (default: 1)                                 |
| pageSize            | Integer | X        | Number of queries (default: 15, max : 1000)              |

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
      "requestDate" :  String,
      "createDate" :  String,
      "receiveDate" : String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "resendResultCode" :  String,
      "resendRequestId" :  String,
      "messageStatus" :  String,
      "resultCode" :  String,
      "resultCodeName" : String
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
| -- recipientSeq             | Integer | Recipient sequence number                                    |
| -- requestDate              | String  | Date and time of request                                     |
| -- createDate               | String  | Date and time of creation                                    |
| -- receiveDate              | String  | Date and time of receiving                                   |
| -- resendStatus             | String  | Status code of resending                                     |
| -- resendStatusName         | String  | Status code name of resending                                |
| -- resendResultCode         | String  | Result code of resending to sms                              |
| -- resendRequestId          | String  | RequestId of resending to sms                                |
| -- messageStatus            | String  | Request status (COMPLETED -> Successful, FAILED -> Failed, CANCEL -> Canceled) |
| -- resultCode               | String  | Result code of receiving                                     |
| -- resultCodeName           | String  | Result code name of receiving                                |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

## 대량 발송
### 대량 발송 요청 목록 조회

#### 요청
[URL]
```
GET /alimtalk/v2.2/appkeys/{appKey}/mass-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 앱키|

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름 |	타입|	설명|
|---|---|---|
|X-Secret-Key|	String|	고유의 비밀 키 |

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
'https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages?requestId='"${REQUEST_ID}" \
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
| -- createDate | String | 생성 날짜 |
| -- createUser | String | 생성 날짜 |
| -- plusFriendId | String | 플러스 친구 ID |
| -- senderKey | String| 발신 키 (40자) |
| -- masterStatusCode | String | 대량 발송 상태 코드 (WAIT, READY, SENDREADY, SENDWAIT, SENDING, COMPLETE, CANCEL, FAIL) |
| -- content | String | 내용 |
| -- buttons | List | 버튼 리스트 |
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
| -- fileId | String | 첨부 파일 ID |
| -- templateCode |	String | 템플릿 코드 (최대 20자) |
| -- templateExtra | String | 템플릿 부가 정보(템플릿 메시지 유형이 [부가 정보형/복합형]일 경우 필수) |
| -- templateAd | String | 템플릿 내 수신 동의 요청 또는 간단한 광고 문구(템플릿 메시지 유형이 [광고 추가형/복합형]일 경우 필수) |
| -- tempalteTitle| String | 템플릿 제목 (최대 50자, Android : 2줄, 23자 이상 말줄임 처리, IOS : 2줄, 27자 이상 말줄임 처리) |
| -- templateSubtitle| String | 템플릿 보조 문구 (최대 50자, Android : 18자 이상 말줄임 처리, IOS : 21자 이상 말줄임 처리) |
| -- autoSendYn | String | 자동 발송 여부 |
| -- statsId | String | 통계 ID |
| -- createDate | String | 생성 날짜 |
| -- createUser | String | 등록자(콘솔에서 발송 시 사용자 UUID로 저장) |
| - totalCount | Integer | 총 개수 |


### 대량 발송 수신자 목록 조회

#### 요청
[URL]
```
GET /alimtalk/v2.2/appkeys/{appKey}/mass-messages/{requestId}/recipients
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
| X-Secret-Key |	String|	고유의 비밀 키 |


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
'https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients?requestId='"${REQUEST_ID}" \
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

| 이름 |	타입|	설명|
|---|---|---|
| header | Object |	헤더 영역 |
| - resultCode |	Integer |	결과 코드 |
| - resultMessage |	String | 결과 메시지 |
| - isSuccessful |	Boolean | 성공 여부 |
| body | Object | 본문 영역 |
| - messages | Object | 메시지 리스트 |
| -- requestId | String | 요청 ID |
| -- recipientSeq | String | 수신자 시퀀스 번호 |
| -- recipientNo | String | 수신 번호 |
| -- requestDate | String | 요청 날짜 |
| -- receiveDate | String | 수신 날짜 |
| -- messageStatus | String | 메시지 상태 |
| -- resultCode | String | 결과 코드 |
| -- resultCodeName | String | 결과 코드 내용 |
| - totalCount | Integer | 총 개수 |

### 대량 발송 수신자 조회

#### 요청
[URL]
```
GET /alimtalk/v2.2/appkeys/{appKey}/mass-messages/{requestId}/recipients/{recipientSeq}
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
| pageNum | optional, Integer | - | X | 페이지 번호 |
| pageSize | optional, Integer | 1000 | X | 검색 수 |

#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients/1?requestId='"${REQUEST_ID}" \
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

| 이름 |	타입|	설명|
|---|---|---|
| header | Object |	헤더 영역 |
| - resultCode |	Integer |	결과 코드 |
| - resultMessage |	String | 결과 메시지 |
| - isSuccessful |	Boolean | 성공 여부 |
| body | Object | 본문 영역 |
| - requestId | String | 요청 ID |
| - recipientSeq | String | 수신자 시퀀스 번호 |
| - plusFriendId | String | 플러스 친구 ID |
| - senderKey | String | 전송자 ID |
| - templateCode |	String | 템플릿 코드 (최대 20자) |
| - recipientNo | String | 수신 번호 |
| - content | String | 내용 |
| - tempalteTitle| String | 템플릿 제목 (최대 50자, Android : 2줄, 23자 이상 말줄임 처리, IOS : 2줄, 27자 이상 말줄임 처리) |
| - templateSubtitle| String | 템플릿 보조 문구 (최대 50자, Android : 18자 이상 말줄임 처리, IOS : 21자 이상 말줄임 처리) |
| - templateExtra | String | 템플릿 부가 정보(템플릿 메시지 유형이 [부가 정보형/복합형]일 경우 필수) |
| - templateAd | String | 템플릿 내 수신 동의 요청 또는 간단한 광고 문구(템플릿 메시지 유형이 [광고 추가형/복합형]일 경우 필수) |
| - requestDate | String | 요청 날짜 |
| - receiveDate | String | 수신 날짜 |
| - createDate | String | 생성 날짜 |
| - resendStatus | String | 대체 발송 상태 코드 (RSC01, RSC02, RSC03, RSC04, RSC05)<br>([[아래 대체 발송 상태 표](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] 참고) |
| - resendStatusName | String | 대체 발송 상태명 |
| - resendResultCode | String | 대체 발송 결과 코드 [SMS 결과 코드](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api) |
| - resendRequestId | String | 대체 발송 요청 ID |
| - messageStatus | String | 대량 수신자 발송 상태 코드 (READY, COMPLETED, FAILED, CANCEL) |
| - resultCode | String | 결과 상태 코드 |
| - resultCodeName | String | 결과 상태명 |
| - createUser | String | 생성 유저 ID |
| - buttons | List | 버튼 리스트 |
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
| - messageOption | Boolean | 메시지 옵션 |
|-- price | Integer |	message(사용자에게 전달될 메시지) 내 포함된 가격/금액/결제금액 (모먼트 광고에 해당) |
|-- currencyType | String |	message(사용자에게 전달될 메시지) 내 포함된 가격/금액/결제금액의 통화단위 KRW, USD, EUR 등 국제 통화 코드 사용 (모먼트 광고에 해당) |


## Templates

### List Template Categories
#### Request
[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/template/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value        | Type   | Description     |
| ------------ | ------ | --------------- |
| appkey       | String | Original appkey |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

#### Response
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "categories": [
    {
      "name": String,
      "subCategories": [
        {
          "code": String,
          "name": String,
          "groupName": String,
          "inclusion": String,
          "exclusion": String
        }
      ]
    }
  ]
}
```

| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - resultCode    | Integer | Result code       |
| - resultMessage | String  | Result message    |
| - isSuccessful  | Boolean | Successful or not |
| categories      |	List    |	List of categories |
| - name          | String  | Category name      |
| - subCategories | List    |	List of subcategories    |
| -- code         | String  | Category code (Used when registering/modifying templates) |
| -- name         | String  |	Category name       |
| -- groupName    | String  |	Category group name |
| -- inclusion    | String  |	Description of templates to which the category applies |
| -- exclusion    | String  | Description of templates to which the category does not apply |

### Register Templates

#### Request

[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value        | Type   | Description     |
| ------------ | ------ | --------------- |
| appkey       | String | Original appkey |
| senderKey    | String | Sender Key   |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "templateCode" : String,
  "templateName" : String,
  "templateContent" : String,
  "templateMessageType": String,
  "templateEmphasizeType" : String,
  "templateExtra": String,
  "templateAd": String,
  "templateTitle" : String,
  "templateSubtitle" : String,
  "templateImageName" : String,
  "templateImageUrl" : String,
  "securityFlag": Boolean,
  "categoryCode": String,
  "buttons" : [
    {
      "ordering" : Integer,
      "type" : String,
      "name" : String,
      "linkMo" : String,
      "linkPc" : String,
      "schemeIos" : String,
      "schemeAndroid" : String
    }
  ]
}
```

| Value               | Type    | Required | Description                                                  |
| ---------------     | ------- | -------- | ------------------------------------------------------------ |
| templateCode        | String  | O        | Template code (up to 20 characters)                          |
| templateName        | String  | O        | Template name (up to 20 characters)                          |
| templateContent     | String  | O        | Template body (up to 1000 characters)                        |
| templateMessageType | String  | X        | Types of Template Message (BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes, default: Basic) |
|templateEmphasizeType| String  | X        | Types of Emphasized Template (NONE: Basic, TEXT: Emphasized, IMAGE: Image type, default:NONE)<br>- TEXT: templateTitle and templateSubtitle fields are required <br>IMAGE: templateImageName and templateImageUrl fields are required |
| templateExtra       | String  | X        | Additional Template Information (Required, if template message type is[Ad Included/Mixed Purposes])                             |
| templateAd          | String  | X        | Request for consent of receiving within template or simple ad phrases (Required, if template message type is[Ad Included/Mixed Purposes]) |
|tempalteTitle        | String  | X        | Template Title (No more than 50 characters, Android: To be abbreviated if it exceeds 2 lines with more than 23 characters, iOS: To be abbreviated if it exceeds 2 lines with more than 27 characters) |
|templateSubtitle    | String   | X        | Auxiliary Template Phrase (No more than 50 characters, Android: To be abbreviated if it exceeds 18 characters, iOS: To be abbreviated if it exceeds 21 characters) |
|templateImageName | String |	X | Image name  |
|templateImageUrl | String |	X | Image URL |
| securityFlag    | Boolean | X        | Security template<br>Set for security messages such as OTP<br>If set, message text is unexposed to all devices except for the main device at the time of sending (default: false) |
| categoryCode    | String  | X        | Template category code (Refer to API to View Template Category, default: 999999)<br>For other categories, screened by the lowest priority. |
| buttons         | List    | X        | List of buttons (up to 5)                                    |
| -ordering       | Integer | X        | Button sequence (1~5)                                        |
| -type           | String  | X        | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added [only for Ad Included/Mixed Purposes Type]) |
| -name           | String  | X        | Button name (required, if there's a button, up to 14 characters) |
| -linkMo         | String  | X        | Mobile web link (required for the WL type, up to 200 characters) |
| -linkPc         | String  | X        | PC web link (optional for the WL type, up to 200 characters) |
| -schemeIos      | String  | X        | iOS app link (required for the AL type, up to 200 characters) |
| -schemeAndroid  | String  | X        | Android app link (required for the AL type, up to 200 characters) |

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

### Modify Templates

#### Request

[URL]

```
PUT  /alimtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value        | Type   | Description     |
| ------------ | ------ | --------------- |
| appkey       | String | Original appkey |
| senderKey    | String | Sender Key      |
| templateCode | String | Template code   |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "templateName" : String,
  "templateContent" : String,
  "templateMessageType": String,
  "templateEmphasizeType" : String,
  "templateExtra": String,
  "templateAd": String,
  "templateTitle" : String,
  "templateSubtitle" : String,
  "templateImageName" : String,
  "templateImageUrl" : String,
  "securityFlag": Boolean,
  "categoryCode": String,
  "buttons" : [
    {
      "ordering" : Integer,
      "type" : String,
      "name" : String,
      "linkMo" : String,
      "linkPc" : String,
      "schemeIos" : String,
      "schemeAndroid" : String
    }
  ]
}
```

| Value           | Type    | Required | Description                                                  |
| --------------- | ------- | -------- | ------------------------------------------------------------ |
| templateName    | String  | O        | Template name (up to 20 characters)                          |
| templateContent | String  | O        | Template body (up to 1000 characters)                        |
| templateMessageType | String  | X        | Types of Template Message (BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes, default: Basic) |
|templateEmphasizeType| String  | X        | Types of Emphasized Template (NONE: Basic, TEXT: Emphasized, IMAGE: Image type, default:NONE)<br>- TEXT: templateTitle and templateSubtitle fields are required <br>IMAGE: templateImageName and templateImageUrl fields are required |
| templateExtra       | String  | X        | Additional Template Information (Required, if template message type is[Ad Included/Mixed Purposes])                             |
| templateAd          | String  | X        | Request for consent of receiving within template or simple ad phrases (Required, if template message type is[Ad Included/Mixed Purposes] |
|tempalteTitle| String | X| Template Title (No more than 50 characters, Android: To be abbreviated if it exceeds 2 lines with more than 23 characters, iOS: To be abbreviated if it exceeds 2 lines with more than 27 characters) |
|templateSubtitle| String | X| Auxiliary Template Phrase (No more than 50 characters, Android: To be abbreviated if it exceeds 18 characters, iOS: To be abbreviated if it exceeds 21 characters) |
|templateImageName | String |	X | Image name  |
|templateImageUrl | String |	X | Image URL |
| securityFlag    | Boolean | X        | Security template<br>Set for security messages such as OTP<br>If set, message text is unexposed to all devices except for the main device at the time of sending (default: false) |
| categoryCode    | String  | X        | Template category code (Refer to API to View Template Category, default: 999999)<br>For other categories, screened by the lowest priority. |
| buttons         | List    | X        | List of buttons (up to 5)                                    |
| -ordering       | Integer | X        | Button sequence (1~5)                                        |
| -type           | String  | X        | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added [only for Ad Included/Mixed Purposes Type]) |
| -name           | String  | X        | Button name (required, if there's a button, up to 14 characters) |
| -linkMo         | String  | X        | Mobile web link (required for the WL type, up to 200 characters) |
| -linkPc         | String  | X        | PC web link (optional for the WL type, up to 200 characters) |
| -schemeIos      | String  | X        | iOS app link (required for the AL type, up to 200 characters) |
| -schemeAndroid  | String  | X        | Android app link (required for the AL type, up to 200 characters) |

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

### Delete Templates

#### Request

[URL]

```
DELETE  /alimtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value        | Type   | Description     |
| ------------ | ------ | --------------- |
| appkey       | String | Original appkey |
| senderKey    | String | Sender Key      |
| templateCode | String | Template code   |

[Header]
```
{
  "X-Secret-Key": String
}
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

### Inquire of Templates

#### Request

[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/comments
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value        | Type   | Description     |
| ------------ | ------ | --------------- |
| appkey       | String | Original appkey |
| senderKey | String | Sender Key   |
| templateCode | String | Template code   |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "comment" : String
}
```

| Value   | Type   | Required | Description |
| ------- | ------ | -------- | ----------- |
| comment | String | O        | Inquiries   |

* When commenting a template in the REJ status, it will be changed to the REQ status.

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

### Attach files to send inquiry on templates
#### Request
[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/comments_file
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value           | Type    | Description       |
|---|---|---|
|appkey|	String|	고유의 Appkey|
|senderKey|	String|	Sender Key |
|templateCode|	String|	Template code |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Note](./sender-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "comment" : String,
  "attachments" : File
}
```

| Value        | Type   | Required | Description                                                  |
|---|---|---|---|
|comment|	String |	O | Content of Inquiry |
|attachments| List<File> | X | List of Attachment (Up to 5) |

* When commenting a template in the REJ status, it will be changed to the REQ status.

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
|---|---|---|
|header|	Object|	Header ARea|
|- resultCode|	Integer| Result Code|
|- resultMessage|	String| Result Message|
|- isSuccessful|	Boolean| Successful or not|

### List Templates

#### Request

[URL]

```
GET  /alimtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value  | Type   | Description     |
| ------ | ------ | --------------- |
| appkey | String | Original appkey |
| senderKey | String | Sender Key |

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

| Value          | Type    | Required | Description                     |
| -------------- | ------- | -------- | ------------------------------- |
| templateCode   | String  | X        | Template code                   |
| templateName   | String  | X        | Template name                   |
| templateStatus | String  | X        | Template status code            |
| pageNum        | Integer | X        | Page number (default:1)         |
| pageSize       | Integer | X        | Number of queries (default: 15, max : 1000) |

| Template Status Code | Description |
| -------------------- | ----------- |
| TSC01                | Requested   |
| TSC02                | Inspecting  |
| TSC03                | Approved    |
| TSC04                | Returned    |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/templates?templateStatus={template status code}"
```

#### Response
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "templateListResponse": {
      "templates": [
          {
              "plusFriendId": String,
              "senderKey": String,
              "plusFriendType": String,
              "templateCode": String,
              "templateName": String,
              "templateContent": String,
              "templateEmphasizeType": String,
              "templateTitle" : String,
              "templateSubtitle" : String,
              "templateImageName" : String,
              "templateImageUrl" : String,
              "templateMessageType" : String,
              "templateExtra" : String,
              "templateAd" : String,
              "buttons": [
                {
                    "ordering":Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String
                }
                ],
                "comments": [
                  {
                      "id": Integer,
                      "content": String,
                      "userName": String,
                      "createdAt": String,
                      "attachment": [{
                        "originalFileName": String,
                        "filePath": String
                      }],
                      "status": String
                    }  
                ],
                "status": String,
                "statusName": String,
                "createDate": String
            }
        ],
        "totalCount": Integer
    }
}
```

| Value                | Type    | Description                                                  |
| -------------------- | ------- | ------------------------------------------------------------ |
| header               | Object  | Header area                                                  |
| - resultCode         | Integer | Result code                                                  |
| - resultMessage      | String  | Result message                                               |
| - isSuccessful       | Boolean | Successful or not                                            |
| templateListResponse | Object  | Body area                                                    |
| - templates          | List    | Template list                                                |
| -- plusFriendId      | String  | PlusFriend ID                                                |
| -- senderKey         | String  | Sender Key                                                   |
| -- plusFriendType    | String  | PlusFriend type (NORMAL, GROUP)                              |
| -- templateCode      | String  | Template code                                                |
| -- templateName      | String  | Template name                                                |
| -- templateContent   | String  | Template body                                                |
|-- templateEmphasizeType| String| Types of Emphasized Template (NONE: Basic, TEXT: Emphasized, IMAGE: Image type, default:NONE) |
|-- tempalteTitle      | String  | Template Title                                               |
|-- templateSubtitle   | String  | Auxiliary Template Phrase                                    |
|-- templateImageName  | String  | Image name                                                   |
|-- templateImageUrl   | String  | Image URL                                                    |
|-- templateMessageType| String  | Types of Template Message (BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes) |
|-- templateExtra      | String  | Additional Template Information                              |
|-- templateAd         | String  | Request for consent of receiving within template or simple ad phrases |
| -- buttons           | List    | List of buttons                                              |
| --- ordering         | Integer | Button sequence (1~5)                                        |
| --- type             | String  | Button type (WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added) |
| --- name             | String  | Button name                                                  |
| --- linkMo           | String  | Mobile web link (required for the WL type)                   |
| --- linkPc           | String  | PC web link (optional for the WL type)                       |
| --- schemeIos        | String  | iOS app link (required for the AL type)                      |
| --- schemeAndroid    | String  | Android app link (required for the AL type)                  |
| -- comments          | List    | Inspection result                                            |
| --- id               | Integer | Inquiry ID                                                   |
| --- content          | String  | Inquiry content                                              |
| --- userName          | String  | Creator                                                      |
| --- createAt          | String  | Date of registration                                         |
| --- attachment        | List    | Attachment                                                   |
| ---- originalFileName | String | Attachment file name                                          |
| ---- filePath         | String | Attachment file path                                          |
| --- status            | String  | Comment status (INQ: Inquired, APR: Approved, REJ: Returned, REP: Replied) |
| -- status            | String  | Template status                                              |
| -- statusName        | String  | Template status name                                         |
| -- createDate        | String  | Date and time of creation                                    |
| - totalCount         | Integer | Total count                                                  |

### List Template modifications

#### Request

[URL]

```
GET  /alimtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/modifications
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value        | Type   | Description     |
| ------------ | ------ | --------------- |
| appkey       | String | Original appkey |
| senderKey    | String | Sender Key   |
| templateCode | String | Template code   |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/modifications"
```

#### Response
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "templateModificationsResponse": {
      "templates": [
          {
              "plusFriendId": String,
              "senderKey": String,
              "plusFriendType": String,
              "templateCode": String,
              "templateName": String,
              "templateContent": String,
              "templateEmphasizeType": String,
              "templateTitle" : String,
              "templateSubtitle" : String,
              "templateImageName" : String,
              "templateImageUrl" : String,
              "templateMessageType" : String,
              "templateExtra" : String,
              "templateAd" : String,
              "buttons": [
                {
                    "ordering":Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String
                }
                ],
                "comments": [
                  {
                      "id": Integer,
                      "content": String,
                      "userName": String,
                      "createdAt": String,
                      "attachment": [{
                        "originalFileName": String,
                        "filePath": String
                      }],
                      "status": String
                    }  
                ],
                "status": String,
                "statusName": String,
                "activated": boolean,
                "createDate": String
            }
        ],
        "totalCount": Integer
    }
}
```

| Value                | Type    | Description                                                  |
| -------------------- | ------- | ------------------------------------------------------------ |
| header               | Object  | Header area                                                  |
| - resultCode         | Integer | Result code                                                  |
| - resultMessage      | String  | Result message                                               |
| - isSuccessful       | Boolean | Successful or not                                            |
| templateModificationsResponse | Object  | Body area                                                    |
| - templates          | List    | Template list                                                |
| -- plusFriendId      | String  | PlusFriend ID                                                |
| -- senderKey         | String  | Sender Key                                                   |
| -- plusFriendType    | String  | PlusFriend type (NORMAL, GROUP)                              |
| -- templateCode      | String  | Template code                                                |
| -- templateName      | String  | Template name                                                |
| -- templateContent   | String  | Template body                                                |
|-- templateEmphasizeType| String| Types of Emphasized Template (NONE: Basic, TEXT: Emphasized, IMAGE: Image type, default:NONE) |
| -- tempalteTitle      | String  | Template Title                                               |
| -- templateSubtitle   | String  | Auxiliary Template Phrase                                    |
| -- templateImageName  | String  | Image name                                                   |
| -- templateImageUrl   | String  | Image URL                                                    |
| -- templateMessageType| String  | Types of Template Message (BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes) |
| -- templateExtra      | String  | Additional Template Information                             |
| -- templateAd         | String  | Request for consent of receiving within template or simple ad phrases |
| -- buttons           | List    | List of buttons                                              |
| --- ordering         | Integer | Button sequence (1~5)                                        |
| --- type             | String  | Button type (WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added) |
| --- name             | String  | Button name                                                  |
| --- linkMo           | String  | Mobile web link (required for the WL type)                   |
| --- linkPc           | String  | PC web link (optional for the WL type)                       |
| --- schemeIos        | String  | iOS app link (required for the AL type)                      |
| --- schemeAndroid    | String  | Android app link (required for the AL type)                  |
| -- comments          | List    | Inspection result                                            |
| --- id               | Integer | Inquiry ID                                                   |
| --- content          | String  | Inquiry content                                              |
| --- userName          | String  | Creator                                                      |
| --- createAt          | String  | Date of registration                                         |
| --- attachment        | List    | Attachment                                                   |
| ---- originalFileName | String | Attachment file name                                          |
| ---- filePath         | String | Attachment file path                                          |
| --- status            | String  | Comment status (INQ: Inquired, APR: Approved, REJ: Returned, REP: Replied) |
| -- status            | String  | Template status                                              |
| -- statusName        | String  | Template status name                                         |
| -- activated         | Boolean | activated or not                                             |
| -- createDate        | String  | Date and time of creation                                    |
| - totalCount         | Integer | Total count                                                  |

### Register Template Image
#### Request
[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/template-image
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
| X-Secret-Key | String | O        | Can be created on console. [[Note](./sender-console-guide/#x-secret-key)] |

[Request parameter]

| Value        | Type   | Required | Description                                                  |
|---|---|---|---|
|file|	File |	O | Template image file |

[예시]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/template-image" -F "file=@alimtalk-template-image.jpeg"
```

#### Response
```
{
  "header" : {
    "resultCode" :  Integer,
    "resultMessage" :  String,
    "isSuccessful" :  boolean
  },
  "templateImage" {
    "templateImageName": String,
    "templateImageUrl": String
  }
}
```

| Value                | Type    | Description                                                  |
| -------------------- | ------- | ------------------------------------------------------------ |
| header               | Object  | Header area                                                  |
| - resultCode         | Integer | Result code                                                  |
| - resultMessage      | String  | Result message                                               |
| - isSuccessful       | Boolean | Successful or not                                            |
| templateImage        | Object  | Body area                                                    |
| - templateImageName  | String  | Image name                                                   |
| - templateImageUrl   | String  | Image URL                                                    |
