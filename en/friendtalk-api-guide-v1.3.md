## Notification > KakaoTalk Bizmessage > Friendtalk > API v1.3 Guide

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

## Send Messages
#### Request of Sending

[URL]

```
POST  /friendtalk/v1.3/appkeys/{appkey}/messages
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
    "requestDate": String,
    "senderGroupingKey": String,
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
                    "schemeAndroid": String
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
| - isAd                 | Boolean | X        | Ad or not (default is true)                                  |
| - recipientGroupingKey | String  | X        | Recipient's grouping key (up to 100 characters)              |

* <b> Send the first-registered Plus Friend, if Plus Friend ID field is not sent.</b>
* <b> Delivery restricted during night (20:00~08:00 on the following day)</b>

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v1.3/appkeys/{appkey}/messages -d '{"plusFriendId":"@Plus Friend","requestDate":"yyyy-MM-dd HH:mm","recipientList":[{"recipientNo":"010-0000-0000","imageSeq":1,"imageLink":"https://toast.com","content":"message","buttons":[{"ordering":1,"type":"WL","name":"button1","linkMo":"https://toast.com","linkPc":"https://toast.com"}]}]}'
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
GET  /friendtalk/v1.3/appkeys/{appkey}/messages
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

| Value                | Type    | Required                      | Description                                            |
| -------------------- | ------- | ----------------------------- | ------------------------------------------------------ |
| requestId            | String  | Conditionally required (no.1) | Request ID                                             |
| startRequestDate     | String  | Conditionally required (no.2) | Start date of delivery request (yyyy-MM-dd HH:mm)      |
| endRequestDate       | String  | Conditionally required (no.2) | End date of delivery request (yyyy-MM-dd HH:mm)        |
| recipientNo          | String  | X                             | Recipient number                                       |
| plusFriendId         | String  | X                             | Plus Friend ID                                         |
| senderGroupingKey    | String  | X                             | Sender's grouping key                                  |
| recipientGroupingKey | String  | X                             | Recipient's grouping key                               |
| messageStatus        | String  | X                             | Request status (COMPLETED: successful, FAILED: failed) |
| resultCode           | String  | X                             | Delivery result (MRC01: successful, MRC02: failed)     |
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
| -- content                  | String  | Body                                                   |
| -- messageStatus            | String  | Request status (COMPLETED: successful, FAILED: failed) |
| -- resendStatus             | String  | Status code of resending                               |
| -- resendStatusName         | String  | Status code name of resending                          |
| -- resultCode               | String  | Result code of receiving                               |
| -- resultCodeName           | String  | Result code name of receiving                          |
| -- senderGroupingKey        | String  | Sender's grouping key                                  |
| -- recipientGroupingKey     | String  | Recipient's grouping key                               |
| - totalCount                | Integer | Total count                                            |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v1.3/appkeys/{appkey}/messages?startRequestDate=2018-05-01%2000:00&endRequestDate=2018-05-30%2023:59"
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
GET  /friendtalk/v1.3/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
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

| Value        | Type    | Required | Description               |
| ------------ | ------- | -------- | ------------------------- |
| requestId    | String  | O        | Request ID                |
| recipientSeq | Integer | O        | Recipient sequence number |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v1.3/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
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
      "receiveDate" : String,
      "content" :  String,
      "messageStatus" :  String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "resultCode" :  String,
      "resultCodeName" : String,
      "imageSeq" : Integer,
      "imageName" : String,
      "imageUrl" : String,
      "imageLink" : String,
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
| - receiveDate          | String  | Date and time of receiving                                   |
| - content              | String  | Body                                                         |
| - messageStatus        | String  | Status of request (COMPLETED: successful, FAILED: failed)    |
| - resendStatus         | String  | Status code of resending                                     |
| - resendStatusName     | String  | Status code name of resending                                |
| - resultCode           | String  | Result code of receiving                                     |
| - resultCodeName       | String  | Result code name of receiving                                |
| - imageSeq             | Integer | Image number                                                 |
| - imageName            | String  | Image name (name of uploaded file)                           |
| - imageUrl             | String  | Image URL                                                    |
| - imageLink            | String  | Image link (required to enter image number)                  |
| - buttons              | List    | List of buttons                                              |
| -- ordering            | Integer | Button sequence                                              |
| -- type                | String  | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery) |
| -- name                | String  | Button name                                                  |
| -- linkMo              | String  | Mobile web link (required for the WL type)                   |
| -- linkPc              | String  | PC web link (optional for the WL type)                       |
| -- schemeIos           | String  | iOS app link (required for the AL type)                      |
| -- schemeAndroid       | String  | Android app link (required for the AL type)                  |
| - isAd                 | Boolean | Ad or not                                                    |
| - senderGroupingKey    | String  | Sender's grouping key                                        |
| - recipientGroupingKey | String  | Recipient's grouping key                                     |

### Query Updated Message Results

#### Request

[URL]

```
GET  /friendtalk/v1.3/appkeys/{appkey}/message-results
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
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./plus-friend-console-guide/#x-secret-key)] |

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
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v1.3/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

## Image Management

### Register Images
#### Request

[URL]

```
POST  /friendtalk/v1.3/appkeys/{appkey}/images
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
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./plus-friend-console-guide/#x-secret-key)] |

[Request parameter]

| Value | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| image | File | O        | Image       |

[Example]

```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v1.3/appkeys/{appkey}/images" -F "image=@friend-ricecake02.jpeg"
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
GET  /friendtalk/v1.3/appkeys/{appkey}/images
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

| Value    | Type    | Required | Description                     |
| -------- | ------- | -------- | ------------------------------- |
| pageNum  | Integer | X        | Page number (default: 1)        |
| pageSize | Integer | X        | Number of queries (default: 15) |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v1.3/appkeys/{appkey}/images?pageNum=1&pageSize=15"
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
| -- createDate   | String  | Date and time of creation                  |
| - totalCount    | Integer | Total count                                |

* Response is sent in the order of latest registration.

### Delete Images
#### Request

[URL]

```
DELETE  /friendtalk/v1.3/appkeys/{appkey}/images
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

| Value    | Type   | Required | Description  |
| -------- | ------ | -------- | ------------ |
| imageSeq | String | O        | Image number |

[Example]

```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v1.3/appkeys/{appkey}/images?imageSeq=1,2,3"
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
