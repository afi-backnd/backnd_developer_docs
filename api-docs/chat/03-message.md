---
sidebar_label: 채팅 메시지
---

# 채팅 메시지

특정 채널에 메시지를 보낼 수 있습니다.

## 시스템 메시지

특정 채널에 시스템 메시지를 보낼 수 있습니다.

### 시스템 메시지 보내기

#### HTTP request

```js
POST https://platformapi.thebackend.io/chat/v1/system_message
```

#### Request body

| Property name  | Type           | Description           |
| :------------ | :--------------- |:--------------- |
| channel_type     | number                | [채널 타입](/api-docs/chat/channel)                |
| channel_group     | string                | 채널 그룹                |
| channel_name     | string                |  채널 이름               |
| channel_id     | number                | 채널 번호                |
| message     | string                | 채팅 메시지                |

```json
{
    "channel_type": 1,
    "channel_group": "global",
    "channel_name": "server-1",
    "channel_id": 1,
    "message": "반갑습니다. Platform API 개발자 문서 입니다."
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
