---
sidebar_label: 매칭 서버 접속 종료 요청
---

# LeaveMatchMakingServer

public void **LeaveMatchMakingServer**();

## 설명

매칭 서버와 접속을 종료를 시도합니다.  

> **OnLeaveMatchMakingServer 이벤트를 통해 매칭 서버에 접속이 종료되었음이 호출되어야 매칭 서버와 접속이 종료된 것입니다.**  

## Example

```js
BackndBase.Match.LeaveMatchMakingServer();
```
