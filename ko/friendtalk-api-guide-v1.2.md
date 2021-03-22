## Notification > KakaoTalk Bizmessage > 친구톡 > API v1.2 Guide

## 친구톡

#### [API 도메인]

<table>
<thead>
<tr>
<th>도메인</th>
</tr>
</thead>
<tbody>
<tr>
<td>https://api-alimtalk.cloud.toast.com</td>
</tr>
</tbody>
</table>

## 메시지 발송
#### 발송 요청

[URL]

```
POST  /friendtalk/v1.2/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 타입     | 설명     |
| ------ | ------ | ------ |
| appkey | String | 고유의 앱키 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 값            | 타입     | 필수   | 설명                                       |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

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

| 값                      | 타입      | 필수   | 설명                                       |
| ---------------------- | ------- | ---- | ---------------------------------------- |
| plusFriendId           | String  | O    | 플러스친구 ID(최대 30자)                         |
| requestDate            | String  | X    | 요청 일시(yyyy-MM-dd HH:mm), 필드를 보내지 않을 경우, 즉시 발송 |
| senderGroupingKey      | String  | X    | 발신 그룹핑 키(최대 100자)                        |
| recipientList          | List    | O    | 수신자 목록(최대 1000명)                         |
| - recipientNo          | String  | O    | 수신 번호                                    |
| - content              | String  | O    | 내용(최대 1000자)<br>이미지 포함 시, 최대 400자        |
| - imageSeq             | Integer | X    | 이미지 번호                                   |
| - imageLink            | String  | X    | 이미지 링크                |
| - buttons              | List    | X    | 버튼                                       |
| -- ordering            | Integer | X    | 버튼 순서(버튼이 있는 경우 필수)                      |
| -- type                | String  | X    | 버튼 타입(WL:웹 링크, AL:앱 링크, BK:봇 키워드, MD:메시지 전달) |
| -- name                | String  | X    | 버튼 이름(버튼이 있는 경우 필수)                      |
| -- linkMo              | String  | X    | 모바일 웹 링크(WL 타입일 경우 필수 필드)                |
| -- linkPc              | String  | X    | PC 웹 링크 (WL 타입일 경우 선택 필드)                |
| -- schemeIos           | String  | X    | iOS 앱 링크(AL 타입일 경우 필수 필드)                |
| -- schemeAndroid       | String  | X    | Android 앱 링크(AL 타입일 경우 필수 필드)            |
| - isAd                 | Boolean | X    | 광고 여부(기본값 true)                          |
| - recipientGroupingKey | String  | X    | 수신자 그룹핑 키(최대 100자)                       |

* <b>플러스친구 ID 필드를 보내지 않을 경우, 첫 번째 등록한 플러스친구로 발송됩니다.</b>
* <b>야간 발송 제한(20:00~다음 날 08:00)</b>
* <b>지정한 대체 발송 타입의 바이트 제한을 초과하는 대체 발송 제목이나 내용은 잘려서 대체 발송될 수 있습니다.([[SMS 주의 사항](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] 참고)</b>
* <b>친구톡 광고 메시지는 광고 SMS API로 대체 발송되므로, 반드시 080 수신 거부 번호를 등록해야 대체 발송됩니다.</b>

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v1.2/appkeys/{appkey}/messages -d '{"plusFriendId":"@플러스친구","requestDate":"yyyy-MM-dd HH:mm","recipientList":[{"recipientNo":"010-0000-0000","imageSeq":1,"imageLink":"https://toast.com","content":"내용","buttons":[{"ordering":1,"type":"WL","name":"버튼1","linkMo":"https://toast.com","linkPc":"https://toast.com"}]}]}'
```

#### 응답

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

| 값                       | 타입      | 설명           |
| ----------------------- | ------- | ------------ |
| header                  | Object  | 헤더 영역        |
| - resultCode            | Integer | 결과 코드        |
| - resultMessage         | String  | 결과 메시지       |
| - isSuccessful          | Boolean | 성공 여부        |
| message                 | Object  | 본문 영역        |
| - requestId             | String  | 요청 ID        |
| - senderGroupingKey     | String  | 발신 그룹핑 키     |
| - sendResults           | Object  | 발송 요청 결과     |
| -- recipientSeq         | Integer | 수신자 시퀀스 번호   |
| -- recipientNo          | String  | 수신 번호        |
| -- resultCode           | Integer | 발송 요청 결과 코드  |
| -- resultMessage        | String  | 발송 요청 결과 메시지 |
| -- recipientGroupingKey | String  | 수신자 그룹핑 키    |

## 발송 리스트 조회

#### 요청

[URL]

```
GET  /friendtalk/v1.2/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 타입     | 설명     |
| ------ | ------ | ------ |
| appkey | String | 고유의 앱키 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 값            | 타입     | 필수   | 설명                                       |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter] 1번 or 2번 조건 필수

| 값                    | 타입      | 필수        | 설명                                |
| -------------------- | ------- | --------- | --------------------------------- |
| requestId            | String  | 조건 필수(1번) | 요청 ID                             |
| startRequestDate     | String  | 조건 필수(2번) | 발송 요청 날짜 시작 값(yyyy-MM-dd HH:mm)   |
| endRequestDate       | String  | 조건 필수(2번) | 발송 요청 날짜 끝 값(yyyy-MM-dd HH:mm)    |
| recipientNo          | String  | X         | 수신 번호                             |
| plusFriendId         | String  | X         | 플러스친구 ID                          |
| senderGroupingKey    | String  | X         | 발신 그룹핑 키                          |
| recipientGroupingKey | String  | X         | 수신자 그룹핑 키                         |
| messageStatus        | String  | X         | 요청 상태(COMPLETED: 성공, FAILED: 실패 ) |
| resultCode           | String  | X         | 발송 결과(MRC01: 성공 MRC02: 실패 )       |
| pageNum              | Integer | X         | 페이지 번호(기본: 1)                     |
| pageSize             | Integer | X         | 조회 건수(기본: 15)                     |

#### 응답
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

| 값                           | 타입      | 설명                                |
| --------------------------- | ------- | --------------------------------- |
| header                      | Object  | 헤더 영역                             |
| - resultCode                | Integer | 결과 코드                             |
| - resultMessage             | String  | 결과 메시지                            |
| - isSuccessful              | Boolean | 성공 여부                             |
| messageSearchResultResponse | Object  | 본문 영역                             |
| - messages                  | List    | 메시지 리스트                           |
| -- requestId                | String  | 요청 ID                             |
| -- recipientSeq             | Integer | 수신자 시퀀스 번호                        |
| -- plusFriendId             | String  | 플러스친구 ID                          |
| -- recipientNo              | String  | 수신 번호                             |
| -- requestDate              | String  | 요청 일시                             |
| -- content                  | String  | 본문                                |
| -- messageStatus            | String  | 요청 상태(COMPLETED: 성공, FAILED: 실패 ) |
| -- resendStatus             | String  | 재발송 상태 코드                         |
| -- resendStatusName         | String  | 재발송 상태 코드명                        |
| -- resultCode               | String  | 수신 결과 코드                          |
| -- resultCodeName           | String  | 수신 결과 코드명                         |
| -- senderGroupingKey        | String  | 발신 그룹핑 키                          |
| -- recipientGroupingKey     | String  | 수신자 그룹핑 키                         |
| - totalCount                | Integer | 총 개수                              |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v1.2/appkeys/{appkey}/messages?startRequestDate=2018-05-01%2000:00&endRequestDate=2018-05-30%2023:59"
```

#### 재발송 상태
| 값     | 설명                              |
| ----- | ------------------------------- |
| RSC01 | 재발송 미대상                         |
| RSC02 | 재발송 대상(발송 결과 실패 시, 재발송이 진행됩니다.) |
| RSC03 | 재발송 중                           |
| RSC04 | 재발송 성공                          |
| RSC05 | 재발송 실패                          |

## 발송 단건 조회

#### 요청

[URL]

```
GET  /friendtalk/v1.2/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 타입     | 설명     |
| ------ | ------ | ------ |
| appkey | String | 고유의 앱키 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 값            | 타입     | 필수   | 설명                                       |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter]

| 값            | 타입      | 필수   | 설명         |
| ------------ | ------- | ---- | ---------- |
| requestId    | String  | O    | 요청 ID      |
| recipientSeq | Integer | O    | 수신자 시퀀스 번호 |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v1.2/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
```

#### 응답
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

| 값                      | 타입      | 설명                                       |
| ---------------------- | ------- | ---------------------------------------- |
| header                 | Object  | 헤더 영역                                    |
| - resultCode           | Integer | 결과 코드                                    |
| - resultMessage        | String  | 결과 메시지                                   |
| - isSuccessful         | Boolean | 성공 여부                                    |
| message                | Object  | 메시지                                      |
| - requestId            | String  | 요청 ID                                    |
| - recipientSeq         | Integer | 수신자 시퀀스 번호                               |
| - plusFriendId         | String  | 플러스친구 ID                                 |
| - recipientNo          | String  | 수신 번호                                    |
| - requestDate          | String  | 요청 일시                                    |
| - receiveDate          | String  | 수신 일시                                    |
| - content              | String  | 본문                                       |
| - messageStatus        | String  | 요청 상태(COMPLETED : 성공, FAILED : 실패 )      |
| - resendStatus         | String  | 재발송 상태 코드                                |
| - resendStatusName     | String  | 재발송 상태 코드명                               |
| - resultCode           | String  | 수신 결과 코드                                 |
| - resultCodeName       | String  | 수신 결과 코드명                                |
| - imageSeq             | Integer | 이미지 번호                                   |
| - imageName            | String  | 이미지명(업로드한 파일명)                           |
| - imageUrl             | String  | 이미지 URL                                  |
| - imageLink            | String  | 이미지 링크                                  |
| - buttons              | List    | 버튼 목록                                    |
| -- ordering            | Integer | 버튼 순서                                    |
| -- type                | String  | 버튼 타입(WL:웹 링크, AL:앱 링크, BK:봇 키워드, MD:메시지 전달) |
| -- name                | String  | 버튼 이름                                    |
| -- linkMo              | String  | 모바일 웹 링크(WL 타입일 경우 필수 필드)                |
| -- linkPc              | String  | PC 웹 링크 (WL 타입일 경우 선택 필드)                |
| -- schemeIos           | String  | iOS 앱 링크(AL 타입일 경우 필수 필드)                |
| -- schemeAndroid       | String  | Android 앱 링크(AL 타입일 경우 필수 필드)            |
| - isAd                 | Boolean | 광고 여부                                    |
| - senderGroupingKey    | String  | 발신 그룹핑 키                                 |
| - recipientGroupingKey | String  | 수신자 그룹핑 키                                |

### 메시지 결과 업데이트 조회

#### 요청

[URL]

```
GET  /friendtalk/v1.2/appkeys/{appkey}/message-results
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 타입     | 설명     |
| ------ | ------ | ------ |
| appkey | String | 고유의 앱키 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 값            | 타입     | 필수   | 설명                                       |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter]

| 값               | 타입      | 필수   | 설명                                 |
| --------------- | ------- | ---- | ---------------------------------- |
| startUpdateDate | String  | O    | 결과 업데이트 조회 시작 시간(yyyy-MM-dd HH:mm) |
| endUpdateDate   | String  | O    | 결과 업데이트 조회 종료 시간(yyyy-MM-dd HH:mm) |
| pageNum         | Integer | X    | 페이지 번호(기본: 1)                      |
| pageSize        | Integer | X    | 조회 건수(기본: 15)                      |

#### 응답
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

| 값                           | 타입      | 설명                                       |
| --------------------------- | ------- | ---------------------------------------- |
| header                      | Object  | 헤더 영역                                    |
| - resultCode                | Integer | 결과 코드                                    |
| - resultMessage             | String  | 결과 메시지                                   |
| - isSuccessful              | Boolean | 성공 여부                                    |
| messageSearchResultResponse | Object  | 본문 영역                                    |
| - messages                  | List    | 메시지 리스트                                  |
| -- requestId                | String  | 요청 ID                                    |
| -- recipientSeq             | Integer | 수신자 시퀀스 번호                               |
| -- plusFriendId             | String  | 플러스친구 ID                                 |
| -- recipientNo              | String  | 수신 번호                                    |
| -- requestDate              | String  | 요청 일시                                    |
| -- receiveDate              | String  | 수신 일시                                    |
| -- content                  | String  | 본문                                       |
| -- messageStatus            | String  | 요청 상태(COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 ) |
| -- resendStatus             | String  | 재발송 상태 코드                                |
| -- resendStatusName         | String  | 재발송 상태 코드명                               |
| -- resultCode               | String  | 수신 결과 코드                                 |
| -- resultCodeName           | String  | 수신 결과 코드명                                |
| -- senderGroupingKey        | String  | 발신 그룹핑 키                                 |
| -- recipientGroupingKey     | String  | 수신자 그룹핑 키                                |
| - totalCount                | Integer | 총 개수                                     |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v1.2/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

## 이미지 관리

### 이미지 등록
#### 요청

[URL]

```
POST  /friendtalk/v1.2/appkeys/{appkey}/images
Content-Type: multipart/form-data
```

[Path parameter]

| 값      | 타입     | 설명     |
| ------ | ------ | ------ |
| appkey | String | 고유의 앱키 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 값            | 타입     | 필수   | 설명                                       |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Request parameter]

| 값     | 타입   | 필수   | 설명   |
| ----- | ---- | ---- | ---- |
| image | File | O    | 이미지  |

[예시]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v1.2/appkeys/{appkey}/images" -F "image=@friend-ricecake02.jpeg"
```

#### 응답
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

| 값               | 타입      | 설명                     |
| --------------- | ------- | ---------------------- |
| header          | Object  | 헤더 영역                  |
| - resultCode    | Integer | 결과 코드                  |
| - resultMessage | String  | 결과 메시지                 |
| - isSuccessful  | Boolean | 성공 여부                  |
| image           | Object  | 본문 영역                  |
| - imageSeq      | Integer | 이미지 번호(친구톡 메시지 발송시 사용) |
| - imageUrl      | String  | 이미지 URL                |
| - imageName     | String  | 이미지명(업로드한 파일명)         |


### 이미지 조회
#### 요청

[URL]

```
GET  /friendtalk/v1.2/appkeys/{appkey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 타입     | 설명     |
| ------ | ------ | ------ |
| appkey | String | 고유의 앱키 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 값            | 타입     | 필수   | 설명                                       |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter]

| 값        | 타입      | 필수   | 설명            |
| -------- | ------- | ---- | ------------- |
| pageNum  | Integer | X    | 페이지 번호(기본: 1) |
| pageSize | Integer | X    | 조회 건수(기본: 15) |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v1.2/appkeys/{appkey}/images?pageNum=1&pageSize=15"
```

#### 응답
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

| 값               | 타입      | 설명                     |
| --------------- | ------- | ---------------------- |
| header          | Object  | 헤더 영역                  |
| - resultCode    | Integer | 결과 코드                  |
| - resultMessage | String  | 결과 메시지                 |
| - isSuccessful  | Boolean | 성공 여부                  |
| imagesResponse  | Object  | 본문 영역                  |
| - image         | Object  | 본문 영역                  |
| -- imageSeq     | Integer | 이미지 번호(친구톡 메시지 발송시 사용) |
| -- imageUrl     | String  | 이미지 URL                |
| -- imageName    | String  | 이미지명(업로드한 파일명)         |
| -- createDate   | String  | 생성 일자                  |
| - totalCount    | Integer | 총 개수                   |

* 이미지는 최근 등록한 순대로 정렬되어 응답합니다.

### 이미지 삭제
#### 요청

[URL]

```
DELETE  /friendtalk/v1.2/appkeys/{appkey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 타입     | 설명     |
| ------ | ------ | ------ |
| appkey | String | 고유의 앱키 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 값            | 타입     | 필수   | 설명                                       |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter]

| 값        | 타입     | 필수   | 설명     |
| -------- | ------ | ---- | ------ |
| imageSeq | String | O    | 이미지 번호 |

[예시]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v1.2/appkeys/{appkey}/images?imageSeq=1,2,3"
```

#### 응답
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  }
}
```

| 값               | 타입      | 설명     |
| --------------- | ------- | ------ |
| header          | Object  | 헤더 영역  |
| - resultCode    | Integer | 결과 코드  |
| - resultMessage | String  | 결과 메시지 |
| - isSuccessful  | Boolean | 성공 여부  |
