---
sidebar_label: 연결 상태 확인
description: "연결 상태 확인"
sidebar_position: 2.5
---

# IsConnected
public bool **IsConnected**;  

## 설명
현재 실시간 알림에 연결되어 있는지 확인합니다.  

## Example
```js
if (!BackndBase.Notification.IsConnected)
{
   BackndBase.Notification.Connect();
}
```

## Return cases
**실시간 알림에 연결된 상태**  
true가 리턴됩니다.  

**실시간 알림에 연결된 상태를 제외한 모든 상태**  
false가 리턴됩니다.  
