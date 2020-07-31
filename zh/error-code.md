## Notification > KakaoTalk Bizmessage > Error Codes

## API Response Codes
| service | isSuccess | resultCode | resultMessage                                                |
| ------- | --------- | ---------- | ------------------------------------------------------------ |
| Common  | true      | 0          | Successful                                                   |
| Common  | false     | -1000      | Invalid appkey                                               |
| Common  | false     | -1001      | Invalid secret key                                           |
| Common  | false     | -1002      | Invalid SMS appkey                                           |
| Common  | false     | -1003      | Invalid SMS sender number                                    |
| Common  | false     | -1004      | Already registered Plus Friend                               |
| Common  | false     | -1005      | Registration requested in excess of 10 Plus Friends          |
| Common  | false     | -1008      | Registration failed for Plus Friend token                    |
| Common  | false     | -1009      | Uploading attached files failed                              |
| Common  | false     | -1014      | Invalid business registration number                         |
| Common  | false     | -1016      | Message is not found                                         |
| Common  | false     | -1017      | Sending in excess of daily volume failed                     |
| Common  | false     | -1018      | Business registration certificate file does not exist        |
| Common  | false     | -2016      | Sending requested in excess of 1,000 recipients              |
| Common  | false     | -2017      | Plus Friend does not exist                                   |
| Common  | false     | -2018      | Invalid button parameter                                     |
| Common  | false     | -2019      | Failed due to template body with above 1,000 characters      |
| Common  | false     | -2020      | Failed due to button name with above 14 characters           |
| Common  | false     | -2021      | Failed due to mobile link/web link (limkMo/linkPc) with above100 characters |
| Common  | false     | -2022      | ImageLink is missing for attached image                      |
| Common  | false     | -2023      | Friendtalk body message exceeding 400 characters (image attached) |
| Common  | false     | -2024      | Friendtalk body message exceeding 1,000 characters           |
| Common  | false     | -2025      | Scheduled date and time is from the past                     |
| Common  | false     | -2026      | Scheduled date and time is 90 days after (available up to 90 days) |
| Common  | false     | -2027      | Error in parsing due to different date format                |
| Common  | false     | -2028      | Invalid request ID                                           |
| Common  | false     | -2029      | Requested message is missing or there is no message to cancel |
| Common  | false     | -2502      | Alternative delivery requested when failed delivery is not configured |
| Common  | false     | -2999      | Request of all recipients requesting delivery failed         |
| Common  | false     | -3000      | Button name/mobile link (linkMo) is required for free button-type templates. |
| Common  | false     | -3001      | Template code or template name already exists                |
| Common  | false     | -3002      | Unable to read request body which is required                |
| Common  | false     | -3003      | Template does not exist                                      |
| Common  | false     | -3004      | Error in template parameter to send                          |
| Common  | false     | -3005      | Error in template status (when requested for delivery before approval) |
| Common  | false     | -3006      | Button URL must include http:// or https://.                 |
| Common  | false     | -3007      | Only free button-type templates allow the input of button name and button URL. |
| Common  | false     | -3008      | Query delivery button type does not allow the input of button URL. |
| Common  | false     | -3009      | Button name does not exist.                                  |
| Common  | false     | -3010      | Template body does not match.                                |
| Common  | false     | -3011      | Template button does not exist.                              |
| Common  | false     | -3100      | Unavailable to inquire of template                           |
| Common  | false     | -4003      | Query range exceeding a month                                |
| Common  | false     | -4004      | Appkey does not exist                                        |
| Common  | false     | -4005      | Appkey closed for service                                    |
| Common  | false     | -4006      | Appkey not registered with basic sender profile              |
| Common  | false     | -4007      | File size exceeding 500KB                                    |
| Common  | false     | -4015      | Invalid request ID (requestId)                               |
| Common  | false     | -4016      | Data unavailable corresponding to requested value            |
| Common  | false     | -4100      | Invalid query period                                         |
| Common  | false     | -4101      | Invalid query parameter for statistics                       |
| Common  | false     | -4103      | Start/End time value of delivery request is unavailable for queries |
| Common  | false     | -4200      | 유효하지 않은 대체 발송 메시지                                      |
| Common  | false     | -5000      | Invalid recipient number                                     |
| Common  | false     | -5001      | Recipient list unavailable for sending                       |
| Common  | false     | -7000      | Vendor request API failed                                    |
| Common  | false     | -8000      | Image sequence (imageSeq) is missing                         |
| Common  | false     | -8001      | Image file is not normal                                     |
| Common  | false     | -8002      | No image available corresponding to image sequence           |
| Common  | false     | -8003      | Deleting image failed                                        |
| Common  | false     | -8005      | No Plus Friend is registered in project to upload images     |
| Common  | false     | -8006      | 인증 메시지 발송 시, 템플릿 내용에 인증 문구가 없는 경우       |
| Common  | false     | -9995      | Called API of a faded version                                |
| Common  | false     | -9996      | Content-type is not application/json                         |
| Common  | false     | -9998      | API does not exist                                           |
| Common  | false     | -9999      | Error in system                                              |


## 발송 결과 코드

<table class="table table-striped table-hover">
<thead>
	<tr>
		<th>Code Value</th>
		<th>Significance</th>
	</tr>
</thead>
<tbody>
	<tr>
		<td>1000</td>
		<td>Successful</td>
	</tr>
  <tr>
		<td>1001</td>
		<td>Server Busy (Queue full for RS internal saving)</td>
	</tr>
  <tr>
		<td>1002</td>
		<td>Format error of recipient number</td>
	</tr>
	<tr>
		<td>1003</td>
		<td>발신 프로필 키가 유효하지 않음</td>
	</tr>
	<tr>
		<td>1004</td>
		<td>Request Body(JSON)에서 name을 찾을 수 없음</td>
	</tr>
  <tr>
		<td>1006</td>
		<td>삭제된 발신 프로필(고객센터에게 문의)</td>
	</tr>
	<tr>
		<td>1007</td>
		<td>차단 상태의 발신 프로필(고객센터에게 문의)</td>
	</tr>
	<tr>
		<td>1011</td>
		<td>계약 정보를 찾을 수 없음(고객센터에게 문의)</td>
	</tr>
  <tr>
		<td>1012</td>
		<td>잘못된 형식의 사용자키 요청</td>
	</tr>
  <tr>
		<td>1013</td>
		<td>유효하지 않은 앱 연결</td>
	</tr>
	<tr>
		<td>1014</td>
		<td>유효하지 않은 사업자번호</td>
	</tr>
	<tr>
		<td>1015</td>
		<td>유효하지 않은 app user id 요청</td>
	</tr>
	<tr>
		<td>1016</td>
		<td>사업자등록번호 불일치</td>
	</tr>
  <tr>
		<td>1021</td>
		<td>차단 상태의 카카오톡 채널</td>
	</tr>
	<tr>
		<td>1022</td>
		<td>차단 상태의 카카오톡 채널</td>
	</tr>
	<tr>
		<td>1023</td>
		<td>삭제된 카카오톡 채널</td>
	</tr>
	<tr>
		<td>1024</td>
		<td>삭제 대기 상태의 카카오톡 채널</td>
	</tr>
	<tr>
		<td>1025</td>
		<td>메시지 차단 상태의 카카오톡 채널</td>
	</tr>
	<tr>
		<td>1030</td>
		<td>잘못된 파라미터 요청</td>
	</tr>
	<tr>
		<td>2000</td>
		<td>Delivery time exceeded</td>
	</tr>
	<tr>
		<td>2001</td>
		<td>Unable to send messages (due to unexpected error)</td>
	</tr>
	<tr>
		<td>2004</td>
		<td>템플릿 일치 확인 시 오류 발생(내부 오류 발생)</td>
	</tr>
	<tr>
		<td>3000</td>
		<td>예기치 않은 오류 발생</td>
	</tr>
	<tr>
		<td>3005</td>
		<td>메시지를 발송했으나 수신 확인이 안 됨(성공 불확실, 서버에는 암호화되어 보관되며 3일 이내 수신 가능)</td>
	</tr>
	<tr>
		<td>3006</td>
		<td>내부 시스템 오류로 메시지 전송 실패</td>
	</tr>
	<tr>
		<td>3008</td>
		<td>전화번호 오류</td>
	</tr>
	<tr>
		<td>3009</td>
		<td>Format error in message </td>
	</tr>
	<tr>
		<td>3010</td>
		<td>예기치 않은 오류 발생</td>
	</tr>
	<tr>
		<td>3011</td>
		<td>메시지가 존재하지 않음</td>
	</tr>
	<tr>
		<td>3012</td>
		<td>메시지 일련번호가 중복됨</td>
	</tr>
	<tr>
		<td>3013</td>
		<td>메시지가 비어 있음</td>
	</tr>
	<tr>
		<td>3014</td>
		<td>Unknown message status</td>
	</tr>
	<tr>
		<td>3015</td>
		<td>msg_type error (neither 1008 nor 1009)</td>
	</tr>
	<tr>
		<td>3018</td>
		<td>메시지를 전송할 수 없음<br>1. 카카오톡 사용했었다가 탈퇴한사람<br>2. 카카오톡 가입한적이 한번도 없는 사람<br>3. 알림톡 수신차단<br>4. 안드로이드 사용자의경우, "핸드폰 유심과 카카오톡 사용번호"가 다른 사람<br>5. 활성 사용자가 아닌 경우(push에 해당)<br>6. 카카오톡 최소 사용버전 및 카톡 미지원, 제재 사용자 등</td>
	</tr>
	<tr>
		<td>3023</td>
		<td>Grammatical error of message (error in JSON format)</td>
	</tr>
	<tr>
		<td>3024</td>
		<td>Invalid sender profile key </td>
	</tr>
	<tr>
		<td>3025</td>
		<td>변수 글자 수 제한 초과</td>
	</tr>
	<tr>
		<td>3026</td>
		<td>Error occurred during consistency checked between message and template </td>
	</tr>
	<tr>
		<td>3027</td>
		<td>Non-Kakaotalk user (phone number error/050 safe number)</td>
	</tr>
	<tr>
		<td>3028</td>
		<td>메시지 강조 표기 타이틀이 템플릿과 일치하지 않음</td>
	</tr>
	<tr>
		<td>3029</td>
		<td>메시지 강조 표기 타이틀 길이 제한 초과(50자)</td>
	</tr>
	<tr>
		<td>3030</td>
		<td>Redundant serial number of message </td>
	</tr>
	<tr>
		<td>3031</td>
		<td>Message is empty</td>
	</tr>
	<tr>
		<td>3032</td>
		<td>Error of length restriction in message (1000 characters including spaces)</td>
	</tr>
	<tr>
		<td>3033</td>
		<td>Template not found </td>
	</tr>
	<tr>
		<td>3034</td>
		<td>Message is not consitent with template </td>
	</tr>
	<tr>
		<td>3040</td>
		<td>Invalid hub partner key </td>
	</tr>
	<tr>
		<td>3041</td>
		<td>Name not found in the request body</td>
	</tr>
	<tr>
		<td>3042</td>
		<td>Sender profile not found </td>
	</tr>
	<tr>
		<td>3043</td>
		<td>Deleted sender profile </td>
	</tr>
	<tr>
		<td>3044</td>
		<td>Blocked sender profile </td>
	</tr>
	<tr>
		<td>3045</td>
		<td>Blocked Plus Friend </td>
	</tr>
	<tr>
		<td>3046</td>
		<td>Closed Plus Friend </td>
	</tr>
	<tr>
		<td>3047</td>
		<td>Deleted Plus Friend </td>
	</tr>
	<tr>
		<td>3048</td>
		<td>Contract information not found </td>
	</tr>
	<tr>
		<td>3049</td>
		<td>Message delivery failed due to internal system error </td>
	</tr>
	<tr>
		<td>3050</td>
		<td>Non-Kakaotalk user <br>
	    User opting to block Alimtalk of users who record no Kakaotalk service use within 72 hours <br>
	    Not a friend of Friendtalk <br></td>
	</tr>
	<tr>
		<td>3051</td>
		<td>Message undelivered </td>
	</tr>
	<tr>
		<td>3054</td>
                <td>Unavailable time to send messages</td>
	</tr>
  <tr>
		<td>3055</td>
		<td>Message group information not found </td>
	</tr>
  <tr>
		<td>3056</td>
		<td>Message delivery result not found </td>
	</tr>
  <tr>
		<td>3060</td>
		<td>Sent to user but not sure if received (Polling)</td>
	</tr>
	<tr>
		<td>4000</td>
		<td>메시지 전송 결과를 찾을 수 없음</td>
	</tr>
	<tr>
		<td>4001</td>
		<td>알 수 없는 메시지 상태</td>
	</tr>
  <tr>
		<td>9998</td>
		<td>Under administrator's checkup for issue occurred in system (currently unavailable) </td>
	</tr>
  <tr>
		<td>9999</td>
		<td> Under administrator's checkup for error occurred in system (unknown error in system)</td>
	</tr>
	<tr>
		<td>E900</td>
		<td>Transfer key is not available</td>
	</tr>
	<tr>
		<td>E901</td>
		<td>Recipient number is not available </td>
	</tr>
	<tr>
		<td>E903</td>
		<td>Title is not available </td>
	</tr>
	<tr>
		<td>E904</td>
		<td>Message is not available </td>
	</tr>
	<tr>
		<td>E905</td>
		<td>Reply number is not available</td>
	</tr>
	<tr>
		<td>E906</td>
		<td>Message key is not available</td>
	</tr>
	<tr>
		<td>E915</td>
		<td>Duplicate message</td>
	</tr>
	<tr>
		<td>E916</td>
		<td>Blocked number at authenticated server </td>
	</tr>
	<tr>
		<td>E917</td>
		<td>Blocked number at customer database </td>
	</tr>
	<tr>
		<td>E918</td>
		<td>USER CALLBACK FAIL</td>
	</tr>
	<tr>
		<td>E919</td>
		<td>Message redelivery is prohibited during delivery restricted hours </td>
	</tr>
	<tr>
		<td>E920</td>
		<td>Message table includes a file group key for Alimtalk messages </td>
	</tr>
	<tr>
		<td>E999</td>
		<td>Other errors </td>
	</tr>
</tbody>
</table>
