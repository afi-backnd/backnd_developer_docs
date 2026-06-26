---
sidebar_label: 대기방 퇴장 요청
---

# LeaveMatchRoom

public void **LeaveMatchRoom**();

## 설명

대기방에서 퇴장합니다.  

- 매칭을 진행 중에는 퇴장할 수 없습니다.  
- 방장인 유저가 대기방에서 퇴장하면 해당 대기방은 삭제됩니다.  
  > 대기방이 삭제되면 대기방에 참여 중인 모든 유저가 대기방에서 퇴장하게 됩니다.  

## Example

```js
Backend.Match.LeaveMatchRoom();
```
