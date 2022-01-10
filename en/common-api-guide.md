## Notification > KakaoTalk Bizmessage > Common > API v2.2 Guide

## Statistics

### [API Domain]

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


### Statistics Search - Event Based
* Statistics collected based on the time when the event occurs.
* Statistics are collected based on the following times:
    * Request count (REQUESTED): Time when the scheduled delivery is registered
    * Delivery count (SENT): Time of delivery to the vendor (scheduled delivery time)
    * Success count (RECEIVED): Delivery result successful (receiving time)
    * Failure count (SENT_FAILED): When the delivery request fails or when the delivery result fails
    * Alternative delivery request count (RESENT): Time when the alternative delivery is requested
    * Alternative delivery failure count (RESENT_FAILED): Time when the alternate delivery request fails

### Get Statistics Information

[URL]

|Http method|   URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/stats |

[Path parameter]

| Name |  Type| Description|
|---|---|---|
| appKey | String | Unique appkey |

[Query parameter]

| Name | Type | Max. Length | Required | Description |
|---|---|---|---|---|
| statsType | String | - | Required | Statistics type<br/>NORMAL: basic, MINUTELY: by minute, HOURLY: by hour, DAILY: by day, BY_DAY: by hour, DAY: by day of the week |
| from | String | - | Required | Start date of statistics search<br/>yyyy-MM-dd HH:mm:ss | 
| to | String | - | Required | End date of statistics search<br/>yyyy-MM-dd HH:mm:ss |
| join | Boolean | - | Optional | When retrieving statistics data, set whether to provide the data in tree form |
| extra1s | List<String> | - | Optional | Sub-product type<br/> ALIMTALK, ALIMTALK_AUTH, FRIENDTALK |
| extra2s | List<String> | - | Optional | senderKey |
| eventTypes | List<String> | - | Optional | Event type<br/> REQUESTED, SENT, RECEIVED, SENT_FAILED, RESENT, RESENT_FAILED |
| eventCategory | String | - | Optional | Event list (Currently only `MESSAGE` is supported)<br/> MESSAGE |
| templateCodes | List<String> | - | Optional | Template code list (FriendTalk unsupported) |
| requestIds | List<String> | 5 | Optional | Request ID list |
| statsIds | List<String> | - | Optional | Statistics ID list |
| statsCriteria | List<String> | - | Optional | Statistics criteria<br/>- EVENT: event (default value)<br/>- EXTRA_1,EVENT: sub product type, event<br/>- EXTRA_2,EVENT: senderKey, event |

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

### Get Count per Event

[URL]

|Http method|   URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/stats/total |

[Path parameter]

| Name |  Type| Description|
|---|---|---|
| appKey | String | Unique appkey |

[Query parameter]

| Name | Type | Max. Length | Required | Description |
|---|---|---|---|---|
| statsType | String | - | Required | Statistics type<br/>NORMAL: basic, MINUTELY: by minute, HOURLY: by hour, DAILY: by day, BY_DAY: by hour, DAY: by day of the week |
| from | String | - | Required | Start date of statistics search<br/>yyyy-MM-dd HH:mm:ss | 
| to | String | - | Required | End date of statistics search<br/>yyyy-MM-dd HH:mm:ss |
| join | Boolean | - | Optional | When retrieving statistics data, set whether to provide the data in tree form |
| extra1s | List<String> | - | Optional | Sub-product type<br/> ALIMTALK, ALIMTALK_AUTH, FRIENDTALK |
| extra2s | List<String> | - | Optional | senderKey |
| eventTypes | List<String> | - | Optional | Event type<br/> REQUESTED, SENT, RECEIVED, SENT_FAILED, RESENT, RESENT_FAILED |
| eventCategory | String | - | Optional | Event list (Currently only `MESSAGE` is supported)<br/> MESSAGE |
| templateCodes | List<String> | - | Optional | Template code list (FriendTalk unsupported) |
| requestIds | List<String> | 5 | Optional | Request ID list |
| statsIds | List<String> | - | Optional | Statistics ID list |
| statsCriteria | List<String> | - | Optional | Statistics criteria<br/>- EVENT: event (default value)<br/>- EXTRA_1,EVENT: sub product type, event<br/>- EXTRA_2,EVENT: senderKey, event |

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
