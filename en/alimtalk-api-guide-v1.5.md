## Notification > KakaoTalk Bizmessage > AlimTalk > API v1.5 Guide

## AlimTalk

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

## Overview of v1.5 API
1. It has been changed to allow emphasized template for Register Template API (for a full-text delivery, the title value can be configured.)
2. Expanded the template type. Ad or additional information can be added.
3. The CreateUser field has been added when sending AlimTalk/Friendtlk messages.
4. Field has been added to query registered time and registrant when querying AlimTalk/FriendTalk messages.
5. Added Attach Files to Send Inquiry on Templates API.


## General Messages

### Request of Sending Replaced Messages

[URL]

```
POST  /alimtalk/v1.5/appkeys/{appkey}/messages
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
    "plusFriendId": String,
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
        "recipientGroupingKey": String
    }]
}
```

| Value                  | Type    | Required | Description                                                  |
| ---------------------- | ------- | -------- | ------------------------------------------------------------ |
| plusFriendId           | String  | O        | PlusFriend ID (up to 30 characters)                          |
| templateCode           | String  | O        | Registered delivery template code (up to 20 characters)      |
| requestDate            | String  | X        | Date and time of request (yyyy-MM-dd HH:mm)<br>(send immediately, if it is left blank) |
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

* <b>Request date and time can be set up to 90 days since a point of calling.</b>
* <b>Since alternative delivery is made in the SMS service, field values must follow the API specifications for SMS (e.g. Sender number registered at the SMS service, or restriction in the field length). </b>
* <b>The SMS Service supports international SMS only. For international receiver numbers, the resendType (alternative delivery type) must be changed to SMS to allow sending without fail. </b>
* <b>Title or content for alternative delivery that exceeds specified byte size may be cut for delivery. (see [[Caution](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] for reference)</b>

[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v1.5/appkeys/{appkey}/messages -d '{"plusFriendId":"{PlusFriend ID}","templateCode":"{template code}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{recipient number}","templateParameter":{"{replaced field}":"{replacement data}"}}]}'
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
POST  /alimtalk/v1.5/appkeys/{appkey}/raw-messages
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
    "plusFriendId": String,
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
                    "schemeAndroid": String
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
    ]
}
```

| Value                  | Type    | Required | Description                                                  |
| ---------------------- | ------- | -------- | ------------------------------------------------------------ |
| plusFriendId           | String  | O        | PlusFriend ID (up to 30 characters)                          |
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
| -- linkMo              | String  | X        | Mobile web link (required for the WL type, up to 500 characters) |
| -- linkPc              | String  | X        | PC web link (optional for the WL type, up to 500 characters) |
| -- schemeIos           | String  | X        | iOS app link (required for the AL type, up to 500 characters) |
| -- schemeAndroid       | String  | X        | Android app link (required for the AL type, up to 500 characters) |
| - resendParameter      |Object   | X        | Alternative delivery information                             |
| -- isResend            | boolean | X        | Whether to send text as alternative, if delivery fails <br/>Resent in default, if delivery failure is set on console. |
| -- resendType          | String  | X        | Alternative delivery type (SMS,LMS)<br/>Categorized by the length of template message, if value is unavailable. |
| -- resendTitle         | String  | X        | Title of alternative delivery for LMS (up to 20 characters)<br/>(resent with PlusFriend ID if value is unavailable.) |
| -- resendContent       | String  | X        | Alternative delivery message (up to 1000 characters)<br/>(resent template message, if value is unavailable.) |
| -- resendSendNo        | String  | X        | Sender number for alternative delivery (up to 13 characters)<br/><span style="color:red">(alternative delivery may fail, if sender number is not registered in the SMS service.)</span> |
| - recipientGroupingKey | String  | X        | Recipient's grouping key (up to 100 characters)              |

* <b>Enter data completed with replacement for the body and button. </b>
* **Request date and time can be set up to 90 days since a point of calling.**
* <b>Delivery is to be replaced by SMS, and field input must follow delivery API specifications of the SMS service (e.g. sender number registered at SMS service, 080 unsubscription, and field length restrictions) </b>
* <b>Only the international SMS service is supported. For an international recipient number, the resendType (alternative delivery type) must be changed to SMS to allow sending normally. </b>
* <b>Title or message of an alternative delivery may be cut in length, if the byte size exceeds restrictions (see [[Cautions for SMS](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)])</b>

[Exapmle]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v1.5/appkeys/{appkey}/raw-messages -d '{"plusFriendId":"{PlusFriend ID}","templateCode":"{template code}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{recipient number}","content":"{body}","buttons":[{"ordering":"{button sequence}","type":"{button type}","name":"{button name}","linkMo":"{mobile web link}"}]}]}'
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
GET  /alimtalk/v1.5/appkeys/{appkey}/messages
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
| plusFriendId         | String  | X                             | PlusFriend ID                                                |
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
          "schemeAndroid": String
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
| -- senderGroupingKey        | String  | Sender's grouping key                                        |
| -- recipientGroupingKey     | String  | Recipient's grouping key                                     |
| - totalCount                | Integer | Total Count                                                  |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.5/appkeys/{appkey}/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
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
GET  /alimtalk/v1.5/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
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
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.5/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
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
          "schemeAndroid": String
        }
      ],
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
POST  /alimtalk/v1.5/appkeys/{appkey}/auth/messages
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
        "recipientGroupingKey": String
    }]
}
```

| Value                  | Type    | Required | Description                                                  |
| ---------------------- | ------- | -------- | ------------------------------------------------------------ |
| plusFriendId           | String  | O        | PlusFriend ID (up to 30 characters)                          |
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

* <b> Request date and time can be set up to 90 days since a point of calling. </b>

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v1.5/appkeys/{appkey}/auth/messages -d '{"plusFriendId":"{PlusFriend ID}","templateCode":"{template code}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{recipient number}","templateParameter":{"{replaced field}":"{replacement data}"}}]}'
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
POST  /alimtalk/v1.5/appkeys/{appkey}/auth/raw-messages
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
    "plusFriendId": String,
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
                    "schemeAndroid": String
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
    ]
}
```

| Value                  | Type    | Required | Description                                                  |
| ---------------------- | ------- | -------- | ------------------------------------------------------------ |
| plusFriendId           | String  | O        | PlusFriend ID (up to 30 characters)                          |
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
| -- linkMo              | String  | X        | Mobile web link (required for the WL type, for up to 500 characters) |
| -- linkPc              | String  | X        | PC web link (required for the WL type, for up to 500 characters) |
| -- schemeIos           | String  | X        | iOS app link (required for the AL type, for up to 500 characters) |
| -- schemeAndroid       | String  | X        | Android app link (required for the AL type, for up to 500 characters) |
| - isResend             | boolean | X        | Whether to send text as alternative, if delivery fails<br>Resent in default, if delivery failure is set on console. |
| - resendType           | String  | X        | Alternative delivery type (SMS,LMS)<br>Categorized by the length of template body, if value is unavailable. |
| - resendTitle          | String  | X        | Title of alternative delivery for LMS (up to 20 characters)<br>(resent with PlusFriend ID, if the value is unavailable.) |
| - resendContent        | String  | X        | Alternative delivery message (up to 1000 characters)<br>(resent with template message if value is unavailable.) |
| - resendSendNo         | String  | X        | Sender number for alternative delivery (up to 13 characters)<br><span style="color:red"> (Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span> |
| - recipientGroupingKey | String  | X        | Recipient's grouping key (up to 100 characters)              |

* <b>Enter data completed with replacement in the body and button. </b>
* <b>Request date and time can be set up to 90 days since a point of calling. </b>

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v1.5/appkeys/{appkey}/auth/raw-messages -d '{"plusFriendId":"{lusFriend ID}","templateCode":"{template code}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{recipient number}","content":"{body message}","buttons":[{"ordering":"{button sequence}","type":"{button type}","name":"{button name}","linkMo":"{mobile web link}"}]}]}'
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
GET  /alimtalk/v1.5/appkeys/{appkey}/auth/messages
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
| plusFriendId         | String  | X                             | PlusFriend ID                                                |
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
          "schemeAndroid": String
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
| -- senderGroupingKey        | String  | Sender's grouping key                                        |
| -- recipientGroupingKey     | String  | Recipient's grouping key                                     |
| - totalCount                | Integer | Total count                                                  |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.5/appkeys/{appkey}/auth/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
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
GET  /alimtalk/v1.5/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}
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
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.5/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}"
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
          "schemeAndroid": String
        }
      ],
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
| - senderGroupingKey    | String  | Sender's grouping key                                        |
| - recipientGroupingKey | String  | Recipient's grouping key                                     |

## Messages
### Cancel Sending Messages

#### Request

[URL]

```
DELETE  /alimtalk/v1.5/appkeys/{appkey}/messages/{requestId}
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
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.5/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

### Query Updates of Message Result

#### Request

[URL]

```
GET  /alimtalk/v1.5/appkeys/{appkey}/message-results
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
| alimtalkMessageType | String  | X        | AlimTalk message type (NORMAL, AUTH)                     |
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
| -- templateCode             | String  | Template code                                                |
| -- recipientNo              | String  | Recipient number                                             |
| -- content                  | String  | Body message                                                 |
| -- requestDate              | String  | Date and time of request                                     |
| -- receiveDate              | String  | Date and time of receiving                                   |
| -- resendStatus             | String  | Status code of resending                                     |
| -- resendStatusName         | String  | Status code name of resending                                |
| -- messageStatus            | String  | Request status (COMPLETED -> Successful, FAILED -> Failed, CANCEL -> Canceled) |
| -- resultCode               | String  | Result code of receiving                                     |
| -- resultCodeName           | String  | Result code name of receiving                                |
| -- buttons                  | List    | List of buttons                                              |
| --- ordering                | Integer | Button sequence                                              |
| --- type                    | String  | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added) |
| --- name                    | String  | Button name                                                  |
| --- linkMo                  | String  | Mobile web link (required for the WL type)                   |
| --- linkPc                  | String  | PC web link (optional for the WL type)                       |
| --- schemeIos               | String  | iOS app link (required for the AL type)                      |
| --- schemeAndroid           | String  | Android app link (required for the AL type)                  |
| -- senderGroupingKey        | String  | Sender's grouping key                                        |
| -- recipientGroupingKey     | String  | Recipient's grouping key                                     |
| - totalCount                | Integer | Total count                                                  |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.5/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

## PlusFriends

### Query PlusFriend by Category

#### Request
[URL]

```
GET  /alimtalk/v1.5/appkeys/{appkey}/plus-friends/categories
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
POST  /alimtalk/v1.5/appkeys/{appkey}/plus-friends
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
| categoryCode | String  | O        | Category code (11 characters) See response for Search Category API  e.g.) 00100010001 Health (001) - Hospital (0001) - General Hospital (0001) |
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
POST  /alimtalk/v1.5/appkeys/{appkey}/plus-friends/{plusFriendId}/tokens
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
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "token" : "Integer"
}
```

| Value | Type    | Required | Description                                                  |
| ----- | ------- | -------- | ------------------------------------------------------------ |
| token | Integer | O        | Authentication token (received on KakaoTalk app, after Register PlusFriend API call) |

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

### Get PlusFriend
#### Request

[URL]

```
GET  /alimtalk/v1.5/appkeys/{appkey}/plus-friends/{plusFriendId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value  | Type   | Description     |
| ------ | ------ | --------------- |
| appkey | String | Original appkey |
| plusFriendId | String  | PlusFriend ID |

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
   "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
   },
   "plusFriend":{  
         "plusFriendId" : String,
         "plusFriendType" : String,
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
                "resendAppKey": String,
                "isResend": Boolean,
                "resendSendNo": String,
                "dailyMaxCount" : Integer,
                "sentCount" : Integer
          },
         "friendtalk": {  
                "resendAppKey": String,
                "isResend": Boolean,
                "resendSendNo": String,
                "resendUnsubscribeNo": String,
                "dailyMaxCount" : Integer,
                "sentCount" : Integer
         },
         "createDate": String
    }
}
```

| Value                     | Type    | Description                                                  |
| ------------------------- | ------- | ------------------------------------------------------------ |
| header                    | Object  | Header area                                                  |
| - resultCode              | Integer | Result code                                                  |
| - resultMessage           | String  | Result message                                               |
| - isSuccessful            | Boolean | Successful or not                                            |
| plusFriend                | Object  | PlusFriend                                                   |
| - plusFriendId            | String  | PlusFriend ID                                                |
| - plusFriendType          | String  | PlusFriend type (NORMAL, GROUP)                              |
| - senderKey               | String  | Sender key                                                   |
| - categoryCode            | String  | Category code                                                |
| - status                  | String  | Status code of TOAST PlusFriend  (YSC02: Ready for registeration, YSC03: Normally registered) |
| - statusName              | String  | Status name of TOAST PlusFriend (ready for registration, normally registered) |
| - kakaoStatus             | String  | Status code of Kakao PlusFriend (A: Normal, S: Blocked, D: Deleted) kakaoStatus is null if the status is YSC02. |
| - kakaoStatusName         | String  | Status name of Kakao PlusFriend (normal, blocked, deleted) kakaoStatusName is null if the status is YSC02. |
| - kakaoProfileStatus      | String  | Status code of Kakao PlusFriend profile  (A: Activated, B: Blocked, C: Deactivated, D:Deleted, E: Deleting) kakaoProfileStatus is null if the status is YSC02. |
| - kakaoProfileStatusName  | String  | Status name of Kakao PlusFriend profile (Activated, Deactivated, Blocked, Deleted, or Deleting) kakaoProfileStatusName is null if the status is YSC02. |
|- alimtalk                 |	Object  |	AlimTalk information                                         |
|-- resendAppKey            | String  | Alternative sms appkey                                       |
|-- isResend                | String  | Whether to send text as alternative, if delivery fails       |
|-- resendSendNo            | String  |	Sender number for alternative delivery                       |
|-- dailyMaxCount           | Integer |	Maximum daily AlimTalk delivery count (no limits for 0)      |
|-- sentCount               | Integer |	Daily AlimTalk delivery count (no limits for 0)              |
|- friendtalk               |	Object  |	FriendTalk information                                       |
|-- resendAppKey            | String  | Alternative sms appkey                                       |
|-- isResend                | String  | Whether to send text as alternative, if delivery fails       |
|-- resendSendNo            | String  |	Sender number for alternative delivery                       |
|-- resendUnsubscribeNo     | String  |	080 unsubscription number for alternative delivery           |
|-- dailyMaxCount           | Integer |	Maximum daily FriendTalk delivery count <br>(no limis for 0) |
|-- sentCount               | Integer |	Daily FriendTalk delivery count <br>(no limits for 0)        |
| - createDate              | String  | Date and time of registration                                |
| totalCount                | Integer | Total count                                                  |

### List PlusFriends

#### Request

[URL]

```
GET  /alimtalk/v1.5/appkeys/{appkey}/plus-friends
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

[Query parameter] No.1 or 2 is conditionally required

| Value               | Type    | Required | Description                                                  |
| ------------------- | ------- | -------- | ------------------------------------------------------------ |
| plusFriendId        | String  | X        | PlusFriend ID                                                |
| status              | String  | X        | Status code of PlusFriend  (YSC02: Ready for token authenticated, YSC03: Normally registered) |
| isSearchKakaoStatus | boolean | X        | Query of Kakao status (null for Kakao status-related fields (e.g. kakaoStatus or kakaoProfileStatus) if it is false) Default: True |

#### Response

```
{  
   "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
   },
   "plusFriends":[  
      {  
         "plusFriendId" : String,
         "plusFriendType" : String,
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
                "resendAppKey": String,
                "isResend": Boolean,
                "resendSendNo": String,
                "dailyMaxCount" : Integer,
                "sentCount" : Integer
          },
         "friendtalk": {  
                "resendAppKey": String,
                "isResend": Boolean,
                "resendSendNo": String,
                "resendUnsubscribeNo": String,
                "dailyMaxCount" : Integer,
                "sentCount" : Integer
         }
      }
   ],
   "totalCount": Integer
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
| - status                  | String  | Status code of TOAST PlusFriend  (YSC02: Ready for registeration, YSC03: Normally registered) |
| - statusName              | String  | Status name of TOAST PlusFriend (ready for registration, normally registered) |
| - kakaoStatus             | String  | Status code of Kakao PlusFriend (A: Normal, S: Blocked, D: Deleted) kakaoStatus is null if the status is YSC02. |
| - kakaoStatusName         | String  | Status name of Kakao PlusFriend (normal, blocked, deleted) kakaoStatusName is null if the status is YSC02. |
| - kakaoProfileStatus      | String  | Status code of Kakao PlusFriend profile  (A: Activated, B: Blocked, C: Deactivated, D:Deleted, E: Deleting) kakaoProfileStatus is null if the status is YSC02. |
| - kakaoProfileStatusName  | String  | Status name of Kakao PlusFriend profile (Activated, Deactivated, Blocked, Deleted, or Deleting) kakaoProfileStatusName is null if the status is YSC02. |
|- alimtalk                 |	Object  |	AlimTalk information                                         |
|-- resendAppKey            | String  | Alternative sms appkey                                       |
|-- isResend                | String  | Whether to send text as alternative, if delivery fails       |
|-- resendSendNo            | String  |	Sender number for alternative delivery                       |
|-- dailyMaxCount           | Integer |	Maximum daily AlimTalk delivery count (no limits for 0)      |
|-- sentCount               | Integer |	Daily AlimTalk delivery count (no limits for 0)              |
|- friendtalk               |	Object  |	FriendTalk information                                        |
|-- resendAppKey            | String  | Alternative sms appkey                                        |
|-- isResend                | String  | Whether to send text as alternative, if delivery fails        |
|-- resendSendNo            | String  |	Sender number for alternative delivery                        |
|-- resendUnsubscribeNo     | String  |	080 unsubscription number for alternative delivery            |
|-- dailyMaxCount           | Integer |	Maximum daily FriendTalk delivery count <br>(no limis for 0)              |
|-- sentCount               | Integer |	Daily FriendTalk delivery count <br>(no limits for 0)                 |
| - createDate              | String  | Date and time of registration                                |
| totalCount                | Integer | Total count                                                  |

## Templates

### List Template Categories
#### Request
[URL]

```
POST  /alimtalk/v1.5/appkeys/{appkey}/template/categories
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
POST  /alimtalk/v1.5/appkeys/{appkey}/plus-friends/{plusFriendId}/templates
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
| templateName        | String  | O        | Template name (up to 150 characters)                          |
| templateContent     | String  | O        | Template body (up to 1000 characters)                        |
| templateMessageType | String  | X        | Types of Template Message (BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes, default: Basic) |
|templateEmphasizeType| String  | X        | Types of Emphasized Template (NONE: Basic, TEXT: Emphasized, default:NONE)<br>- TEXT: templateTitle and templateSubtitle fields are required |
| templateExtra       | String  | X        | Additional Template Information (Required, if template message type is[Ad Included/Mixed Purposes])                             |
| templateAd          | String  | X        | Request for consent of receiving within template or simple ad phrases (Required, if template message type is[Ad Included/Mixed Purposes]) |
|tempalteTitle        | String  | X        | Template Title (No more than 50 characters, Android: To be abbreviated if it exceeds 2 lines with more than 23 characters, iOS: To be abbreviated if it exceeds 2 lines with more than 27 characters) |
|templateSubtitle    | String   | X        | Auxiliary Template Phrase (No more than 50 characters, Android: To be abbreviated if it exceeds 18 characters, iOS: To be abbreviated if it exceeds 21 characters) |
| securityFlag    | Boolean | X        | a |
| categoryCode    | String  | X        | 템플릿 카테고리 코드 (템플릿 카테고리 조회 API 참고, default: 999999)<br>카테고리 입력한 템플릿을 우선 심사 |
| buttons         | List    | X        | List of buttons (up to 5)                                    |
| -ordering       | Integer | X        | Button sequence (1~5)                                        |
| -type           | String  | X        | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added [only for Ad Included/Mixed Purposes Type]) |
| -name           | String  | X        | Button name (required, if there's a button, up to 14 characters) |
| -linkMo         | String  | X        | Mobile web link (required for the WL type, up to 500 characters) |
| -linkPc         | String  | X        | PC web link (optional for the WL type, up to 500 characters) |
| -schemeIos      | String  | X        | iOS app link (required for the AL type, up to 500 characters) |
| -schemeAndroid  | String  | X        | Android app link (required for the AL type, up to 500 characters) |

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
PUT  /alimtalk/v1.5/appkeys/{appkey}/plus-friends/{plusFriendId}/templates/{templateCode}
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
| templateName    | String  | O        | Template name (up to 150 characters)                          |
| templateContent | String  | O        | Template body (up to 1000 characters)                        |
| templateMessageType | String  | X        | Types of Template Message (BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes, default: Basic) |
|templateEmphasizeType| String  | X        | Types of Emphasized Template (NONE: Basic, TEXT: Emphasized, default:NONE)<br>- TEXT: templateTitle and templateSubtitle fields are required |
| templateExtra       | String  | X        | Additional Template Information (Required, if template message type is[Ad Included/Mixed Purposes])                             |
| templateAd          | String  | X        | Request for consent of receiving within template or simple ad phrases (Required, if template message type is[Ad Included/Mixed Purposes] |
|tempalteTitle| String | X| Template Title (No more than 50 characters, Android: To be abbreviated if it exceeds 2 lines with more than 23 characters, iOS: To be abbreviated if it exceeds 2 lines with more than 27 characters) |
|templateSubtitle| String | X| Auxiliary Template Phrase (No more than 50 characters, Android: To be abbreviated if it exceeds 18 characters, iOS: To be abbreviated if it exceeds 21 characters) |
| securityFlag    | Boolean | X        | Security template<br>Set for security messages such as OTP<br>If set, message text is unexposed to all devices except for the main device at the time of sending (default: false) |
| categoryCode    | String  | X        | 템플릿 카테고리 코드 (템플릿 카테고리 조회 API 참고, default: 999999)<br>카테고리 입력한 템플릿을 우선 심사 |
| buttons         | List    | X        | List of buttons (up to 5)                                    |
| -ordering       | Integer | X        | Button sequence (1~5)                                        |
| -type           | String  | X        | Button type (WL: Web Link, AL: App Link, DS: Delivery Search, BK: Bot Keyword, MD: Message Delivery, BC: Bot for Consultation, BT: Bot Transfer, CA: Channel Added [only for Ad Included/Mixed Purposes Type]) |
| -name           | String  | X        | Button name (required, if there's a button, up to 14 characters) |
| -linkMo         | String  | X        | Mobile web link (required for the WL type, up to 500 characters) |
| -linkPc         | String  | X        | PC web link (optional for the WL type, up to 500 characters) |
| -schemeIos      | String  | X        | iOS app link (required for the AL type, up to 500 characters) |
| -schemeAndroid  | String  | X        | Android app link (required for the AL type, up to 500 characters) |

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
DELETE  /alimtalk/v1.5/appkeys/{appkey}/plus-friends/{plusFriendId}/templates/{templateCode}
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
PUT  /alimtalk/v1.5/appkeys/{appkey}/plus-friends/{plusFriendId}/templates/{templateCode}/comments
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
POST  /alimtalk/v1.5/appkeys/{appkey}/plus-friends/{plusFriendId}/templates/{templateCode}/comments_file
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value           | Type    | Description       |
|---|---|---|
|appkey|	String|	고유의 Appkey|
|plusFriendId|	String|	PlusFriend ID |
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
GET  /alimtalk/v1.5/appkeys/{appkey}/templates
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
| TSC04                | Rejected    |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.5/appkeys/{appkey}/templates?plusFriendId={PlusFriend ID}&templateStatus={template status code}"
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
              "templateEmphasizeType": String,
              "templateTitle" : String,
              "templateSubtitle" : String,
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
| -- plusFriendType    | String  | PlusFriend type (NORMAL, GROUP)                              |
| -- templateCode      | String  | Template code                                                |
| -- templateName      | String  | Template name                                                |
| -- templateContent   | String  | Template body                                                |
|-- templateEmphasizeType| String| Types of Emphasized Template (NONE: Basic, TEXT: Emphasized, default:NONE) |
|-- tempalteTitle      | String  | Template Title                                               |
|-- templateSubtitle   | String  | Auxiliary Template Phrase                                    |
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
| ---userName          | String  | Creator                                                      |
| ---createAt          | String  | Date of registration                                         |
| ---status            | String  | Comment status (INQ: Inquired, APR: Approved, REJ: Rejected, REP: Replied) |
| -- status            | String  | Template status                                              |
| -- statusName        | String  | Template status name                                         |
| -- createDate        | String  | Date and time of creation                                    |
| - totalCount         | Integer | Total count                                                  |

### List Template modifications

#### Request

[URL]

```
GET  /alimtalk/v1.5/appkeys/{appkey}/plus-friends/{plusFriendId}/templates/{templateCode}/modifications
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
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.5/appkeys/{appkey}/plus-friends/{plusFriendId}/templates/{templateCode}/modifications"
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
              "plusFriendType": String,
              "templateCode": String,
              "templateName": String,
              "templateContent": String,
              "templateEmphasizeType": String,
              "templateTitle" : String,
              "templateSubtitle" : String,
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
| -- plusFriendType    | String  | PlusFriend type (NORMAL, GROUP)                              |
| -- templateCode      | String  | Template code                                                |
| -- templateName      | String  | Template name                                                |
| -- templateContent   | String  | Template body                                                |
| -- templateEmphasizeType| String | Types of Emphasized Template (NONE: Basic, TEXT: Emphasized, default:NONE) |
| -- tempalteTitle      | String  | Template Title                                               |
| -- templateSubtitle   | String  | Auxiliary Template Phrase                                    |
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
| ---userName          | String  | Creator                                                      |
| ---createAt          | String  | Date of registration                                         |
| ---status            | String  | Comment status (INQ: Inquired, APR: Approved, REJ: Rejected, REP: Replied) |
| -- status            | String  | Template status                                              |
| -- statusName        | String  | Template status name                                         |
| -- activated         | Boolean | activated or not                                             |
| -- createDate        | String  | Date and time of creation                                    |
| - totalCount         | Integer | Total count                                                  |
