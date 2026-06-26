---
sidebar_label: 신고
---

# 신고

신고 목록을 조회할 수 있습니다.

### 신고 목록 가져오기

#### HTTP request

```js
GET https://platformapi.thebackend.io/chat/v1/report_list
```

#### Responses

```json title="Status: 200 OK"
[
  {
    "report_id":       1,
    "to_gamer_id":     "0",
    "to_gamer_name":   "a0",
    "from_gamer_id":   "1",
    "from_gamer_name": "a1",
    "message":         "XXXXXXXXXXXXXXXXXX",
    "keyword":         "INAPPROPRIATE",
    "reason":          "비속어 및 성적 발언",
    "status":          1,
    "value":           0,
    "update_at":       "2024-06-17 09:50:23",
    "create_at":       "2024-06-17 09:50:23",
  }
]
```

#### Error

```json title="Status: 400 Bad Request"
{
  "message": "There is a problem with the user credentials.",
  "code": 400701,
  "error": true
}
```
