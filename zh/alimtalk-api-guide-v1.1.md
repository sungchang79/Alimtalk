## Notification > KakaoTalk Bizmessage > Alimtalk > API v1.1 Guide

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

## Messages

### Request of Sending Replaced Messages

[URL]

```
POST  /alimtalk/v1.1/appkeys/{appkey}/messages
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
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./plus-friend-console-guide/#x-secret-key)] |

[Request body]

```
{
    "plusFriendId": String,
    "templateCode": String,
    "requestDate": String,
    "recipientList": [{
        "recipientNo": String,
        "templateParameter": {
            String: String
        },
        "isResend" : boolean,
        "resendType" : String,
        "resendTitle" : String,
        "resendContent" : String,
        "resendSendNo" : String
    }]
}
```

| Value               | Type    | Required | Description                                                  |
| ------------------- | ------- | -------- | ------------------------------------------------------------ |
| plusFriendId        | String  | X        | PlusFriend ID (up to 30 characters)                          |
| templateCode        | String  | O        | Registered delivery template code (up to 20 characters)      |
| requestDate         | String  | X        | Date and time of request (yyyy-MM-dd HH:mm)<br>(send immediately, if it is left blank) |
| recipientList       | List    | O        | List of recipients (up to 1000 persons)                      |
| - recipientNo       | String  | O        | Recipient number (up to 15 characters)                       |
| - templateParameter | Object  | X        | Template parameter<br>(required, if it includes a variable to be replaced for template) |
| -- key              | String  | X        | Replacement key (#{key})                                     |
| -- value            | String  | X        | Value which is mapped for replacement key                    |
| - isResend          | boolean | X        | Whether to send text as alternative, if delivery fails<br>Resent in default, if delivery failure is set on console. |
| - resendType        | String  | X        | Alternative delivery type(SMS,LMS)<br>Categorized by the length of template body if value is unavailable. |
| - resendTitle       | String  | X        | Title of alternative delivery for LMS (up to 20 characters)<br>(resent with PlusFriend ID if value is unavailable.) |
| - resendContent     | String  | X        | Alternative delivery message (up to 1000 characters)<br>(resent with template message if value is unavailable.) |
| - resendSendNo      | String  | X        | Sender number for alternative delivery (up to 13 characters)<br><span style="color:red">(Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span> |

* <b>Sent to first-registered PlusFriend if the PlusFriend ID field is not sent. </b>
* <b>Request date and time can be set up to 90 days since a point of calling.</b>

[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v1.1/appkeys/{appkey}/messages -d '{"plusFriendId":"{PlusFriend ID}","templateCode":"{template code}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{recipient number}","templateParameter":{"{replaced field}":"{replacement data}"}}]}'
```

### Request of Sending Full Text

[URL]

```
POST  /alimtalk/v1.1/appkeys/{appkey}/raw-messages
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
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./plus-friend-console-guide/#x-secret-key)] |

[Request Body]

```
{
    "plusFriendId": String,
    "templateCode": String,
    "requestDate": String,
    "recipientList": [
        {
            "recipientNo": String,
            "content": String,
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
            "isResend" : boolean,
            "resendType" : String,
            "resendTitle" : String,
            "resendContent" : String,
            "resendSendNo" : String
        }
    ]
}
```

| Value            | Type    | Required | Description                                                  |
| ---------------- | ------- | -------- | ------------------------------------------------------------ |
| plusFriendId     | String  | X        | PlusFriend ID (up to 30 characters)                          |
| templateCode     | String  | O        | Registered delivery template code (up to 20 characters)      |
| requestDate      | String  | X        | Date and time of request (yyyy-MM-dd HH:mm)<br>(sent immediately if it is left blank) |
| recipientList    | List    | O        | List of recipients (up to 1,000 persons)                     |
| - recipientNo    | String  | O        | Recipient number (up to 15 characters)                       |
| - content        | String  | O        | Message  (up to 1000 characters)                             |
| - buttons        | List    | X        | List of buttons (up to 5)                                    |
| -- ordering      | Integer | X        | Button sequence (required, if there is a button)             |
| -- type          | String  | X        | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery) |
| -- name          | String  | X        | Button name (required if there is a button, up to 14 characters) |
| -- linkMo        | String  | X        | Mobile web link (required for the WL type, up to 200 characters) |
| -- linkPc        | String  | X        | PC web link (optional for the WL type, up to 200 characters) |
| -- schemeIos     | String  | X        | iOS app link (required for the AL type, up to 200 characters) |
| -- schemeAndroid | String  | X        | Android app link (required for the AL type, up to 200 characters) |
| - isResend       | boolean | X        | Whether to send text as alternative, if delivery fails <br>Resent in default, if delivery failure is set on console. |
| - resendType     | String  | X        | Alternative delivery type (SMS,LMS)<br>Categorized by the length of template message, if value is unavailable. |
| - resendTitle    | String  | X        | Title of alternative delivery for LMS (up to 20 characters)<br>(resent with PlusFriend ID if value is unavailable.) |
| - resendContent  | String  | X        | Alternative delivery message (up to 1000 characters)<br>(resent template message, if value is unavailable.) |
| - resendSendNo   | String  | X        | Sender number for alternative delivery (up to 13 characters)<br><span style="color:red">(alternative delivery may fail, if sender number is not registered in the SMS service.)</span> |


* <b>Sent to the first-registered PlusFriend, if the PlusFriend ID field is not sent.</b>
* <b>Enter data completed with replacement for the body and button. </b>
* <b>Request date and time can be set up to 90 days since a point of calling.</b>

[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v1.1/appkeys/{appkey}/raw-messages -d '{"plusFriendId":"{PlusFriend ID}","templateCode":"{template code}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{recipient number}","content":"{message}","buttons":[{"ordering":"{button sequence}","type":"{button type}","name":"{button name}","linkMo":"{mobile web link}"}]}]}'
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
    "requestId": String
  }
}
```

| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - resultCode    | Integer | Result code       |
| - resultMessage | String  | Result message    |
| - isSuccessful  | Boolean | Successful or not |
| message         | Object  | Body area         |
| - requestId     | String  | Request ID        |


### Cancel Sending Messages

#### Request

[URL]

```
DELETE  /alimtalk/v1.1/appkeys/{appkey}/messages/{requestId}
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
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter]

| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| recipientSeq | String | X        | Recipient's sequence number<br>(to cancel all deliveries of request ID, if it is left blank) |

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
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.1/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

### List Messages

#### Request

[URL]

```
GET  /alimtalk/v1.1/appkeys/{appkey}/messages
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
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter] No.1 or 2 is conditionally required

| Value            | Type    | Required                      | Description                                                  |
| ---------------- | ------- | ----------------------------- | ------------------------------------------------------------ |
| requestId        | String  | Conditionally required (no.1) | Request ID                                                   |
| startRequestDate | String  | Conditionally required (no.2) | Start date of delivery request (yyyy-MM-dd HH:mm)            |
| endRequestDate   | String  | Conditionally required (no.2) | End date of delivery request (yyyy-MM-dd HH:mm)              |
| recipientNo      | String  | X                             | Recipient number                                             |
| plusFriendId     | String  | X                             | PlusFriend ID                                                |
| templateCode     | String  | X                             | Template code                                                |
| messageStatus    | String  | X                             | Request status (COMPLETED -> successful, FAILED -> failed, CANCEL -> canceled ) |
| resultCode       | String  | X                             | Delivery result (MRC01 -> successful, MRC02 -> failed)       |
| pageNum          | Integer | X                             | Page number (default: 1)                                     |
| pageSize         | Integer | X                             | Number of queries (default: 15, max : 1000)                  |

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
      "templateCode" :  String,
      "recipientNo" :  String,
      "content" :  String,
      "requestDate" :  String,
      "receiveDate" : String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "messageStatus" :  String,
      "resultCode" :  String,
      "resultCodeName" : String,
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
      ]
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
| -- templateCode             | String  | Template code                                                |
| -- recipientNo              | String  | Recipient number                                             |
| -- content                  | String  | Body                                                         |
| -- requestDate              | String  | Date and time of request                                     |
| -- receiveDate              | String  | Date and time of receiving                                   |
| -- resendStatus             | String  | Status code of resending                                     |
| -- resendStatusName         | String  | Status code name of resending                                |
| -- messageStatus            | String  | Request status (COMPLETED -> successful, FAILED -> failed, CANCEL -> canceled) |
| -- resultCode               | String  | Result code of receiving                                     |
| -- resultCodeName           | String  | Result code name of receiving                                |
| -- buttons                  | List    | List of buttons                                              |
| --- ordering                | Integer | Button sequence                                              |
| --- type                    | String  | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery) |
| --- name                    | String  | Button name                                                  |
| --- linkMo                  | String  | Mobile web link (required for the WL type)                   |
| --- linkPc                  | String  | PC web link (optional for the WL type)                       |
| --- schemeIos               | String  | iOS app link (required for the AL type)                      |
| --- schemeAndroid           | String  | Android app link (required for the AL type)                  |
| - totalCount                | Integer | Total count                                                  |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.1/appkeys/{appkey}/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
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
GET  /alimtalk/v1.1/appkeys/{appkey}/message/{requestId}/{recipientSeq}
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
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./plus-friend-console-guide/#x-secret-key)] |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.1/appkeys/{appkey}/message/{requestId}/{recipientSeq}"
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
      "templateCode" :  String,
      "recipientNo" :  String,
      "content" :  String,
      "requestDate" :  String,
      "receiveDate" : String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "messageStatus" :  String,
      "resultCode" :  String,
      "resultCodeName" : String,
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
      ]
  }
}
```

| Value              | Type    | Description                                                  |
| ------------------ | ------- | ------------------------------------------------------------ |
| header             | Object  | Header area                                                  |
| - resultCode       | Integer | Result code                                                  |
| - resultMessage    | String  | Result message                                               |
| - isSuccessful     | Boolean | Successful or not                                            |
| message            | Object  | Message                                                      |
| - requestId        | String  | Request ID                                                   |
| - recipientSeq     | Integer | Recipient sequence number                                    |
| - plusFriendId     | String  | PlusFriend ID                                                |
| - templateCode     | String  | Template code                                                |
| - recipientNo      | String  | Recipient number                                             |
| - content          | String  | Body                                                         |
| - requestDate      | String  | Date and time of request                                     |
| - receiveDate      | String  | Date and time of receiving                                   |
| - resendStatus     | String  | Status code of resending                                     |
| - resendStatusName | String  | Status code name of resending                                |
| - messageStatus    | String  | Request status (COMPLETED -> successful, FAILED ->failed, CANCEL -> canceled) |
| - resultCode       | String  | Result code of receiving                                     |
| - resultCodeName   | String  | Result code name of receiving                                |
| - buttons          | List    | List of buttons                                              |
| -- ordering        | Integer | Button sequence                                              |
| -- type            | String  | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery) |
| -- name            | String  | Button name                                                  |
| -- linkMo          | String  | Mobile web link (required for the WL type)                   |
| -- linkPc          | String  | PC web link (optional for the WL type)                       |
| -- schemeIos       | String  | iOS app link (required for the AL type)                      |
| -- schemeAndroid   | String  | Android app link (required for the AL type)                  |

## PlusFriends

### Query PlusFriend by Category

#### Request
[URL]

```
GET  /alimtalk/v1.1/appkeys/{appkey}/plus-friends/categories
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
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./plus-friend-console-guide/#x-secret-key)] |

#### Response
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

| Value            | Type    | Description       |
| ---------------- | ------- | ----------------- |
| header           | Object  | Header area       |
| - resultCode     | Integer | Result code       |
| - resultMessage  | String  | Result message    |
| - isSuccessful   | Boolean | Successful or not |
| categories       | Object  | Category          |
| - parentCode     | String  | Parent code       |
| - depth          | Integer | Depth of category |
| - code           | String  | Category code     |
| - name           | String  | Category name     |
| - subCategories  | Object  | Sub-category      |
| -- parentCode    | String  | Parent code       |
| -- depth         | Integer | Depth of category |
| -- code          | String  | Category code     |
| -- name          | String  | Category name     |
| -- subCategories | Object  | Sub-category      |
| --- parentCode   | String  | Parent code       |
| --- depth        | Integer | Depth of category |
| --- code         | String  | Category code     |
| --- name         | String  | Category name     |

### Register PlusFriends
#### Request
[URL]

```
POST  /alimtalk/v1.1/appkeys/{appkey}/plus-friends
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
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./plus-friend-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "plusFriendId" : String,
  "phoneNo" : String,
  "licenseNo" : String,
  "categoryCode" : String,
  "fileSeq" : Integer
}
```

| Value        | Type    | Required | Description                                                  |
| ------------ | ------- | -------- | ------------------------------------------------------------ |
| plusFriendId | String  | O        | PlusFriend ID (up to 30 characters)                          |
| phoneNo      | String  | O        | Mobile number of administrator (up to 15 characters)         |
| categoryCode | String  | O        | Category code (11 characters)<br>See response for Search Category API <br>e.g.) 00100010001 Health (001) - Hospital (0001) - General Hospital (0001) |
| fileSeq      | Integer | O        | File sequence                                                |

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

### Authenticate Tokens for PlusFriends
#### Request
[URL]

```
POST  /alimtalk/v1.1/appkeys/{appkey}/plus-friends/{plusFriendId}/tokens
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value        | Type   | Description     |
| ------------ | ------ | --------------- |
| appkey       | String | Original appkey |
| plusFriendId | String | PlusFriend ID   |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./plus-friend-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "token" : "Integer"
}
```

| Value | Type    | Required | Description                                                  |
| ----- | ------- | -------- | ------------------------------------------------------------ |
| token | Integer | O        | Authentication token (received on Kakaotalk app, after Register PlusFriend API call) |

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

### List PlusFriends
#### Requet

[URL]

```
GET  /alimtalk/v1.1/appkeys/{appkey}/plus-friends
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
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter] No.1 or 2 is conditionally required

| Value               | Type    | Required | Description                                                  |
| ------------------- | ------- | -------- | ------------------------------------------------------------ |
| plusFriendId        | String  | X        | PlusFriend ID                                                |
| status              | String  | X        | Status code of PlusFriend <br>(YSC02: Ready for token authenticated, YSC03: Normally registered) |
| isSearchKakaoStatus | boolean | X        | Query of Kakao status (null for Kakao status-related fields (e.g. kakaoStatus or kakaoProfileStatus) if it is false)<br>Default: True |

#### Response
```
{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "plusFriends" : [
    {
      "plusFriendId" : String,
      "plusFriendType" : String,
      "senderKey" : String,
      "categoryCode" : String,
      "alimtalkDailyMaxCount" : Integer,
      "friendtalkDailyMaxCount" : Integer,
      "alimtalkSentCount" : Integer,
      "friendtalkSentCount" : Integer,
      "status" : String,
      "statusName" : String,
      "kakaoStatus" : String,
      "kakaoStatusName" : String,
      "kakaoProfileStatus" : String,
      "kakaoProfileStatusName" : String,
      "resendYn" : String,
      "smsSendNo" : Integer,
      "createDate" : String
    }
  ],
  "totalCount" : Integer
  }
}
```

| Value                     | Type    | Description                                                  |
| ------------------------- | ------- | ------------------------------------------------------------ |
| header                    | Object  | Header area                                                  |
| - resultCode              | Integer | Result code                                                  |
| - resultMessage           | String  | Result message                                               |
| - isSuccessful            | Boolean | Successful or not                                            |
| plusFriends               | Object  | PlusFriend                                                   |
| - plusFriendId            | String  | PlusFriend ID                                                |
| - plusFriendType          | String  | PlusFriend type (NORMAL, GROUP)                              |
| - senderKey               | String  | Sender key                                                   |
| - categoryCode            | String  | Category code                                                |
| - alimtalkDailyMaxCount   | Integer | Number of maximum daily Alimtalk deliveries <br>(not limited if it is 0) |
| - friendtalkDailyMaxCount | Integer | Number of maximum daily Friendtalk deliveries<br>(not limited if it is 0) |
| - alimtalkSentCount       | Integer | Number of daily Alimtalk deliveries <br>(not limited if it is 0) |
| - friendtalkSentCount     | Integer | Number of daily Friendtalk deliveries<br>(not limited if it is 0) |
| - status                  | String  | Status code of NHN Cloud PlusFriend <br>(YSC02: Ready for registeration, YSC03: Normally registered) |
| - statusName              | String  | Status name of NHN Cloud PlusFriend (ready for registration, normally registered) |
| - kakaoStatus             | String  | Status code of Kakao PlusFriend<br>(A: Normal, S: Blocked, D: Deleted)<br>kakaoStatus is null if the status is YSC02. |
| - kakaoStatusName         | String  | Status name of Kakao PlusFriend (normal, blocked, deleted)<br>kakaoStatusName is null if the status is YSC02. |
| - kakaoProfileStatus      | String  | Status code of Kakao PlusFriend profile <br>(A: Activated, B: Blocked, C: Deactivated, D:Deleted, E: Deleting)<br>kakaoProfileStatus is null if the status is YSC02. |
| - kakaoProfileStatusName  | String  | Status name of Kakao PlusFriend profile (Activated, Deactivated, Blocked, Deleted, or Deleting)<br>kakaoProfileStatusName is null if the status is YSC02. |
| - resendYn                | String  | Set delivery failure (resending) or not                      |
| - smsSendNo               | String  | Sender number for tc-sms, to resend                          |
| - createDate              | String  | Date and time of registration                                |
| totalCount                | Integer | Total count                                                  |

## Templates

### Register Templates
#### Request
[URL]

```
POST  /alimtalk/v1.1/appkeys/{appkey}/plus-friends/{plusFriendId}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value        | Type   | Description     |
| ------------ | ------ | --------------- |
| appkey       | String | Original appkey |
| plusFriendId | String | PlusFriend ID   |

[Header]

```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./plus-friend-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "templateCode" : String,
  "templateName" : String,
  "templateContent" : String,
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
| templateCode    | String  | O        | Template code (up to 20 characters)                          |
| templateName    | String  | O        | Template name (up to 20 characters)                          |
| templateContent | String  | O        | Template body (up to 1000 characters)                        |
| buttons         | List    | X        | List of buttons (up to 5)                                    |
| -ordering       | Integer | X        | Button sequence (1~5)                                        |
| -type           | String  | X        | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery) |
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
PUT  /alimtalk/v1.1/appkeys/{appkey}/plus-friends/{plusFriendId}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value        | Type   | Description     |
| ------------ | ------ | --------------- |
| appkey       | String | Original appkey |
| plusFriendId | String | PlusFriend ID   |
| templateCode | String | Template code   |

[Header]

```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./plus-friend-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "templateName" : String,
  "templateContent" : String,
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
| buttons         | List    | X        | List of buttons (up to 5)                                    |
| -ordering       | Integer | X        | Button sequence (1~5)                                        |
| -type           | String  | X        | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery) |
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
DELETE  /alimtalk/v1.1/appkeys/{appkey}/plus-friends/{plusFriendId}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value        | Type   | Description     |
| ------------ | ------ | --------------- |
| appkey       | String | Original appkey |
| plusFriendId | String | PlusFriend ID   |
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
PUT  /alimtalk/v1.1/appkeys/{appkey}/plus-friends/{plusFriendId}/templates/{templateCode}/comments
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value        | Type   | Description     |
| ------------ | ------ | --------------- |
| appkey       | String | Original appkey |
| plusFriendId | String | PlusFriend ID   |
| templateCode | String | Template code   |

[Header]

```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./plus-friend-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "comment" : String
}
```

| Value   | Type   | Required | Description |
| ------- | ------ | -------- | ----------- |
| comment | String | O        | Inquiries   |

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

### List Templates

#### Request

[URL]

```
GET  /alimtalk/v1.1/appkeys/{appkey}/templates
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
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter]

| Value          | Type    | Required | Description                     |
| -------------- | ------- | -------- | ------------------------------- |
| plusFriendId   | String  | X        | PlusFriend ID                   |
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
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.1/appkeys/{appkey}/templates?plusFriendId={PlusFriend ID}&templateStatus={template status code}"
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
              "plusFriendType": String,
              "templateCode": String,
              "templateName": String,
              "templateContent": String,
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
| - templates          | List    | List of templates                                            |
| -- plusFriendId      | String  | PlusFriend ID                                                |
| -- plusFriendType    | String  | PlusFriend type (NORMAL, GROUP)                              |
| -- templateCode      | String  | Template code                                                |
| -- templateName      | String  | Template name                                                |
| -- templateContent   | String  | Template body                                                |
| -- buttons           | List    | List of buttons                                              |
| --- ordering         | Integer | Button sequence (1~5)                                        |
| --- type             | String  | Button type (WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery) |
| --- name             | String  | Button name                                                  |
| --- linkMo           | String  | Mobile web link (required for the WL type)                   |
| --- linkPc           | String  | PC web link (optional for the WL type)                       |
| --- schemeIos        | String  | iOS app link (required for the AL type)                      |
| --- schemeAndroid    | String  | Android app link (required for the AL type)                  |
| -- comments          | List    | Inspection result                                            |
| --- id               | Integer | Inquiry ID                                                   |
| --- content          | String  | Inquiry content                                              |
| ---userName          | String  | Creator                                                      |
| ---createAt          | String  | Date of registration                                         |
| ---status            | String  | Comment status (INQ: Inquired, APR: Approved, REJ: Returned, REP: Replied) |
| -- status            | String  | Template status                                              |
| -- statusName        | String  | Template status name                                         |
| -- createDate        | String  | Date and time of creation                                    |
| - totalCount         | Integer | Total count                                                  |
