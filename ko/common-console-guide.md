## Notification > KakaoTalk Bizmessage > 콘솔 사용 가이드

## 발신 프로필 등록/인증
* 카카오톡 비즈 메시지를 발송하려면 발신 프로필을 먼저 등록해야 합니다.
* 카카오톡 채널은 카카오톡 홈페이지( https://center-pf.kakao.com )에서 무료로 만들 수 있습니다.
* 비즈니스 인증을 받은 카카오톡 채널만 NHN Cloud KakaoTalk Bizmessage 서비스에 추가할 수 있습니다.([[플러스친구 비즈니스 인증](https://static.toastoven.net/prod_alimtalk/plusfriend_business_certify_guide_20190311.pdf)] 참고)

## 발신 프로필 추가

발신 프로필 등록이 완료되면 관리자 휴대폰으로 카카오톡 토큰 메시지가 전달됩니다.
관리자로 등록된 휴대폰으로만 카카오톡 토큰 메시지가 전달됩니다.

![plusfriend_01_201812.png](https://static.toastoven.net/prod_alimtalk/plusfriend_01_201904.png)

* 플러스친구 ID는 플러스친구를 개설할 때 등록한 검색용 아이디를 입력해야 합니다.
* 고객이 받는 카카오톡 비즈 메시지는 카카오톡에 등록한 플러스친구 이름으로 표시됩니다.

## 토큰 등록

관리자 휴대폰으로 받은 토큰 메시지를 입력하면 등록이 완료됩니다.

![plusfriend_02_201812.png](https://static.toastoven.net/prod_alimtalk/plusfriend_02_201904.png)

<b><span style="color:red">발신 프로필 등록 시, 초기 일별 최대 발송량은 1,000건으로 제한됩니다.</span></b>
일별 최대 발송량을 변경하려면 고객 센터(support@toast.com)에 별도로 요청해야 합니다.

## 대체 발송 관리

발신 프로필 별로 '대체 발송 설정'을 할 수 있습니다.

* 발송 실패 설정을 한 발신 프로필의 메시지만 LMS 또는 SMS로 대체 발송됩니다.
* SMS 앱키 수정 시, 모든 발신 프로필의 발송 실패 설정은 초기화됩니다.

![plusfriend_03_201812.png](https://static.toastoven.net/prod_alimtalk/plusfriend_03_201812.png)

## 개인정보 수탁사 고지 안내
'고객'이 NHN Cloud > Notification > KakaoTalk Bizmessage 서비스 이용 시, '고객' - '당사' 간 개인정보 처리에 관한 업무 위수탁 관계가 발생하는 바 정보통신망법 및 개인정보보호법에 따라 위탁자인 '고객'은 개인정보 처리방침을 통해 '당사'에 개인정보를 위탁한 현황(수탁자 및 업무의 내용)을 공개해야 합니다.

이에, '당사'에서는 '고객'이 NHN Cloud의 KakaoTalk Bizmessage 서비스를 이용함에 있어 관련 법령을 준수하고, 위탁 현황 미공개로 인하여 과태료 등의 불이익을 받지 않도록 아래와 같이 가이드할 수 있습니다.

(예시)<br>
[개인정보 수탁사 고지 안내]<br>
KakaoTalk Bizmessage 서비스 이용 시 고객사에서 운영하는 '개인정보 처리방침 > 위탁 현황'에 다음의 내용을 표기해주세요.<br>
수탁사: 엔에이치엔<br>
업무의 내용: 카카오톡 비즈 메시지 발송 대행<br>

## 발송 설정
메시지 보관 기간 정책에 따라, 90일이 지난 발송 이력 데이터를 백업할 수 있습니다.
알림톡 백업 여부, 파일 확장자, 파일을 업로드할 저장소 정보를 입력하면, 해당 저장소에 백업 일자가 포함된 파일이 생성됩니다.

## 웹훅 관리
지정한 이벤트 발생 시, URL을 지정하여 웹훅 이벤트를 받을 수 있습니다.

## 통계 이벤트 키 설정
이벤트 키를 등록하여, 해당 키로 발송 시, 통계 이벤트 키 별로 통계 데이터를 수집할 수 있습니다.
