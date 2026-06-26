---
sidebar_label: 채널
description: "채널"
---

# 채널

채널을 생성 및 삭제하거나 채널 목록을 조회할 수 있습니다.

## 채널 타입

뒤끝 채팅은 3가지의 채널 타입을 제공합니다.  
오픈 채널, 프라이빗 채널, (베이스의 길드 기능을 사용하는 경우에 유용한) 길드 채널로 타입이 구분되어 있으며  
채널 그룹, 채널 이름, 채널 번호 속성을 가집니다.

:::info
현재 Platform API는 오픈 채널에 대한 기능만 지원합니다.
:::  

아래는 API 사용 시 채널 타입(type)으로 넣어야 하는 값입니다.

| Channel Type  | Value           |
| :------------ | :--------------- |
| 오픈 채널      | 1                |
| 길드 채널      | 2                |
| 프라이빗 채널   | 3               |

## 오픈 채널

전체 채널, 언어별 채널 등 인원이 유동적으로 변하는 경우에 적합한 타입입니다.  
인원이 많아짐에 따라 자동으로 채널 번호가 확장되고,  
자동으로 입장이 처리되며 Fallback 채널을 지정할 수 있습니다.

### 오픈 채널 그룹 리스트 가져오기

#### HTTP request

```js
GET https://platformapi.thebackend.io/chat/v1/open_channel_groups
```

#### Responses

```json title="Status: 200 OK"
[
  "global"
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

### 오픈 채널 리스트 가져오기

#### HTTP request

```js
GET https://platformapi.thebackend.io/chat/v1/open_channels
```

#### Parameters

| Parameter name  | Type           | Description           |
| :------------ | :--------------- |:--------------- |
| channel_group     | string                | 오픈 채널 그룹                |

```js title="Query string example"
?channel_group=global
```

#### Responses

```json title="Status: 200 OK"
[
  {
    "channel_name": "server-1",
    "channel_id": 1,
    "max_count": 50,
    "is_default": true,
    "is_autoscaling": true,
    "players": [{"gamer_id": "1234", "gamer_name": "a1234"}],
    "last_chat_time": "2024-06-17 09:50:23",
    "create_at": "2024-06-17 09:47:11",
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

### 오픈 채널 생성하기

#### HTTP request

```js
PUT https://platformapi.thebackend.io/chat/v1/open_channels
```

#### Request body

| Property name  | Type           | Description           |
| :------------ | :--------------- |:--------------- |
| channel_group     | string                | 오픈 채널 그룹                |
| channel_name     | string                | 오픈 채널 이름                |
| max_count     | number                | 최대 인원                |
| is_default     | bool                | Fallback 여부                |

```json
{
  "channel_group": "global",
  "channel_name": "server-1",
  "max_count": 50,
  "is_default": true
}
```

#### Responses

```json title="Status: 200 OK"
{
  "channel_group": "global",
  "channel_name": "server-1",
  "channel_id": 1,
  "max_count": 50,
  "is_default": true,
  "create_at": "2024-06-21 07:34:59"
}
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

### 오픈 채널 삭제하기

#### HTTP request

```js
DELETE https://platformapi.thebackend.io/chat/v1/open_channels
```

#### Request body

| Property name  | Type           | Description           |
| :------------ | :--------------- |:--------------- |
| channel_group     | string                | 오픈 채널 그룹                |
| channel_name     | string                | 오픈 채널 이름                |

```json
{
  "channel_group": "global",
  "channel_name": "server-1"
}
```

#### Responses

```json title="Status: 200 OK"
[
  {
    "channel_group": "global",
    "channel_name": "server-1",
    "channel_id": 1
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
