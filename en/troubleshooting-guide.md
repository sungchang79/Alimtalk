## Notification > KakaoTalk Bizmessage > 문제 해결 가이드

### Message for Query Delivery Button

Fill out the courier name and invoice number, and add a button to query delivery. Then, the courier name and invoice number are exported from the message, and the links for each courier’s query delivery page are automatically created. In case an AlimTalk message includes any courier and invoice number which are not supported by KakaoTalk, the query delivery button is not created.

Here is the list of couriers for which KakaoTalk supports the query of delivery.

List of Couriers Available to Query :
KGB택배 우체국택배 로젠택배 일양로지스 GTX로지스 FedEx 한진택배 경동택배 합동택배 롯데택배 농협택배 호남택배 CU 편의점택배 CVSnet편의점택배 TNT Express USPS EMS 천일택배 DHL 대신택배 건영택배 한덱스

<span style="color:red">**Please note that the list is subject to change by contracts between Kakotalk and each courier.**</span>
<b>CJ대한통운</b>
* 카카오 비즈메시지의 배송조회 버튼 관련 변경으로 인해, 메시지 내 CJ대한통운 송장번호가 포함된 경우, '배송조회' 버튼 없이 발송됩니다. (추후 카카오에서 변경 시, 재공지)

invoice number format

```
우체국택배: 13 Numeric or 6 Numeric + 7 Numeric (Separator '-' or '_')
example) 1234567890123, 123456-1234567, 123456_1234567

로젠택배: 11 Numeric or 3 Numeric + 4 Numeric + 4 Numeric (Separator '-' or '_'))
example) 12345678901, 123-1234-1234, 123_1234_1234

일양로지스: 9~11 Numeric
example) 123456789, 1234567890, 12345678901

FedEx: 12 Numeric
example) 123456789012

한진택배: 10 Numeric or 12 Numeric
example) 1234567890, 123456789012

경동택배: 9~16 Numeric or 4 Numeric + 3 Numeric + 6 Numeric (Separator '-')
example) 123456789, 1234567890123456, 1234-123-123456

합동택배: 9~16 Numeric
example) 123456789, 1234567890123456

롯데택배: 12 Numeric or 4 Numeric + 4 Numeric + 4 Numeric (Separator '-')
example) 123456789012, 1234-1234-1234

농협택배: 12 Numeric
example) 123456789012

호남택배: 10 Numeric
example) 1234567890

천일택배: 11 Numeric
example) 12345678901

대신택배: 13 Numeric
example) 1234567890123

건영택배: 10 Numeric
example) 1234567890

CU편의점택배: 10 Numeric or 12 Numeric or 4 Numeric + 4 Numeric + 4 Numeric (Separator '-' or '_')
example) 1234567890, 123456789012, 1234-1234-1234, 1234_1234_1234

CVSnet편의점택배: 10 Numeric or 12 Numeric or 4 Numeric + 4 Numeric + 4 Numeric (Separator '-' or '_')
example) 1234567890, 123456789012, 1234-1234-1234, 1234_1234_1234

한덱스: 10 Numeric or 14 Numeric
example) 1234567890, 12345678901234

TNT Express: 8~9 Numeric
example) 12345678, 123456789

USPS: 10 Numeric or 22 Numeric or 2 capital Alphabet + 9 Numeric + 2 capital Alphabet (No Separator)
example) 1234567890, 1234567890123456789012, AB123456789AB

EMS: 2 capital Alphabet + 9 Numeric + 2 capital Alphabet (No Separator)
example) AB1234567890AB

DHL: 10 Numeric
example) 1234567890

굿투럭: 12 Numeric or 4 Numeric + 4 Numeric + 4 Numeric (Separator '-')
예시) 1234-1234-1234

```
