## Notification > KakaoTalk Bizmessage > Sender > API v2.0 Guide

## v2.0 API 소개
#### 개선된 점
1. 카카오 채널 추가 시, 발급 받은 senderKey 필드로 API 호출이 되도록 변경 되었습니다. (plusFriendId 필드 대체)
2. API uri가 변경 되었습니다. (/plus-friends -> /senders)
3. 카카오 채널 그룹 기능이 추가 되었습니다.
4. 발신 프로필 삭제 API가 추가되었습니다.

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

## 발신 프로필

### 발신 프로필 카테고리 조회

#### 요청
[URL]

```
GET  /alimtalk/v2.0/appkeys/{appkey}/sender/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./sender-console-guide/#x-secret-key)] |

#### 응답
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

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|categories|	Object|	카테고리|
|- parentCode | String |	부모 코드 |
|- depth | Integer |	카테고리 깊이 |
|- code | String |	카테고리 코드 |
|- name | String |	카테고리 이름 |
|- subCategories | Object |	서브 카테고리 |
|-- parentCode | String |	부모 코드 |
|-- depth | Integer |	카테고리 깊이 |
|-- code | String |	카테고리 코드 |
|-- name | String |	카테고리 이름 |
|-- subCategories | Object |	서브 카테고리 |
|--- parentCode | String |	부모 코드 |
|--- depth | Integer |	카테고리 깊이 |
|--- code | String |	카테고리 코드 |
|--- name | String |	카테고리 이름 |

### 발신 프로필 등록
#### 요청
[URL]

```
POST  /alimtalk/v2.0/appkeys/{appkey}/senders
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./sender-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "plusFriendId" : String,
  "phoneNo" : String,
  "categoryCode" : String
}
```

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|plusFriendId|	String|	O | 카카오톡 채널 검색용 ID (최대 30자) |
|phoneNo|	String |	O | 관리자 핸드폰 번호 (최대 15자) |
|categoryCode|	String |	O | 카테고리 코드(11자)<br>카테고리 조회 API의 응답 참고<br>ex) 00100010001 건강(001) - 병원(0001) - 종합병원(0001) |

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

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|

### 발신 프로필 토큰 인증
#### 요청
[URL]

```
POST  /alimtalk/v2.0/appkeys/{appkey}/sender/token
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./sender-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "plusFriendId" : String,
  "token" : Integer
}
```

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|plusFriendId|	String|	O | 카카오톡 채널 검색용 ID |
|token|	Integer |	O | 인증 토큰 (플러스친구 등록 API 호출 후, 카카오톡 앱으로 받은 인증 토큰) |

#### 응답
```
{
  "header" : {
    "resultCode" :  Integer,
    "resultMessage" :  String,
    "isSuccessful" :  boolean
  },
  "sender": {
    "plusFriendId": String,
    "senderKey": String,
    "categoryCode": String,
    "status": String
  }
}
```

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|sender|	Object|	발신 프로필|
|- plusFriendId | String |	플러스친구 ID |
|- senderKey | String |	발신 키 |
|- categoryCode | String |	카테고리 코드 |
|- status | String |	NHN Cloud 플러스친구 상태 코드 <br>(YSC02: 등록 대기중, YSC03: 정상 등록) |

### 발신 프로필 삭제
#### 요청

[URL]

```
DELETE  /alimtalk/v2.0/appkeys/{appkey}/senders/{senderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey|
|senderKey| String | 발신 키 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./sender-console-guide/#x-secret-key)] |

* 발신 프로필 삭제 시, 등록한 템플릿 데이터가 함께 삭제 됩니다.
* 발신 프로필 삭제 시, 복구가 불가능합니다.

#### 응답
```
{  
   "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
   }
}
```

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|

### 발신 프로필 단건 조회
#### 요청

[URL]

```
GET  /alimtalk/v2.0/appkeys/{appkey}/senders/{senderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey|
|senderKey| String | 발신 키 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./sender-console-guide/#x-secret-key)] |

#### 응답
```
{  
   "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
   },
   "sender":{  
         "plusFriendId" : String,
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

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|sender |	Object|	발신 프로필|
|- plusFriendId | String |	플러스친구 ID |
|- senderKey | String |	발신 키 |
|- categoryCode | String |	카테고리 코드 |
|- status | String |	NHN Cloud 플러스친구 상태 코드 <br>(YSC02: 등록 대기중, YSC03: 정상 등록) |
|- statusName | String |	NHN Cloud 플러스친구 상태명 (등록 대기중, 정상 등록) |
|- kakaoStatus | String |	카카오 플러스친구 상태 코드<br>(A: 정상, S: 차단)<br>status가 YSC02일 경우, kakaoStatus null 값을 가집니다. |
|- kakaoStatusName | String |	카카오 플러스친구 상태명 (정상, 차단)<br>status가 YSC02일 경우, kakaoStatusName null 값을 가집니다. |
|- kakaoProfileStatus | String |	카카오 플러스친구 프로필 상태 코드<br>(A: 활성화, B:차단, C: 비활성화, D:삭제 E:삭제 처리 중)<br>status가 YSC02일 경우, kakaoProfileStatus null 값을 가집니다.|
|- kakaoProfileStatusName | String | 카카오 플러스친구 프로필 상태명 (활성화, 비활성화, 차단, 삭제 처리 중, 삭제)<br>status가 YSC02일 경우, kakaoProfileStatusName null 값을 가집니다. |
|- alimtalk|	Object|	알림톡 설정 정보|
|-- resendAppKey | String | 대체 발송으로 설정할 SMS 서비스 앱키 |
|-- isResend | String | 대체 발송 설정(재발송) 여부|
|-- resendSendNo | String |	재발송 시, tc-sms 발신 번호 |
|-- dailyMaxCount | Integer |	알림톡 일별 최대 발송 건수<br>(값이 0일 경우 건수 제한없음) |
|-- sentCount | Integer |	알림톡 일별 발송 건수<br>(값이 0일 경우 건수 제한없음) |
|- friendtalk|	Object|	친구톡 설정 정보|
|-- resendAppKey | String | 대체 발송으로 설정할 SMS 서비스 앱키 |
|-- isResend | String | 대체 발송 설정(재발송) 여부|
|-- resendSendNo | String |	재발송 시, tc-sms 발신 번호 |
|-- resendUnsubscribeNo | String |	재발송 시, tc-sms 080 수신 거부 번호 |
|-- dailyMaxCount | Integer |	친구톡 일별 최대 발송 건수<br>(값이 0일 경우 건수 제한없음) |
|-- sentCount | Integer |	친구톡 일별 발송 건수<br>(값이 0일 경우 건수 제한없음) |
|- createDate | String |	등록 일자 |

### 발신 프로필 리스트 조회
#### 요청

[URL]

```
GET  /alimtalk/v2.0/appkeys/{appkey}/senders
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./sender-console-guide/#x-secret-key)] |

[Query parameter] 1번 or 2번 조건 필수

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|plusFriendId|	String|	X | 플러스친구 ID |
| senderKey | String | X | 발신 키 |
|status|	String|	X | 플러스친구 상태 코드 <br>(YSC02: 토큰 인증 대기중, YSC03: 정상 등록)|
|isSearchKakaoStatus|	boolean| X | 카카오 상태 조회 여부(false일 경우, 카카오 상태 관련 필드 (kakaoStatus, kakaoProfileStatus 등) null값)<br>default값 : true |
|pageNum|	Integer|	X|	페이지 번호(Default : 1)|
|pageSize|	Integer|	X|	조회 건수(Default : 15, Max : 1000)|

#### 응답
```
{  
   "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
   },
   "senders":[  
      {  
         "plusFriendId" : String,
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

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|senders|	Object|	발신 프로필 List|
|- plusFriendId | String |	플러스친구 ID |
|- senderKey | String |	발신 키 |
|- categoryCode | String |	카테고리 코드 |
|- status | String |	NHN Cloud 플러스친구 상태 코드 <br>(YSC02: 등록 대기중, YSC03: 정상 등록) |
|- statusName | String |	NHN Cloud 플러스친구 상태명 (등록 대기중, 정상 등록) |
|- kakaoStatus | String |	카카오 플러스친구 상태 코드<br>(A: 정상, S: 차단)<br>status가 YSC02일 경우, kakaoStatus null 값을 가집니다. |
|- kakaoStatusName | String |	카카오 플러스친구 상태명 (정상, 차단)<br>status가 YSC02일 경우, kakaoStatusName null 값을 가집니다. |
|- kakaoProfileStatus | String |	카카오 플러스친구 프로필 상태 코드<br>(A: 활성화, B:차단, C: 비활성화, D:삭제 E:삭제 처리 중)<br>status가 YSC02일 경우, kakaoProfileStatus null 값을 가집니다.|
|- kakaoProfileStatusName | String | 카카오 플러스친구 프로필 상태명 (활성화, 비활성화, 차단, 삭제 처리 중, 삭제)<br>status가 YSC02일 경우, kakaoProfileStatusName null 값을 가집니다. |
|- alimtalk|	Object|	알림톡 설정 정보|
|-- resendAppKey | String | 대체 발송으로 설정할 SMS 서비스 앱키 |
|-- isResend | String | 대체 발송 설정(재발송) 여부|
|-- resendSendNo | String |	재발송 시, tc-sms 발신 번호 |
|-- dailyMaxCount | Integer |	알림톡 일별 최대 발송 건수<br>(값이 0일 경우 건수 제한없음) |
|-- sentCount | Integer |	알림톡 일별 발송 건수<br>(값이 0일 경우 건수 제한없음) |
|- friendtalk|	Object|	친구톡 설정 정보|
|-- resendAppKey | String | 대체 발송으로 설정할 SMS 서비스 앱키 |
|-- isResend | String | 대체 발송 설정(재발송) 여부|
|-- resendSendNo | String |	재발송 시, tc-sms 발신 번호 |
|-- resendUnsubscribeNo | String |	재발송 시, tc-sms 080 수신 거부 번호 |
|-- dailyMaxCount | Integer |	친구톡 일별 최대 발송 건수<br>(값이 0일 경우 건수 제한없음) |
|-- sentCount | Integer |	친구톡 일별 발송 건수<br>(값이 0일 경우 건수 제한없음) |
|- createDate | String |	등록 일자 |
|totalCount | Integer | 총 개수 |

## 발신 프로필 그룹

### 발신 프로필 그룹 조회

#### 요청
[URL]

```
GET  /alimtalk/v2.0/appkeys/{appkey}/sender-groups/{groupSenderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey|
|groupSenderKey|	String|	발신 프로필 그룹 발신 키|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./sender-console-guide/#x-secret-key)] |

#### 응답
```
{
    "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
    },
    "senderGroup": {
        "groupName": String,
        "senderKey": String,
        "status": String,
        "senders": [
            {
                "plusFriendId": String,
                "senderKey": String,
                "createDate": String
            }
        ],
        "createDate": String,
        "updateDate": String
    }
}
```

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|senderGroup|	Object|	발신 프로필 그룹 |
|- groupName | String |	발신 프로필 그룹명 |
|- senderKey | String |	발신 키 |
|- status | String |	NHN Cloud 플러스친구 상태 코드 <br>(YSC02: 등록 대기중, YSC03: 정상 등록) |
|- senders | List |	그룹에 속한 발신 프로필 List |
|-- plusFriendId | String |	카카오톡 채널 검색용 ID |
|-- senderKey | String |	발신 키 |
|-- createDate | String | 그룹 등록 일자 |
|- createDate | String | 등록 일자 |
|- updateDate |	String|	수정 일자 |

### 그룹에 발신 프로필 추가

#### 요청
[URL]

```
POST  /alimtalk/v2.0/appkeys/{appkey}/sender-groups/{groupSenderKey}/senders/{senderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey|
|groupSenderKey|	String|	발신 프로필 그룹 발신 키|
|senderKey|	String|	발신 프로필 발신 키|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./sender-console-guide/#x-secret-key)] |

#### 응답
```
{
    "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
    }
}
```

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|

### 그룹에 발신 프로필 삭제

#### 요청
[URL]

```
DELETE  /alimtalk/v2.0/appkeys/{appkey}/sender-groups/{groupSenderKey}/senders/{senderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey|
|groupSenderKey|	String|	발신 프로필 그룹 발신 키|
|senderKey|	String|	발신 프로필 발신 키|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./sender-console-guide/#x-secret-key)] |

#### 응답
```
{
    "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
    }
}
```

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
