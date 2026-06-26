---
sidebar_label: 프로젝트 상태 변경 핸들러
---

# OnServerStatusChanged

public OnServerStatusChangeNotification **OnServerStatusChanged**;  

## 파라미터

| Value            | Type             | Description                    |
| ---------------- | ---------------- | ------------------------------ |
| serverStatusType | ServerStatusType | {Online, Offline, Maintenance} |

## 설명

뒤끝 콘솔의 서버 설정 > 프로젝트에서 프로젝트 상태를 변경할 경우 호출되는 핸들러입니다.  
변경된 프로젝트 상태를 확인할 수 있습니다.  

## Example

```js
BackndBase.Notification.OnServerStatusChanged = (ServerStatusType serverStatusType) => {
    Debug.Log(
        $"[OnServerStatusChanged(서버 상태 변경)]\n" +
        $"| ServerStatusType : {serverStatusType}\n"
    );
};
```
