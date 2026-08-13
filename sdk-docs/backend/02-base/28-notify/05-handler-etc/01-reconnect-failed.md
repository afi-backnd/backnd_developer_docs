---
sidebar_label: "재연결 실패 핸들러"
description: "OnReconnectFailed"
---

# OnReconnectFailed

public OnReconnectFailed **OnReconnectFailed**;

## 설명
네트워크 문제로 연결이 종료되어 재연결을 시도했을 때, 최종적으로 재연결이 실패하면 발생하는 이벤트 핸들러입니다.  
해당 이벤트가 발생하면 더 이상 재연결을 시도하지 않으므로 직접 Connect를 호출해야만 서버에 연결할 수 있습니다.  
재연결 시도 횟수는 Connect를 호출할 때, reconnectionAttempts 값으로 지정할 수 있습니다.  

## Example
```js
Backend.Notification.OnReconnectFailed = () =>
{
   Debug.Log($"실시간 알림 [OnReconnectFailed(재접속 실패)]");

   // 이 후, 처리 코드 작성.
};
```
