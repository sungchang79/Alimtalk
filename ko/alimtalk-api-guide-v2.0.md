## Notification > KakaoTalk Bizmessage > Alimtalk > API v2.0 Guide

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

## v2.0 API 소개
1. 카카오 채널 추가 시, 발급 받은 senderKey 필드로 API 호출이 되도록 변경 되었습니다. (plusFriendId 필드 대체)
2. API uri가 변경 되었습니다. (/plus-friends -> /senders)
3. 카카오 채널 그룹 기능이 추가 되었습니다.

## 일반 메시지

### 메시지 치환 발송 요청

[URL]

```
POST  /alimtalk/v2.0/appkeys/{appkey}/messages
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
        "recipientGroupingKey": String
    }],
    "messageOption": {
      "price": Integer,
      "currencyType": String
    }
}
```

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|senderKey|	String|	O | 발신 키 (40자) |
|templateCode|	String|	O | 등록한 발송 템플릿 코드 (최대 20자) |
|requestDate| String | X| 요청 일시 (yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송) |
|senderGroupingKey| String | X| 발신 그룹핑 키 (최대 100자) |
|createUser| String | X| 등록자 (콘솔에서 발송 시 사용자 UUID로 저장)|
|recipientList|	List|	O|	수신자 리스트 (최대 1000명) |
|- recipientNo|	String|	O|	수신번호 (최대 15자) |
|- templateParameter|	Object|	X|	템플릿 파라미터<br>(템플릿에 치환할 변수 포함 시, 필수) |
|-- key|	String|	X |	치환 키(#{key})|
|-- value| String |	X |	치환 키에 매핑되는 Value값|
|- resendParameter|	Object|	X| 대체 발송 정보 |
|-- isResend|	boolean|	X|	발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 대체 발송됩니다. |
|-- resendType|	String|	X|	대체 발송 타입(SMS,LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다. |
|-- resendTitle|	String|	X|	LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 대체 발송됩니다.) |
|-- resendContent|	String|	X|	대체 발송 내용<br>(값이 없을 경우, [메시지 본문과 웹링크 버튼명 - 웹링크 Mobile 링크]으로 대체 발송됩니다.) |
|-- resendSendNo | String| X| 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span> |
|- recipientGroupingKey|	String|	X|	수신자 그룹핑 키 (최대 100자) |
|messageOption | Object |	X | 메시지 옵션 |
|- price | Integer |	X | message(사용자에게 전달될 메시지) 내 포함된 가격/금액/결제금액 (모먼트 광고에 해당) |
|- currencyType | String |	X| message(사용자에게 전달될 메시지) 내 포함된 가격/금액/결제금액의 통화단위 KRW, USD, EUR 등 국제 통화 코드 사용 (모먼트 광고에 해당) |

* <b>요청 일시는 호출하는 시점부터 90일 후까지 설정 가능합니다.</b>
* <b>SMS 서비스에서 대체 발송되므로, SMS 서비스의 발송 API 명세에 따라 필드를 입력해야 합니다.(SMS 서비스에 등록된 발신 번호, 각종 필드 길이 제한 등)</b>
* <b>SMS 서비스는 국제 SMS만 지원합니다. 국제 수신자 번호일 경우, resendType(대체 발송 타입)을 SMS로 변경해야 정상적으로 대체 발송할 수 있습니다.</b>
* <b>지정한 대체 발송 타입의 바이트 제한을 초과하는 대체 발송 제목이나 내용은 잘려서 대체 발송될 수 있습니다.([[SMS 주의사항](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] 참고)</b>

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.0/appkeys/{appkey}/messages -d '{"senderKey":"{발신 키}","templateCode":"{템플릿 코드}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{수신번호}","templateParameter":{"{치환자 필드}":"{치환 데이터}"}}]}'
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

| 이름 |	타입|	설명|
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
POST  /alimtalk/v2.0/appkeys/{appkey}/raw-messages
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
    ],
    "messageOption": {
      "price": Integer,
      "currencyType": String
    }
}
```

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|senderKey|	String|	O | 발신 키(40자) |
|templateCode|	String|	O | 등록한 발송 템플릿 코드 (최대 20자) |
|requestDate| String | X| 요청 일시 (yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송) |
|senderGroupingKey| String | X| 발신 그룹핑 키 (최대 100자) |
|createUser| String | X| 등록자 (콘솔에서 발송 시 사용자 UUID로 저장)|
|recipientList|	List|	O|	수신자 리스트 (최대 1,000명) |
|- recipientNo|	String|	O|	수신번호 (최대 15자) |
|- content|	String|	O|	내용 (최대 1000자) |
|- templateTitle| String| X| 제목 (최대 50자) |
|- buttons|	List |	X | 버튼 리스트 (최대 5개) |
|-- ordering|	Integer|	X |	버튼 순서 (버튼이 있는 경우 필수)|
|-- type| String |	X |	버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가) |
|-- name| String |	X |	버튼 이름 (버튼이 있는 경우 필수, 최대 14자)|
|-- linkMo| String |	X |	모바일 웹 링크 (WL 타입일 경우 필수 필드, 최대 200자)|
|-- linkPc | String |	X |PC 웹 링크  (WL 타입일 경우 선택 필드, 최대 200자) |
|-- schemeIos | String | X |	IOS 앱 링크 (AL 타입일 경우 필수 필드, 최대 200자) |
|-- schemeAndroid | String | X |	Android 앱 링크 (AL 타입일 경우 필수 필드, 최대 200자) |
|- resendParameter|	Object|	X| 대체 발송 정보 |
|-- isResend|	boolean|	X|	발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 대체 발송됩니다. |
|-- resendType|	String|	X|	대체 발송 타입(SMS,LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다. |
|-- resendTitle|	String|	X|	LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 대체 발송됩니다.) |
|-- resendContent|	String|	X|	대체 발송 내용<br>(값이 없을 경우, [메시지 본문과 웹링크 버튼명 - 웹링크 Mobile 링크]으로 대체 발송됩니다.) |
|-- resendSendNo | String| X| 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span> |
|- recipientGroupingKey|	String|	X|	수신자 그룹핑 키 (최대 100자) |
| messageOption | Object |	X | 메시지 옵션 |
|- price | Integer |	X | message(사용자에게 전달될 메시지) 내 포함된 가격/금액/결제금액 (모먼트 광고에 해당) |
|- currencyType | String |	X| message(사용자에게 전달될 메시지) 내 포함된 가격/금액/결제금액의 통화단위 KRW, USD, EUR 등 국제 통화 코드 사용 (모먼트 광고에 해당) |

* <b>본문과 버튼에 치환이 완성된 데이터를 넣어주세요.</b>
* <b>요청 일시는 호출하는 시점부터 90일 후까지 설정 가능합니다.</b>
* <b>SMS 서비스에서 대체 발송되므로, SMS 서비스의 발송 API 명세에 따라 필드를 입력해야 합니다.(SMS 서비스에 등록된 발신 번호, 각종 필드 길이 제한 등)</b>
* <b>SMS 서비스는 국제 SMS만 지원합니다. 국제 수신자 번호일 경우, resendType(대체 발송 타입)을 SMS로 변경해야 정상적으로 대체 발송할 수 있습니다.</b>
* <b>지정한 대체 발송 타입의 바이트 제한을 초과하는 대체 발송 제목이나 내용은 잘려서 대체 발송될 수 있습니다.([[SMS 주의사항](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] 참고)</b>

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.0/appkeys/{appkey}/raw-messages -d '{"senderKey":"{발신 키}","templateCode":"{템플릿 코드}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{수신번호}","content":"{내용}","buttons":[{"ordering":"{버튼 순서}","type":"{버튼 타입}","name":"{버튼 이름}","linkMo":"{모바일 웹 링크}"}]}]}'
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

| 이름 |	타입|	설명|
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
GET  /alimtalk/v2.0/appkeys/{appkey}/messages
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

[Query parameter] 1번 or (2번, 3번) 조건 필수

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|requestId|	String|	조건 필수 (1번) | 요청 아이디 |
|startRequestDate|	String|	조건 필수 (2번) | 발송 요청 날짜 시작 값(yyyy-MM-dd HH:mm)|
|endRequestDate|	String| 조건 필수 (2번) |	발송 요청 날짜 끝 값(yyyy-MM-dd HH:mm) |
|startCreateDate|  String| 조건 필수 (3번) | 등록 날짜 시작값(yyyy-MM-dd HH:mm)|
|endCreateDate|  String| 조건 필수 (3번) | 등록 날짜 끝값(yyyy-MM-dd HH:mm) |
|recipientNo|	String|	X |	수신번호 |
|senderKey|	String|	X |	발신 키 |
|templateCode|	String|	X |	템플릿 코드|
|senderGroupingKey| String | X| 발신 그룹핑 키 |
|recipientGroupingKey|	String|	X|	수신자 그룹핑 키 |
|messageStatus| String |	X | 요청 상태 ( COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 )	|
|resultCode| String |	X | 발송 결과 ( MRC01 -> 성공 MRC02 -> 실패 )	|
|createUser| String | X| 등록자 (콘솔에서 발송 시 사용자 UUID로 저장)|
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

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|messageSearchResultResponse|	Object|	본문 영역|
|- messages | List |	메시지 리스트 |
|-- requestId | String |	요청 아이디 |
|-- recipientSeq | Integer |	수신자 시퀀스 번호 |
|-- plusFriendId | String |	플러스친구 ID |
|-- senderKey    | String | 발신 키    |
|-- templateCode | String |	템플릿 코드 |
|-- recipientNo | String |	수신 번호 |
|-- content | String |	본문 |
|-- requestDate | String |	요청 일시 |
|-- createDate | String | 등록 일시 |
|-- receiveDate | String |	수신 일시 |
|-- resendStatus | String |	대체 발송 상태 코드 (RSC01, RSC02, RSC03, RSC04, RSC05)<br>([[아래 대체 발송 상태 표](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] 참고) |
|-- resendStatusName | String |	대체 발송 상태 코드명 |
|-- messageStatus | String |	요청 상태 ( COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 ) |
|-- createUser | String | 등록자(콘솔에서 발송 시 사용자 UUID로 저장) |
|-- resultCode | String |	수신 결과 코드 |
|-- resultCodeName | String |	수신 결과 코드명 |
|-- buttons | List |	버튼 리스트 |
|--- ordering | Integer |	버튼 순서 |
|--- type | String |	버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가) |
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
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.0/appkeys/{appkey}/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

### 메시지 단건 조회

#### 요청

[URL]

```
GET  /alimtalk/v2.0/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey |
|requestId|	String|	요청 아이디 |
|recipientSeq|	Integer|	수신자 시퀀스 번호 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./sender-console-guide/#x-secret-key)] |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.0/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
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
      "senderKey" : String,
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
      "messageOption": {
        "price": Integer,
        "currencyType": String
      },
      "senderGroupingKey": String,
      "recipientGroupingKey": String
  }
}
```

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|message|	Object|	메시지|
|- requestId | String |	요청 아이디 |
|- recipientSeq | Integer |	수신자 시퀀스 번호 |
|- plusFriendId | String |	플러스친구 ID |
|- senderKey    | String |  발신 키    |
|- templateCode | String |	템플릿 코드 |
|- recipientNo | String |	수신 번호 |
|- content | String |	본문 |
|- templateTitle | String | 템플릿 제목 |
|- templateSubtitle | String | 템플릿 보조 문구 |
|- templateExtra | String | 템플릿 부가 내용 |
|- templateAd | String | 템플릿 내 수신 동의 요청 또는 간단한 광고 문구 |
|- requestDate | String |	요청 일시 |
|- receiveDate | String |	수신 일시 |
|- createDate | String | 등록 일시 |
|- resendStatus | String |	대체 발송 상태 코드 (RSC01, RSC02, RSC03, RSC04, RSC05)<br>([[아래 대체 발송 상태 표](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] 참고) |
|- resendStatusName | String |	대체 발송 상태 코드명 |
|- resendResultCode | String | 대체 발송 결과 코드 [SMS 결과 코드](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api) |
|- resendRequestId | String | 대체 발송 SMS 요청 ID |
|- messageStatus | String |	요청 상태 ( COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 ) |
|- resultCode | String |	수신 결과 코드 |
|- resultCodeName | String |	수신 결과 코드명 |
|- createUser | String | 등록자(콘솔에서 발송 시 사용자 UUID로 저장) |
|- buttons | List |	버튼 리스트 |
|-- ordering | Integer |	버튼 순서 |
|-- type | String |	버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가) |
|-- name | String |	버튼 이름 |
|-- linkMo | String |	모바일 웹 링크 (WL 타입일 경우 필수 필드) |
|-- linkPc | String |	PC 웹 링크  (WL 타입일 경우 선택 필드) |
|-- schemeIos | String |	IOS 앱 링크 (AL 타입일 경우 필수 필드) |
|-- schemeAndroid | String |	Android 앱 링크 (AL 타입일 경우 필수 필드) |
|- messageOption | Object |	메시지 옵션 |
|-- price | Integer |	message(사용자에게 전달될 메시지) 내 포함된 가격/금액/결제금액 (모먼트 광고에 해당) |
|-- currencyType | String |	message(사용자에게 전달될 메시지) 내 포함된 가격/금액/결제금액의 통화단위 KRW, USD, EUR 등 국제 통화 코드 사용 (모먼트 광고에 해당) |
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
POST  /alimtalk/v2.0/appkeys/{appkey}/auth/messages
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
        "recipientGroupingKey": String
    }],
    "messageOption": {
      "price": Integer,
      "currencyType": String
    }
}
```

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|senderKey|	String|	O | 발신 키 (40자) |
|templateCode|	String|	O | 등록한 발송 템플릿 코드 (최대 20자) |
|requestDate| String | X| 요청 일시 (yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송) |
|senderGroupingKey| String | X| 발신 그룹핑 키 (최대 100자) |
|createUser | String | X| 등록자(콘솔에서 발송 시 사용자 UUID로 저장) |
|recipientList|	List|	O|	수신자 리스트 (최대 1000명) |
|- recipientNo|	String|	O|	수신번호 (최대 15자) |
|- templateParameter|	Object|	X|	템플릿 파라미터<br>(템플릿에 치환할 변수 포함 시, 필수) |
|-- key|	String|	X |	치환 키(#{key})|
|-- value| String |	X |	치환 키에 매핑되는 Value값|
|- resendParameter|	Object|	X| 대체 발송 정보 |
|-- isResend|	boolean|	X|	발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 대체 발송됩니다. |
|-- resendType|	String|	X|	대체 발송 타입(SMS,LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다. |
|-- resendTitle|	String|	X|	LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 대체 발송됩니다.) |
|-- resendContent|	String|	X|	대체 발송 내용<br>(값이 없을 경우, [메시지 본문과 웹링크 버튼명 - 웹링크 Mobile 링크]으로 대체 발송됩니다.) |
|-- resendSendNo | String| X| 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span> |
|- recipientGroupingKey|	String|	X|	수신자 그룹핑 키 (최대 100자) |
|messageOption | Object |	X | 메시지 옵션 |
|- price | Integer |	X | message(사용자에게 전달될 메시지) 내 포함된 가격/금액/결제금액 (모먼트 광고에 해당) |
|- currencyType | String |	X| message(사용자에게 전달될 메시지) 내 포함된 가격/금액/결제금액의 통화단위 KRW, USD, EUR 등 국제 통화 코드 사용 (모먼트 광고에 해당) |

* <b>요청 일시는 호출하는 시점부터 90일 후까지 설정 가능합니다.</b>
* <b>SMS 서비스에서 대체 발송되므로, SMS 서비스의 발송 API 명세에 따라 필드를 입력해야 합니다.(SMS 서비스에 등록된 발신 번호, 각종 필드 길이 제한 등)</b>
* <b>지정한 대체 발송 타입의 바이트 제한을 초과하는 대체 발송 제목이나 내용은 잘려서 대체 발송될 수 있습니다.([[SMS 주의사항](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] 참고)</b>

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.0/appkeys/{appkey}/auth/messages -d '{"senderKey":"{발신 키}","templateCode":"{템플릿 코드}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{수신번호}","templateParameter":{"{치환자 필드}":"{치환 데이터}"}}]}'
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

| 이름 |	타입|	설명|
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
POST  /alimtalk/v2.0/appkeys/{appkey}/auth/raw-messages
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
    ],
    "messageOption": {
      "price": Integer,
      "currencyType": String
    }
}
```

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|senderKey|	String|	O | 발신 키 (40자) |
|templateCode|	String|	O | 등록한 발송 템플릿 코드 (최대 20자) |
|requestDate| String | X| 요청 일시 (yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송) |
|senderGroupingKey| String | X| 발신 그룹핑 키 (최대 100자) |
|createUser | String | 등록자(콘솔에서 발송 시 사용자 UUID로 저장) |
|recipientList|	List|	O|	수신자 리스트 (최대 1,000명) |
|- recipientNo|	String|	O|	수신번호 (최대 15자) |
|- content|	String|	O|	내용 (최대 1000자) |
|- templateTitle| String | X| 제목 (최대 50자) |  
|- buttons|	List |	X | 버튼 리스트 (최대 5개) |
|-- ordering|	Integer|	X |	버튼 순서 (버튼이 있는 경우 필수)|
|-- type| String |	X |	버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가) |
|-- name| String |	X |	버튼 이름 (버튼이 있는 경우 필수, 최대 14자)|
|-- linkMo| String |	X |	모바일 웹 링크 (WL 타입일 경우 필수 필드, 최대 200자)|
|-- linkPc | String |	X |PC 웹 링크  (WL 타입일 경우 선택 필드, 최대 200자) |
|-- schemeIos | String | X |	IOS 앱 링크 (AL 타입일 경우 필수 필드, 최대 200자) |
|-- schemeAndroid | String | X |	Android 앱 링크 (AL 타입일 경우 필수 필드, 최대 200자) |
|- resendParameter|	Object|	X| 대체 발송 정보 |
|-- isResend|	boolean|	X|	발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 대체 발송됩니다. |
|-- resendType|	String|	X|	대체 발송 타입(SMS,LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다. |
|-- resendTitle|	String|	X|	LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 대체 발송됩니다.) |
|-- resendContent|	String|	X|	대체 발송 내용<br>(값이 없을 경우, [메시지 본문과 웹링크 버튼명 - 웹링크 Mobile 링크]으로 대체 발송됩니다.) |
|-- resendSendNo | String| X| 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span> |
|- recipientGroupingKey|	String|	X|	수신자 그룹핑 키 (최대 100자) |
|messageOption | Object |	X | 메시지 옵션 |
|- price | Integer |	X | message(사용자에게 전달될 메시지) 내 포함된 가격/금액/결제금액 (모먼트 광고에 해당) |
|- currencyType | String |	X| message(사용자에게 전달될 메시지) 내 포함된 가격/금액/결제금액의 통화단위 KRW, USD, EUR 등 국제 통화 코드 사용 (모먼트 광고에 해당) |

* <b>본문과 버튼에 치환이 완성된 데이터를 넣어주세요.</b>
* <b>요청 일시는 호출하는 시점부터 90일 후까지 설정 가능합니다.</b>

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.0/appkeys/{appkey}/auth/raw-messages -d '{"senderKey":"{발신 키}","templateCode":"{템플릿 코드}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{수신번호}","content":"{내용}","buttons":[{"ordering":"{버튼 순서}","type":"{버튼 타입}","name":"{버튼 이름}","linkMo":"{모바일 웹 링크}"}]}]}'
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

| 이름 |	타입|	설명|
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
GET  /alimtalk/v2.0/appkeys/{appkey}/auth/messages
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

[Query parameter] 1번 or (2번, 3번) 조건 필수

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|requestId|	String|	조건 필수 (1번) | 요청 아이디 |
|startRequestDate|	String|	조건 필수 (2번) | 발송 요청 날짜 시작 값(yyyy-MM-dd HH:mm)|
|endRequestDate|	String| 조건 필수 (2번) |	발송 요청 날짜 끝 값(yyyy-MM-dd HH:mm) |
|startCreateDate|  String| 조건 필수 (3번) | 등록 날짜 시작값(yyyy-MM-dd HH:mm)|
|endCreateDate|  String| 조건 필수 (3번) | 등록 날짜 끝값(yyyy-MM-dd HH:mm) |
|recipientNo|	String|	X |	수신번호 |
|senderKey   |  String| X | 발신 키 |
|templateCode|	String|	X |	템플릿 코드|
|senderGroupingKey| String | X| 발신 그룹핑 키 |
|recipientGroupingKey|	String|	X|	수신자 그룹핑 키 |
|messageStatus| String |	X | 요청 상태 ( COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 )	|
|resultCode| String |	X | 발송 결과 ( MRC01 -> 성공 MRC02 -> 실패 )	|
|createUser | String | X | 등록자(콘솔에서 발송 시 사용자 UUID로 저장) |
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
      "senderKey"    :  String,
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

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|messageSearchResultResponse|	Object|	본문 영역|
|- messages | List |	메시지 리스트 |
|-- requestId | String |	요청 아이디 |
|-- recipientSeq | Integer |	수신자 시퀀스 번호 |
|-- plusFriendId | String |	플러스친구 ID |
|-- senderKey    | String | 발신 키    |
|-- templateCode | String |	템플릿 코드 |
|-- recipientNo | String |	수신 번호 |
|-- content | String |	본문 |
|-- requestDate | String | 요청 일시 |
|-- createDate | String |	등록 일시 |
|-- receiveDate | String |	수신 일시 |
|-- resendStatus | String |	대체 발송 상태 코드 (RSC01, RSC02, RSC03, RSC04, RSC05)<br>([[아래 대체 발송 상태 표](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] 참고) |
|-- resendStatusName | String |	대체 발송 상태 코드명 |
|-- messageStatus | String |	요청 상태 ( COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 ) |
|-- resultCode | String |	수신 결과 코드 |
|-- resultCodeName | String |	수신 결과 코드명 |
|-- createUser | String | 등록자(콘솔에서 발송 시 사용자 UUID로 저장) |
|-- buttons | List |	버튼 리스트 |
|--- ordering | Integer |	버튼 순서 |
|--- type | String |	버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가) |
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
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.0/appkeys/{appkey}/auth/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

### 메시지 단건 조회

#### 요청

[URL]

```
GET  /alimtalk/v2.0/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey |
|requestId|	String|	요청 아이디 |
|recipientSeq|	Integer|	수신자 시퀀스 번호 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./sender-console-guide/#x-secret-key)] |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.0/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}"
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
      "senderKey"    :  String,
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
      "messageOption": {
        "price": Integer,
        "currencyType": String
      },
      "senderGroupingKey": String,
      "recipientGroupingKey": String
  }
}
```

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|message|	Object|	메시지|
|- requestId | String |	요청 아이디 |
|- recipientSeq | Integer |	수신자 시퀀스 번호 |
|- plusFriendId | String |	플러스친구 ID |
|- senderKey    | String |  발신 키    |
|- templateCode | String |	템플릿 코드 |
|- recipientNo | String |	수신 번호 |
|- content | String |	본문 |
|- templateTitle | String | 템플릿 제목 |
|- templateSubtitle | String | 템플릿 보조 문구 |
|- templateExtra | String | 템플릿 부가 내용 |
|- templateAd | String | 템플릿 내 수신 동의 요청 또는 간단한 광고 문구 |
|- requestDate | String | 요청 일시 |
|- createDate | String |	등록 일시 |
|- receiveDate | String |	수신 일시 |
|- resendStatus | String |	대체 발송 상태 코드 (RSC01, RSC02, RSC03, RSC04, RSC05)<br>([[아래 대체 발송 상태 표](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] 참고) |
|- resendStatusName | String |	대체 발송 상태 코드명 |
|- resendResultCode | String | 대체 발송 결과 코드 [SMS 결과 코드](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api) |
|- resendRequestId | String | 대체 발송 SMS 요청 ID |
|- messageStatus | String |	요청 상태 ( COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 ) |
|- resultCode | String |	수신 결과 코드 |
|- resultCodeName | String |	수신 결과 코드명 |
|- createUser | String | 등록자(콘솔에서 발송 시 사용자 UUID로 저장) |
|- buttons | List |	버튼 리스트 |
|-- ordering | Integer |	버튼 순서 |
|-- type | String |	버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가) |
|-- name | String |	버튼 이름 |
|-- linkMo | String |	모바일 웹 링크 (WL 타입일 경우 필수 필드) |
|-- linkPc | String |	PC 웹 링크  (WL 타입일 경우 선택 필드) |
|-- schemeIos | String |	IOS 앱 링크 (AL 타입일 경우 필수 필드) |
|-- schemeAndroid | String |	Android 앱 링크 (AL 타입일 경우 필수 필드) |
|- messageOption | Object |	메시지 옵션 |
|-- price | Integer |	message(사용자에게 전달될 메시지) 내 포함된 가격/금액/결제금액 (모먼트 광고에 해당) |
|-- currencyType | String |	message(사용자에게 전달될 메시지) 내 포함된 가격/금액/결제금액의 통화단위 KRW, USD, EUR 등 국제 통화 코드 사용 (모먼트 광고에 해당) |
|- senderGroupingKey | String | 발신 그룹핑 키 |
|- recipientGroupingKey | String |	수신자 그룹핑 키 |

## 메시지
### 메시지 발송 취소

#### 요청

[URL]

```
DELETE  /alimtalk/v2.0/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 앱키|
|requestId| String| 요청 ID|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| 이름 |	타입|	필수|	설명|
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

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|

[예시]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.0/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

### 메시지 결과 업데이트 조회

#### 요청

[URL]

```
GET  /alimtalk/v2.0/appkeys/{appkey}/message-results
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 앱키|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| 이름 |	타입|	필수|	설명|
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

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|messages|	List|	메시지 리스트|
|- requestId | String |	요청 ID |
|- recipientSeq | Integer |	수신자 시퀀스 번호 |
|- requestDate | String |	요청 일시 |
|- createDate  | String |	생성 일시 |
|- receiveDate | String |	수신 일시 |
|- resendStatus | String |	대체 발송 상태 코드 (RSC01, RSC02, RSC03, RSC04, RSC05)<br>([[아래 대체 발송 상태 표](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] 참고) |
|- resendStatusName | String |	대체 발송 상태 코드명 |
|- resendResultCode | String | 대체 발송 결과 코드 [SMS 결과 코드](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api) |
|- resendRequestId | String | 대체 발송 SMS 요청 ID |
|- messageStatus | String |	요청 상태(COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 ) |
|- resultCode | String |	수신 결과 코드 |
|- resultCodeName | String |	수신 결과 코드명 |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.0/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

### SMS/LMS 대체 발송 상태 코드
| 이름 |	설명|
|---|---|
|RSC01|	대체 발송 미대상|
|RSC02|	대체 발송 대상 (발송 결과 실패 시, 대체 발송이 진행됩니다.)|
|RSC03|	대체 발송 중|
|RSC04|	대체 발송 성공|
|RSC05|	대체 발송 실패|

## 템플릿

### 템플릿 카테고리 조회
#### 요청
[URL]

```
POST  /alimtalk/v2.0/appkeys/{appkey}/template/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey |

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

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|categories|	List|	카테고리 리스트 |
|- name | String | 카테고리 이름 |
|- subCategories | List |	서브 카테고리 리스트 |
|-- code | String | 카테고리 코드 (템플릿 등록/수정 시, 사용) |
|-- name | String |	카테고리 이름 |
|-- groupName | String |	카테고리 그룹명 |
|-- inclusion | String |	카테고리 적용 대상 템플릿 설명 |
|-- exclusion| String| 카테고리 제외 대상 템플릿 설명 |

### 템플릿 등록
#### 요청
[URL]

```
POST  /alimtalk/v2.0/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey |
|senderKey|	String|	발신 키 |

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

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|templateCode|	String |	O | 템플릿 코드 (최대 20자) |
|templateName|	String |	O | 템플릿명 (최대 20자) |
|templateContent|	String |	O | 템플릿 본문 (최대 1000자) |
|templateMessageType| String | X |템플릿 메시지 유형(BA: 기본형, EX: 부가 정보형, AD: 광고 추가형, MI: 복합형, default: BA) |
|templateEmphasizeType| String| X| 템플릿 강조 표시 타입(NONE : 기본, TEXT : 강조 표시, default : NONE)<br>- TEXT: templateTitle, templateSubtitle 필드 필수|
|templateExtra | String | X | 템플릿 부가 정보(템플릿 메시지 유형이 [부가 정보형/복합형]일 경우 필수) |
|templateAd | String | X | 템플릿 내 수신 동의 요청 또는 간단한 광고 문구(템플릿 메시지 유형이 [광고 추가형/복합형]일 경우 필수) |
|tempalteTitle| String | X| 템플릿 제목 (최대 50자, Android : 2줄, 23자 이상 말줄임 처리, IOS : 2줄, 27자 이상 말줄임 처리) |
|templateSubtitle| String | X| 템플릿 보조 문구 (최대 50자, Android : 18자 이상 말줄임 처리, IOS : 21자 이상 말줄임 처리) |
|securityFlag| Boolean | X| 보안 템플릿 여부<br>OTP등 보안 메시지 일 경우 설정<br>발신 당시의 메인 디바이스를 제외한 모든 디바이스에 메시지 텍스트 미노출(default: false) |
|categoryCode| String | X | 템플릿 카테고리 코드 (템플릿 카테고리 조회 API 참고, default: 999999)<br>카테고리 기타일 경우, 최하위 우선순위로 심사 |
|buttons|	List |	X | 버튼 리스트 (최대 5개) |
|-ordering|	Integer |	X | 버튼 순서(1~5) |
|-type|	String |	X | 버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가[광고 추가/복합형만]) |
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

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|

### 템플릿 수정
#### 요청
[URL]

```
PUT  /alimtalk/v2.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey |
|senderKey|	String|	발신 키 |
|templateCode|	String|	템플릿 코드 |

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

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|templateName|	String |	O | 템플릿명 (최대 20자) |
|templateContent|	String |	O | 템플릿 본문 (최대 1000자) |
|templateMessageType| String | X | 템플릿 메시지 유형(BA: 기본형, EX: 부가 정보형, AD: 광고 추가형, MI: 복합형) |
|templateEmphasizeType| String| X| 템플릿 강조 표시 타입(NONE : 기본, TEXT : 강조 표시, default : NONE)<br>- TEXT: templateTitle, templateSubtitle 필드 필수|
|templateExtra | String | X | 템플릿 부가 정보(템플릿 메시지 유형이 [부가 정보형/복합형]일 경우 필수) |
|templateAd | String | X | 템플릿 내 수신 동의 요청 또는 간단한 광고 문구(템플릿 메시지 유형이 [광고 추가형/복합형]일 경우 필수) |
|tempalteTitle| String | X| 템플릿 제목 (최대 50자, Android : 2줄, 23자 이상 말줄임 처리, IOS : 2줄, 27자 이상 말줄임 처리) |
|templateSubtitle| String | X| 템플릿 보조 문구 (최대 50자, Android : 18자 이상 말줄임 처리, IOS : 21자 이상 말줄임 처리) |
|securityFlag| Boolean | X| 보안 템플릿 여부<br>OTP등 보안 메시지 일 경우 설정<br>발신 당시의 메인 디바이스를 제외한 모든 디바이스에 메시지 텍스트 미노출(default: false) |
|categoryCode| String | X | 템플릿 카테고리 코드 (템플릿 카테고리 조회 API 참고, default: 999999)<br>카테고리 기타일 경우, 최하위 우선순위로 심사 |
|buttons|	List |	X | 버튼 리스트 (최대 5개) |
|-ordering|	Integer |	X | 버튼 순서(1~5) |
|-type|	String |	X | 버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가[광고 추가/복합형만]) |
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

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|

### 템플릿 삭제
#### 요청
[URL]

```
DELETE  /alimtalk/v2.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey|
|senderKey|	String|	발신 키 |
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

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|

### 템플릿 문의하기
#### 요청
[URL]

```
POST  /alimtalk/v2.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/comments
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey|
|senderKey|	String|	발신 키 |
|templateCode|	String|	템플릿 코드 |

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
  "comment" : String
}
```

| 이름 |	타입|	필수|	설명|
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

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|

### 파일 첨부하여 템플릿 문의하기
#### 요청
[URL]

```
POST  /alimtalk/v2.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/comments_file
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey|
|senderKey|	String|	발신 키 |
|templateCode|	String|	템플릿 코드 |

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
  "comment" : String,
  "attachments" : File
}
```

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|comment|	String |	O | 문의 내용 |
|attachments| List<File> | X | 첨부 파일 목록(최대 5개) |

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

### 템플릿 리스트 조회

#### 요청

[URL]

```
GET  /alimtalk/v2.0/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey|
|senderKey|	String|	발신 키 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
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
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.0/appkeys/{appkey}/senders/{senderKey}/templates?templateStatus={템플릿 상태 코드}"
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

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|templateListResponse|	Object|	본문 영역|
|- templates | List |	템플릿 리스트 |
|-- plusFriendId | String |	카카오톡 채널 검색용 ID 또는 발신 프로필 그룹명 |
|-- senderKey    | String | 발신 키    |
|-- plusFriendType | String | 플러스친구 타입(NORMAL, GROUP) |
|-- templateCode | String |	템플릿 코드 |
|-- templateName | String |	템플릿명 |
|-- templateMessageType| String | 템플릿 메시지 유형(BA: 기본형, EX: 부가 정보형, AD: 광고 추가형, MI: 복합형) |
|-- templateEmphasizeType| String| 템플릿 강조 표시 타입(NONE : 기본, TEXT : 강조 표시, default : NONE) |
|-- templateContent | String |	템플릿 본문 |
|-- templateExtra | String | 템플릿 부가 정보 |
|-- templateAd | String | 템플릿 내 수신 동의 요청 또는 간단한 광고 문구 |
|-- tempalteTitle| String | 템플릿 제목 |
|-- templateSubtitle| String | 템플릿 보조 문구 |
|-- buttons | List |	버튼 리스트 |
|--- ordering | Integer |	버튼 순서(1~5) |
|--- type | String |	버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가) |
|--- name | String |	버튼 이름 |
|--- linkMo | String |	모바일 웹 링크 (WL 타입일 경우 필수 필드) |
|--- linkPc | String |	PC 웹 링크  (WL 타입일 경우 선택 필드) |
|--- schemeIos | String |	IOS 앱 링크 (AL 타입일 경우 필수 필드) |
|--- schemeAndroid | String |	Android 앱 링크 (AL 타입일 경우 필수 필드) |
|-- comments | List | 검수 결과 |
|--- id | Integer | 문의 아이디 |
|--- content |  String | 문의 내용 |
|--- userName | String | 작성자 |
|--- createAt | String | 등록 날짜 |
|--- attachment | List | 첨부 파일 |
|---- originalFileName | String | 첨부 파일명 |
|---- filePath | String | 첨부 파일 경로 |
|--- status | String | 댓글 상태(INQ: 문의, APR: 승인, REJ: 반려, REP: 답변) |
|-- status| String | 템플릿 상태 |
|-- statusName | String | 템플릿 상태명 |
|-- securityFlag| Boolean | 보안 템플릿 여부 |
|-- categoryCode| String | 템플릿 카테고리 코드  |
|-- createDate | String | 생성일자 |
|-- updateDate | String | 수정일자 |
|- totalCount | Integer | 총 개수 |

### 템플릿 수정 리스트 조회

#### 요청

[URL]

```
GET  /alimtalk/v2.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/modifications
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 Appkey|
|senderKey|	String|	발신 키 |
|templateCode|	String|	템플릿 코드 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다. [[참고](./sender-console-guide/#x-secret-key)] |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/modifications"
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

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|templateModificationsResponse|	Object|	본문 영역|
|- templates | List |	템플릿 리스트 |
|-- plusFriendId | String |	카카오톡 채널 검색용 ID 또는 발신 프로필 그룹명 |
|-- senderKey    | String | 발신 키    |
|-- plusFriendType | String | 플러스친구 타입(NORMAL, GROUP) |
|-- templateCode | String |	템플릿 코드 |
|-- templateName | String |	템플릿명 |
|-- templateMessageType| String | 템플릿 메시지 유형(BA: 기본형, EX: 부가 정보형, AD: 광고 추가형, MI: 복합형) |
|-- templateEmphasizeType| String| 템플릿 강조 표시 타입(NONE : 기본, TEXT : 강조 표시, default : NONE) |
|-- templateContent | String |	템플릿 본문 |
|-- templateExtra | String | 템플릿 부가 정보 |
|-- templateAd | String | 템플릿 내 수신 동의 요청 또는 간단한 광고 문구 |
|-- tempalteTitle| String | 템플릿 제목 |
|-- templateSubtitle| String | 템플릿 보조 문구 |
|-- buttons | List |	버튼 리스트 |
|--- ordering | Integer |	버튼 순서(1~5) |
|--- type | String |	버튼 타입(WL:웹링크, AL:앱링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가) |
|--- name | String |	버튼 이름 |
|--- linkMo | String |	모바일 웹 링크 (WL 타입일 경우 필수 필드) |
|--- linkPc | String |	PC 웹 링크  (WL 타입일 경우 선택 필드) |
|--- schemeIos | String |	IOS 앱 링크 (AL 타입일 경우 필수 필드) |
|--- schemeAndroid | String |	Android 앱 링크 (AL 타입일 경우 필수 필드) |
|-- comments | List | 검수 결과 |
|--- id | Integer | 문의 아이디 |
|--- content |  String | 문의 내용 |
|--- userName | String | 작성자 |
|--- createAt | String | 등록 날짜 |
|--- attachment | List | 첨부 파일 |
|---- originalFileName | String | 첨부 파일명 |
|---- filePath | String | 첨부 파일 경로 |
|--- status | String | 댓글 상태(INQ: 문의, APR: 승인, REJ: 반려, REP: 답변) |
|-- status| String | 템플릿 상태 |
|-- statusName | String | 템플릿 상태명 |
|-- securityFlag| Boolean | 보안 템플릿 여부 |
|-- categoryCode| String | 템플릿 카테고리 코드  |
|-- activated | Boolean | 활성화 여부 |
|-- createDate | String | 생성일자 |
|-- updateDate | String | 수정일자 |
|- totalCount | Integer | 총 개수 |

## 대체 발송 관리
### SMS AppKey 등록

[URL]

```
POST  /alimtalk/v2.0/appkeys/{appkey}/failback/appkey
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


[Request body]

```
{
    "resendAppKey": String
}
```

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|resendAppKey|	String|	O | 대체 발송으로 설정할 SMS 서비스 앱키 |

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.0/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
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
POST  /alimtalk/v2.0/appkeys/{appkey}/failback
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


[Request body]

```
{  
   "senderKey": String,
   "isResend": Boolean,
   "resendSendNo": String
}
```

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|senderKey|	String|	O | 발신 키 |
|isResend|	Boolean|	O | 발송 실패 시, 문자 대체발송 여부<br>Console에서 대체 발송 설정 시, default로 대체 발송 됩니다. |
|resendSendNo|	String|	O | 대체 발송 발신번호<br><span style="color:red">(SMS 상품에 등록된 발신번호가 아닐 경우, 대체발송이 실패할 수 있습니다.)</span> |

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.0/appkeys/{appkey}/failback/appkey -d '{"senderKey": "0be23c29de88d6888798aeda57062516354d74ba","isResend": true,"resendSendNo": "01012341234" }
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
