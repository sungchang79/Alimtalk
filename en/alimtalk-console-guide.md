## Notification > KakaoTalk Bizmessage > AlimTalk > Console Guide

## General Delivery

To send AlimTalk, go to **NHN Cloud Console** and **Notification > KakaoTalk Bizmessage > AlimTalk**.

![alimtalk_01_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_01_201812.png)

Select a Plus Friend and load a registered template.
You can check your Plus Friends on **Notification > KakaoTalk Bizmessage > Plus Friend Management**.
You can manage templates on the **Notification > KakaoTalk Bizmessage > AlimTalk > Manage Templates**.

Enter replacement key of the template from **General Delivery** at the bottom, and enter recipient number.

After that, click **Send** on the right to send AlimTalk immediately.

## Mass Delivery

### Request Mass Delivery

Select **Mass Delivery** at the bottom.

![alimtalk_02_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_02_201812.png)

![alimtalk_02-1_201904.png](https://static.toastoven.net/prod_alimtalk/alimtalk_02-1_201904.png)

* Select a template and click **Download Templates** to download CSV, XLSX file which includes a template replacer. Sending fails if a template replacer is not included.  
* If you save a CSV file after opening it in Excel, Korean characters may not be saved properly, so it is recommended to check whether the replacement has been performed normally by sending it after inspection.
* Only CSV and XLSX files with the maximum size of 20 MB can be uploaded, and the maximum number of recipients you can send to is 1,000,000.
* Using split delivery, you can specify the number of splits and delivery interval.

![alimtalk_03_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_03_201812.png)

* Click **Send**, and select either **Proceed after Inspection** or **Send Immediately**.

![alimtalk_04_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_04_201812.png)

## Query Delivery Results

![alimtalk_05_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_05_201812.png)

You can query the results of delivery through general delivery.
You can query delivery results by selecting only one of **Date and Time of Registration**, or **Date and Time of Delivery**, which is a required condition.
To check details of each delivery message, click the item on the search results and **Query Details** appears.  

![alimtalk_06_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_06_201812.png)

### Cancel Delivery

Among general delivery, the scheduled delivery which is requested for a future date can be canceled.
If you query a scheduled delivery request, you can find a checkbox on the left of a request ID.
Checkboxes are available only for scheduled delivery requests that are not canceled.
To cancel a request, select the checkbox of the request you want to cancel, and press **Cancel Selected Schedule**.
You can also select and cancel the whole list using the checkbox on top of the list.

## Query Mass Delivery

### Send/Cancel

If you select **Proceed after Inspection** when performing mass delivery of AlimTalk, you can send or cancel AlimTalk in the **Query Mass Delivery** tab.

![alimtalk_07_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_07_201812.png)

* Click **Send** to proceed with mass delivery. It is recommended to click an item from the search results and check its replacement value on the **Query Details** window.  
* If you click the **Cancel** button when the status is Delivering, only some of the recipients may receive messages.

* Click a particular row of search result from the Query Mass Delivery tab, and recipients will show up.  

![alimtalk_08_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_08_201812.png)

* Select a recipient from **Query by Recipient** to check if delivery message has been properly replaced.

![alimtalk_09_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_09_201812.png)

### Status of Mass Delivery
  - <b>Waiting:</b> Template file data are yet to be read.
  - <b>Preparing:</b> Loading template file data.
  - <b>Ready:</b> All template file data are loaded and delivery is ready. Select a request (column on the list) and you can find recipient numbers and delivery information from the list at the bottom.
  - <b>Waiting for Delivery:</b> Delivery is yet to be processed.
  - <b>Delivering:</b> Delivery is currently underway.
  - <b>Delivery Completed:</b> Request for delivery has been properly completed.
  - <b>Delivery Failed:</b> Error occurred during delivery.
  - <b>Delivery Canceled:</b> User has canceled delivery.


## Manage Templates

### Register Templates

Click **Register Templates** to register a template.  

![alimtalk_10_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_10_201812.png)

* Only English and numbers up to 10 characters can be registered.  
* Up to 150 characters are allowed for a template name.
* Template can have up to 1,000 characters, including variables and URL,  be it  Korean or English.
* Up to 500 characters are allowed for a button link URL.
* Register a template replacer, like #{replacer}.
* To use a replacer for a button link, make sure to enter http:// or https:// protocol. e.g.) http://#{URL} or https://#{URL}
* You can perform mass registration using the Excel file. If there is any duplicate or invalid template while performing mass registration, only normal templates are registered.
* While registering templates, the status is updated in the order of <b>Requested -> Inspection Underway -> Approved/Rejected</b>.
* If a template is rejected, you may request for re-inspection via <b>Register Inquiries</b> and <b>Modify</b>.
* Rejected templates can be re-registered after **deleted**.
* For more details, see [[Template Inspection Guide](https://www.bizmsg.kr/collected_statics/assets_landing/doc/alimtalk_template_guide.pdf)].

#### Modify Templates

* Once approved template is modified and inspected, the content of existing template shall be replaced by modified message.

#### Delete Templates

* Only templates in the <b><span style="color:red">Requested/Rejected</span></b> status can be deleted.

#### Register Inquiries of Templates

* You may register inquiries of templates which are only in the <b><span style="color:red">Inspection Underway/Rejected</span></b> status.
* Inquiries that are registered are added to inspection results, which shall be confirmed by an inspector at KakaoTalk.
* Inspection result includes inquiries on the usage or reasons of returning a template.

## Manage Alternative Delivery

* You can set the alternative delivery for Plus Friend according to each message type of Alimktalk/FriendTalk.
* Only messages for Plus Friends with alternative delivery set will be resent with LMS or SMS.
* If you edit the SMS app key, all Plus Friends' alternative delivery settings will be initialized.
* Because the alternative delivery is performed with SMS service, fields must be entered according to the specification of the SMS service's delivery API. (sender number registered in SMS service, various field length limits, etc.)
* Subject or content of an alternative delivery that exceeds the byte limit of the specified alternative delivery type may be truncated and sent. (See [[SMS cautions](https://docs.toast.com/en/Notification/SMS/en/api-guide/#_1)])
* Alternative delivery content is sent based on EUC-KR, and unsupported emoticons will fail to be sent.

![plusfriend_03_201812.png](https://static.toastoven.net/prod_alimtalk/plusfriend_03_201904.png)

## Statistics
### Query Statistics

![alimtalk_11_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_11_201812.png)

You can check the statistics by delivery request period, Plus Friend, or template.
Request of delivery, or successful, failed, or replaced delivery can be found on graph or table.
