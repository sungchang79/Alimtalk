## Notification > KakaoTalk Bizmessage > Release Notes
### 2020.05.26
* [API] 친구톡 와이드 이미지 기능
    * 친구톡 와이드 이미지를 업로드하여, 발송할 수 있는 기능이 추가되었습니다.
* [Console] 토큰 미등록 플러스친구 삭제 기능
    * 토큰 미등록 플러스친구 삭제 기능이 추가되었습니다.

### Nov. 26, 2019
* [Console] Template Registration Using File Uploads
    * Added the feature of file uploading for mass templates
* [Console] Template Query Upgrades
    * Added the feature of querying both original template status and approval status for final templates
* [Console] Query by Registered Date for Delivery Results
    * Added the feature of querying by registered date for the query of delivery results

### Oct. 29, 2019
* [API] Tighter validity checks for the delivery of certification messages
    * Message delivery is unavailable when authentication message is not included
    * For more details, see [[API User Guide](./alimtalk-api-guide/#precautions-authword)].

### Sept. 24, 2019
* [Console] Canceling Scheduled Delivery of Alimtalk/FriendTalk
    - Added the feature of canceling scheduled delivery of Alimtalk/FriendTalk from the **Query Delivery Result** tab, if it is yet to be delivered.
    - Canceling is available by querying time after scheduled delivery is requested.
* [Console] Validity Checks Added for Uploading Bulk Files for Alimtalk/FriendTalk Delivery
    - With validity checks for uploading bulk delivery files, you can receive feedbacks before delivery.
* [Console] Maximum Recipients Raised for Bulk Alimtalk/FriendTalk Delivery
    - Increased the number of maximum recipients for Alimtalk/FriendTalk from 10,000 to 100,000.
* [Console] Name Change from Kakaotalk PlusFriend to Kakaotalk Channel
    - As of September 17 of 2019, the service name has changed from 'PlusFriend' to 'Kakaotalk Channel'.

### 2019. 07. 30.
* [Console] Field Added for Result Code of Alternative SMS Delivery Request
    - To query details of alternative delivery message, result code of SMS request has been added.
* [System] Server Replacement for Service Stabilization

### 2019. 06. 27.
* [Console] Allowed alternative delivery, and added split delivery, for mass delivery of Friendtalk messages
    - Fields related to alternative delivery can be specified, such as content of alternative delivery/sender number/alternative delivery.
    - Features have been added to send in splits by specifying split times/interval.
* [API] Added API to cancel scheduled delivery of Friendtalk   
    - Scheduled Friendtalk message can be cancelled, if it is yet to be delivered.
* [API] Added API to cancel scheduled delivery of Alimtalk for authentication
    - Scheduled Alimtalk message for authentication can be cancelled, if it is yet to be delivered
* [Console] Improved mass delivery of Alimtalk/Friendtalk  
    - With [Proceed after Inspect], notification mail is sent, unless Send is clicked.  
      + Email receiving targets: All project members
      + Mail delivery condition: Click [Proceed after Inspect], and send two times in total, including one time after a day, and another in 6 days
    - For mass scheduled delivery, Proceed after Inspect is not available.
* [Console] Improved Search of Plus Friends
    - To search for a Plus Friend, search by conditions has been added.
* [Console] Fixed bugs in messages  
    - Fixed errors in messages for the status of Plus Friend, and deleting templates.
* [Console] For registring PlusFriend, <b>must be certified for business</b>
    - For registring PlusFriend, <b>must be certified for business</b> [[Related announcements](https://center-pf.kakao.com/notices/311)]


### 2019.05.28
* [API] For delivery, country code can be included to recipient numbers.  
    - The recipientNo field can now include country code for delivery.
    - Available to send to users authenticated for overseas mobile phone on the Kakaotalk appliation.
* [API] Added v1.3 for Alimtalk Delivery
    - The field configuration related for alternative delivery for Alimtalk Delivery API has been updated to the same level of Friendtalk Delivery API.  
* [API] Added Set Alternative Delivery API
    - Set Alternative Delivery API for PlusFriend has been added.
* [API] Updated Query PlusFriend API
    - Pagination has been added to Query PlusFriend API.
    - Response field of Alimtalk/Friendtalk alternative delivery has been added to Query PlusFriend API.  (v1.3)


### 2019.04.30
* [Console] 플러스친구 등록 시, 비지니스 인증 방식 롤백
    - 플러스친구의 비지니스 인증이 장시간 소요되어, 플러스친구 등록 방식을 이전 방식으로 변경하였습니다.

### 2019.04.23
* [Console] 알림톡 대량 발송 시, <b>대체발송 지정 기능, 분할 발송 기능</b> 추가
    - 대체발송 내용/발신번호/대체발송 여부 등 대체발송 관련 필드를 지정할 수 있는 기능이 추가되었습니다.
    - 분할 횟수/시간 간격을 지정하여 분할 발송할 수 있는 기능이 추가되었습니다.
* [Console] 알림톡/친구톡 <b>대체발송 설정 분리</b>
    - 알림톡/친구톡 각각의 플러스친구에 대체발송 설정을 할 수 있는 기능이 추가되었습니다.
    <br>ex) 동일한 플러스친구에서 알림톡만 대체발송 설정 시, 친구톡은 발송 실패하여도 대체발송 되지 않습니다.
* [Console] 플러스친구 등록 시, <b>비지니스 인증 도입</b>
    - 플러스친구 등록 시 플러스친구의 <b>비즈니스 인증</b>된 경우만 등록 가능하도록 변경됩니다.​ [[관련 공지사항](https://center-pf.kakao.com/notices/311)]
* [Console] 템플릿 선택 모달창에서 like 검색 기능 추가
    - 템플릿 선택 모달창에서 손쉽게 템플릿을 선택할 수 있도록 like 검색 기능이 추가되었습니다. (템플릿코드, 템플릿명으로 검색)
* [API] 광고 친구톡, <b>광고 SMS API로 대체발송</b> 되도록 개선
    - 광고 친구톡 발송 실패 시, 광고 SMS API로 대체발송 되도록 개선되었습니다.
    - 광고 친구톡 대체 발송 설정 시, 080수신거부번호를 반드시 등록해야 정상적으로 대체발송 됩니다.
* [API] 친구톡 발송 API 대체 발송 고도화
    - 발송 시, 대체 발송 제목 필드가 추가 되었습니다.
    - 대체 발송 필드를 이용하여, 대체발송 될 제목/내용/발신번호/080수신거부번호/대체발송 유무를 선택할 수 있습니다.
* [API] 알림톡/친구톡 발송 요청 실패 메시지 저장하지 않도록 개선
    - 발송 수신자 길이 제한, 유효하지 않은 수신번호 등 요청 오류로 인한 메시지는 저장하지 않도록 개선되었습니다.
    - API 응답 필드 sendResults 필드를 통해 요청 성공/실패를 확인할 수 있습니다.

### 2019.03.26
* [Console] 알림톡 미리보기 UI 추가
    - 알림톡 수신 화면 미리보기 UI가 추가 되었습니다.
* [API] 인증용 알림톡 발송을 위한 Auth API 추가
    - 인증용 알림톡 발송을 위해 Auth용 API가 추가되어, 발송 pool이 분리되었습니다.

### 2019.02.26
* [Console] 알림톡 대량 발송 시, 발송 실패 버그 수정
    - 일부 유효하지 않은 수신번호로 인해, 발송 실패되는 버그를 수정하였습니다.
* [Console] 친구톡 대량 발송 수신번호 마스킹되는 버그 수정
    - 친구톡 대량 발송 수신자 상세 조회 시, 수신번호 마스킹되는 버그를 수정하였습니다.

### 2019.01.29
* [API] 친구톡 v1.2 API 추가
    - 발송 시, 발신/수신자 그룹핑키 필드가 추가되었습니다.
    - 발송 응답 필드에 각 수신자 별 <b>요청 성공/실패</b> 필드가 추가되었습니다.
* [API] 친구톡 발송 결과 업데이트 조회 API 추가
    - 결과 업데이트 시간으로 조회할 수 있는 신규 API가 추가되었습니다.
* [Console] 알림톡 발송 화면 고도화
    - 복수의 수신자에게 발송할 수 있게 개선되었습니다.
    - 예약 발송을 할 수 있게 개선되었습니다.
    - 대체 발송 내용을 선택할 수 있게 개선되었습니다.
* [Console] 친구톡 대량 발송 기능 추가
    - csv 파일을 이용하여, 대량 발송 기능이 추가되었습니다.
* [Console] 친구톡 대량 발송 조회 기능 추가
    - 친구톡 대량 발송 조회 탭에서 대량 발송한 메시지를 조회할 수 있습니다.
* [Console] 알림톡 템플릿 수정 기능 고도화
    - 기존 반려 상태의 템플릿만 수정할 수 있었지만, 승인 상태의 템플릿도 수정할 수 있도록 개선되었습니다.
    - 승인 상태의 템플릿을 수정 시 재검수를 받게 되고 재검수 후, 승인된 템플릿은 <b>기존 템플릿의 내용과 대체</b>됩니다.
    - 재검수를 받는 동안, 이전 승인된 템플릿은 정상적으로 발송 가능합니다.
* [Console] 수신 번호 마스킹 기능 추가
    - 별도 요청한 프로젝트는 수신 번호 마스킹 기능이 추가되었습니다.
* [Console] 알림톡 템플릿 본문 글자 수 검사 버그 수정
    - 템플릿 본문 중, 공백이 2글자로 계산되는 버그 수정되었습니다.

### 2018.12.04
* [API] 알림톡 v1.2 API 추가
    - 발송 시, 발신/수신자 그룹핑키 필드가 추가되었습니다.
    - 발송 응답 필드에 각 수신자 별 <b>요청 성공/실패</b> 필드가 추가되었습니다.
* [API] 알림톡 발송 결과 업데이트 조회 API 추가
    - 결과 업데이트 시간으로 조회할 수 있는 신규 API가 추가되었습니다.
* [API] 발송 시, 대체발송 발신번호 필드 추가
    - 발송 시, resendSendNo 필드를 통하여 대체발송 발신번호를 지정할 수 있는 기능이 추가되었습니다.
* [API] 발송 메시지 90일까지 보관하도록 개선
    - 조회 시, 90일 이전 데이터는 조회되지 않도록 개선되었습니다.
* [Console] 플러스친구 상세 상태 추가
    - 플러스친구 조회 화면에 TOAST 플러스친구, 카카오 플러스친구, 카카오 플러스친구 프로필 상태 필드가 추가되었습니다.

### 2018.11.13
* [API] 알림톡 발송 API 대체 발송 고도화
    - 발송 시, 대체 발송 제목 필드가 추가 되었습니다.
    - 대체 발송 제목 필드를 이용하여, LMS로 대체 발송 시, 제목을 지정할 수 있습니다.
    - 템플릿 본문 불일치 (-3010), 템플릿 버튼 불일치 (-3011)으로 요청 실패할 경우, 대체 발송 되도록 변경되었습니다. 대체 발송이 필요없는 메시지는 isResend 필드를 이용하여, 조작 가능합니다.
* [API] 예약 발송 버그 수정
    - 예약 발송 시, 요청 아이디의 일자 데이터 오류로 인해, 예약 발송 취소가 정상적으로 되지 않는 버그를 수정하였습니다.

### 2018.10.23
* [API] 알림톡 발송 API 예약 기능 추가
    - 예약 기능을 사용하여 원하는 시간에 메시지를 발송할 수 있습니다.
    - 예약한 메시지가 발송 전이라면 언제든지 취소할 수 있습니다.
    - 자세한 사항은 [[알림톡 발송 API](./alimtalk-api-guide/#_3)] 를 참고하시기 바랍니다.
* [API] 알림톡 발송 API 대체 발송 고도화
    - 발송 시, 대체 발송 타입(SMS/LMS), 대체 발송 여부(true/false), 대체 발송 내용 필드가 추가 되었습니다.
    - 해당 필드를 사용하여, 대체 발송을 더 확장성 있게 사용할 수 있습니다.
* [API] 플러스친구, 템플릿 관련 API 추가
    - 플러스친구 카테고리 조회, 사업자등록증 업로드, 등록, 토큰 인증, 리스트 조회 API가 추가되었습니다.
    - 템플릿 등록, 수정, 삭제, 문의하기 API가 추가되었습니다.
* [Console] 템플릿 코드 Like 검색 기능 추가
    - Console에서 템플릿 조회 시, Like 검색 기능이 추가되었습니다.

### 2018.08.28
* [Console] 발송 결과 조회 개선
    - 알림톡 발송 결과 조회 시, 템플릿 코드를 직접 입력할 수 있도록 개선되었습니다.

### 2018.07.24
* [Console] 서비스명 변경
    - Alimtalk 에서 KakaoTalkBizmessage로 서비스명이 변경되었습니다.
* [Console] 친구톡 기능 추가
    - 친구톡은 친구를 맺은 이용자를 대상으로 하며, 광고성 메시지도 발송이 가능합니다.
    - 콘솔에서 메시지 발송, 조회, 이미지 관리, 통계 기능을 제공합니다.
    - 자세한 사항은 [[친구톡 개요](./friendtalk-overview/)] 를 참고하시기 바랍니다.
* [API] 친구톡 API 추가
    - 친구톡 API는 메시지 발송, 목록 조회, 단건 조회, 이미지 관리 기능을 지원합니다.
    - 자세한 사항은 [[친구톡 API 가이드](./friendtalk-api-guide/)] 를 참고하시기 바랍니다.
* [API] 알림톡, 친구톡 일별 발송 제한 추가
    - 7월 24일 점검 이후, 생성된 플러스 친구에 한하여, 일별 1,000건의 발송량 제한 기능이 추가 되었습니다.
    - 일별 최대 발송량이 초과한 발송 요청건은 실패 처리됩니다.
* [API] 발송 실패 재발송 기능 수정
    - 재발송 본문 내용이 아래와 같이 수정되었습니다.

```
발송 본문 내용
- 웹 링크 버튼명: 웹 링크
- 웹 링크 버튼명2: 웹 링크2
...
```

### 2018.06.26
#### 기능 개선
* [Console] 플러스친구 등록 시, 요청 필드 추가
    - 카카오 발급절차 강화로 인해 사업자등록번호, 사업자 카테고리를 입력받도록 수정되었습니다.

#### 버그 수정
* [Console] 통계 차트 버그 개선 버전으로 변경
    - IE 브라우져에서 Export xls 기능 버그 개선 버전으로 변경하였습니다.

### 2018.05.29
#### 기능 추가
* [API] 발송 결과 단건 조회 API 추가
    - 특정 발송 결과를 조회할 수 있는 API가 추가되었습니다.
    - 자세한 사항은 [[알림톡 API 가이드](./alimtalk-api-guide/#_14)] 를 참고하시기 바랍니다.
* [API] 발송 결과 리스트 조회 API v1.1 추가
    - 응답에 recipientSeq (수신자 시퀀스) 필드가 추가되면서, v1.1 API가 추가되었습니다.

#### 버그 수정
* [API] 치환 발송 API 버그 수정
    - Request Body의 templateParameter 필드가 필수 값으로 인식되는 버그가 개선되었습니다.

### 2018.04.24
#### 기능 추가
* [Console] 템플릿 챗버블 기능 추가
    - 다중 버튼 템플릿 기능이 추가되었습니다.
    - 버튼 타입 추가되었습니다. ( 배송 조회, 웹 링크, 앱 링크, 봇 키워드, 메세지 전달 )
* [API] 템플릿 조회 API 추가
    - 템플릿을 조회할 수 있는 API가 추가되었습니다.
    - 자세한 사항은 [[알림톡 API 가이드](./alimtalk-api-guide/#_46)] 를 참고하시기 바랍니다.

#### 기능 개선/변경
* [API] 발송 실패 재발송 기능 수정
    - 본문 길이에 따라 SMS/LMS로 구분지어 발송하도록 수정되었습니다.
    - 재발송 본문 내용이 수정되었습니다. ( 본문 + 웹 링크 버튼명 + 버튼 링크 )

### 2018.03.22
#### 기능 추가
* [Console] 템플릿 수정, 삭제, 문의 등록 기능 추가
    - 템플릿 수정, 삭제 기능이 추가되었습니다.
    - 검수 담당자에게 문의를 등록 기능이 추가되었습니다.
    - 자세한 사항은 [[알림톡 콘솔 사용 가이드](./alimtalk-console-guide/#_12)] 를 참고하시기 바랍니다.
* [API] 전문 발송 API 추가
    - 치환 데이터가 아닌 전문 내용으로 발송할 수 있는 API가 추가되었습니다.
    - 자세한 사항은 [[알림톡 API 가이드](./alimtalk-api-guide/#_4)] 를 참고하시기 바랍니다.

### 2018.02.22
#### 기능 추가
* [Console] 대량 발송 기능 추가
    - csv 파일을 이용하여, 대량 발송 기능이 추가되었습니다.

### 2018.01.25
#### 기능 개선/변경
* [Console] 템플릿 등록 개선
    - 버튼명 앞뒤 공백 제거 기능이 추가 되었습니다.
    - 길이 제한 수정되었습니다. ( 템플릿 코드: 10자 -> 20자, 버튼명: 10자 -> 14자)
    - 배송 조회 템플릿 등록 시, 버튼명을 직접 입력할 수 있게 수정되었습니다.

#### 기능 추가
* [API] 발송 결과 조회 API 추가

### 2017.11.23
#### 기능 추가
* [Console] 다중 플러스친구 등록
    - 1개의 플러스친구 등록 구조에서 다중 플러스친구 구조로 변경되었습니다.
* [Console] 템플릿 반려 시, 템플릿 상세보기에서 반려 사유 제공.
* [API] 다중 플러스친구 적용에 따른 발송 API 필드 추가
    - requestBody에 plusFriendId 필드12가 추가되었습니다.
    - plusFriendId 필드를 입력하지 않을 경우, 처음 등록된 플러스친구 아이디로 발송됩니다.

### 2017.08.24
#### 기능 추가
* [Console] 알림톡 발송 통계 화면 제공
    - 날짜별/시간대별/요일별 통계 화면이 제공됩니다.
    - 발송한 일자와 템플릿으로 조회할 수 있습니다.

#### 기능 개선/변경
* [Console] 자유버튼 템플릿 등록 시 URL 검증 변경
    - 자유버튼에 URL 등록 시, http:// or https:// 가 필수로 포함 -> #{url}과 같이 템플릿 치환자도 등록할 수 있게 변경하였습니다.
    - 템플릿 치환자 #{url} 형식의 템플릿 치환자가 아닐 경우 http:// or https:// 검증은 유지됩니다.
* [API] Content-type 에러 응답 메세지 수정
    - 요청 header에 Content-type: application/json이 아닐 경우 실패 응답 메세지 수정되었습니다.

### 2017.07.20
#### 기능 추가
* [Console] 알림톡 발송 결과 조회 추가
    - 발신 일시, 수신번호, 템플릿 등의 조건으로 발신한 메세지의 전송 결과를 조회할 수 있습니다.

### 2017.06.22
#### 기능 개선/변경
* [Console] 발신프로필 관리 페이지 추가
* [Console] 테스트 발송 페이지 추가
* [Console] 템플릿 등록 조회 페이지 추가

#### 기능 추가
* [Console] 발송 실패 설정 추가
    - 알림톡 발송 실패 시, LMS로 대체 발송하는 기능
* [Console] 템플릿 자유 버튼 타입에 링크 치환 기능
    - 템플릿 자유 버튼 타입일 경우, 버튼 링크를 치환자로 등록 가능. ex) buttonURL: #{url}


### 2017.05.25
#### 기능 개선/변경
* [Console] 메인페이지 마크업 개선

### 2017.04.20
#### 신규 상품 출시
* Alimtalk은 휴대폰 번호를 기반으로 친구 추가 없이 카카오톡 사용자에게 배송, 예약 시간 등의 정보성 메시지를 발송할 수 있는 상품입니다.
* 손쉬운 연동을 위한 RESTful API를 제공합니다.
