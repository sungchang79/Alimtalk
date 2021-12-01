## Notification > KakaoTalk Bizmessage > Release Notes
### 2021.12.02.
* [API] 알림톡 템플릿 문의하기 API 변경
    * 카카오 API 스펙 변경으로 인해, 반려 상태의 템플릿 문의 시 검수중 상태로 변경됩니다.
* [API] 템플릿 리스트 조회 API 버그 수정
    * categoryCode 필드 값을 정상적으로 응답하도록 수정하였습니다.

### 2021.10.26.
* [API] 알림톡/친구톡 대량 발송 조회 API 추가
    * 알림톡/친구톡 대량 발송 조회 API가 추가되었습니다.
* [API] 통계 API 추가
    * 통계 API가 추가되었습니다.
* [API] chatExtra, chatEvent 필드가 추가
    * 메시지 발송 시, BC(상담톡 전환) 타입 버튼에 chatExtra 필드가 추가되었습니다.
    * 메시지 발송 시, BT(봇 전환) 타입 버튼에 chatExtra, chatEvent 필드가 추가되었습니다.
* [API] 웹 링크 타입 버튼에 아웃링크 기능
    * 버튼 클릭 시, 인앱 브라우져가 아닌 모바일 기기의 브라우져로 열 수 있는 기능
    * 메시지 발송 시, target 필드가 추가되어, 아웃링크 기능이 추가되었습니다.
* [API] 앱 링크 타입 버튼 linkMo, linkPc 필드 추가
    * 앱 링크 타입 버튼 linkMo, linkPc 필드 추가되었습니다.

### 2021.08.24.
* [Console] 이미지 알림톡 기능
    * 이미지 알림톡 기능이 추가되었습니다.

### 2021.07.27.
* [Console] 신규 통계 기능
    * 신규 통계가 추가되어, 고도화 되었습니다.
    * (구)통계 서비스는 2021년 7월 31일까지 수집되며, 2021년 12월 31일 종료될 예정입니다.

### 2021.06.29.
* [Console] 발송 결과 업데이트 시, 웹훅 기능
    * 발송 결과 업데이트 시, 웹훅 기능이 추가되었습니다.

### 2021.05.25.
* [Console] 카카오 템플릿 코드 필드 추가
    * 카카오에 실제 등록되는 템플릿 코드 필드가 추가되었습니다.
* [Console] 템플릿 상태/문의 내용 변경 시, 웹훅 기능
    * 템플릿 상태/문의 내용 변경 시, 웹훅 기능이 추가되었습니다.
* [API] 발송 시, price, currencyType 필드 추가
    * 알림톡 광고 모먼트 필드인 price, currencyType 필드가 추가되었습니다.
    * 알림톡 발송 API 및 단건 조회 API messageOption 필드가 추가되었습니다.

### April 27, 2021
* [Console] Added a feature to register the same sender's profile
    * Improved the system so that the same sender's profile can be registered for other projects.
    * Even though it is the same sender's profile, its data such as template/sender profile per project and profile group/send history is independently treated.

### March 23, 2021
* [Console] Added a sender profile group feature
    * Templates evaluated and approved as a group can be used by the sender profile belonging to the group.
    * It is useful when the same template is shared among multiple sender profiles.
* [Console] Added a feature to delete sender profiles
    * Added a feature that deletes sender profiles regardless of their status.

### January 26, 2021
* [Console] Added a feature to back up the Buddytalk dispatch results.
    * Added a feature to back up the Buddytalk dispatch results.
    * An Excel file containing the dispatch results can be created based on the search conditions used on console.
    * Dispatch result files are deleted in 7 days.

### November 24, 2020
* [API] Alimtalk template category code added
    * Category Code field added for registration or modification of Alimtalk template.
    * The template with a category entered is screened first.
* [Console] Backup function of Alimtalk sent list data more than 180 days old added
    * A function that adds a backup file to customers' object storage or AWS S3 for a sent list (normal/batch) more than 180 days old has been added.
    * Backup settings can be set in the **Send Settings** tab.
* [API] Alimtalk template code limit changed
    * Changed to allow the following in the Alimtalk templateCode field: (alphabet letters, numbers, -, _).

### October 27, 2020
* [API] Changed the fields of the Alimtalk that are exposed to/hidden from PC
    * The pcFlag field has been changed to securityFlag field. (Default: false.)
    * A field used to show whether there is a security template. It must be configured for security messages like OTP.
    * If it is true, the message text will not be displayed on any devices except the main device.
* [API] Changed the type of the Add Channel button for Alimtalk
    * Changed the type of the **Add Channel** button from CA to AC.
    * The **Add Channel** button can be registered only when it is on top of the buttons list or it is the only button available.
      *The name of the **Add Channel** button can only be 'Add Channel.'
    * Buttons of which type is changed to AC will be displayed as a new button with the highlight effect and new icon.
* [Console] Added a feature to back up the dispatch results of Alimtalk.
    * Added a feature to back up the dispatch results of Alimtalk.
    * An Excel file containing the dispatch results can be created based on the search conditions used on console.
    * Dispatch result files are deleted in 7 days.
* [Console] Improved the detailed information modal for Buddytalk dispatch results
    * The detailed report of Buddytalk dispatch results now provides additional information such as the presence of ads and the result of alternative dispatch request.

### August 25, 2020
* [API] Show/Not Show Alimtalk on PC
    * Added the feature of selecting Show/Not Show on PC, when registering a template
* [Console] Supports Excel Files for Bulk Delivery
    * Allows excel extension for sending bulk messages, or for uploading recipients' file

### July 28, 2020
* [API] Template Emphasizing Alimtalk Messages  
    * Officially added as a feature, with CBT closed.
* [API] More Types for Alimtalk Template Messages  
    * Expanded types for Alimtalk template messages (BA: Basic, EX: Extra Information, AD: Ads Inclded, MI: Mixed Purposes)
* [API] Query of Attachments for Alimitalk Templates
    * Added the feature of querying on Alimtalk templates with files attached

### June 23, 2020
* [API] Allowed Alimtalk Emphasized template
    * It has been changed to allow emphasized template for Register Template API

### May 26, 2020
* [API] Friendtalk in Wide Images
    * Added the feature of uploading and sending friendtalk messages in wide images.
* [Console] Delete Plus Friends with Unregistered Tokens
    * Added the feature of deleting Plus Friends with unregistered tokens

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

### 2019.07.30
* [Console] Field Added for Result Code of Alternative SMS Delivery Request
    - To query details of alternative delivery message, result code of SMS request has been added.
* [System] Server Replacement for Service Stabilization

### 2019.06.27
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


### April 30, 2019
* [Console] Rolled back the business verification method used when adding a Plus friend
    - Reverted the way to verify a Plus friend's business to the previous method because the previous method took too much time to verify.

### April 23, 2019
* [Console] Added a feature to be used to specify <b>alternative dispatch and split dispatch </b>when mass sending Alimtalk messages
    - Added a feature that specifies the fields related to alternative dispatch, including the alternative dispatch content/sender number/whether to apply alternative dispatch fields.
    - Added a feature of split dispatching over a specific number of times in a specific interval.
* [Console] Separated the <b>alternative dispatch settings</b> of Alimtalk/Buddytalk
    - Added a feature that configures the alternative dispatch for each Plus friend on Alimtalk/Buddytalk.<br>For example,) when only Alimtalk is set to dispatch alternatively for the same Plus friend, Buddytalk will not use the alternative dispatch even if it fails to dispatch.
* [Console] Introduced the <b>business verification feature</b> when adding a Plus friend
    - Only the friends whose <b>business is verified</b> can now be added as Plus friends. [[Related notice:](https://center-pf.kakao.com/notices/311)]
* [Console] Added the Like search feature in the template selection modal window
    - Added the Like search feature to the template selection modal window so that templates can easily be selected. (Use template code and name to search templates)
* [API] Improved the system so that advertisement Buddytalk and <b>advertisement SMS API</b> can be used for alternative dispatch
    - Improved the system so that advertisement SMS API can be used as an alternative when advertisement Buddytalk dispatch fails.
    - For alternative dispatch to work, an 080 opt-out number must be registered when configuring the advertisement Buddytalk alternative dispatch.
* [API] Advanced Buddytalk's alternative dispatch API
    - Added the alternative dispatch title field for dispatching.
    - The title/content/dispatch number/080 opt-out number/whether to use alternative dispatch can be selected using the alternative dispatch field.
* [API] Improved the system so that it will not store the Alimtalk/Buddytalk dispatch failure messages
    - Improved the system so that it will not store dispatch recipient's length limit, invalid receiver number and other request error messages.
    - API response field sendResults can be used to check if the request succeeded/failed.

### March 26, 2019
* [Console] Added the Alimtalk preview UI
    - Added the UI for Alimtalk inbox screen preview.
* [API] Added the Auth API for sending an Alimtalk for verification
    - The send pool has been separated as the Auth API for sending a verification Alimtalk.

### February 26, 2019
* [Console] Fixed the bug in which dispatching fails when mass sending Alimtalk messages
    - Fixed a bug in which dispatch would fail due to some invalid recipient numbers.
* [Console] Fixed a bug in which batch dispatch recipient numbers would be masked when mass sending Buddytalk messages.
    - Fixed a bug in which recipient numbers are masked when a Buddytalk message is dispatched to a large number of recipients and their details are looked up.

### January 29, 2019
* [API] Added Buddytalk v1.2 API
    - Added the sender/receiver grouping key field when sending a message.
    - Added the <b>request success/failure</b> field per recipient in the dispatch response field.
* [API] Added an API that views Buddytalk dispatch result update
    - Added a new API that views the results based on the result-updated time.
* [Console] Advanced the Alimtalk dispatch screen
    - Improved the system so that messages can be sent to multiple recipients.
    - Improved the system so that users can now schedule a dispatch.
    - Improved the system so that uses can now select the body text for alternative dispatch.
* [Console] Added a feature of mass sending Buddytalk messages
    - Users can now send a Buddytalk message to a large number of recipients using a csv file.
* [Console] Added a feature that views Buddytalk messages that were sent to a large number of recipients
    - Users can now look up messages that they sent to a large number of recipients from the Buddytalk bulk dispatch tab.
* [Console] Advanced the Alimtalk template edit feature
    - Previously, only rejected templates could be edited. Approved templates can also be edited now.
    - When an approved template is edited, it will be reviewed again. After that, its content will <b>replace the content of the existing template</b>.
    - While the template is in the review stage, the previously approved template can be normally dispatched.
* [Console] Added a feature that is used to mask recipient numbers
    - Added a feature that is used to mask recipient numbers for projects that were individually asked to do so.
* [Console] Fixed a bug that would occur while counting the number of words in the Alimtalk template body
    - Fixed a bug in which a blank space is counted as 2 characters in the template body.

### December 4, 2018
* [API] Added Alimtalk v1.2 API
    - Added the sender/receiver grouping key field when sending a message.
    - Added the <b>request success/failure</b> field per recipient in the dispatch response field.
* [API] Added an API that views the updated Alimtalk dispatch result
    - Added a new API that views the results based on the result-updated time.
* [API] Added the alternative dispatch number field when dispatching a message
    - Added a feature that specifies an alternative sender number via the resendSendNo field when sending a message.
* [API] Improved the system so that it will retain sent messages up to 90 days
    - Improved the system so that the data created more than 90 days ago will not be displayed when retrieving data.
* [Console] Added the detailed status of a Plus friend
    - Added the TOAST Plus friend, Kakao Plus friend, Kakao Plus friend profile status fields to the Plus friend view screen.

### November 13, 2018
* [API] Advanced Alimtalk dispatch API alternative dispatch
    - Added the alternative dispatch title field for dispatching.
    - The name of LMS can be specified when alternatively dispatching LMS using the alternative dispatch title field.
    - Changed to use the alternative dispatch when Template context mismatch (-3010) or Template button dispatch (-3011) request fails. Messages that don't need alternative dispatch can be controlled using the isResend field.
* [API] Fixed a bug regarding scheduled dispatch
    - Fixed a bug in which scheduled dispatches could not normally be canceled due to an error in the date data of the request ID when sending a scheduled message.

### October 23, 2018
* [API] Added a feature that schedules Alimtalk dispatch API
    - A message can be dispatched at any time using the schedule feature.
    - The scheduled message can be canceled at any time before it is sent.
    - For more information, see [[Alimtalk dispatch API](./alimtalk-api-guide/#_3)].
* [API] Advanced Alimtalk dispatch API alternative dispatch
    - Added the alternative dispatch type (SMS/LMS), whether to alternative dispatch (true/false), alternative dispatch content fields when dispatching.
    - The alternative dispatch can be used at scale using the fields.
* [API] Added the API related to Plus friends and templates
    - Added the APIs for Plus friend category view, business license upload, registration, token authentication, and list view.
    - Added the API for template registration, edit, delete, and query.
* [Console] Added a feature that searches for Likes to the template code
    - Added the Like search feature when viewing templates on console.

### August 28, 2018
* [Console] Improved the dispatch result view
    - Improved the system so that template code can be manually entered when viewing Alimtalk dispatch results.

### July 24, 2018
* [Console] Changed the name of a service
    - The service name of Alimtalk has been changed to KakaoTalkBizmessage.
* [Console] Added a feature to Buddytalk
    - Buddytalk targets users who became friends with Buddytalk, and ad message can be dispatched using this.
    - The message dispatch, view, image management, statistics features are provided on console.
    - For more information, see [[Buddytalk overview ](./friendtalk-overview/)].
* [API] Added the Buddytalk API
    - The Buddytalk API supports the message dispatch, list view, single item view, image management features.
    - For more information, see [[Buddytalk API guide](./friendtalk-api-guide/)].
* [API] Added a limit to the number of Alimtalk and Buddytalk messages sent
    - Added a feature to limit the number of messages to 1,000 a day, which are sent to the Plus friends who became friends since the maintenance on July 24.
    - Dispatch requests exceeding this limit will fail.
* [API] Edited the re-dispatch feature after a failed dispatch
    - The body of the re-dispatch was changed as below:
    ```
    Dispatch body text
    - Web link button name: Web link
    - Web link button name 2: Web link 2
    ...
    ```

### June 26, 2018
#### Feature Updates
* [Console] Added the request field when adding a Plus friend
    - To comply with the latest Kakao issuance procedure, a business license number and business category are needed when adding a Plus friend.
#### Bug Fixes
* [Console] Changed the statistics chart to a version where bugs are fixed
    - Changed the statistics chart to a version where the bugs related to exporting .xls files on IE are fixed.

### May 29, 2018
#### More Features
* [API] Added the API that views a single dispatch result
    - Added the API that views a specific dispatch result.
    - For more information, see [[Alimtalk API guide](./alimtalk-api-guide/#_14)].
* [API] Added dispatch result list view API v1.1
    - Added v1.1 API as the recipientSeq (Recipient sequence) field is added to response.
#### Bug Fixes
* [API] Fixed a bug related to the substitution dispatch API
    - Fixed a bug in which the templateParameter field of Request Body would be recognized as a required field.

### April 24, 2018
#### More Features
* [Console] Added a feature that is used for template chat bubble
    - Added a feature for multiple button template.
    - Added new button types: (delivery view, web link, app link, bot keyword, and message forwarding)
* [API] Added the API for viewing templates
    - Added the API that views templates.
    - For more information, see [[ Alimtalk API guide ](./alimtalk-api-guide/#_46)].
#### Feature Updates
* [API] Edited the re-dispatch feature after a failed dispatch
    - Fixed the system so that messages are sent as SMS or LMS according to their length.
    - Changed the body text of re-dispatched message: (Body + web link button name + button link)

### March 22, 2018
#### More Features
* [Console] Added a feature that is used to edit or delete templates and register queries
    - Added a feature that is used to edit or delete templates.
    - Added a feature that is used to register a query to reviewer.
    - For more information, see [[Alimtalk console user guide ](./alimtalk-console-guide/#_12)].
* [API] Added the API that is used to dispatch full body text
    - Added the API that is used to send a message in full text, not in substituted data.
    - For more information, see [[ Alimtalk API guide ](./alimtalk-api-guide/#_4)].

### February 22, 2018
#### More Features
* [Console] Added a feature that is used to send messages to a large number of recipients
    - Users can now send a Buddytalk message to a large number of recipients using a csv file.

### January 25, 2018
#### Feature Updates
* [Console] Improved template registration
    - Added a feature that is used to remove spaces in front and back of a button name.
    - Edited the length limit: (Template code: 10 characters -> 20 characters, button name: 10 characters -> 14 characters)
    - Modified the system so that user can manually enter the button name when registering a delivery view template.
#### More Features
* [API] Added the API that views dispatch result

### November 23, 2017
#### More Features
* [Console] Register multiple Plus friends
    - Previously, only 1 Plus friend could be added at a time. Now, multiple Plus friends can be added at a time.
* [Console] When a template is rejected, provides the reason.
* [API] Added the dispatch API field as now multiple Plus friends can be added at a time
    - Added 12 plusFriendId fields to requestBody.
    - The message will be sent to the Plus friend ID who was added first when plusFriendId is left empty.

### August 24, 2017
#### More Features
* [Console] Provided the Alimtalk dispatch statistics screen
    - Provides the by date/by time/by day of a week statistics screen.
    - Can look up by dispatched date and template.
#### Feature Updates
* [Console] Changed the URL verification when adding a free button template
    - http:// or https:// must be included when adding a URL to a free button -> Like #{url}, the template substituter can now be added.
    - If it is not a template substituter in the form of #{url}, the http:// or https:// verification will be maintained.
* [API] Modified the error response message for content-type
    - Modified the failure response message if it's not content-type: application/json in the request header.

### July 20, 2017
#### More Features
    * [Console] Added Alimtalk dispatch result view
        - The message dispatch result can be viewed with conditions such as date of dispatch, recipient number, and template.

### June 22, 2017
#### Feature Updates
* [Console] Added the dispatch profile management page
* [Console] Added the test dispatch page
* [Console] Added the added template view page

#### More Features
* [Console] Added the dispatch failure setting
    - This is a feature that is used to send the message via LMS when Alimtalk dispatch fails
* [Console] Added a feature that is used to substitute the free button type of a template
    - For template free button type, the button link can be added as a substituter (e.g. buttonURL: #{url})

### May 25, 2017
#### Feature Updates
* [Console] Improved the main page markup

### April 20, 2017
#### New Product Release
    * Alimtalk is a product based on mobile phones with which users can send informative messages such as delivery message, schedule notification, and others without adding the recipient as a friend.
    * It provides RESTful API for users to easily link it to apps.
