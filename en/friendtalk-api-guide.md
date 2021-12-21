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
1. Added Friendtalk mass delivery query API.
2. Added `chatExtra`, `chatEvent`, and `target` fields to the buttons field when sending messages.
3. Added `chatExtra`, `chatEvent`, and `target` fields to the buttons field when querying messages.

## Send Messages
#### Request of Sending

[URL]

```
POST  /friendtalk/v2.2/appkeys/{appkey}/messages
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

| Name |  Type| Required| Description|
|---|---|---|---|
|senderKey| String| O | Sender key (40 characters) |
|requestDate|   String| X | Date and time of request (yyyy-MM-dd HH:mm), to be sent immediately if field is not sent |
|senderGroupingKey| String | X| Sender's grouping key (up to 100 characters) |
| createUser | String | X | Registrant (saved as user UUID when sending from console) |
|recipientList| List|   O|  List of recipients (up to 1000) |
|- recipientNo| String| O|  Recipient number |
|- content| String| O| Body message (up to 1000 characters)<br>Up to 400, if image is included<br>Up to 76, if wide image is included |
|- imageSeq|    Integer|    X|  Image number |
|- imageLink|   String| X|  Image link   |
|- buttons| List|   X|  Button<br>1 link button, if wide image is included |
|-- ordering|   Integer|    X | Button sequence (required, if there is a button)|
|-- type| String |  X | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery) |
|-- name| String |  X | Button name (required, if there is a button)|
|-- linkMo| String |    X | Mobile web link (required for the WL type)|
|-- linkPc | String |   X |PC web link (optional for the WL type) |
|-- schemeIos | String | X |    iOS app link (required for the AL type) |
|-- schemeAndroid | String | X |    Android app link (required for the AL type) |
|-- chatExtra|  String| X| Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons |
|-- chatEvent|  String| X| Bot event name to connect for BT (Bot Transfer) type button |
|-- target| String| X | In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
|- resendParameter| Object| X| Alternative delivery information |
|-- isResend|   boolean|    X|  Whether to resend text, if delivery fails<br/>Resent by default, if alternative delivery is set on console. |
|-- resendType| String| X|  Alternative delivery type (SMS,LMS)<br>Categorized by the length of template body, if value is unavailable. |
|-- resendTitle|    String| X|  Title of alternative delivery for LMS<br/>(resent with PlusFriend ID if value is unavailable.) |
|-- resendContent|  String| X|  Alternative delivery content<br/>(resent with [Message body and web link button name - web link mobile link] if value is unavailable.) |
|-- resendSendNo | String| X| Sender number for alternative delivery (up to 13 characters)<br><span style="color:red"> (Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span> |
|-- resendUnsubscribeNo | String| X| Alternative delivery 080 blocked number <br> <span style="color:red"> (If it is not the 080 blocked number registered in the SMS service, alternative delivery may fail.) </span> |
|- isAd | Boolean | X | Ad or not (default is true) |
|- recipientGroupingKey|    String| X|  Recipient's grouping key (up to 100 characters) |

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

## List Deliveries

#### Request

[URL]

```
GET  /friendtalk/v2.2/appkeys/{appkey}/messages
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

[Query parameter] No.1 or (2, 3) is conditionally required

| Name |  Type| Required| Description|
|---|---|---|---|
|requestId| String| Conditionally required (no.1) | Request ID |
|startRequestDate|  String| Conditionally required (no.2) | Start date of delivery request (yyyy-MM-dd HH:mm)|
|endRequestDate|    String| Conditionally required (no.2) | End date of delivery request (yyyy-MM-dd HH:mm) |
|startCreateDate| String| Conditionally required (no.3) | Start date of registration (mm:HH dd-MM-yyyy)|
|endCreateDate|  String| Conditionally required (no.3) |  End date of registration (mm:HH dd-MM-yyyy) |
|recipientNo|   String| X | Recipient number |
|senderKey| String| X | Sender key |
|senderGroupingKey| String | X| Sender's grouping key |
|recipientGroupingKey|  String| X|  Recipient's grouping key |
|messageStatus| String |    X | Request status (COMPLETED: successful, FAILED: failed)   |
|resultCode| String |   X | Delivery result (MRC01: successful, MRC02: failed) |
|createUser| String | X | Registrant (saved as user UUID when sending from console) |
|pageNum|   Integer|    X|  Page number (default: 1)|
|pageSize|  Integer|    X|  Number of queries (default: 15, max: 1000)|

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
          "recipientNo" :  String,
          "requestDate" : String,
          "createDate" : String,
          "receiveDate" : String,
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
|-- plusFriendId | String | Plus Friend ID |
|-- senderKey   | String | Sender Key   |
|-- recipientNo | String |  Recipient number |
|-- requestDate | String |  Date and time of request |
|-- createDate | String | Registered date and time |
|-- receiveDate | String |  Date and time of receiving |
|-- content | String |  Body |
|-- messageStatus | String |    Request status (COMPLETED: successful, FAILED: failed) |
|-- resendStatus | String | Status code of resending |
|-- resendStatusName | String | Status code name of resending |
|-- resultCode | String |   Result code of receiving |
|-- resultCodeName | String |   Result code name of receiving |
|-- createUser | String | Registrant (saved as user UUID when sending from console) |
|-- senderGroupingKey | String | Sender's grouping key |
|-- recipientGroupingKey | String | Recipient's grouping key |
|- totalCount | Integer | Total count |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/messages?startRequestDate=2018-05-01%2000:00&endRequestDate=2018-05-30%2023:59"
```

#### Status of Resending
| Name |  Description|
|---|---|
|RSC01| No target of resending|
|RSC02| Target of resending (If sending fails, resending is performed.)|
|RSC03| Resending in progress|
|RSC04| Resending successful|
|RSC05| Resending failed|

## Get Deliveries

#### Request

[URL]

```
GET  /friendtalk/v2.2/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
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
|requestId| String| O | Request ID |
|recipientSeq|  Integer |   O | Recipient sequence number |

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
      "senderKey" :  String,
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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|message|   Object| Message|
|- requestId | String | Request ID |
|- recipientSeq | Integer | Recipient sequence number |
|- plusFriendId | String |  Plus Friend ID |
|- senderKey   | String | Sender Key   |
|- recipientNo | String |   Recipient number |
|- requestDate | String |   Date and time of request |
|- createDate | String | Registered date and time |
|- receiveDate | String |   Date and time of receiving |
|- content | String |   Body |
|- messageStatus | String | Status of request (COMPLETED: successful, FAILED: failed) |
|- resendStatus | String |  Status code of resending |
|- resendStatusName | String |  Status code name of resending |
|- resendResultCode | String | Alternative delivery result code SMS result code |
|- resendRequestId | String | Resending SMS request ID |
|- resultCode | String |    Result code of receiving |
|- resultCodeName | String |    Result code name of receiving |
|- createUser | String | Registrant (saved as user UUID when sending from console) |
|- imageSeq|    Integer|  Image number |
|- imageName|   String|  Image name (name of uploaded file) |
|- imageUrl|    String|  Image URL |
|- imageLink|   String| Image link   |
|- wide     | Boolean | Image is wide or not |
|- buttons | List | List of buttons |
|-- ordering | Integer |    Button sequence |
|-- type | String | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery) |
|-- name | String | Button name |
|-- linkMo | String |   Mobile web link (required for the WL type) |
|-- linkPc | String |   PC web link (optional for the WL type) |
|-- schemeIos | String |    iOS app link (required for the AL type) |
|-- schemeAndroid | String |    Android app link (required for the AL type) |
|-- chatExtra|  String| Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons |
|-- chatEvent|  String| Bot event name to connect for BT (Bot Transfer) type button |
|-- target| String| In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
|- isAd | Boolean | Ad or not |
|- senderGroupingKey | String | Sender's grouping key |
|- recipientGroupingKey | String |  Recipient's grouping key |

## Message
### Cancel Sending Messages

#### Request

[URL]

```
DELETE  /friendtalk/v2.2/appkeys/{appkey}/messages/{requestId}
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
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

### Query Updated Message Results

#### Request

[URL]

```
GET  /friendtalk/v2.2/appkeys/{appkey}/message-results
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

| Value |  Type| Required| Description|
|---|---|---|---|
|startUpdateDate|   String| O | Start time of querying result updates (yyyy-MM-dd HH:mm)|
|endUpdateDate| String| O | End time of querying result updates (yyyy-MM-dd HH:mm) |
|pageNum|   Integer|    X|  Page number (default: 1)|
|pageSize|  Integer|    X|  Number of queries (default: 15)|

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
      "senderKey"    :  String,
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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|messageSearchResultResponse|   Object| Body area|
|- messages | List |    List of messages |
|-- requestId | String |    Request ID |
|-- recipientSeq | Integer |    Recipient's sequence number |
|-- plusFriendId | String | Plus Friend ID |
|-- senderKey | String |    Sender Key |
|-- recipientNo | String |  Recipient number |
|-- requestDate | String |  Date and time of request |
|-- receiveDate | String |  Date and time of receiving |
|-- content | String |  Body |
|-- messageStatus | String |    Request status (COMPLETED -> successful, FAILED -> failed, CANCEL -> canceled) |
|-- resendStatus | String | Status code of resending |
|-- resendStatusName | String | Status code name of resending |
|-- resultCode | String |   Result code of receiving |
|-- resultCodeName | String |   Result code name of receiving |
|-- senderGroupingKey | String | Sender's grouping key |
|-- recipientGroupingKey | String | Recipient's grouping key |
|- totalCount | Integer | Total count |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

### List Mass Delivery Requests

#### Request
[URL]
```
GET /friendtalk/v2.2/appkeys/{appKey}/mass-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
| appKey | String | Unique appkey |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name |  Type| Description|
|---|---|---|
| X-Secret-Key | String | Unique secret key |

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
'https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages?requestId='"${REQUEST_ID}" \
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
| -- plusFriendId | String | PlusFriend ID |
| -- senderKey | String | Sender ID |
| -- masterStatusCode | String | Mass delivery status code (WAIT, READY, SENDREADY, SENDWAIT, SENDING, COMPLETE, CANCEL, FAIL) |
| -- content | String | Content |
| -- buttons | List | Button sequence |
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
| -- isAd | Boolean | Ad or not |
| -- imageSeq | Integer | Image sequence |
| -- imageLink | Boolean | Image URL |
| -- fileId | String | Attachment ID |
| -- autoSendYn | String | Auto sending or not |
| -- statsId | String | Statistics ID |
| -- createDate | String | Date of creation |
| -- createUser | String | User who created the request (saved as user UUID when sending from console) |
| - totalCount | Integer | Total count |


### List Mass Delivery Recipients

#### Request
[URL]
```
GET /friendtalk/v2.2/appkeys/{appKey}/mass-messages/{requestId}/recipients
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
| X-Secret-Key | String | Unique secret key |


| Name |  Type| Max. Length | Required| Description|
|---|---|---|---|---|
| requestId | String | - | O | Request ID |
| startRequestDate | String | - | X | - |
| endRequestDate | String | - | X | - |
| startCreateDate | String| - | X | - |
| endCreateDate |   String| - | X | End date of registration |
| pageNum | optional, Integer | - | X | Page number |
| pageSize | optional, Integer | 1000 | X | Search count |

#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients?requestId='"${REQUEST_ID}" \
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

| Name | Type| Description |
|---|---|---|
| header | Object | Header area |
| - resultCode |    Integer |   Result code |
| - resultMessage | String | Result message |
| - isSuccessful |  Boolean| Successful or not |
| body | Object | Body area |
| - recipients | List | List of recipients |
| -- requestId | String | Request ID |
| -- recipientSeq | Integer | Recipient sequence number |
| -- recipientNo | String | Recipient number |
| -- requestDate | String | Date of request |
| -- receiveDate | String | Date of receiving |
| -- messageStatus | String | Mass recipient delivery status code (READY, COMPLETED, FAILED, CANCEL) |
| -- resultCode | String | Result code of receiving |
| -- resultCodeName | String | Result code name of receiving |
| - totalCount | Integer | Total count |

### Get a Mass Delivery Recipient

#### Request
[URL]
```
GET /friendtalk/v2.2/appkeys/{appKey}/mass-messages/{requestId}/recipients/{recipientSeq}
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


#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients/${RECIPIENT_SEQ}?requestId='"${REQUEST_ID}" \
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

| Name |  Type| Description|
|---|---|---|
| header | Object | Header area |
| - resultCode |    Integer |   Result code |
| - resultMessage | String | Result message |
| - isSuccessful |  Boolean| Successful or not |
| body | Object | Body area |
| - requestId | String | Request ID |
| - recipientSeq | Integer | Recipient sequence number |
| - plusFriendId | String | PlusFriend ID |
| - senderKey | String | Sender key (40 characters)|
| - recipientNo | String | Recipient number |
| - requestDate | String | Date of request |
| - receiveDate | String | Date of receiving |
| - content | String | Body message |
| - messageStatus | String | Mass recipient delivery status code (READY, COMPLETED, FAILED, CANCEL) |
| - resendStatus | String | Status code of resending (RSC01, RSC02, RSC03, RSC04, RSC05)<br>([Refer to [Status code of resending table](http://docs.toast.com/en/Notification/KakaoTalk%20Bizmessage/en/alimtalk-api-guide/#smslms)] below) |
| - resendStatusName | String | Status code name of resending |
| - resendRequestId | String | Resending SMS request ID |
| - resendResultCode | String | Result code of resending [Result code of SMS sending](https://docs.toast.com/en/Notification/SMS/en/error-code/#api) |
| - resultCode | String |   Result code of receiving |
| - resultCodeName | String |   Result code name of receiving |
| - imageSeq | Integer | Image sequence |
| - imageLink | Integer | Image URL |
| - buttons | List | Button sequence |
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
| - isAd | Boolean | Ad or not |
| - createDate | String | Request |


## Image Management

### Register Images
#### Request

[URL]

```
POST  /friendtalk/v2.2/appkeys/{appkey}/images
Content-Type: multipart/form-data
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

[Request parameter]

| Name |  Type| Required| Description|
|---|---|---|---|
|image| File|   O | Image |
|wide| boolean | X | Image is wide or not (Default: false) |

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|image| Object| Body area|
|- imageSeq | Integer | Image number (to send Friendtalk messages)|
|- imageUrl | String |  Image URL |
|- imageName | String | Image name (name of uploaded file) |


### Query Images
#### Request

[URL]

```
GET  /friendtalk/v2.2/appkeys/{appkey}/images
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
|pageNum|   Integer|    X|  Page number (default: 1)|
|pageSize|  Integer|    X|  Number of queries (default: 15)|

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|imagesResponse| Object| Body area|
|- image|   Object| Body area|
|-- imageSeq | Integer |    Image number (to send Friendtalk messages)|
|-- imageUrl | String | Image URL |
|-- imageName | String |    Image name (name of uploaded file) |
|-- wide | boolean |    Image is wide or not |
|-- createDate | String |   Date and time of creation |
|- totalCount | Integer | Total count |

* Response is sent in the order of latest registration.

### Delete Images
#### Request

[URL]

```
DELETE  /friendtalk/v2.2/appkeys/{appkey}/images
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
|imageSeq|  String| O|  Image number |

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|


## Manage Alternative Delivery
### Register an SMS AppKey

[URL]

```
POST  /friendtalk/v2.2/appkeys/{appkey}/failback/appkey
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
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
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
POST  /friendtalk/v2.2/appkeys/{appkey}/failback
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
   "resendSendNo": String,
   "resendUnsubscribeNo": String
}
```

| Name |  Type| Required| Description|
|---|---|---|---|
|senderKey| String| O | Sender Key |
|isResend|  Boolean|    O | Whether to resend text, if delivery fails<br/>Resent by default, if alternative delivery is set on console. |
|resendSendNo|  String| O | Sender number for alternative delivery<br/><span style="color:red">(Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span> |
|resendUnsubscribeNo|   String| X | Alternative delivery 080 blocked number <br> <span style="color:red"> (If it is not the 080 blocked number registered in the SMS service, alternative delivery may fail.) </span> |

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.2/appkeys/{appkey}/failback/appkey -d '{"senderKey": "9e0afe2c12aaaaaaaaaa7520052880b555f1a60a","isResend": true,"resendSendNo": "01012341234", "resendUnsubscribeNo": "0801234567" }
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
