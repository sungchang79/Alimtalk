## Notification > KakaoTalk Bizmessage > 알림톡 > 콘솔 사용 가이드

## 알림톡 일반 발송

알림톡 발송 화면입니다.

![alimtalk_01_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_01_201812.png)

플러스친구를 선택한 후 등록된 템플릿을 불러옵니다.
플러스친구는 **Notification > KakaoTalk Bizmessage > 플러스친구 관리** 탭에서 확인할 수 있습니다.
템플릿은 **Notification > KakaoTalk Bizmessage > 알림톡 > 템플릿 관리** 탭에서 관리할 수 있습니다.

하단 **일반 발송**탭에서 템플릿에 존재하는 치환키값을 입력한 후 수신자 번호를 입력합니다.

입력 완료 후 오른쪽 **발송** 버튼을 클릭하여 즉시 전송합니다.

## 알림톡 대량 발송

### 대량 발송 요청

하단 탭에서 **대량 발송**을 선택합니다.

![alimtalk_02_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_02_201812.png)

![alimtalk_02-1_201904.png](https://static.toastoven.net/prod_alimtalk/alimtalk_02-1_201904.png)

* 템플릿을 선택 후, **템플릿 다운로드** 버튼을 클릭 시 템플릿 치환자가 포함된 CSV, XLSX 파일을 다운로드할 수 있습니다. 템플릿 치환자가 포함되지 않을 경우 발송 실패처리 됩니다.
* CSV 파일은 Excel로 열어 저장할 시, 한글이 정상적으로 저장되지 않을 수 있으므로 검수 후 발송으로 치환이 정상적으로 되었는지 확인을 권장합니다.
* 파일 업로드는 CSV, XLSX 파일만 가능하며, 크기는 최대 20MB이고, 발송할 수 있는 최대 수신자는 1,000,000명입니다.
* 분할 발송을 이용하여, 분할 횟수 / 발송 간격 시간을 지정할 수 있습니다.

![alimtalk_03_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_03_201812.png)

* **발송** 버튼 클릭 시 **검수 후 진행**, **즉시 발송** 2가지를 선택하여 발송할 수 있습니다.

![alimtalk_04_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_04_201812.png)

## 발송 결과 조회

![alimtalk_05_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_05_201812.png)

일반 발송을 통한 발송 결과를 조회할 수 있습니다.
**등록 일시**, **발신 일시**는 둘 중 하나만 선택하여 조회가 가능하며, 이는 필수 조건입니다.
각 발송 메시지의 자세한 내용을 확인하려면 검색 결과 항목을 클릭 시 **상세 조회** 창이 나타납니다.

![alimtalk_06_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_06_201812.png)

### 발송 취소

일반 발송 중, 발신 요청 일시를 미래로 요청한 예약 발송의 경우 취소가 가능합니다.
예약 발신 요청을 조회하면 요청 ID 좌측에 체크 박스를 확인할 수 있습니다.
체크 박스는 취소되지 않은 예약 요청에만 나타납니다.
취소를 원하는 요청의 체크 박스를 선택한 후 상단의 **선택 예약 취소** 버튼을 누르면 해당 요청이 취소됩니다.
조회 목록 머리글에 있는 체크 박스를 통해 해당 목록 전체 선택 및 취소가 가능합니다

## 대량 발송 조회

### 발송/취소

대량 알림톡 발송 시 **검수 후 진행**을 선택하면 **대량 발송 조회** 탭에서 알림톡을 발송하거나 취소할 수 있습니다.

![alimtalk_07_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_07_201812.png)

* **발송** 버튼을 클릭하면 대량 발송을 진행합니다. 검색 결과에서 항목을 클릭하고 **상세 조회** 창에서 치환값을 확인할 것을 권장합니다.
* 발송 중 상태에서 **취소** 버튼을 클릭하면 일부 수신자에게 발송이 진행될 수 있습니다.

* 대량 발송 조회 탭에서 검색한 결과의 특정 열을 클릭할 경우 수신자 항목이 나타납니다.

![alimtalk_08_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_08_201812.png)

* **수신자별 조회**에서 해당 수신자를 선택하여, 발송 내용이 정상적으로 치환되었는지 확인할 수 있습니다.

![alimtalk_09_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_09_201812.png)

### 대량 발송 진행 상태
  - <b>대기:</b> 템플릿 파일 데이터를 읽는 작업을 진행하기 전 상태입니다.
  - <b>발송 준비:</b> 템플릿 파일 데이터를 로드 중인 상태입니다.
  - <b>발송 준비 완료:</b> 템플릿 파일 데이터를 모두 로드하여 발송 준비가 완료된 상태입니다. 요청 건(목록의 행)을 선택하면 하단의 목록에서 수신 번호와 발송 내용을 확인할 수 있습니다.
  - <b>발송 대기:</b> 발송 작업을 진행하기 전 상태입니다.
  - <b>발송 중:</b> 발송이 진행 중인 상태입니다.
  - <b>발송 완료:</b> 발송 요청이 정상적으로 완료된 상태입니다.
  - <b>발송 실패:</b> 발송 진행 중 발송 오류가 발생한 경우입니다.
  - <b>발송 취소:</b> 사용자가 발송을 취소한 상태입니다.


## 템플릿 관리

### 템플릿 등록

**템플릿 등록** 버튼을 클릭해 템플릿을 등록할 수 있습니다.

![alimtalk_10_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_10_201812.png)

* 템플릿 코드는 10글자 이내의 영문, 숫자만 등록할 수 있습니다.
* 템플릿명은 150글자까지 등록할 수 있습니다.
* 템플릿 내용은 한/영 구분 없이 변수 및 URL 포함 1,000자까지 등록할 수 있습니다.
* 버튼 링크 URL은 500자까지 등록할 수 있습니다.
* 템플릿 치환자는 #{치환자}와 같이 등록하면 됩니다.
* 버튼 링크에 치환자를 사용할 때 http:// 또는 https:// 프로토콜을 반드시 입력해야 합니다. 예) http://#{URL} 또는 https://#{URL}
* Excel 파일을 이용하여 대량 등록이 가능합니다. 대량 등록 시 중복되거나, 잘못된 템플릿이 존재하는 경우 정상적인 템플릿만 등록됩니다.
* 템플릿 등록 시, <b>요청 -> 검수 중 -> 승인/반려</b> 상태 순서로 업데이트됩니다.
* 템플릿 반려 시, <b>문의 등록</b> 및 <b>수정</b> 기능으로 재검수할 수 있습니다.
* 반려된 템플릿은 <b>삭제</b> 후, 재등록할 수 있습니다.
* 자세한 사항은 [[템플릿 검수 가이드](https://www.bizmsg.kr/collected_statics/assets_landing/doc/alimtalk_template_guide.pdf)]를 참고하시기 바랍니다.

#### 템플릿 수정

* 승인된 템플릿을 수정한 후 검수가 완료되면, 기존 템플릿 내용이 수정한 내용으로 대체됩니다.

#### 템플릿 삭제

* <b><span style="color:red">요청/반려</span></b> 상태의 템플릿만 삭제할 수 있습니다.

#### 템플릿 문의 등록

* <b><span style="color:red">검수 중/반려</span></b> 상태의 템플릿만 문의를 등록할 수 있습니다.
* 등록한 문의는 검수 결과에 추가되며, 카카오 검수자가 이를 확인합니다.
* 검수 결과에 템플릿 용도에 관한 문의, 반려 사유 내용이 추가됩니다.

## 대체 발송 관리

* 알림톡/친구톡 각 메시지 타입에 따라 플러스친구의 대체 발송을 설정할 수 있습니다.
* 대체 발송을 설정한 플러스친구의 메시지만 LMS 또는 SMS로 대체 발송됩니다.
* SMS 앱키를 수정하면 모든 플러스친구의 대체 발송 설정은 초기화됩니다.
* SMS 서비스로 대체 발송되므로, SMS 서비스의 발송 API 명세에 따라 필드를 입력해야 합니다.(SMS 서비스에 등록된 발신 번호, 각종 필드 길이 제한 등)
* 지정한 대체 발송 타입의 바이트 제한을 초과하는 대체 발송 제목이나 내용은 잘려서 대체 발송될 수 있습니다.([[SMS 주의 사항](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] 참고)
* 대체 발송 내용은 EUC-KR 기준으로 발송되며, 지원하지 않는 이모티콘은 대체 발송에 실패합니다.

![plusfriend_03_201812.png](https://static.toastoven.net/prod_alimtalk/plusfriend_03_201904.png)

## 통계
### 통계 조회

![alimtalk_11_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_11_201812.png)

발송 요청 기간, 플러스친구, 템플릿별로 통계를 확인할 수 있습니다.
발송 요청, 성공, 실패, 대체 발송 건을 그래프와 표로 확인할 수 있습니다.
