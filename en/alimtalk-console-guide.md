## Notification > KakaoTalk Bizmessage > Alimtalk > Console Guide

## General Delivery

To send Alimtalk, go to **TOAST Console** and **Notification > KakaoTalk Bizmessage > Alimtalk**.

![alimtalk_01_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_01_201812.png)

Select a Plus Friend and load a registered template.
Check your Plus Friends on **Notification > KakaoTalk Bizmessage > Plus Friend Management **.
Templates are available on the **Notification > KakaoTalk Bizmessage > Alimtalk > Template Management**.

Enter replacement key of the template from **General Delivery** at the bottom, and enter recipient number.

Then, click **Send** on the right to immediately send.  

## Mass Delivery

### Request of Mass Delivery

Select **Mass Delivery** at the bottom.

![alimtalk_02_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_02_201812.png)

* Select a template and click **Download Templates** to download CSV file which includes a template replacer. Sending fails if a template replacer is not included.  
* It is recommended to send after inspection to see if it was properly replaced; saving CSV file in excel may cause broken Korean.  
* Only CSV files, sized no more than 20 MB, can be uploaded, up to 100,000 recipients.

![alimtalk_03_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_03_201812.png)

* Click **Send**, and select either **Proceed after Inspection** or **Send Immediately**.

![alimtalk_04_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_04_201812.png)

## Query Delivery Result

Check results of general delivery.

Date and time of sending are required.

![alimtalk_05_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_05_201812.png)

It is available to query by selecting only one of **Registered Date, or Sent Date**, which is a required condition.
To check details of each delivery message, click search result and **Query Details** pops up.  

![alimtalk_06_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_06_201812.png)

### Canceling Delivery

Scheduled delivery which is requested for a future date can be canceled.
By searching for a request of scheduled delivery, you can find a checkbox on the left of a request ID.
Check boxes are available only for those scheduled delivery requests which are not canceled. To cancel a request, check the box you need to cancel, and press **Cancel Selected Schedule**.
The whole list can be selected and canceled, by selecting the check box on top of the list.

## Query Mass Delivery

### Send/Cancel

In sending mass Alimtalk, select **Proceed after Inspect** to send or cancel Alimtalk.

![alimtalk_07_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_07_201812.png)

* Click **Send** to send messages in mass. It is recommended to click an item from search result and check its replacement value on the **Query Details** window.  
* If **Cancel** is clicked while delivery is underway, some recipients may receive messages.

* Click a particular row of search result from the query of mass delivery tab, and recipients will show.  

![alimtalk_08_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_08_201812.png)

* Select a recipient from **Query by Recipient** to check if delivery message has been properly replaced.

![alimtalk_09_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_09_201812.png)

### Status of Mass Delivery
  - <b>Waiting:</b> Template file data are yet to be read.
  - <b>Preparing:</b> Loading template file data.
  - <b>Ready:</b> All template file data are loaded and delivery is ready. Select a request (column on the list) and you can find recipient numbers and delivery information from the list at the bottom.
  - <b>Wating for Delivery:</b> Delivery is yet to be processed.
  - <b>Delivering:</b> Delivery is currently underway.
  - <b>Delivery Completed:</b> Request for delivery has been properly completed.
  - <b>Delivery Failed:</b> Error occurred during delivery.
  - <b>Delivery Canceled:</b> User has canceled delivery.


## Template Management

### Register Templates

Click **Register Templates** to register a template.  

![alimtalk_10_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_10_201812.png)

* Only English and numbers up to 10 characters can be registered.  
* Up to 20 characters are allowed for a template name.
* Template can have up to 1,000 characters, including variables and URL,  be it  Korean or English.
* Up to 100 characters are allowed for a button link URL.
* Register a template replacer, like #{replacer}.
* To use a replacer for a button link, make sure to enter http:// or https:// protocol.
  e.g.) http://#{URL} or https://#{URL}
* Mass registration is available in excel. If there is any duplicate or invalid template for mass registration, only normal templates can be registered.
* For template registration, it is updated in the order of  <b>Requested -> Inspection Underway -> Approved/Returned</b>.
* If template is returned, you may request for re-inspection via <b>Register Inquiries</b> and <b>Modify</b>.
* Returned templates can be re-registered after **deleted**.
* For more details, see [[Template Inspection Guide](https://www.bizmsg.kr/collected_statics/assets_landing/doc/alimtalk_template_guide.pdf)].

#### Modify Templates

* 승인된 템플릿을 수정한 후 검수가 완료되면, 기존 템플릿 내용이 수정한 내용으로 대체됩니다.

#### Delete Templates

* Only templates in the <b><span style="color:red">Requested/Returned</span></b> status can be deleted.

#### Register Inquiries of Templates

* You may register inquiries of templates which are only in the <b><span style="color:red">Inspection Underway/Returned</span></b> status.
* Inquiries that are registered are added to inspection results, which shall be confirmed by an inspector at Kakaotalk.
* Inspection result includes inquiries on the usage or reasons of returning a template.

## Statistics
### Query Statistics

Statistics are available by delivery request period, Plus Friend, or template.
Request of delivery, or successful, failed, or replaced delivery can be found on graph or table.

![alimtalk_11_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_11_201812.png)
