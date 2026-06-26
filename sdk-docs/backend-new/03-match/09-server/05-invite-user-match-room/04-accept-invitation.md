---
sidebar_label: 초대 수락 요청
---

# AcceptInvitation

public void **AcceptInvitation**(SessionId **roomId**, string **roomToken**);

## 파라미터

| Value     | Type      | Description                                          |
| :-------- | :-------- | :--------------------------------------------------- |
| roomId    | SessionId | OnMatchMakingRoomSomeoneInvited에서 리턴된 RoomID    |
| roomToken | string    | OnMatchMakingRoomSomeoneInvited에서 리턴된 RoomToken |

## 설명

유저가 보낸 초대를 수락합니다.  
초대를 수락함과 동시에 대기방에 입장됩니다.  

## Example

```js
BackndBase.Match.AcceptInvitation(roomId, roomToken));
```
