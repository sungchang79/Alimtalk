## Notification > KakaoTalk Bizmessage > Sender > API v2.0 Guide

## Overview of v2.0 API
#### What's the diffrence
1. 카카오 채널 추가 시, 발급 받은 senderKey 필드로 API 호출이 되도록 변경 되었습니다. (plusFriendId 필드 대체)
2. API uri가 변경 되었습니다. (/plus-friends -> /senders)
3. 카카오 채널 그룹 기능이 추가 되었습니다.

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

## PlusFriends

### Query PlusFriend by Category

#### Request
[URL]

```
GET  /alimtalk/v2.0/appkeys/{appkey}/plus-friends/categories
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
POST  /alimtalk/v2.0/appkeys/{appkey}/plus-friends
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
POST  /alimtalk/v2.0/appkeys/{appkey}/plus-friends/{plusFriendId}/tokens
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

### Get PlusFriend
#### Request

[URL]

```
GET  /alimtalk/v2.0/appkeys/{appkey}/plus-friends/{plusFriendId}
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
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./plus-friend-console-guide/#x-secret-key)] |

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
| - status                  | String  | Status code of NHN Cloud PlusFriend  (YSC02: Ready for registeration, YSC03: Normally registered) |
| - statusName              | String  | Status name of NHN Cloud PlusFriend (ready for registration, normally registered) |
| - kakaoStatus             | String  | Status code of Kakao PlusFriend (A: Normal, S: Blocked, D: Deleted) kakaoStatus is null if the status is YSC02. |
| - kakaoStatusName         | String  | Status name of Kakao PlusFriend (normal, blocked, deleted) kakaoStatusName is null if the status is YSC02. |
| - kakaoProfileStatus      | String  | Status code of Kakao PlusFriend profile  (A: Activated, B: Blocked, C: Deactivated, D:Deleted, E: Deleting) kakaoProfileStatus is null if the status is YSC02. |
| - kakaoProfileStatusName  | String  | Status name of Kakao PlusFriend profile (Activated, Deactivated, Blocked, Deleted, or Deleting) kakaoProfileStatusName is null if the status is YSC02. |
|- alimtalk                 |	Object  |	Alimtalk information                                         |
|-- resendAppKey            | String  | Alternative sms appkey                                       |
|-- isResend                | String  | Whether to send text as alternative, if delivery fails       |
|-- resendSendNo            | String  |	Sender number for alternative delivery                       |
|-- dailyMaxCount           | Integer |	Maximum daily Alimtalk delivery count (no limits for 0)      |
|-- sentCount               | Integer |	Daily Alimtalk delivery count (no limits for 0)              |
|- friendtalk               |	Object  |	Friendtalk information                                       |
|-- resendAppKey            | String  | Alternative sms appkey                                       |
|-- isResend                | String  | Whether to send text as alternative, if delivery fails       |
|-- resendSendNo            | String  |	Sender number for alternative delivery                       |
|-- resendUnsubscribeNo     | String  |	080 unsubscription number for alternative delivery           |
|-- dailyMaxCount           | Integer |	친구톡 일별 최대 발송 건수<br>(값이 0일 경우 건수 제한없음)              |
|-- sentCount               | Integer |	친구톡 일별 발송 건수<br>(값이 0일 경우 건수 제한없음)                  |
| - createDate              | String  | Date and time of registration                                |
| totalCount                | Integer | Total count                                                  |

### List PlusFriends

#### Request

[URL]

```
GET  /alimtalk/v2.0/appkeys/{appkey}/plus-friends
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
| - status                  | String  | Status code of NHN Cloud PlusFriend  (YSC02: Ready for registeration, YSC03: Normally registered) |
| - statusName              | String  | Status name of NHN Cloud PlusFriend (ready for registration, normally registered) |
| - kakaoStatus             | String  | Status code of Kakao PlusFriend (A: Normal, S: Blocked, D: Deleted) kakaoStatus is null if the status is YSC02. |
| - kakaoStatusName         | String  | Status name of Kakao PlusFriend (normal, blocked, deleted) kakaoStatusName is null if the status is YSC02. |
| - kakaoProfileStatus      | String  | Status code of Kakao PlusFriend profile  (A: Activated, B: Blocked, C: Deactivated, D:Deleted, E: Deleting) kakaoProfileStatus is null if the status is YSC02. |
| - kakaoProfileStatusName  | String  | Status name of Kakao PlusFriend profile (Activated, Deactivated, Blocked, Deleted, or Deleting) kakaoProfileStatusName is null if the status is YSC02. |
|- alimtalk                 |	Object  |	Alimtalk information                                         |
|-- resendAppKey            | String  | Alternative sms appkey                                       |
|-- isResend                | String  | Whether to send text as alternative, if delivery fails       |
|-- resendSendNo            | String  |	Sender number for alternative delivery                       |
|-- dailyMaxCount           | Integer |	Maximum daily Alimtalk delivery count (no limits for 0)      |
|-- sentCount               | Integer |	Daily Alimtalk delivery count (no limits for 0)              |
|- friendtalk               |	Object  |	Friendtalk information                                        |
|-- resendAppKey            | String  | Alternative sms appkey                                        |
|-- isResend                | String  | Whether to send text as alternative, if delivery fails        |
|-- resendSendNo            | String  |	Sender number for alternative delivery                        |
|-- resendUnsubscribeNo     | String  |	080 unsubscription number for alternative delivery            |
|-- dailyMaxCount           | Integer |	친구톡 일별 최대 발송 건수<br>(값이 0일 경우 건수 제한없음)              |
|-- sentCount               | Integer |	친구톡 일별 발송 건수<br>(값이 0일 경우 건수 제한없음)                  |
| - createDate              | String  | Date and time of registration                                |
| totalCount                | Integer | Total count                                                  |
