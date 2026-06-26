---
sidebar_label: 유저 제재
description: "유저 제재"
---

# 유저 제재

제재된 유저의 목록을 조회하거나,  
특정 유저에게 채팅 금지를 적용하고 해제할 수 있습니다.

### 제재된 유저 목록 가져오기

#### HTTP request

```js
GET https://platformapi.thebackend.io/chat/v1/block_user_list
```

#### Responses

```json title="Status: 200 OK"
[
  {
    "gamer_id": "1234",
    "count": 1,
    "start_time": "2024-06-20T12:06:34Z",
    "end_time": "2024-06-21T12:06:34Z"
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

### 특정 유저에게 채팅 금지 제재하기

#### HTTP request

```js
PUT https://platformapi.thebackend.io/chat/v1/block_user_chat
```

#### Request body

| Property name  | Type           | Description           |
| :------------ | :--------------- |:--------------- |
| gamer_id     | string                | 유저의 고유 아이디               |
| days     | number                | 제재 일수                |

```json
{
  "gamer_id": "1234",
  "days": 3
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

------

### 특정 유저의 채팅 금지 해제하기

#### HTTP request

```js
DELETE https://platformapi.thebackend.io/chat/v1/block_user_chat
```

#### Request body

| Property name  | Type           | Description           |
| :------------ | :--------------- |:--------------- |
| gamer_id     | string                | 유저의 고유 아이디               |

```json
{
  "gamer_id": "1234",
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

