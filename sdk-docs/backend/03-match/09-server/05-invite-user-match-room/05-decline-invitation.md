---
sidebar_label: "초대 거절 요청"
description: "DeclineInvitation"
---

# DeclineInvitation

public void **DeclineInvitation**(SessionId **roomId**, string **roomToken**);

## 파라미터

| Value     | Type      | Description                                          |
| :-------- | :-------- | :--------------------------------------------------- |
| roomId    | SessionId | OnMatchMakingRoomSomeoneInvited에서 리턴된 RoomID    |
| roomToken | string    | OnMatchMakingRoomSomeoneInvited에서 리턴된 RoomToken |

## 설명

유저가 보낸 초대를 거절합니다

## Example

```js
Backend.Match.DeclineInvitation(roomId, roomToken));
```
