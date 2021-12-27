## Notification > KakaoTalk Bizmessage > Common > API v2.2 Guide

## 통계

### [API 도메인]

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


### 통계 검색 - 이벤트 기반
* 이벤트 발생 시간 기준으로 수집된 통계입니다.
* 다음 시간 기준으로 통계가 수집됩니다.
    * 요청 개수(REQUESTED): 예약 발송 등록 시간
    * 발송 개수(SENT): 벤더로 발송 시점 (예약 발송 시간)
    * 성공 개수(RECEIVED): 발송 결과 성공 (수신 시간)
    * 실패 개수(SENT_FAILED): 발송 요청 실패 or 발송 결과 실패 시점
    * 대체 발송 요청 개수(RESENT): 대체 발송 요청 시점
    * 대체 발송 실패 개수(RESENT_FAILED):대체 발송 요청 실패 시점

### 통계 정보 조회

[URL]

|Http method|	URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/stats |

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
| appKey | String | 고유의 앱키 |

[Query parameter]

| 이름 | 타입 | 최대 길이 | 필수 | 설명 |
|---|---|---|---|---|
| statsType | String | - | 필수 | 통계 구분<br/>NORMAL:기본, MINUTELY:분별, HOURLY:시간별, DAILY:일별, BY_DAY:시간별, DAY:요일별 |
| from | String | - | 필수 | 통계 검색 시작 날짜<br/>yyyy-MM-dd HH:mm:ss | 
| to | String | - | 필수 | 통계 검색 종료 날짜<br/>yyyy-MM-dd HH:mm:ss |
| join | Boolean | - | 옵션 | 통계 데이터 조회 시, 트리 형태로 제공할 지 설정 |
| extra1s | List<String> | - | 옵션 | 하위 상품 구분<br/> ALIMTALK, ALIMTALK_AUTH, FRIENDTALK |
| extra2s | List<String> | - | 옵션 | senderKey |
| eventTypes | List<String> | - | 옵션 | 이벤트 종류<br/> REQUESTED, SENT, RECEIVED, SENT_FAILED, RESENT, RESENT_FAILED |
| eventCategory | String | - | 옵션 | 이벤트 목록(현재 `MESSAGE`만 지원)<br/> MESSAGE |
| templateCodes | List<String> | - | 옵션 | 템플릿 코드 목록 (친구톡 미지원) |
| requestIds | List<String> | 5 | 옵션 | 요청 ID 목록 |
| statsIds | List<String> | - | 옵션 | 통계 ID 목록 |
| statsCriteria | List<String> | - | 옵션 | 통계 기준<br/>- EVENT: 이벤트(기본 값)<br/>- EXTRA_1,EVENT: 하위 상품 구분, 이벤트<br/>- EXTRA_2,EVENT: senderKey, 이벤트 |

[Response body]
```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "stats": [
         {
            "eventDateTime": "2021-10-13T00:00:00.000Z",
            "events": {
                "RECEIVED": 5,
                "RESENT_FAILED": 0,
                "SENT_FAILED": 0,
                "REQUESTED": 1,
                "RESENT": 0,
                "SENT": 5
            }
        },
        {
            "eventDateTime": "2021-10-14T00:00:00.000Z",
            "events": {
                "RECEIVED": 25,
                "RESENT_FAILED": 1,
                "SENT_FAILED": 19,
                "REQUESTED": 55,
                "RESENT": 0,
                "SENT": 27
            }
        }
    ]
}
```

### 이벤트별 개수 조회

[URL]

|Http method|	URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/stats/total |

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
| appKey | String | 고유의 앱키 |

[Query parameter]

| 이름 | 타입 | 최대 길이 | 필수 | 설명 |
|---|---|---|---|---|
| statsType | String | - | 필수 | 통계 구분<br/>NORMAL:기본, MINUTELY:분별, HOURLY:시간별, DAILY:일별, BY_DAY:시간별, DAY:요일별 |
| from | String | - | 필수 | 통계 검색 시작 날짜<br/>yyyy-MM-dd HH:mm:ss | 
| to | String | - | 필수 | 통계 검색 종료 날짜<br/>yyyy-MM-dd HH:mm:ss |
| join | Boolean | - | 옵션 | 통계 데이터 조회 시, 트리 형태로 제공할 지 설정 |
| extra1s | List<String> | - | 옵션 | 하위 상품 구분<br/> ALIMTALK, ALIMTALK_AUTH, FRIENDTALK |
| extra2s | List<String> | - | 옵션 | senderKey |
| eventTypes | List<String> | - | 옵션 | 이벤트 종류<br/> REQUESTED, SENT, RECEIVED, SENT_FAILED, RESENT, RESENT_FAILED |
| eventCategory | String | - | 옵션 | 이벤트 목록(현재 `MESSAGE`만 지원)<br/> MESSAGE |
| templateCodes | List<String> | - | 옵션 | 템플릿 코드 목록 (친구톡 미지원) |
| requestIds | List<String> | 5 | 옵션 | 요청 ID 목록 |
| statsIds | List<String> | - | 옵션 | 통계 ID 목록 |
| statsCriteria | List<String> | - | 옵션 | 통계 기준<br/>- EVENT: 이벤트(기본 값)<br/>- EXTRA_1,EVENT: 하위 상품 구분, 이벤트<br/>- EXTRA_2,EVENT: senderKey, 이벤트 |

[Response body]
```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "total": {
        "RECEIVED": 45,
        "REQUESTED": 56,
        "RESENT": 0,
        "RESENT_FAILED": 1,
        "SENT": 47,
        "SENT_FAILED": 19
    }
}
```