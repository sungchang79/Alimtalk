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
1. Added Alimtalk mass delivery query and statistics query API.
2. Added a `buttons` field to the response body of replaced message sending API.
3. Added `chatExtra`, `chatEvent`, and `target` fields to the `buttons` field in the response body of the full text message sending API.
4. Added `chatExtra`, `chatEvent`, and `target` fields to the `buttons` field in the response body of the message query API.

## General Messages

### Request of Sending Replaced Messages

[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

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

| Name |  Type| Required| Description|
|---|---|---|---|
|senderKey| String| O | Sender key (40 characters) |
|templateCode|  String| O | Registered delivery template code (up to 20 characters) |
|requestDate| String | X| Date and time of request (yyyy-MM-dd HH:mm)<br>(send immediately, if it is left blank)<br>Can be scheduled up to 30 days later |
|senderGroupingKey| String | X| Sender's grouping key (up to 100 characters) |
|createUser| String | X| Registrant (saved as user UUID when sending from console)|
|recipientList| List|   O|  List of recipients (up to 1000 persons) |
|- recipientNo| String| O|  Recipient number (up to 15 characters) |
|- templateParameter|   Object| X|  Template parameter<br>(required, if it includes a variable to be replaced for template) |
|-- key|    String| X | Replacement key (#{key})|
|-- value| String | X | Value which is mapped for replacement key|
|- resendParameter| Object| X| Alternative delivery information |
|-- isResend|   boolean|    X|  Whether to resend text, if delivery fails<br/>Resent by default, if alternative delivery is set on console. |
|-- resendType| String| X|  Alternative delivery type (SMS,LMS)<br/>Categorized by the length of template body if value is unavailable. |
|-- resendTitle|    String| X|  Title of alternative delivery for LMS<br/>(resent with PlusFriend ID if value is unavailable.) |
|-- resendContent|  String| X|  Alternative delivery content<br/>(resent with [Message body and web link button name - web link mobile link] if value is unavailable.) |
|-- resendSendNo | String| X| Sender number for alternative delivery<br/><span style="color:red">(Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span> |
|- buttons| List|   X| Additional information for buttons |
|-- ordering            | Integer  | X        | Button sequence (required, if there is a button)|
|-- chatExtra|  String| X| Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons |
|-- chatEvent|  String| X| Bot event name to connect for BT (Bot Transfer) type button |
|-- target| String| X | In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
|- recipientGroupingKey|    String| X|  Recipient grouping key (up to 100 characters) |
|messageOption | Object |   X | Message Option |
|- price | Integer |    X | Price/amount/payment amount included in message (message to be delivered to user)(related to moment advertisement) |
|- currencyType | String |  X| Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message (message to be delivered to the user) (related to moment advertisement) |

* <b>Request date and time can be set up to 90 days since a point of calling.</b>
* <b>Since alternative delivery is made in the SMS service, field values must follow the API specifications for SMS (e.g. Sender number registered at the SMS service, or restriction in the field length). </b>
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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|message|   Object| Body area|
|- requestId | String | Request ID |
|- senderGroupingKey | String | Sender's grouping key |
|- sendResults | Object | Result of delivery request |
|-- recipientSeq | Integer | Recipient sequence number |
|-- recipientNo | String | Recipient number |
|-- resultCode | Integer | Result code of delivery request |
|-- resultMessage | String | Result message of delivery request |
|-- recipientGroupingKey | String | Recipient's grouping key |

### Request of Sending Full Text

[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/raw-messages
Content-Type: application/json;charset=UTF-8
```

[Path Parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

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

| Name |  Type| Required| Description|
|---|---|---|---|
|senderKey| String| O | Sender key (40 characters) |
|templateCode|  String| O | Registered delivery template code (up to 20 characters) |
|requestDate| String | X| Date and time of request (yyyy-MM-dd HH:mm)<br>(send immediately, if it is left blank)<br>Can be scheduled up to 30 days later |
|senderGroupingKey| String | X| Sender's grouping key (up to 100 characters) |
|createUser| String | X| Registrant (saved as user UUID when sending from console)|
|recipientList| List|   O|  List of recipients (up to 1,000 persons) |
|- recipientNo| String| O|  Recipient number (up to 15 characters) |
|- content| String| O|  Message  (up to 1000 characters) |
|- templateTitle| String| X| Title (up to 50 characters) |
|- buttons| List |  X | List of buttons (up to 5) |
|-- ordering|   Integer|    X | Button sequence (required, if there is a button)|
|-- type| String |  X | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added) |
|-- name| String |  X | Button name (required if there is a button, up to 14 characters)|
|-- linkMo| String |    X | Mobile web link (required for the WL type, up to 200 characters)|
|-- linkPc | String |   X |PC web link (optional for the WL type, up to 200 characters) |
|-- schemeIos | String | X |    iOS app link (required for the AL type, up to 200 characters) |
|-- schemeAndroid | String | X |    Android app link (required for the AL type, up to 200 characters) |
|-- chatExtra|  String| X| Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons |
|-- chatEvent|  String| X| Bot event name to connect for BT (Bot Transfer) type button |
|-- target| String| X | In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
|- resendParameter| Object| X| Alternative delivery information |
|-- isResend|   boolean|    X|  Whether to resend text, if delivery fails<br/>Resent by default, if alternative delivery is set on console. |
|-- resendType| String| X|  Alternative delivery type (SMS,LMS)<br/>Categorized by the length of template message, if value is unavailable. |
|-- resendTitle|    String| X|  Title of alternative delivery for LMS<br/>(resent with PlusFriend ID if value is unavailable.) |
|-- resendContent|  String| X|  Alternative delivery content<br/>(resent with [Message body and web link button name - web link mobile link] if value is unavailable.) |
|-- resendSendNo | String| X| Sender number for alternative delivery<br/><span style="color:red">(Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span> |
|- recipientGroupingKey|    String| X|  Recipient's grouping key (up to 100 characters) |
| messageOption | Object |  X | Message Option |
|- price | Integer |    X | Price/amount/payment amount included in message (message to be delivered to user)(related to moment advertisement) |
|- currencyType | String |  X| Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message (message to be delivered to the user) (related to moment advertisement) |

* <b>Enter data completed with replacement for the body and button. </b>
* **Request date and time can be set up to 90 days since a point of calling.**
* <b>Delivery is to be replaced by SMS, and field input must follow delivery API specifications of the SMS service (e.g. sender number registered at SMS service, 080 unsubscription, and field length restrictions) </b>
* <b>Only the international SMS service is supported. For an international recipient number, the resendType (alternative delivery type) must be changed to SMS to allow sending normally. </b>
* <b>Title or message of an alternative delivery may be cut in length, if the byte size exceeds restrictions (see [[Cautions for SMS](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)])</b>

[Example]
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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|message|   Object| Body area|
|- requestId | String | Request ID |
|- senderGroupingKey | String | Sender's grouping key |
|- sendResults | Object | Result of delivery request |
|-- recipientSeq | Integer | Recipient's sequence number |
|-- recipientNo | String | Recipient number |
|-- resultCode | Integer | Result code of delivery request |
|-- resultMessage | String | Result message of delivery request |
|-- recipientGroupingKey | String | Recipient's grouping key |

### List Messages

#### Request

[URL]

```
GET  /alimtalk/v2.2/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Query parameter] No. 1 or (2, 3) is conditionally required

| Name |  Type| Required| Description|
|---|---|---|---|
|requestId| String| Conditionally required (no.1) | Request ID |
|startRequestDate|  String| Conditionally required (no.2) | Start date of delivery request (yyyy-MM-dd HH:mm)|
|endRequestDate|    String| Conditionally required (no.2) |    End date of delivery request (yyyy-MM-dd HH:mm) |
|startCreateDate|  String| Conditionally required (no.3) | Start date of registration (mm:HH dd-MM-yyyy)|
|endCreateDate|  String| Conditionally required (no.3) | End date of registration (mm:HH dd-MM-yyyy) |
|recipientNo|   String| X | Recipient number |
|senderKey| String| X | Sender key |
|templateCode|  String| X | Template code|
|senderGroupingKey| String | X| Sender's grouping key |
|recipientGroupingKey|  String| X|  Recipient's grouping key |
|messageStatus| String |    X | Request status (COMPLETED -> Successful, FAILED -> Failed, CANCEL -> Canceled)   |
|resultCode| String |   X | Delivery result (MRC01 -> Successful, MRC02 ->Failed)   |
|createUser| String | X| Registrant (saved as user UUID when sending from console)|
|pageNum|   Integer|    X|  Page number (default: 1)|
|pageSize|  Integer|    X|  Number of queries (default: 15, max : 1000)|

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|messageSearchResultResponse|   Object| Body area|
|- messages | List |    List of messages |
|-- requestId | String |    Request ID |
|-- recipientSeq | Integer |    Recipient sequence number |
|-- plusFriendId | String | PlusFriend ID |
|-- senderKey    | String | Sender Key    |
|-- templateCode | String | Template code |
|-- recipientNo | String |  Recipient number |
|-- content | String |  Body message |
|-- requestDate | String |  Date and time of request |
|-- createDate | String | Registered date and time |
|-- receiveDate | String |  Date and time of receiving |
|-- resendStatus | String | Status code of resending (RSC01, RSC02, RSC03, RSC04, RSC05)<br>([Refer to [Status code of resending table](http://docs.toast.com/en/Notification/KakaoTalk%20Bizmessage/en/alimtalk-api-guide/#smslms)] below) |
|-- resendStatusName | String | Status code name of resending |
|-- messageStatus | String |    Request status (COMPLETED -> successful, FAILED -> failed, CANCEL -> canceled ) |
|-- createUser | String | Registrant (saved as user UUID when sending from console) |
|-- resultCode | String |   Result code of receiving |
|-- resultCodeName | String |   Result code name of receiving |
|-- buttons | List |    List of buttons |
|--- ordering | Integer |   Button sequence |
|--- type | String |    Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added) |
|--- name | String |    Button name |
|--- linkMo | String |  Mobile web link  (required for the WL type) |
|--- linkPc | String |  PC web link (optional for the WL type) |
|--- schemeIos | String |   iOS app link (required for the AL type) |
|--- schemeAndroid | String |   Android app link (required for the AL type) |
|--- chatExtra| String| Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons |
|--- chatEvent| String| Bot event name to connect for BT (Bot Transfer) type button |
|--- target|    String| In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
|-- senderGroupingKey | String | Sender's grouping key |
|-- recipientGroupingKey | String | Recipient's grouping key |
|- totalCount | Integer | Total Count |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

### Get Messages

#### Request

[URL]

```
GET  /alimtalk/v2.2/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Typ| Description|
|---|---|---|
|appkey|    String| Unique appkey |
|requestId| String| Request ID |
|recipientSeq|  Integer|    Recipient sequence number |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|message|   Object| Message|
|- requestId | String | Request ID |
|- recipientSeq | Integer | Recipient sequence number |
|- plusFriendId | String |  PlusFriend ID |
|- senderKey    | String |  Sender Key    |
|- templateCode | String |  Template code |
|- recipientNo | String |   Recipient number |
|- content | String |   Body message |
|- templateTitle | String | Template title |
|- templateSubtitle | String | Auxiliary template phrase |
|- templateExtra | String | Additional template information |
|- templateAd | String | Request for consent of receiving within template or simple ad phrases |
|- requestDate | String |   Date and time of request |
|- receiveDate | String |   Date and time of receiving |
|- createDate | String | Registered date and time |
|- resendStatus | String |  Status code of resending (RSC01, RSC02, RSC03, RSC04, RSC05)<br>([Refer to [Status code of resending table](http://docs.toast.com/en/Notification/KakaoTalk%20Bizmessage/en/alimtalk-api-guide/#smslms)] below) |
|- resendStatusName | String |  Status code name of resending |
|- resendResultCode | String | Result code of resending [Result code of SMS sending](https://docs.toast.com/en/Notification/SMS/en/error-code/#api) |
|- resendRequestId | String | Resending SMS request ID |
|- messageStatus | String | Request status (COMPLETED -> successful, FAILED -> failed, CANCEL -> cancelled ) |
|- resultCode | String |    Result code of receiving |
|- resultCodeName | String |    Result code name of receiving |
|- createUser | String | Registrant (saved as user UUID when sending from console) |
|- buttons | List | List of buttons |
|-- ordering | Integer |    Button sequence |
|-- type | String | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK:Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added) |
|-- name | String | Button name |
|-- linkMo | String |   Mobile web link (required for the WL type) |
|-- linkPc | String |   PC web link (optional for the WL type) |
|-- schemeIos | String |    iOS app link (required for the AL type) |
|-- schemeAndroid | String |    Android app link (required for the AL type) |
|-- chatExtra|  String| Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons |
|-- chatEvent|  String| Bot event name to connect for BT (Bot Transfer) type button |
|-- target| String| In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
|- messageOption | Object | Message Option |
|-- price | Integer |   Price/amount/payment amount included in message (message to be delivered to user)(related to moment advertisement) |
|-- currencyType | String | Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message (message to be delivered to the user) (related to moment advertisement) |
|- senderGroupingKey | String | Sender's grouping key |
|- recipientGroupingKey | String |  Recipient grouping key |

## Authentication Messages

<span id="precautions-authword"></span>
1. Guide for authentication words required to be included for Authentication Messages API

| Category  | Authentication Words |
| --- | --- |
| Authentication Messages | auth, password, verif, にんしょう, 認証, 비밀번호, 인증 |

- Example 1-1) Delivery shall fail if the full text (including template replacement) does not include authentication words, in the request of Authentication Messages API (for emergency)
- Example 1-2) Validity for English words shall be checked regardless of small or capital letters


### Request of Sending Replaced Messages

[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/auth/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

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

| Name |  Type| Required| Description|
|---|---|---|---|
|senderKey| String| O | Sender key (40 characters) |
|templateCode|  String| O | Registered delivery template code (up to 20 characters) |
|requestDate| String | X| Date of request (yyyy-MM-dd HH:mm)<br>(immediately sent, if it is left blank) |
|senderGroupingKey| String | X| Sender's grouping key (up to 100 characters) |
|createUser | String | X| Registrant (saved as user UUID when sending from console) |
|recipientList| List|   O|  List of recipients (up to 1000 persons) |
|- recipientNo| String| O|  Recipient number (up to 15 characters) |
|- templateParameter|   Object| X|  Template parameter<br>(required, if it includes a variable to be replaced for template) |
|-- key|    String| X | Replacement key (#{key})|
|-- value| String | X | Value which is mapped for replacement key|
|- resendParameter|	Object|	X| Alternative delivery information |
|- isResend| boolean| X| Whether to resend text, if delivery fails<br/>Resent by default, if alternative delivery is set on console. |
|- resendType|   String|    X| Alternative delivery type (SMS,LMS)<br>Categorized by the length of template body, if value is unavailable.  |
|- resendTitle| String| X| Title of alternative delivery for LMS<br/>(resent with PlusFriend ID if value is unavailable.)  |
|- resendContent|    String| X| Alternative delivery content<br/>(resent with [Message body and web link button name - web link mobile link] if value is unavailable.)  |
|- resendSendNo|  String| X| Sender number for alternative delivery<br/><span style="color:red">(Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span>  |
|- buttons| List|   X| Additional information for buttons |
|-- ordering            | Integer  | X        | Button sequence (required, if there is a button)|
|-- chatExtra|  String| X| Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons |
|-- chatEvent|  String| X| Bot event name to connect for BT (Bot Transfer) type button |
|-- target| String| X | In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
|- recipientGroupingKey|    String| X|  Recipient's grouping key (up to 100 characters) |
|messageOption | Object |   X | Message Option |
|- price | Integer |    X | Price/amount/payment amount included in message (message to be delivered to user)(related to moment advertisement) |
|- currencyType | String |  X| Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message (message to be delivered to the user) (related to moment advertisement) |

* <b> Request date and time can be set up to 90 days since a point of calling. </b>
* <b>Since alternative delivery is made in the SMS service, field values must follow the API specifications for SMS (e.g. Sender number registered at the SMS service, or restriction in the field length). </b>
* <b>Title or content for alternative delivery that exceeds specified byte size may be cut for delivery. (see [[Caution](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] for reference)</b>

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|message|   Object| Body area|
|- requestId | String | Request ID |
|- senderGroupingKey | String | Sender's grouping key |
|- sendResults | Object | Result of delivery request |
|-- recipientSeq | Integer | Recipient sequence number |
|-- recipientNo | String | Recipient number |
|-- resultCode | Integer | Result code of delivery request |
|-- resultMessage | String | Result message of delivery request |
|-- recipientGroupingKey | String | Recipient's grouping key |

### Request of Sending Full Text

[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/auth/raw-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

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

| Name |  Type| Required| Description|
|---|---|---|---|
|senderKey| String| O | Sender key (40 characters) |
|templateCode|  String| O | Registered delivery template code (up to 20 characters) |
|requestDate| String | X| Date and time of request (yyyy-MM-dd HH:mm)<br>(sent immediately, if it is left blank) |
|senderGroupingKey| String | X| Sender's grouping key (up to 100 characters) |
|createUser | String | X | Registrant (saved as user UUID when sending from console) |
|recipientList| List|   O|  List of recipients (up to 1,000 persons) |
|- recipientNo| String| O|  Recipient number (up to 15 characters) |
|- content| String| O|  Body message (up to 1000 characters) |
|- templateTitle| String | X| Title (up to 50 characters) |  
|- buttons| List |  X | List of buttons (up to 5) |
|-- ordering|   Integer|    X | Button sequence (required if there a button)|
|-- type| String |  X | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added) |
|-- name| String |  X | Button name (required if there is a button, for up to 14 characters)|
|-- linkMo| String |    X | Mobile web link (required for the WL type, for up to 200 characters)|
|-- linkPc | String |   X |PC web link (required for the WL type, for up to 200 characters) |
|-- schemeIos | String | X |    iOS app link (required for the AL type, for up to 200 characters) |
|-- schemeAndroid | String | X |    Android app link (required for the AL type, for up to 200 characters) |
|-- chatExtra|  String| X| Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons |
|-- chatEvent|  String| X| Bot event name to connect for BT (Bot Transfer) type button |
|-- target| String| X | In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
|- resendParameter| Object| X| Alternative delivery information |
|- isResend|   boolean|    X|  Whether to resend text, if delivery fails<br/>Resent by default, if alternative delivery is set on console. |
|- resendType| String| X|  Alternative delivery type (SMS,LMS)<br>Categorized by the length of template body, if value is unavailable. |
|- resendTitle|    String| X|  Title of alternative delivery for LMS<br/>(resent with PlusFriend ID if value is unavailable.) |
|- resendContent|  String| X|  Alternative delivery content<br/>(resent with [Message body and web link button name - web link mobile link] if value is unavailable.) |
|- resendSendNo | String| X| Sender number for alternative delivery<br/><span style="color:red">(Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span> |
|- recipientGroupingKey|    String| X|  Recipient's grouping key (up to 100 characters) |
|messageOption | Object |   X | Message Option |
|- price | Integer |    X | Price/amount/payment amount included in message (message to be delivered to user)(related to moment advertisement) |
|- currencyType | String |  X| Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message (message to be delivered to the user) (related to moment advertisement) |

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|message|   Object| Body area|
|- requestId | String | Request ID |
|- senderGroupingKey | String | Sender's grouping key |
|- sendResults | Object | Result of delivery request |
|-- recipientSeq | Integer | Recipient sequence number |
|-- recipientNo | String | Recipient number |
|-- resultCode | Integer | Result code of delivery request |
|-- resultMessage | String | Result message of delivery request |
|-- recipientGroupingKey | String | Recipient's grouping key |

### List Messages

#### Request

[URL]

```
GET  /alimtalk/v2.2/appkeys/{appkey}/auth/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Query parameter] No. 1 or (2, 3) is conditionally required

| Name |  Type| Required| Description|
|---|---|---|---|
|requestId| String| Conditionally required (no.1) | Request ID |
|startRequestDate|  String| Conditionally required (no.2) | Start date of delivery request (yyyy-MM-dd HH:mm)|
|endRequestDate|    String| Conditionally required (no.2) |    End date of delivery request (yyyy-MM-dd HH:mm) |
|startCreateDate|  String| Conditionally required (no.3) | Start date of registration (mm:HH dd-MM-yyyy)|
|endCreateDate|  String| Conditionally required (no.3) | End date of registration (mm:HH dd-MM-yyyy) |
|recipientNo|   String| X | Recipient number |
|senderKey   |  String| X | Sender Key |
|templateCode|  String| X | Template code|
|senderGroupingKey| String | X| Sender's grouping key |
|recipientGroupingKey|  String| X|  Recipient's grouping key |
|messageStatus| String |    X | Request status (COMPLETED -> successful, FAILED -> failed, CANCEL -> canceled )   |
|resultCode| String |   X | Delivery result (MRC01 -> successful, MRC02 -> failed )   |
|createUser | String | X | Registrant (saved as user UUID when sending from console) |
|pageNum|   Integer|    X|  Page number (default: 1)|
|pageSize|  Integer|    X|  Number of queries (default: 15, max : 1000)|

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|messageSearchResultResponse|   Object| Body area|
|- messages | List |    List of messages |
|-- requestId | String |    Request ID |
|-- recipientSeq | Integer |    Recipient sequence number |
|-- plusFriendId | String | PlusFriend ID |
|-- senderKey    | String | Sender Key    |
|-- templateCode | String | Template code |
|-- recipientNo | String |  Recipient number |
|-- content | String |  Body message |
|-- requestDate | String | Date and time of request |
|-- createDate | String |   Registered date and time |
|-- receiveDate | String |  Date and time of receiving |
|-- resendStatus | String | Status code of resending (RSC01, RSC02, RSC03, RSC04, RSC05)<br>([Refer to [Status code of resending table](http://docs.toast.com/en/Notification/KakaoTalk%20Bizmessage/en/alimtalk-api-guide/#smslms)] below) |
|-- resendStatusName | String | Status code name of resending |
|-- messageStatus | String |    Request status (COMPLETED -> successful, FAILED ->failed, CANCEL -> canceled ) |
|-- resultCode | String |   Result code of receiving |
|-- resultCodeName | String |   Result code name of receiving |
|-- createUser | String | Registrant (saved as user UUID when sending from console) |
|-- buttons | List |    List of buttons |
|--- ordering | Integer |   Button sequence |
|--- type | String |    Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added) |
|--- name | String |    Button name |
|--- linkMo | String |  Mobile web link (required for the WL type) |
|--- linkPc | String |  PC web link (optional for the WL type) |
|--- schemeIos | String |   iOS app link (required for the AL type) |
|--- schemeAndroid | String |   Android app link (required for the AL type) |
|--- chatExtra| String| Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons |
|--- chatEvent| String| Bot event name to connect for BT (Bot Transfer) type button |
|--- target|    String| In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
|-- senderGroupingKey | String | Sender's grouping key |
|-- recipientGroupingKey | String | Recipient's grouping key |
|- totalCount | Integer | Total count |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/auth/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

### Get Messages

#### Request

[URL]

```
GET  /alimtalk/v2.2/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey |
|requestId| String| Request ID |
|recipientSeq|  Integer|    Recipient sequence number |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|message|   Object| Message|
|- requestId | String | Request ID |
|- recipientSeq | Integer | Recipient sequence number |
|- plusFriendId | String |  PlusFriend ID |
|- senderKey    | String |  Sender Key    |
|- templateCode | String |  Template code |
|- recipientNo | String |   Recipient number |
|- content | String |   Body message |
|- templateTitle | String | Template title |
|- templateSubtitle | String | Auxiliary template phrase |
|- templateExtra | String | Additional template information |
|- templateAd | String | Request for consent of receiving within template or simple ad phrases |
|- requestDate | String | Date and time of request |
|- createDate | String |    Registered date and time |
|- receiveDate | String |   Date and time of receiving |
|- resendStatus | String |  Status code of resending (RSC01, RSC02, RSC03, RSC04, RSC05)<br>([Refer to [Status code of resending table](http://docs.toast.com/en/Notification/KakaoTalk%20Bizmessage/en/alimtalk-api-guide/#smslms)] below) |
|- resendStatusName | String |  Status code name of resending |
|- resendResultCode | String | Result code of resending [Result code of SMS sending](https://docs.toast.com/en/Notification/SMS/en/error-code/#api) |
|- resendRequestId | String | ID requesting of resending SMS |
|- messageStatus | String | Request status (COMPLETED -> successful, FAILED -> failed, CANCEL -> canceled) |
|- resultCode | String |    Result code of receiving |
|- resultCodeName | String |    Result code name of receiving |
|- createUser | String | Registrant (saved as user UUID when sending from console) |
|- buttons | List | List of buttons |
|-- ordering | Integer |    Button sequence |
|-- type | String | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK:Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added) |
|-- name | String | Button name |
|-- linkMo | String |   Mobile web link (required for the WL type) |
|-- linkPc | String |   PC web link (optional for the WL type) |
|-- schemeIos | String |    iOS app link (required for the AL type) |
|-- schemeAndroid | String |    Android app link (required for the AL type) |
|-- chatExtra|  String| Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons |
|-- chatEvent|  String| Bot event name to connect for BT (Bot Transfer) type button |
|-- target| String| In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
|- messageOption | Object | Message Option |
|-- price | Integer |   Price/amount/payment amount included in message (message to be delivered to user)(related to moment advertisement) |
|-- currencyType | String | Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message (message to be delivered to the user) (related to moment advertisement) |
|- senderGroupingKey | String | Sender's grouping key |
|- recipientGroupingKey | String |  Recipient's grouping key |

## Messages
### Cancel Sending Messages

#### Request

[URL]

```
DELETE  /alimtalk/v2.2/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|
|requestId| String| Request ID|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| Name |  Type| Required| Description|
|---|---|---|---|
|recipientSeq|  String| X | Recipient sequence number<br>(to cancel all deliveries of request ID, if the value is left blank) |

* Both general and authentication messages can be canceled by the same API.

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|

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

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| Name |  Type| Required| Description|
|---|---|---|---|
|startUpdateDate|   String| O | Start date of querying result updates (yyyy-MM-dd HH:mm)|
|endUpdateDate| String| O | End date of querying result updates (yyyy-MM-dd HH:mm) |
|alimtalkMessageType|   String| X | Alimtalk message type (NORMAL, AUTH) |
|pageNum|   Integer|    X|  Page number (default: 1)|
|pageSize|  Integer|    X|  Number of queries (default: 15, max : 1000)|

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|messages|  List|   List of messages|
|- requestId | String | Request ID |
|- recipientSeq | Integer | Recipient sequence number |
|- requestDate | String |   Date and time of request |
|- createDate  | String |   Date and time of creation |
|- receiveDate | String |   Date and time of receiving |
|- resendStatus | String |  Status code of resending (RSC01, RSC02, RSC03, RSC04, RSC05)<br>([Refer to [Status code of resending table](http://docs.toast.com/en/Notification/KakaoTalk%20Bizmessage/en/alimtalk-api-guide/#smslms)] below) |
|- resendStatusName | String |  Status code name of resending |
|- resendResultCode | String | Result code of resending [Result code of SMS sending](https://docs.toast.com/en/Notification/SMS/en/error-code/#api) |
|- resendRequestId | String | Resending SMS request ID |
|- messageStatus | String | Request status (COMPLETED -> Successful, FAILED -> Failed, CANCEL -> Canceled) |
|- resultCode | String |    Result code of receiving |
|- resultCodeName | String |    Result code name of receiving |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

### Status Code of SMS/LMS Resending
| Name |  Description|
|---|---|
|RSC01| No target of resending|
|RSC02| Target of resending (If sending fails, resending is performed.)|
|RSC03| Resending in progress|
|RSC04| Resending successful|
|RSC05| Resending failed|

## Mass Delivery
### List Mass Delivery Requests

#### Request
[URL]
```
GET /alimtalk/v2.2/appkeys/{appKey}/mass-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appKey|    String| Unique appkey|

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name |  Type| Description|
|---|---|---|
|X-Secret-Key|  String| Unique secret key |

[Query parameter]
* One of the following is required: requestId, startRequestDate + endRequestDate, or startCreateDate + endCreateDate.

| Name |  Type| Max. Length | Required| Description|
|---|---|---|---|---|
| requestId | String | - | O | Request ID |
| startRequestDate | String | - | O | Start date of delivery |
| endRequestDate | String | - | O | End date of delivery |
| startCreateDate | String| - | O | Start date of registration |
| endCreateDate |   String| - | O | End date of registration |
| pageNum | optional, Integer | - | X | Page number |
| pageSize | optional, Integer | 1000 | X | Search count |

#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### Response
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

| Name |  Type| Description|
|---|---|---|
| header | Object | Header area |
| - resultCode |    Integer |   Result code |
| - resultMessage | String | Result message |
| - isSuccessful |  Boolean | Successful or not |
| body | Object | Body area |
| - messages | Object | List of messages |
| -- requestId | String | Request ID |
| -- requestDate | String | Date of request |
| -- createDate | String | Date of creation |
| -- createUser | String | Date of creation |
| -- plusFriendId | String | PlusFriend ID |
| -- senderKey | String| Sender key (40 characters) |
| -- masterStatusCode | String | Mass delivery status code (WAIT, READY, SENDREADY, SENDWAIT, SENDING, COMPLETE, CANCEL, FAIL) |
| -- content | String | Content |
| -- buttons | List | List of buttons |
| --- ordering | String | Button sequence |
| --- type | String | Button type<br/> - WL: Web Link<br/> - AL: App Link<br/> - DS: Delivery Search<br/> - BK: Bot Keyword<br/> - MD: Message Delivery<br/> - BC: Bot for Consultation<br/> - BT: Bot Transfer<br/> - AC: Channel Added [only for Ad Included/Mixed Purposes Type] |
| --- name | String | Button name |
| --- linkMo | String | Mobile web link (required for the WL type) |
| --- linkPc | String | PC web link (optional for the WL type)|
| --- schemeIos | String | iOS app link (required for the AL type) |
| --- schemeAndroid | String | Android app link (required for the AL type) |
| --- chatExtra | String | BC: Meta information to be delivered when switching to consultation talk<br/> BT: Meta information to be delivered when switching to bot |
| --- chatEvent | String | BT: Bot event name to connect when switching to bot |
| --- target|   String| In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
| -- fileId | String | Attachment ID |
| -- templateCode | String | Template code (up to 20 characters) |
| -- templateExtra | String | Additional template information (Required, if template message type is[Ad Included/Mixed Purposes]) |
| -- templateAd | String | Request for consent of receiving within template or simple ad phrases (Required, if template message type is [Ad Included/Mixed Purposes] |
| -- tempalteTitle| String | Template title (No more than 50 characters, Android: To be abbreviated if it exceeds 2 lines with more than 23 characters, iOS: To be abbreviated if it exceeds 2 lines with more than 27 characters) |
| -- templateSubtitle| String | Auxiliary template phrase (No more than 50 characters, Android: To be abbreviated if it exceeds 18 characters, iOS: To be abbreviated if it exceeds 21 characters) |
| -- autoSendYn | String | Auto sending or not |
| -- statsId | String | Statistics ID |
| -- createDate | String | Date of creation |
| -- createUser | String | User who created the request (saved as user UUID when sending from console) |
| - totalCount | Integer | Total count |


### List Mass Delivery Recipients

#### Request
[URL]
```
GET /alimtalk/v2.2/appkeys/{appKey}/mass-messages/{requestId}/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
| appKey |  String |    Unique appkey |
| requestId |   String |    Request ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name |  Type| Description|
|---|---|---|
| X-Secret-Key |    String| Unique secret key |


| Name |  Type| Max. Length | Required| Description|
|---|---|---|---|---|
| requestId | String | - | O | Request ID |
| startRequestDate | String | - | X | Start date of delivery |
| endRequestDate | String | - | X | End date of delivery |
| startCreateDate | String| - | X | Start date of registration |
| endCreateDate |   String| - | X | End date of registration |
| pageNum | optional, Integer | - | X | Page Number |
| pageSize | optional, Integer | 1000 | X | Search count |

#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### Response
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

| Name |  Type| Description|
|---|---|---|
| header | Object | Header area |
| - resultCode |    Integer |   Result code |
| - resultMessage | String | Result message |
| - isSuccessful |  Boolean | Successful or not |
| body | Object | Body area |
| - messages | Object | List of messages |
| -- requestId | String | Request ID |
| -- recipientSeq | String | Recipient sequence number |
| -- recipientNo | String | Recipient number |
| -- requestDate | String | Date of request |
| -- receiveDate | String | Date of receiving |
| -- messageStatus | String | Message status |
| -- resultCode | String | Result code |
| -- resultCodeName | String | Result code content |
| - totalCount | Integer | Total count |

### Get a Mass Delivery Recipient

#### Request
[URL]
```
GET /alimtalk/v2.2/appkeys/{appKey}/mass-messages/{requestId}/recipients/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
| appKey |  String | Unique appkey |
| requestId |   String | Request ID |
| recipientSeq | String | Recipient sequence |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name |  Type| Description|
|---|---|---|
|X-Secret-Key|  String| Unique secret key |


| Name |  Type| Max. Length | Required| Description|
|---|---|---|---|---|
| requestId | String | - | O | Request ID |
| startRequestDate | String | - | X | Start date of delivery |
| endRequestDate | String | - | X | End date of delivery |
| startCreateDate | String| - | X | Start date of registration |
| endCreateDate |   String| - | X | End date of registration |
| pageNum | optional, Integer | - | X | Page number |
| pageSize | optional, Integer | 1000 | X | Search count |

#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients/1?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### Response
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

| Name |  Type| Description|
|---|---|---|
| header | Object | Header area |
| - resultCode |    Integer |   Result code |
| - resultMessage | String | Result message |
| - isSuccessful |  Boolean | Successful or not |
| body | Object | Body area |
| - requestId | String | Request ID |
| - recipientSeq | String | Recipient sequence number |
| - plusFriendId | String | PlusFriend ID |
| - senderKey | String | Sender ID |
| - templateCode |  String | Template code (up to 20 characters) |
| - recipientNo | String | Recipient number |
| - content | String | Content |
| - tempalteTitle| String | Template title (No more than 50 characters, Android: To be abbreviated if it exceeds 2 lines with more than 23 characters, iOS: To be abbreviated if it exceeds 2 lines with more than 27 characters) |
| - templateSubtitle| String | Auxiliary template phrase (No more than 50 characters, Android: To be abbreviated if it exceeds 18 characters, iOS: To be abbreviated if it exceeds 21 characters) |
| - templateExtra | String | Additional template information (Required, if template message type is[Ad Included/Mixed Purposes]) |
| - templateAd | String | Request for consent of receiving within template or simple ad phrases (Required, if template message type is[Ad Included/Mixed Purposes] |
| - requestDate | String | Date of request |
| - receiveDate | String | Date of receiving |
| - createDate | String | Date of creation |
| - resendStatus | String | Status code of resending (RSC01, RSC02, RSC03, RSC04, RSC05)<br>([Refer to [Status code of resending table](http://docs.toast.com/en/Notification/KakaoTalk%20Bizmessage/en/alimtalk-api-guide/#smslms)] below) |
| - resendStatusName | String | Status name of resending |
| - resendResultCode | String | Result code of resending [Result code of SMS sending](https://docs.toast.com/en/Notification/SMS/en/error-code/#api) |
| - resendRequestId | String | Resending SMS request ID |
| - messageStatus | String | Mass recipient delivery status code (READY, COMPLETED, FAILED, CANCEL) |
| - resultCode | String | Result status code |
| - resultCodeName | String | Result status name |
| - createUser | String | User who created the request (saved as user UUID when sending from console) |
| - buttons | List | List of buttons |
| -- ordering | String | Button sequence |
| -- type | String | Button type<br/> - WL: Web Link<br/> - AL: App Link<br/> - DS: Delivery Search<br/> - BK: Bot Keyword<br/> - MD: Message Delivery<br/> - BC: Bot for Consultation<br/> - BT: Bot Transfer<br/> - AC: Channel Added [only for Ad Included/Mixed Purposes Type] |
| -- name | String | Button name |
| -- linkMo | String | Mobile web link (required for the WL type) |
| -- linkPc | String | PC web link (optional for the WL type)|
| -- schemeIos | String | iOS app link (required for the AL type) |
| -- schemeAndroid | String | Android app link (required for the AL type) |
| -- chatExtra | String | BC: Meta information to be delivered when switching to consultation talk<br/> BT: Meta information to be delivered when switching to bot |
| -- chatEvent | String | BT: Bot event name to connect when switching to bot |
| -- target|    String| In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
| - messageOption | Boolean | Message Option |
|-- price | Integer |   Price/amount/payment amount included in message (message to be delivered to user) (related to moment advertisement) |
|-- currencyType | String | Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message (message to be delivered to the user) (related to moment advertisement) |

## Templates

### List Template Categories
#### Request
[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/template/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|categories|    List|   List of categories |
|- name | String | Category name |
|- subCategories | List |   List of subcategories |
|-- code | String | Category code (Used when registering/modifying templates) |
|-- name | String | Category name |
|-- groupName | String |    Category group name |
|-- inclusion | String |    Description of templates to which the category applies |
|-- exclusion| String| Description of templates to which the category does not apply |

### Register Templates
#### Request
[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey |
|senderKey| String| Sender Key |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

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

| Name |  Type| Required| Description|
|---|---|---|---|
|templateCode|  String |    O | Template code (up to 20 characters) |
|templateName|  String |    O | Template name (up to 20 characters) |
|templateContent|   String |    O | Template body (up to 1000 characters) |
|templateMessageType| String | X |Types of template message (BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes, default: Basic) |
|templateEmphasizeType| String| X| Types of emphasized template (NONE: Basic, TEXT: Emphasized, IMAGE: Image type, default:NONE)<br>- TEXT: templateTitle and templateSubtitle fields are required <br>IMAGE: templateImageName and templateImageUrl fields are required|
|templateExtra | String | X | Additional template information (Required, if template message type is[Ad Included/Mixed Purposes]) |
|templateAd | String | X | Request for consent of receiving within template or simple ad phrases (Required, if template message type is[Ad Included/Mixed Purposes]) |
|tempalteTitle| String | X| Template title (No more than 50 characters, Android: To be abbreviated if it exceeds 2 lines with more than 23 characters, iOS: To be abbreviated if it exceeds 2 lines with more than 27 characters) |
|templateSubtitle| String | X| Auxiliary template phrase (No more than 50 characters, Android: To be abbreviated if it exceeds 18 characters, iOS: To be abbreviated if it exceeds 21 characters) |
|templateImageName | String |   X | Image name (name of uploaded file) |
|templateImageUrl | String |    X | Image URL |
|securityFlag| Boolean | X| Security template<br>Set for security messages such as OTP<br>If set, message text is unexposed to all devices except for the main device at the time of sending (default: false) |
|categoryCode| String | X | Template category code (Refer to API to View Template Category, default: 999999)<br>For other categories, screened by the lowest priority. |
|buttons|   List |  X | List of buttons (up to 5) |
|-ordering| Integer |   X | Button sequence (1~5) |
|-type| String |    X | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added [only for Ad Included/Mixed Purposes Type]) |
|-name| String |    X | Button name (required, if there's a button, up to 14 characters)|
|-linkMo| String |  X | Mobile web link (required for the WL type, up to 200 characters)|
|-linkPc | String | X |PC web link (optional for the WL type, up to 200 characters) |
|-schemeIos | String | X |  iOS app link (required for the AL type, up to 200 characters) |
|-schemeAndroid | String | X |  Android app link (required for the AL type, up to 200 characters) |

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|

### Modify Templates
#### Request
[URL]

```
PUT  /alimtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey |
|senderKey| String| Sender Key |
|templateCode|  String| Template code |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

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

| Name |  Type| Required| Description|
|---|---|---|---|
|templateName|  String |    O | Template name (up to 20 characters) |
|templateContent|   String |    O | Template body (up to 1000 characters) |
|templateMessageType| String | X | Types of template message (BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes, default: Basic) |
|templateEmphasizeType| String| X| Types of emphasized template (NONE: Basic, TEXT: Emphasized, IMAGE: Image type, default:NONE)<br>- TEXT: templateTitle and templateSubtitle fields are required <br>IMAGE: templateImageName and templateImageUrl fields are required|
|templateExtra | String | X | Additional template information (Required, if template message type is[Ad Included/Mixed Purposes]) |
|templateAd | String | X | Request for consent of receiving within template or simple ad phrases (Required, if template message type is[Ad Included/Mixed Purposes] |
|tempalteTitle| String | X| Template title (No more than 50 characters, Android: To be abbreviated if it exceeds 2 lines with more than 23 characters, iOS: To be abbreviated if it exceeds 2 lines with more than 27 characters) |
|templateSubtitle| String | X| Auxiliary template phrase (No more than 50 characters, Android: To be abbreviated if it exceeds 18 characters, iOS: To be abbreviated if it exceeds 21 characters) |
|templateImageName | String |   X | Image name (name of uploaded file) |
|templateImageUrl | String |    X | Image URL |
|securityFlag| Boolean | X| Security template<br>Set for security messages such as OTP<br>If set, message text is unexposed to all devices except for the main device at the time of sending (default: false) |
|categoryCode| String | X | Template category code (Refer to API to View Template Category, default: 999999)<br>For other categories, screened by the lowest priority. |
|buttons|   List |  X | List of buttons (up to 5) |
|-ordering| Integer |   X | Button sequence (1~5) |
|-type| String |    X | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added [only for Ad Included/Mixed Purposes Type]) |
|-name| String |    X | Button name (required, if there's a button, up to 14 characters)|
|-linkMo| String |  X | Mobile web link (required for the WL type, up to 200 characters)|
|-linkPc | String | X |PC web link (optional for the WL type, up to 200 characters) |
|-schemeIos | String | X |  iOS app link (required for the AL type, up to 200 characters) |
|-schemeAndroid | String | X |  Android app link (required for the AL type, up to 200 characters) |

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|

### Delete Templates
#### Request
[URL]

```
DELETE  /alimtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|
|senderKey| String| Sender Key |
|templateCode|  String| Template code |

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|

### Inquire of Templates
#### Request
[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/comments
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|
|senderKey| String| Sender Key |
|templateCode|  String| Template code |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "comment" : String
}
```

| Name |  Type| Required| Description|
|---|---|---|---|
|comment|   String |    O | Inquiries |

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|

### Attach files to send inquiry on templates
#### Request
[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/comments_file
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|
|senderKey| String| Sender Key |
|templateCode|  String| Template code |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "comment" : String,
  "attachments" : File
}
```

| Name |  Type| Required| Description|
|---|---|---|---|
|comment|   String |    O | Content of Inquiry |
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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header ARea|
|- resultCode|  Integer|    Result Code|
|- resultMessage|   String| Result Message|
|- isSuccessful|    Boolean| Successful or not|

### List Templates

#### Request

[URL]

```
GET  /alimtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|
|senderKey| String| Sender Key |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| Name |  Type| Required| Description|
|---|---|---|---|
|templateCode|  String| X | Template code|
|templateName|  String| X | Template name|
|templateStatus| String |   X | Template status code|
|pageNum|   Integer|    X|  Page number (default:1)|
|pageSize|  Integer|    X|  Number of queries (default: 15, max : 1000)|

|Template Status Code| Description|
|---|---|
| TSC01 | Requested |
| TSC02 | Inspecting |
| TSC03 | Approved |
| TSC04 | Returned |

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
              "templateMessageType" : String,
              "templateEmphasizeType": String,
              "templateContent": String,
              "templateExtra" : String,
              "templateAd" : String,
              "templateTitle" : String,
              "templateSubtitle" : String,
              "templateImageName" : String,
              "templateImageUrl" : String,
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
                "securityFlag": Boolean,
                "categoryCode": String,
                "createDate": String,
                "updateDate": String
            }
        ],
        "totalCount": Integer
    }
}
```

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|templateListResponse|  Object| Body area|
|- templates | List |   Template list |
|-- plusFriendId | String | PlusFriend ID |
|-- senderKey    | String | Sender Key    |
|-- plusFriendType | String | PlusFriend type (NORMAL, GROUP) |
|-- templateCode | String | Template code |
|-- templateName | String | Template name |
|-- templateMessageType| String | Types of template message (BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes) |
|-- templateEmphasizeType| String| Types of emphasized template (NONE: Basic, TEXT: Emphasized, IMAGE: Image type, default:NONE) |
|-- templateContent | String |  Template body |
|-- templateExtra | String | Additional template information |
|-- templateAd | String | Request for consent of receiving within template or simple ad phrases |
|-- tempalteTitle| String | Template title |
|-- templateSubtitle| String | Auxiliary template phrase |
|-- templateImageName | String | Image name (name of uploaded file) |
|-- templateImageUrl | String | Image URL |
|-- buttons | List |    List of buttons |
|--- ordering | Integer |   Button sequence (1~5) |
|--- type | String |    Button type (WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added) |
|--- name | String |    Button name |
|--- linkMo | String |  Mobile web link (required for the WL type) |
|--- linkPc | String |  PC web link (optional for the WL type) |
|--- schemeIos | String |   iOS app link (required for the AL type) |
|--- schemeAndroid | String |   Android app link (required for the AL type) |
|-- comments | List | Inspection result |
|--- id | Integer | Inquiry ID |
|--- content |  String | Inquiry content |
|--- userName | String | Creator |
|--- createAt | String | Date of registration |
|--- attachment | List | Attachment |
|---- originalFileName | String | Attachment file name |
|---- filePath | String | Attachment file path |
|--- status | String | Comment status (INQ: Inquired, APR: Approved, REJ: Returned, REP: Replied) |
|-- status| String | Template status |
|-- statusName | String | Template status name |
|-- securityFlag| Boolean | Whether it is a security template |
|-- categoryCode| String | Template category code  |
|-- createDate | String | Date of creation |
|-- updateDate | String | Date of modification |
|- totalCount | Integer | Total count |

### List Template modifications

#### Request

[URL]

```
GET  /alimtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/modifications
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|
|senderKey| String| Sender Key |
|templateCode|  String| Template code |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

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
              "templateMessageType" : String,
              "templateEmphasizeType": String,
              "templateContent": String,
              "templateExtra" : String,
              "templateAd" : String,
              "templateTitle" : String,
              "templateSubtitle" : String,
              "templateImageName" : String,
              "templateImageUrl" : String,
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
                "securityFlag": Boolean,
                "categoryCode": String,
                "activated": boolean,
                "createDate": String,
                "updateDate": String
            }
        ],
        "totalCount": Integer
    }
}
```

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|templateModificationsResponse| Object| Body area|
|- templates | List |   Template list |
|-- plusFriendId | String | ID for KakaoTalk channel search or sending profile group name  |
|-- senderKey    | String | Sender Key    |
|-- plusFriendType | String | PlusFriend type (NORMAL, GROUP) |
|-- templateCode | String | Template code |
|-- templateName | String | Template name |
|-- templateMessageType| String | Types of template message (BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes) |
|-- templateEmphasizeType| String| Types of emphasized template (NONE: Basic, TEXT: Emphasized, IMAGE: Image type, default:NONE) |
|-- templateContent | String |  Template body |
|-- templateExtra | String | Additional template information |
|-- templateAd | String | Request for consent of receiving within template or simple ad phrases |
|-- tempalteTitle| String | Template title |
|-- templateSubtitle| String | Auxiliary template phrase |
|-- templateImageName | String | Image name (name of uploaded file) |
|-- templateImageUrl | String | Image URL |
|-- buttons | List |    List of buttons |
|--- ordering | Integer |   Button sequence (1~5) |
|--- type | String |    Button type (WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added) |
|--- name | String |    Button name |
|--- linkMo | String |  Mobile web link (required for the WL type) |
|--- linkPc | String |  PC web link (optional for the WL type) |
|--- schemeIos | String |   iOS app link (required for the AL type) |
|--- schemeAndroid | String |   Android app link (required for the AL type) |
|-- comments | List | Inspection result |
|--- id | Integer | Inquiry ID |
|--- content |  String | Inquiry content |
|--- userName | String | Creator |
|--- createAt | String | Date of registration |
|--- attachment | List | Attachment |
|---- originalFileName | String | Attachment file name |
|---- filePath | String | Attachment file path |
|--- status | String | Comment status (INQ: Inquired, APR: Approved, REJ: Returned, REP: Replied) |
|-- status| String | Template status |
|-- statusName | String | Template status name |
|-- securityFlag| Boolean | Whether it is a security template |
|-- categoryCode| String | Template category code  |
|-- activated | Boolean | Activated or not |
|-- createDate | String | Date of creation |
|-- updateDate | String | Date of modification |
|- totalCount | Integer | Total count |

### Register Template Image
#### Request
[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/template-image
Content-Type: multipart/form-data
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Request parameter]

| Name |  Type| Required| Description|
|---|---|---|---|
|file|  File|   O | Template image file |

[Example]
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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|templateImage| Object| Body area|
|- templateImageName | String | Image name (name of uploaded file) |
|- templateImageUrl | String |  Image URL |

## Manage Alternative Delivery
### Register an SMS AppKey

[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/failback/appkey
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |


[Request body]

```
{
    "resendAppKey": String
}
```

| Name |  Type| Required| Description|
|---|---|---|---|
|resendAppKey|  String| O | SMS service appkey to set for alternative delivery |

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
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

### Register Alternative Delivery Settings

[URL]

```
POST  /alimtalk/v2.2/appkeys/{appkey}/failback
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |


[Request body]

```
{  
   "senderKey": String,
   "isResend": Boolean,
   "resendSendNo": String
}
```

| Name |  Type| Required| Description|
|---|---|---|---|
|senderKey| String| O | Sender Key |
|isResend|  Boolean|    O | Whether to resend text, if delivery fails<br/>Resent by default, if alternative delivery is set on console. |
|resendSendNo|  String| O | Sender number for alternative delivery<br/><span style="color:red">(Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span> |

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appkey}/failback/appkey -d '{"senderKey": "0be23c29de88d6888798aeda57062516354d74ba","isResend": true,"resendSendNo": "01012341234" }
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
