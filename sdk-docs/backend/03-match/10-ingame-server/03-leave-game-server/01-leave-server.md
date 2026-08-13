---
sidebar_label: "인게임 서버 접속 종료 요청"
description: "LeaveGameServer"
---

# LeaveGameServer

public void **LeaveGameServer**();

## 설명

인게임 서버와 접속을 종료를 시도합니다.  

- 이 함수를 명시적으로 호출하지 않아도 **게임방에서 게임이 종료되고 결과 종합까지 완료되었다면 자동으로 인게임 서버와 접속이 종료**됩니다.  

## Example

```js
Backend.Match.LeaveGameServer();
```
