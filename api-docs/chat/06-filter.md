---
sidebar_label: 비속어
description: "비속어"
---

# 비속어

비속어 표현 목록을 기반으로 유저의 채팅 메시지를 실시간으로 필터링할 수 있습니다.  
API를 통해 비속어 목록을 조회하고 관리하세요.

### 비속어 리스트 가져오기

#### HTTP request

```js
GET https://platformapi.thebackend.io/chat/v1/bad_word_list
```

#### Responses

```json title="Status: 200 OK"
[
  {
    "filters": ["GM", "MASTER"],
    "language": "all"
  },
  {
    "filters": ["지엠", "마스터"],
    "language": "ko-kr"
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

------

### 비속어 목록 수정하기

#### HTTP request

```js
PUT https://platformapi.thebackend.io/chat/v1/bad_word_list
```

#### Request body

| Property name  | Type           | Description           |
| :------------ | :--------------- |:--------------- |
| language     | string                | 처리 해야 할 언어 ([language code list](https://www.localeplanet.com/dotnet/index.html))               |
| filters     | string array                | 비속어 리스트                |

```json
{
  "language": "ko-kr",
  "filters": ["지엠", "마스터"]
}
```

#### Responses

성공하면 Request body로 결과를 반환합니다.

#### Error

```json title="Status: 400 Bad Request"
{
  "message": "There is a problem with the user credentials.",
  "code": 400701,
  "error": true
}
```
