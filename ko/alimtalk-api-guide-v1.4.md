## Notification > KakaoTalk Bizmessage > Alimtalk > API v1.4 Guide

## 알림톡

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

## v1.4 API 소개
1. 인증 메시지 API에 대한 본문 유효성 검사가 추가되었습니다.
   - 자세한 사항은 [[인증 메시지 API](./alimtalk-api-guide/#precautions-authword)] 참고하시기 바랍니다.

## 일반 메시지

### 메시지 치환 발송 요청

[URL]

```
POST  /alimtalk/v1.4/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Request body]

```
{
    "plusFriendId": String,
    "templateCode": String,
    "requestDate": String,
    "senderGroupingKey": String,
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

|값|	타입|	필수|	설명|
|---|---|---|---|
|plusFriendId|	String|	O | 플러스친구 아이디 (최대 30자) |
|templateCode|	String|	O | 등록한 발송 템플릿 코드 (최대 20자) |
|requestDate| String | X| 요청 일시 (yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송) |
|senderGroupingKey| String | X| 발신 그룹핑 키 (최대 100자) |
|recipientList|	List|	O|	수신자 리스트 (최대 1000명) |
|- recipientNo|	String|	O|	수신번호 (최대 15자) |
|- templateParameter|	Object|	X|	템플릿 파라미터<br>(템플릿에 치환할 변수 포함 시, 필수) |
|-- key|	String|	X |	치환 키(#{key})|
|-- value| String |	X |	치환 키에 매핑되는 Value값|
|- resendParameter|	Object|	X| 대체 발송 정보 |
|-- isResend|	boolean|	X|	발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 재발송됩니다. |
|-- resendType|	String|	X|	대체 발송 타입(SMS,LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다. |
|-- resendTitle|	String|	X|	LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 재발송됩니다.) |
|-- resendContent|	String|	X|	대체 발송 내용<br>(값이 없을 경우, 템플릿 내용으로 재발송됩니다.) |
|-- resendSendNo | String| X| 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span> |
|- recipientGroupingKey|	String|	X|	수신자 그룹핑 키 (최대 100자) |

* <b>요청 일시는 호출하는 시점부터 90일 후까지 설정 가능합니다.</b>
* <b>SMS 서비스에서 대체 발송되므로, SMS 서비스의 발송 API 명세에 따라 필드를 입력해야 합니다.(SMS 서비스에 등록된 발신 번호, 각종 필드 길이 제한 등)</b>
* <b>SMS 서비스는 국제 SMS만 지원합니다. 국제 수신자 번호일 경우, resendType(대체 발송 타입)을 SMS로 변경해야 정상적으로 대체 발송할 수 있습니다.</b>
* <b>지정한 대체 발송 타입의 바이트 제한을 초과하는 대체 발송 제목이나 내용은 잘려서 대체 발송될 수 있습니다.([[SMS 주의사항](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] 참고)</b>

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v1.4/appkeys/{appkey}/messages -d '{"plusFriendId":"{플러스친구 아이디}","templateCode":"{템플릿 코드}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{수신번호}","templateParameter":{"{치환자 필드}":"{치환 데이터}"}}]}'
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|message|	Object|	본문 영역|
|- requestId | String |	요청 아이디 |
|- senderGroupingKey | String |	발신 그룹핑 키 |
|- sendResults | Object | 발송 요청 결과 |
|-- recipientSeq | Integer | 수신자 시퀀스 번호 |
|-- recipientNo | String | 수신 번호 |
|-- resultCode | Integer | 발송 요청 결과 코드 |
|-- resultMessage | String | 발송 요청 결과 메시지 |
|-- recipientGroupingKey | String | 수신자 그룹핑 키 |

### 메시지 전문 발송 요청

[URL]

```
POST  /alimtalk/v1.4/appkeys/{appkey}/raw-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

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

|값|	타입|	필수|	설명|
|---|---|---|---|
|plusFriendId|	String|	O | 플러스친구 아이디 (최대 30자) |
|templateCode|	String|	O | 등록한 발송 템플릿 코드 (최대 20자) |
|requestDate| String | X| 요청 일시 (yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송) |
|senderGroupingKey| String | X| 발신 그룹핑 키 (최대 100자) |
|recipientList|	List|	O|	수신자 리스트 (최대 1,000명) |
|- recipientNo|	String|	O|	수신번호 (최대 15자) |
|- content|	String|	O|	내용 (최대 1000자) |
|- buttons|	List |	X | 버튼 리스트 (최대 5개) |
|-- ordering|	Integer|	X |	버튼 순서 (버튼이 있는 경우 필수)|
|-- type| String |	X |	버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달) |
|-- name| String |	X |	버튼 이름 (버튼이 있는 경우 필수, 최대 14자)|
|-- linkMo| String |	X |	모바일 웹 링크 (WL 타입일 경우 필수 필드, 최대 200자)|
|-- linkPc | String |	X |PC 웹 링크  (WL 타입일 경우 선택 필드, 최대 200자) |
|-- schemeIos | String | X |	IOS 앱 링크 (AL 타입일 경우 필수 필드, 최대 200자) |
|-- schemeAndroid | String | X |	Android 앱 링크 (AL 타입일 경우 필수 필드, 최대 200자) |
|- resendParameter|	Object|	X| 대체 발송 정보 |
|-- isResend|	boolean|	X|	발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 재발송됩니다. |
|-- resendType|	String|	X|	대체 발송 타입(SMS,LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다. |
|-- resendTitle|	String|	X|	LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 재발송됩니다.) |
|-- resendContent|	String|	X|	대체 발송 내용<br>(값이 없을 경우, 템플릿 내용으로 재발송됩니다.) |
|-- resendSendNo | String| X| 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span> |
|- recipientGroupingKey|	String|	X|	수신자 그룹핑 키 (최대 100자) |

* <b>본문과 버튼에 치환이 완성된 데이터를 넣어주세요.</b>
* <b>요청 일시는 호출하는 시점부터 90일 후까지 설정 가능합니다.</b>
* <b>SMS 서비스에서 대체 발송되므로, SMS 서비스의 발송 API 명세에 따라 필드를 입력해야 합니다.(SMS 서비스에 등록된 발신 번호, 각종 필드 길이 제한 등)</b>
* <b>SMS 서비스는 국제 SMS만 지원합니다. 국제 수신자 번호일 경우, resendType(대체 발송 타입)을 SMS로 변경해야 정상적으로 대체 발송할 수 있습니다.</b>
* <b>지정한 대체 발송 타입의 바이트 제한을 초과하는 대체 발송 제목이나 내용은 잘려서 대체 발송될 수 있습니다.([[SMS 주의사항](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] 참고)</b>

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v1.4/appkeys/{appkey}/raw-messages -d '{"plusFriendId":"{플러스친구 아이디}","templateCode":"{템플릿 코드}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{수신번호}","content":"{내용}","buttons":[{"ordering":"{버튼 순서}","type":"{버튼 타입}","name":"{버튼 이름}","linkMo":"{모바일 웹 링크}"}]}]}'
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|message|	Object|	본문 영역|
|- requestId | String |	요청 아이디 |
|- senderGroupingKey | String |	발신 그룹핑 키 |
|- sendResults | Object | 발송 요청 결과 |
|-- recipientSeq | Integer | 수신자 시퀀스 번호 |
|-- recipientNo | String | 수신 번호 |
|-- resultCode | Integer | 발송 요청 결과 코드 |
|-- resultMessage | String | 발송 요청 결과 메시지 |
|-- recipientGroupingKey | String | 수신자 그룹핑 키 |

### 메시지 리스트 조회

#### 요청

[URL]

```
GET  /alimtalk/v1.4/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter] 1번 or 2번 조건 필수

|값|	타입|	필수|	설명|
|---|---|---|---|
|requestId|	String|	조건 필수 (1번) | 요청 아이디 |
|startRequestDate|	String|	조건 필수 (2번) | 발송 요청 날짜 시작 값(yyyy-MM-dd HH:mm)|
|endRequestDate|	String| 조건 필수 (2번) |	발송 요청 날짜 끝 값(yyyy-MM-dd HH:mm) |
|recipientNo|	String|	X |	수신번호 |
|plusFriendId|	String|	X |	플러스친구 아이디 |
|templateCode|	String|	X |	템플릿 코드|
|senderGroupingKey| String | X| 발신 그룹핑 키 |
|recipientGroupingKey|	String|	X|	수신자 그룹핑 키 |
|messageStatus| String |	X | 요청 상태 ( COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 )	|
|resultCode| String |	X | 발송 결과 ( MRC01 -> 성공 MRC02 -> 실패 )	|
|pageNum|	Integer|	X|	페이지 번호(Default : 1)|
|pageSize|	Integer|	X|	조회 건수(Default : 15, Max : 1000)|

* 90일 이전 발송 요청 데이터는 조회되지 않습니다.
* 발송 요청 일시의 범위는 최대 30일입니다.

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|messageSearchResultResponse|	Object|	본문 영역|
|- messages | List |	메시지 리스트 |
|-- requestId | String |	요청 아이디 |
|-- recipientSeq | Integer |	수신자 시퀀스 번호 |
|-- plusFriendId | String |	플러스친구 아이디 |
|-- templateCode | String |	템플릿 코드 |
|-- recipientNo | String |	수신 번호 |
|-- content | String |	본문 |
|-- requestDate | String |	요청 일시 |
|-- receiveDate | String |	수신 일시 |
|-- resendStatus | String |	재발송 상태 코드 |
|-- resendStatusName | String |	재발송 상태 코드명 |
|-- messageStatus | String |	요청 상태 ( COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 ) |
|-- resultCode | String |	수신 결과 코드 |
|-- resultCodeName | String |	수신 결과 코드명 |
|-- buttons | List |	버튼 리스트 |
|--- ordering | Integer |	버튼 순서 |
|--- type | String |	버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달) |
|--- name | String |	버튼 이름 |
|--- linkMo | String |	모바일 웹 링크 (WL 타입일 경우 필수 필드) |
|--- linkPc | String |	PC 웹 링크  (WL 타입일 경우 선택 필드) |
|--- schemeIos | String |	IOS 앱 링크 (AL 타입일 경우 필수 필드) |
|--- schemeAndroid | String |	Android 앱 링크 (AL 타입일 경우 필수 필드) |
|-- senderGroupingKey | String | 발신 그룹핑 키 |
|-- recipientGroupingKey | String |	수신자 그룹핑 키 |
|- totalCount | Integer | 총 개수 |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.4/appkeys/{appkey}/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

#### SMS/LMS 재발송 상태
|값|	설명|
|---|---|
|RSC01|	재발송 미대상|
|RSC02|	재발송 대상 (발송 결과 실패 시, 재발송이 진행됩니다.)|
|RSC03|	재발송 중|
|RSC04|	재발송 성공|
|RSC05|	재발송 실패|

### 메시지 단건 조회

#### 요청

[URL]

```
GET  /alimtalk/v1.4/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey |
|requestId|	String|	요청 아이디 |
|recipientSeq|	Integer|	수신자 시퀀스 번호 |

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.4/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
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
}
```

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|message|	Object|	메시지|
|- requestId | String |	요청 아이디 |
|- recipientSeq | Integer |	수신자 시퀀스 번호 |
|- plusFriendId | String |	플러스친구 아이디 |
|- templateCode | String |	템플릿 코드 |
|- recipientNo | String |	수신 번호 |
|- content | String |	본문 |
|- requestDate | String |	요청 일시 |
|- receiveDate | String |	수신 일시 |
|- resendStatus | String |	재발송 상태 코드 |
|- resendStatusName | String |	재발송 상태 코드명 |
|- messageStatus | String |	요청 상태 ( COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 ) |
|- resultCode | String |	수신 결과 코드 |
|- resultCodeName | String |	수신 결과 코드명 |
|- buttons | List |	버튼 리스트 |
|-- ordering | Integer |	버튼 순서 |
|-- type | String |	버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달) |
|-- name | String |	버튼 이름 |
|-- linkMo | String |	모바일 웹 링크 (WL 타입일 경우 필수 필드) |
|-- linkPc | String |	PC 웹 링크  (WL 타입일 경우 선택 필드) |
|-- schemeIos | String |	IOS 앱 링크 (AL 타입일 경우 필수 필드) |
|-- schemeAndroid | String |	Android 앱 링크 (AL 타입일 경우 필수 필드) |
|- senderGroupingKey | String | 발신 그룹핑 키 |
|- recipientGroupingKey | String |	수신자 그룹핑 키 |

## 인증 메시지

<span id="precautions-authword"></span>
1. 인증 메시지 발송 시 포함되어야 할 인증 문구 안내

| 구분  | 인증 문구 |
| --- | --- |
| 인증 메시지 | auth, password, verif, にんしょう, 認証, 비밀번호, 인증 |

- 예시 1-1) 인증 메시지 API 요청시 전문(템플릿 치환자 포함)에 인증 문구가 포함되어 있지 않은 경우 발송 실패됩니다.
- 예시 1-2) 인증 문구가 영문인 경우 대소문자 구분 없이 유효성 검사가 진행됩니다.


### 메시지 치환 발송 요청

[URL]

```
POST  /alimtalk/v1.4/appkeys/{appkey}/auth/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Request body]

```
{
    "plusFriendId": String,
    "templateCode": String,
    "requestDate": String,
    "senderGroupingKey": String,
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

|값|	타입|	필수|	설명|
|---|---|---|---|
|plusFriendId|	String|	O | 플러스친구 아이디 (최대 30자) |
|templateCode|	String|	O | 등록한 발송 템플릿 코드 (최대 20자) |
|requestDate| String | X| 요청 일시 (yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송) |
|senderGroupingKey| String | X| 발신 그룹핑 키 (최대 100자) |
|recipientList|	List|	O|	수신자 리스트 (최대 1000명) |
|- recipientNo|	String|	O|	수신번호 (최대 15자) |
|- templateParameter|	Object|	X|	템플릿 파라미터<br>(템플릿에 치환할 변수 포함 시, 필수) |
|-- key|	String|	X |	치환 키(#{key})|
|-- value| String |	X |	치환 키에 매핑되는 Value값|
|- resendParameter|	Object|	X| 대체 발송 정보 |
|-- isResend|	boolean|	X|	발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 재발송됩니다. |
|-- resendType|	String|	X|	대체 발송 타입(SMS,LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다. |
|-- resendTitle|	String|	X|	LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 재발송됩니다.) |
|-- resendContent|	String|	X|	대체 발송 내용<br>(값이 없을 경우, 템플릿 내용으로 재발송됩니다.) |
|-- resendSendNo | String| X| 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span> |
|- recipientGroupingKey|	String|	X|	수신자 그룹핑 키 (최대 100자) |

* <b>요청 일시는 호출하는 시점부터 90일 후까지 설정 가능합니다.</b>
* <b>SMS 서비스에서 대체 발송되므로, SMS 서비스의 발송 API 명세에 따라 필드를 입력해야 합니다.(SMS 서비스에 등록된 발신 번호, 각종 필드 길이 제한 등)</b>
* <b>지정한 대체 발송 타입의 바이트 제한을 초과하는 대체 발송 제목이나 내용은 잘려서 대체 발송될 수 있습니다.([[SMS 주의사항](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] 참고)</b>

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v1.4/appkeys/{appkey}/auth/messages -d '{"plusFriendId":"{플러스친구 아이디}","templateCode":"{템플릿 코드}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{수신번호}","templateParameter":{"{치환자 필드}":"{치환 데이터}"}}]}'
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|message|	Object|	본문 영역|
|- requestId | String |	요청 아이디 |
|- senderGroupingKey | String |	발신 그룹핑 키 |
|- sendResults | Object | 발송 요청 결과 |
|-- recipientSeq | Integer | 수신자 시퀀스 번호 |
|-- recipientNo | String | 수신 번호 |
|-- resultCode | Integer | 발송 요청 결과 코드 |
|-- resultMessage | String | 발송 요청 결과 메시지 |
|-- recipientGroupingKey | String | 수신자 그룹핑 키 |

### 메시지 전문 발송 요청

[URL]

```
POST  /alimtalk/v1.4/appkeys/{appkey}/auth/raw-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

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

|값|	타입|	필수|	설명|
|---|---|---|---|
|plusFriendId|	String|	O | 플러스친구 아이디 (최대 30자) |
|templateCode|	String|	O | 등록한 발송 템플릿 코드 (최대 20자) |
|requestDate| String | X| 요청 일시 (yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송) |
|senderGroupingKey| String | X| 발신 그룹핑 키 (최대 100자) |
|recipientList|	List|	O|	수신자 리스트 (최대 1,000명) |
|- recipientNo|	String|	O|	수신번호 (최대 15자) |
|- content|	String|	O|	내용 (최대 1000자) |
|- buttons|	List |	X | 버튼 리스트 (최대 5개) |
|-- ordering|	Integer|	X |	버튼 순서 (버튼이 있는 경우 필수)|
|-- type| String |	X |	버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달) |
|-- name| String |	X |	버튼 이름 (버튼이 있는 경우 필수, 최대 14자)|
|-- linkMo| String |	X |	모바일 웹 링크 (WL 타입일 경우 필수 필드, 최대 200자)|
|-- linkPc | String |	X |PC 웹 링크  (WL 타입일 경우 선택 필드, 최대 200자) |
|-- schemeIos | String | X |	IOS 앱 링크 (AL 타입일 경우 필수 필드, 최대 200자) |
|-- schemeAndroid | String | X |	Android 앱 링크 (AL 타입일 경우 필수 필드, 최대 200자) |
|- resendParameter|	Object|	X| 대체 발송 정보 |
|-- isResend|	boolean|	X|	발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 재발송됩니다. |
|-- resendType|	String|	X|	대체 발송 타입(SMS,LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다. |
|-- resendTitle|	String|	X|	LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 재발송됩니다.) |
|-- resendContent|	String|	X|	대체 발송 내용<br>(값이 없을 경우, 템플릿 내용으로 재발송됩니다.) |
|-- resendSendNo | String| X| 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span> |
|- recipientGroupingKey|	String|	X|	수신자 그룹핑 키 (최대 100자) |

* <b>본문과 버튼에 치환이 완성된 데이터를 넣어주세요.</b>
* <b>요청 일시는 호출하는 시점부터 90일 후까지 설정 가능합니다.</b>

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v1.4/appkeys/{appkey}/auth/raw-messages -d '{"plusFriendId":"{플러스친구 아이디}","templateCode":"{템플릿 코드}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{수신번호}","content":"{내용}","buttons":[{"ordering":"{버튼 순서}","type":"{버튼 타입}","name":"{버튼 이름}","linkMo":"{모바일 웹 링크}"}]}]}'
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|message|	Object|	본문 영역|
|- requestId | String |	요청 아이디 |
|- senderGroupingKey | String |	발신 그룹핑 키 |
|- sendResults | Object | 발송 요청 결과 |
|-- recipientSeq | Integer | 수신자 시퀀스 번호 |
|-- recipientNo | String | 수신 번호 |
|-- resultCode | Integer | 발송 요청 결과 코드 |
|-- resultMessage | String | 발송 요청 결과 메시지 |
|-- recipientGroupingKey | String | 수신자 그룹핑 키 |

### 메시지 리스트 조회

#### 요청

[URL]

```
GET  /alimtalk/v1.4/appkeys/{appkey}/auth/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter] 1번 or 2번 조건 필수

|값|	타입|	필수|	설명|
|---|---|---|---|
|requestId|	String|	조건 필수 (1번) | 요청 아이디 |
|startRequestDate|	String|	조건 필수 (2번) | 발송 요청 날짜 시작 값(yyyy-MM-dd HH:mm)|
|endRequestDate|	String| 조건 필수 (2번) |	발송 요청 날짜 끝 값(yyyy-MM-dd HH:mm) |
|recipientNo|	String|	X |	수신번호 |
|plusFriendId|	String|	X |	플러스친구 아이디 |
|templateCode|	String|	X |	템플릿 코드|
|senderGroupingKey| String | X| 발신 그룹핑 키 |
|recipientGroupingKey|	String|	X|	수신자 그룹핑 키 |
|messageStatus| String |	X | 요청 상태 ( COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 )	|
|resultCode| String |	X | 발송 결과 ( MRC01 -> 성공 MRC02 -> 실패 )	|
|pageNum|	Integer|	X|	페이지 번호(Default : 1)|
|pageSize|	Integer|	X|	조회 건수(Default : 15, Max : 1000)|

* 90일 이전 발송 요청 데이터는 조회되지 않습니다.
* 발송 요청 일시의 범위는 최대 30일입니다.

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|messageSearchResultResponse|	Object|	본문 영역|
|- messages | List |	메시지 리스트 |
|-- requestId | String |	요청 아이디 |
|-- recipientSeq | Integer |	수신자 시퀀스 번호 |
|-- plusFriendId | String |	플러스친구 아이디 |
|-- templateCode | String |	템플릿 코드 |
|-- recipientNo | String |	수신 번호 |
|-- content | String |	본문 |
|-- requestDate | String |	요청 일시 |
|-- receiveDate | String |	수신 일시 |
|-- resendStatus | String |	재발송 상태 코드 |
|-- resendStatusName | String |	재발송 상태 코드명 |
|-- messageStatus | String |	요청 상태 ( COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 ) |
|-- resultCode | String |	수신 결과 코드 |
|-- resultCodeName | String |	수신 결과 코드명 |
|-- buttons | List |	버튼 리스트 |
|--- ordering | Integer |	버튼 순서 |
|--- type | String |	버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달) |
|--- name | String |	버튼 이름 |
|--- linkMo | String |	모바일 웹 링크 (WL 타입일 경우 필수 필드) |
|--- linkPc | String |	PC 웹 링크  (WL 타입일 경우 선택 필드) |
|--- schemeIos | String |	IOS 앱 링크 (AL 타입일 경우 필수 필드) |
|--- schemeAndroid | String |	Android 앱 링크 (AL 타입일 경우 필수 필드) |
|-- senderGroupingKey | String | 발신 그룹핑 키 |
|-- recipientGroupingKey | String |	수신자 그룹핑 키 |
|- totalCount | Integer | 총 개수 |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.4/appkeys/{appkey}/auth/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

#### SMS/LMS 재발송 상태
|값|	설명|
|---|---|
|RSC01|	재발송 미대상|
|RSC02|	재발송 대상 (발송 결과 실패 시, 재발송이 진행됩니다.)|
|RSC03|	재발송 중|
|RSC04|	재발송 성공|
|RSC05|	재발송 실패|

### 메시지 단건 조회

#### 요청

[URL]

```
GET  /alimtalk/v1.4/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey |
|requestId|	String|	요청 아이디 |
|recipientSeq|	Integer|	수신자 시퀀스 번호 |

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.4/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}"
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
}
```

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|message|	Object|	메시지|
|- requestId | String |	요청 아이디 |
|- recipientSeq | Integer |	수신자 시퀀스 번호 |
|- plusFriendId | String |	플러스친구 아이디 |
|- templateCode | String |	템플릿 코드 |
|- recipientNo | String |	수신 번호 |
|- content | String |	본문 |
|- requestDate | String |	요청 일시 |
|- receiveDate | String |	수신 일시 |
|- resendStatus | String |	재발송 상태 코드 |
|- resendStatusName | String |	재발송 상태 코드명 |
|- messageStatus | String |	요청 상태 ( COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 ) |
|- resultCode | String |	수신 결과 코드 |
|- resultCodeName | String |	수신 결과 코드명 |
|- buttons | List |	버튼 리스트 |
|-- ordering | Integer |	버튼 순서 |
|-- type | String |	버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달) |
|-- name | String |	버튼 이름 |
|-- linkMo | String |	모바일 웹 링크 (WL 타입일 경우 필수 필드) |
|-- linkPc | String |	PC 웹 링크  (WL 타입일 경우 선택 필드) |
|-- schemeIos | String |	IOS 앱 링크 (AL 타입일 경우 필수 필드) |
|-- schemeAndroid | String |	Android 앱 링크 (AL 타입일 경우 필수 필드) |
|- senderGroupingKey | String | 발신 그룹핑 키 |
|- recipientGroupingKey | String |	수신자 그룹핑 키 |

## 메시지
### 메시지 발송 취소

#### 요청

[URL]

```
DELETE  /alimtalk/v1.4/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 앱키|
|requestId| String| 요청 ID|

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|recipientSeq|	String|	X | 수신자 시퀀스 번호<br>(입력하지 않으면 요청 ID의 모든 발송 건을 취소) |

* 일반/인증 메시지 모두 동일한 API로 취소할 수 있습니다.

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|

[예시]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.4/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

### 메시지 결과 업데이트 조회

#### 요청

[URL]

```
GET  /alimtalk/v1.4/appkeys/{appkey}/message-results
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 앱키|

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|startUpdateDate|	String|	O | 결과 업데이트 조회 시작 시간(yyyy-MM-dd HH:mm)|
|endUpdateDate|	String| O |	결과 업데이트 조회 종료 시간(yyyy-MM-dd HH:mm) |
|alimtalkMessageType|	String| X |	알림톡 메시지 타입(NORMAL, AUTH) |
|pageNum|	Integer|	X|	페이지 번호(기본: 1)|
|pageSize|	Integer|	X|	조회 건수(Default : 15, Max : 1000)|

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|messageSearchResultResponse|	Object|	본문 영역|
|- messages | List |	메시지 리스트 |
|-- requestId | String |	요청 ID |
|-- recipientSeq | Integer |	수신자 시퀀스 번호 |
|-- plusFriendId | String |	플러스친구 ID |
|-- templateCode | String |	템플릿 코드 |
|-- recipientNo | String |	수신 번호 |
|-- content | String |	본문 |
|-- requestDate | String |	요청 일시 |
|-- receiveDate | String |	수신 일시 |
|-- resendStatus | String |	재발송 상태 코드 |
|-- resendStatusName | String |	재발송 상태 코드명 |
|-- messageStatus | String |	요청 상태(COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 ) |
|-- resultCode | String |	수신 결과 코드 |
|-- resultCodeName | String |	수신 결과 코드명 |
|-- buttons | List |	버튼 리스트 |
|--- ordering | Integer |	버튼 순서 |
|--- type | String |	버튼 타입(WL:웹 링크, AL:앱 링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달) |
|--- name | String |	버튼 이름 |
|--- linkMo | String |	모바일 웹 링크(WL 타입일 경우 필수 필드) |
|--- linkPc | String |	PC 웹 링크(WL 타입일 경우 선택 필드) |
|--- schemeIos | String | iOS 앱 링크(AL 타입일 경우 필수 필드) |
|--- schemeAndroid | String |	Android 앱 링크(AL 타입일 경우 필수 필드) |
|-- senderGroupingKey | String | 발신 그룹핑 키 |
|-- recipientGroupingKey | String |	수신자 그룹핑 키 |
|- totalCount | Integer | 총 개수 |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.4/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```


## 플러스친구

### 플러스친구 카테고리 조회

#### 요청
[URL]

```
GET  /alimtalk/v1.4/appkeys/{appkey}/plus-friends/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

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

|값|	타입|	설명|
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

### 플러스친구 사업자등록증 업로드
#### 요청
[URL]

```
POST  /alimtalk/v1.4/appkeys/{appkey}/business-licenses
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "fileName" : String,
  "fileBody" : "{byte[] -> Base64 인코딩한 값}"
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|fileName|	String |	O | 파일 이름 |
|fileBody|	Byte[] |	O | 파일 byte[]를 Base64로 인코딩한 값.(최대 500KB)<br>또는 byte 배열 값 |

#### 응답
```
{
  "header" : {
    "resultCode" :  Integer,
    "resultMessage" :  String,
    "isSuccessful" :  boolean
  },
  "attachFile" : {
    "fileSeq" : Integer
  }
}
```

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|attachFile|	Object|	첨부파일|
|- fileSeq | Integer |	파일 시퀀스 |


### 플러스친구 등록
#### 요청
[URL]

```
POST  /alimtalk/v1.4/appkeys/{appkey}/plus-friends
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "plusFriendId" : String,
  "phoneNo" : String,
  "categoryCode" : String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|plusFriendId|	String|	O | 플러스친구 아이디 (최대 30자) |
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|

### 플러스친구 토큰 인증
#### 요청
[URL]

```
POST  /alimtalk/v1.4/appkeys/{appkey}/plus-friends/{plusFriendId}/tokens
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|
|plusFriendId|	String|	플러스친구 아이디 |

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "token" : "Integer"
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|token|	Integer |	O | 인증 토큰 (플러스친구 등록 API 호출 후, 카카오톡 앱으로 받은 인증 토큰) |

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|

### 플러스친구 단건 조회
#### 요청

[URL]

```
GET  /alimtalk/v1.4/appkeys/{appkey}/plus-friends/{plusFriendId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|
|plusFriendId| String | 플러스친구 아이디 |

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

#### 응답
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
                "isResend": Boolean,
                "resendSendNo": String,
                "dailyMaxCount" : Integer,
                "sentCount" : Integer
          },
         "friendtalk": {  
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|plusFriends|	Object|	플러스친구|
|- plusFriendId | String |	플러스친구 아이디 |
|- plusFriendType | String | 플러스친구 타입(NORMAL, GROUP) |
|- senderKey | String |	발신키 |
|- categoryCode | String |	카테고리 코드 |
|- status | String |	TOAST 플러스친구 상태 코드 <br>(YSC02: 등록 대기중, YSC03: 정상 등록) |
|- statusName | String |	TOAST 플러스친구 상태명 (등록 대기중, 정상 등록) |
|- kakaoStatus | String |	카카오 플러스친구 상태 코드<br>(A: 정상, S: 차단, D:삭제)<br>status가 YSC02일 경우, kakaoStatus null 값을 가집니다. |
|- kakaoStatusName | String |	카카오 플러스친구 상태명 (정상, 차단, 삭제)<br>status가 YSC02일 경우, kakaoStatusName null 값을 가집니다. |
|- kakaoProfileStatus | String |	카카오 플러스친구 프로필 상태 코드<br>(A: 활성화, B:차단, C: 비활성화, D:삭제 E:삭제 처리 중)<br>status가 YSC02일 경우, kakaoProfileStatus null 값을 가집니다.|
|- kakaoProfileStatusName | String | 카카오 플러스친구 프로필 상태명 (활성화, 비활성화, 차단, 삭제 처리 중, 삭제)<br>status가 YSC02일 경우, kakaoProfileStatusName null 값을 가집니다. |
|- alimtalk|	Object|	알림톡 설정 정보|
|-- isResend | String | 대체 발송 설정(재발송) 여부|
|-- resendSendNo | String |	재발송 시, tc-sms 발신 번호 |
|-- dailyMaxCount | Integer |	알림톡 일별 최대 발송 건수<br>(값이 0일 경우 건수 제한없음) |
|-- sentCount | Integer |	알림톡 일별 발송 건수<br>(값이 0일 경우 건수 제한없음) |
|- friendtalk|	Object|	친구톡 설정 정보|
|-- isResend | String | 대체 발송 설정(재발송) 여부|
|-- resendSendNo | String |	재발송 시, tc-sms 발신 번호 |
|-- resendUnsubscribeNo | String |	재발송 시, tc-sms 080 수신 거부 번호 |
|-- dailyMaxCount | Integer |	친구톡 일별 최대 발송 건수<br>(값이 0일 경우 건수 제한없음) |
|-- sentCount | Integer |	친구톡 일별 발송 건수<br>(값이 0일 경우 건수 제한없음) |
|- createDate | String |	등록 일자 |

### 플러스친구 리스트 조회
#### 요청

[URL]

```
GET  /alimtalk/v1.4/appkeys/{appkey}/plus-friends
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter] 1번 or 2번 조건 필수

|값|	타입|	필수|	설명|
|---|---|---|---|
|plusFriendId|	String|	X | 플러스친구 아이디 |
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
                "isResend": Boolean,
                "resendSendNo": String,
                "dailyMaxCount" : Integer,
                "sentCount" : Integer
          },
         "friendtalk": {  
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|plusFriends|	Object|	플러스친구|
|- plusFriendId | String |	플러스친구 아이디 |
|- plusFriendType | String | 플러스친구 타입(NORMAL, GROUP) |
|- senderKey | String |	발신키 |
|- categoryCode | String |	카테고리 코드 |
|- status | String |	TOAST 플러스친구 상태 코드 <br>(YSC02: 등록 대기중, YSC03: 정상 등록) |
|- statusName | String |	TOAST 플러스친구 상태명 (등록 대기중, 정상 등록) |
|- kakaoStatus | String |	카카오 플러스친구 상태 코드<br>(A: 정상, S: 차단, D:삭제)<br>status가 YSC02일 경우, kakaoStatus null 값을 가집니다. |
|- kakaoStatusName | String |	카카오 플러스친구 상태명 (정상, 차단, 삭제)<br>status가 YSC02일 경우, kakaoStatusName null 값을 가집니다. |
|- kakaoProfileStatus | String |	카카오 플러스친구 프로필 상태 코드<br>(A: 활성화, B:차단, C: 비활성화, D:삭제 E:삭제 처리 중)<br>status가 YSC02일 경우, kakaoProfileStatus null 값을 가집니다.|
|- kakaoProfileStatusName | String | 카카오 플러스친구 프로필 상태명 (활성화, 비활성화, 차단, 삭제 처리 중, 삭제)<br>status가 YSC02일 경우, kakaoProfileStatusName null 값을 가집니다. |
|- alimtalk|	Object|	알림톡 설정 정보|
|-- isResend | String | 대체 발송 설정(재발송) 여부|
|-- resendSendNo | String |	재발송 시, tc-sms 발신 번호 |
|-- dailyMaxCount | Integer |	알림톡 일별 최대 발송 건수<br>(값이 0일 경우 건수 제한없음) |
|-- sentCount | Integer |	알림톡 일별 발송 건수<br>(값이 0일 경우 건수 제한없음) |
|- friendtalk|	Object|	친구톡 설정 정보|
|-- isResend | String | 대체 발송 설정(재발송) 여부|
|-- resendSendNo | String |	재발송 시, tc-sms 발신 번호 |
|-- resendUnsubscribeNo | String |	재발송 시, tc-sms 080 수신 거부 번호 |
|-- dailyMaxCount | Integer |	친구톡 일별 최대 발송 건수<br>(값이 0일 경우 건수 제한없음) |
|-- sentCount | Integer |	친구톡 일별 발송 건수<br>(값이 0일 경우 건수 제한없음) |
|- createDate | String |	등록 일자 |
|totalCount | Integer | 총 개수 |

## 템플릿

### 템플릿 등록
#### 요청
[URL]

```
POST  /alimtalk/v1.4/appkeys/{appkey}/plus-friends/{plusFriendId}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey |
|plusFriendId|	String|	플러스친구 아이디 |

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

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

|값|	타입|	필수|	설명|
|---|---|---|---|
|templateCode|	String |	O | 템플릿 코드 (최대 20자) |
|templateName|	String |	O | 템플릿명 (최대 20자) |
|templateContent|	String |	O | 템플릿 본문 (최대 1000자) |
|buttons|	List |	X | 버튼 리스트 (최대 5개) |
|-ordering|	Integer |	X | 버튼 순서(1~5) |
|-type|	String |	X | 버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달) |
|-name| String |	X |	버튼 이름 (버튼이 있는 경우 필수, 최대 14자)|
|-linkMo| String |	X |	모바일 웹 링크 (WL 타입일 경우 필수 필드, 최대 200자)|
|-linkPc | String |	X |PC 웹 링크  (WL 타입일 경우 선택 필드, 최대 200자) |
|-schemeIos | String | X |	IOS 앱 링크 (AL 타입일 경우 필수 필드, 최대 200자) |
|-schemeAndroid | String | X |	Android 앱 링크 (AL 타입일 경우 필수 필드, 최대 200자) |

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|

### 템플릿 수정
#### 요청
[URL]

```
PUT  /alimtalk/v1.4/appkeys/{appkey}/plus-friends/{plusFriendId}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey |
|plusFriendId|	String|	플러스친구 아이디 |
|templateCode|	String|	템플릿 코드 |

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

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

|값|	타입|	필수|	설명|
|---|---|---|---|
|templateName|	String |	O | 템플릿명 (최대 20자) |
|templateContent|	String |	O | 템플릿 본문 (최대 1000자) |
|buttons|	List |	X | 버튼 리스트 (최대 5개) |
|-ordering|	Integer |	X | 버튼 순서(1~5) |
|-type|	String |	X | 버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달) |
|-name| String |	X |	버튼 이름 (버튼이 있는 경우 필수, 최대 14자)|
|-linkMo| String |	X |	모바일 웹 링크 (WL 타입일 경우 필수 필드, 최대 200자)|
|-linkPc | String |	X |PC 웹 링크  (WL 타입일 경우 선택 필드, 최대 200자) |
|-schemeIos | String | X |	IOS 앱 링크 (AL 타입일 경우 필수 필드, 최대 200자) |
|-schemeAndroid | String | X |	Android 앱 링크 (AL 타입일 경우 필수 필드, 최대 200자) |

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|

### 템플릿 삭제
#### 요청
[URL]

```
DELETE  /alimtalk/v1.4/appkeys/{appkey}/plus-friends/{plusFriendId}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|
|plusFriendId|	String|	플러스친구 아이디 |
|templateCode|	String|	템플릿 코드 |

[Header]
```
{
  "X-Secret-Key": String
}
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|

### 템플릿 문의하기
#### 요청
[URL]

```
PUT  /alimtalk/v1.4/appkeys/{appkey}/plus-friends/{plusFriendId}/templates/{templateCode}/comments
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|
|plusFriendId|	String|	플러스친구 아이디 |
|templateCode|	String|	템플릿 코드 |

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "comment" : String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|comment|	String |	O | 문의 내용 |

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|

### 템플릿 리스트 조회

#### 요청

[URL]

```
GET  /alimtalk/v1.4/appkeys/{appkey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|plusFriendId|	String|	X |	플러스친구 아이디 |
|templateCode|	String|	X |	템플릿 코드|
|templateName|	String|	X |	템플릿 이름|
|templateStatus| String |	X | 템플릿 상태 코드|
|pageNum|	Integer|	X|	페이지 번호(Default : 1)|
|pageSize|	Integer|	X|	조회 건수(Default : 15, Max : 1000)|

|템플릿 상태 코드| 설명|
|---|---|
| TSC01 | 요청 |
| TSC02 | 검수중 |
| TSC03 | 승인 |
| TSC04 | 반려 |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.4/appkeys/{appkey}/templates?plusFriendId={플러스친구 아이디}&templateStatus={템플릿 상태 코드}"
```

#### 응답
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|templateListResponse|	Object|	본문 영역|
|- templates | List |	템플릿 리스트 |
|-- plusFriendId | String |	플러스친구 아이디 |
|-- plusFriendType | String | 플러스친구 타입(NORMAL, GROUP) |
|-- templateCode | String |	템플릿 코드 |
|-- templateName | String |	템플릿명 |
|-- templateContent | String |	템플릿 본문 |
|-- buttons | List |	버튼 리스트 |
|--- ordering | Integer |	버튼 순서(1~5) |
|--- type | String |	버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달) |
|--- name | String |	버튼 이름 |
|--- linkMo | String |	모바일 웹 링크 (WL 타입일 경우 필수 필드) |
|--- linkPc | String |	PC 웹 링크  (WL 타입일 경우 선택 필드) |
|--- schemeIos | String |	IOS 앱 링크 (AL 타입일 경우 필수 필드) |
|--- schemeAndroid | String |	Android 앱 링크 (AL 타입일 경우 필수 필드) |
|-- comments | List | 검수 결과 |
|--- id | Integer | 문의 아이디 |
|--- content |  String | 문의 내용 |
|---userName | String | 작성자 |
|---createAt | String | 등록 날짜 |
|---status | String | 댓글 상태(INQ: 문의, APR: 승인, REJ: 반려, REP: 답변) |
|-- status| String | 템플릿 상태 |
|-- statusName | String | 템플릿 상태명 |
|-- createDate | String | 생성일자 |
|- totalCount | Integer | 총 개수 |

### 템플릿 수정 리스트 조회

#### 요청

[URL]

```
GET  /alimtalk/v1.4/appkeys/{appkey}/plus-friends/{plusFriendId}/templates/{templateCode}/modifications
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|
|plusFriendId|	String|	플러스친구 아이디 |
|templateCode|	String|	템플릿 코드 |

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.4/appkeys/{appkey}/plus-friends/{plusFriendId}/templates/{templateCode}/modifications"
```

#### 응답
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|templateModificationsResponse|	Object|	본문 영역|
|- templates | List |	템플릿 리스트 |
|-- plusFriendId | String |	플러스친구 아이디 |
|-- plusFriendType | String | 플러스친구 타입(NORMAL, GROUP) |
|-- templateCode | String |	템플릿 코드 |
|-- templateName | String |	템플릿명 |
|-- templateContent | String |	템플릿 본문 |
|-- buttons | List |	버튼 리스트 |
|--- ordering | Integer |	버튼 순서(1~5) |
|--- type | String |	버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달) |
|--- name | String |	버튼 이름 |
|--- linkMo | String |	모바일 웹 링크 (WL 타입일 경우 필수 필드) |
|--- linkPc | String |	PC 웹 링크  (WL 타입일 경우 선택 필드) |
|--- schemeIos | String |	IOS 앱 링크 (AL 타입일 경우 필수 필드) |
|--- schemeAndroid | String |	Android 앱 링크 (AL 타입일 경우 필수 필드) |
|-- comments | List | 검수 결과 |
|--- id | Integer | 문의 아이디 |
|--- content |  String | 문의 내용 |
|---userName | String | 작성자 |
|---createAt | String | 등록 날짜 |
|---status | String | 댓글 상태(INQ: 문의, APR: 승인, REJ: 반려, REP: 답변) |
|-- status| String | 템플릿 상태 |
|-- statusName | String | 템플릿 상태명 |
|-- activated | Boolean | 활성화 여부 |
|-- createDate | String | 생성일자 |
|- totalCount | Integer | 총 개수 |

## 대체 발송 관리
### SMS AppKey 등록

[URL]

```
POST  /alimtalk/v1.4/appkeys/{appkey}/failback/appkey
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |


[Request body]

```
{
    "resendAppKey": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|resendAppKey|	String|	O | 대체 발송으로 설정할 SMS 서비스 앱키 |

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v1.4/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
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

### 대체 발송 설정 등록

[URL]

```
POST  /alimtalk/v1.4/appkeys/{appkey}/failback
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |


[Request body]

```
{  
   "plusFriendId": String,
   "isResend": Boolean,
   "resendSendNo": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|plusFriendId|	String|	O | 플러스친구 아이디 |
|isResend|	Boolean|	O | 발송 실패 시, 문자 대체발송 여부<br>Console에서 대체 발송 설정 시, default로 재발송 됩니다. |
|resendSendNo|	String|	O | 대체 발송 발신번호<br><span style="color:red">(SMS 상품에 등록된 발신번호가 아닐 경우, 대체발송이 실패할 수 있습니다.)</span> |

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v1.4/appkeys/{appkey}/failback/appkey -d '{"plusFriendId": "@플러스친구","isResend": true,"resendSendNo": "01012341234" }
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
