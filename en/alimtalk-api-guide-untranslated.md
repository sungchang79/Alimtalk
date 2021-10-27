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
1. 알림톡 대량 발송 조회, 통계 조회 API가 추가되었습니다.
2. 치환 메시지 발송 시, buttons 필드가 추가되었습니다.
3. 전문 메시지 발송 시, buttons 필드에 `chatExtra`, `chatEvent`, `target` 필드가 추가되었습니다.
4. 메시지 조회 시, buttons 필드에 `chatExtra`, `chatEvent`, `target` 필드가 추가되었습니다.


## 대량 발송
### 대량 발송 요청 목록 조회

#### 요청
[URL]
```
GET /alimtalk/v2.2/appkeys/{appKey}/mass-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 앱키|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	설명|
|---|---|---|
|X-Secret-Key|	String|	고유의 시크릿 키 |

[Query parameter]
* requestId 또는 startRequestDate + endRequestDate 또는 startCreateDate + endCreateDate는 필수입니다.

|값|	타입| 최대 길이 |	필수|	설명|
|---|---|---|---|---|
| requestId | String | - | O | 요청 ID |
| startRequestDate | String | - | O | 발송 날짜 시작 |
| endRequestDate | String | - | O | 발송 날짜 종료 |
| startCreateDate |	String| - |	O |	등록 날짜 시작 |
| endCreateDate |	String| - |	O |	등록 날짜 종료 |
| pageNum | optional, Integer | - | X | 페이지 번호 |
| pageSize | optional, Integer | 1000 | X | 검색 수 |

#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### 응답
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

|값|	타입|	설명|
|---|---|---|
| header | Object |	헤더 영역 |
| - resultCode |	Integer |	결과 코드 |
| - resultMessage |	String | 결과 메시지 |
| - isSuccessful |	Boolean | 성공 여부 |
| body | Object | 본문 영역 |
| - messages | Object | 메시지 리스트 |
| -- requestId | String | 요청 ID |
| -- requestDate | String | 요청 날짜 |
| -- createDate | String | 생성 날짜 |
| -- createUser | String | 생성 날짜 |
| -- plusFriendId | String | 플러스 친구 ID |
| -- senderKey | String| 발신 키 (40자) |
| -- masterStatusCode | String | 대량 발송 상태 코드 (WAIT, READY, SENDREADY, SENDWAIT, SENDING, COMPLETE, CANCEL, FAIL) |
| -- content | String | 내용 |
| -- buttons | List | 버튼 리스트 |
| --- ordering | String | 버튼 순서 |
| --- type | String | 버튼 종류<br/> - WL: 웹링크<br/> - AL: 앱링크<br/> - DS: 배송 조회<br/> - BK: 봇 키워드<br/> - MD: 메시지 전달<br/> - BC: 상담톡 전환<br/> - BT: 봇 전환<br/> - AC: 채널 추가[광고 추가/복합형만] |
| --- name | String | 버튼 이름 |
| --- linkMo | String | 모바일 웹 링크 (WL 타입일 경우 필수 필드) |
| --- linkPc | String | PC 웹 링크  (WL 타입일 경우 선택 필드)|
| --- schemeIos | String | IOS 앱 링크 (AL 타입일 경우 필수 필드) |
| --- schemeAndroid | String | Android 앱 링크 (AL 타입일 경우 필수 필드) |
| --- chatExtra | String | BC: 상담톡 전환시 전달할 메타 정보<br/> BT: 봇 전환 시 전달할 메타 정보 |
| --- chatEvent | String | BT: 봇 전환 시 연결할 봇 이벤트명 |
| --- target|	String|	웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| -- fileId | String | 첨부 파일 ID |
| -- templateCode |	String | 템플릿 코드 (최대 20자) |
| -- templateExtra | String | 템플릿 부가 정보(템플릿 메시지 유형이 [부가 정보형/복합형]일 경우 필수) |
| -- templateAd | String | 템플릿 내 수신 동의 요청 또는 간단한 광고 문구(템플릿 메시지 유형이 [광고 추가형/복합형]일 경우 필수) |
| -- tempalteTitle| String | 템플릿 제목 (최대 50자, Android : 2줄, 23자 이상 말줄임 처리, IOS : 2줄, 27자 이상 말줄임 처리) |
| -- templateSubtitle| String | 템플릿 보조 문구 (최대 50자, Android : 18자 이상 말줄임 처리, IOS : 21자 이상 말줄임 처리) |
| -- autoSendYn | String | 자동 발송 여부 |
| -- statsId | String | 통계 ID |
| -- createDate | String | 생성 날짜 |
| -- createUser | String | 등록자(콘솔에서 발송 시 사용자 UUID로 저장) |
| - totalCount | Integer | 총 개수 |


### 대량 발송 대량 발송 수신자 목록 조회

#### 요청
[URL]
```
GET /alimtalk/v2.2/appkeys/{appKey}/mass-messages/{requestId}/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
| appKey |	String |	고유의 앱키 |
| requestId |	String |	요청 ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	설명|
|---|---|---|
| X-Secret-Key |	String|	고유의 시크릿 키 |


|값|	타입| 최대 길이 |	필수|	설명|
|---|---|---|---|---|
| requestId | String | - | O | 요청 ID |
| startRequestDate | String | - | X | 발송 날짜 시작 |
| endRequestDate | String | - | X | 발송 날짜 종료 |
| startCreateDate |	String| - |	X |	등록 날짜 시작 |
| endCreateDate |	String| - |	X |	등록 날짜 종료 |
| pageNum | optional, Integer | - | X | 페이지 번호 |
| pageSize | optional, Integer | 1000 | X | 검색 수 |

#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### 응답
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

|값|	타입|	설명|
|---|---|---|
| header | Object |	헤더 영역 |
| - resultCode |	Integer |	결과 코드 |
| - resultMessage |	String | 결과 메시지 |
| - isSuccessful |	Boolean | 성공 여부 |
| body | Object | 본문 영역 |
| - messages | Object | 메시지 리스트 |
| -- requestId | String | 요청 ID |
| -- recipientSeq | String | 수신자 시퀀스 번호 |
| -- recipientNo | String | 수신 번호 |
| -- requestDate | String | 요청 날짜 |
| -- receiveDate | String | 수신 날짜 |
| -- messageStatus | String | 메시지 상태 |
| -- resultCode | String | 결과 코드 |
| -- resultCodeName | String | 결과 코드 내용 |
| - totalCount | Integer | 총 개수 |

### 대량 발송 대량 발송 수신자 조회

#### 요청
[URL]
```
GET /alimtalk/v2.2/appkeys/{appKey}/mass-messages/{requestId}/recipients/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
| appKey |	String | 고유의 앱키 |
| requestId |	String | 요청 ID |
| recipientSeq | String | 수신자 순서 |

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	설명|
|---|---|---|
|X-Secret-Key|	String|	고유의 시크릿 키 |


|값|	타입| 최대 길이 |	필수|	설명|
|---|---|---|---|---|
| requestId | String | - | O | 요청 ID |
| startRequestDate | String | - | X | 발송 날짜 시작 |
| endRequestDate | String | - | X | 발송 날짜 종료 |
| startCreateDate |	String| - |	X |	등록 날짜 시작 |
| endCreateDate |	String| - |	X |	등록 날짜 종료 |
| pageNum | optional, Integer | - | X | 페이지 번호 |
| pageSize | optional, Integer | 1000 | X | 검색 수 |

#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/alimtalk/v2.2/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients/1?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### 응답
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

|값|	타입|	설명|
|---|---|---|
| header | Object |	헤더 영역 |
| - resultCode |	Integer |	결과 코드 |
| - resultMessage |	String | 결과 메시지 |
| - isSuccessful |	Boolean | 성공 여부 |
| body | Object | 본문 영역 |
| - requestId | String | 요청 ID |
| - recipientSeq | String | 수신자 시퀀스 번호 |
| - plusFriendId | String | 플러스 친구 ID |
| - senderKey | String | 전송자 ID |
| - templateCode |	String | 템플릿 코드 (최대 20자) |
| - recipientNo | String | 수신 번호 |
| - content | String | 내용 |
| - tempalteTitle| String | 템플릿 제목 (최대 50자, Android : 2줄, 23자 이상 말줄임 처리, IOS : 2줄, 27자 이상 말줄임 처리) |
| - templateSubtitle| String | 템플릿 보조 문구 (최대 50자, Android : 18자 이상 말줄임 처리, IOS : 21자 이상 말줄임 처리) |
| - templateExtra | String | 템플릿 부가 정보(템플릿 메시지 유형이 [부가 정보형/복합형]일 경우 필수) |
| - templateAd | String | 템플릿 내 수신 동의 요청 또는 간단한 광고 문구(템플릿 메시지 유형이 [광고 추가형/복합형]일 경우 필수) |
| - requestDate | String | 요청 날짜 |
| - receiveDate | String | 수신 날짜 |
| - createDate | String | 생성 날짜 |
| - resendStatus | String | 대체 발송 상태 코드 (RSC01, RSC02, RSC03, RSC04, RSC05)<br>([[아래 대체 발송 상태 표](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] 참고) |
| - resendStatusName | String | 대체 발송 상태명 |
| - resendResultCode | String | 대체 발송 결과 코드 [SMS 결과 코드](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api) |
| - resendRequestId | String | 대체 발송 요청 ID |
| - messageStatus | String | 대량 수신자 발송 상태 코드 (READY, COMPLETED, FAILED, CANCEL) |
| - resultCode | String | 결과 상태 코드 |
| - resultCodeName | String | 결과 상태명 |
| - createUser | String | 생성 유저 ID |
| - buttons | List | 버튼 리스트 |
| -- ordering | String | 버튼 순서 |
| -- type | String | 버튼 종류<br/> - WL: 웹링크<br/> - AL: 앱링크<br/> - DS: 배송 조회<br/> - BK: 봇 키워드<br/> - MD: 메시지 전달<br/> - BC: 상담톡 전환<br/> - BT: 봇 전환<br/> - AC: 채널 추가[광고 추가/복합형만] |
| -- name | String | 버튼 이름 |
| -- linkMo | String | 모바일 웹 링크 (WL 타입일 경우 필수 필드) |
| -- linkPc | String | PC 웹 링크  (WL 타입일 경우 선택 필드)|
| -- schemeIos | String | IOS 앱 링크 (AL 타입일 경우 필수 필드) |
| -- schemeAndroid | String | Android 앱 링크 (AL 타입일 경우 필수 필드) |
| -- chatExtra | String | BC: 상담톡 전환시 전달할 메타 정보<br/> BT: 봇 전환 시 전달할 메타 정보 |
| -- chatEvent | String | BT: 봇 전환 시 연결할 봇 이벤트명 |
| -- target|	String|	웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| - messageOption | Boolean | 메시지 옵션 |
|-- price | Integer |	message(사용자에게 전달될 메시지) 내 포함된 가격/금액/결제금액 (모먼트 광고에 해당) |
|-- currencyType | String |	message(사용자에게 전달될 메시지) 내 포함된 가격/금액/결제금액의 통화단위 KRW, USD, EUR 등 국제 통화 코드 사용 (모먼트 광고에 해당) |
