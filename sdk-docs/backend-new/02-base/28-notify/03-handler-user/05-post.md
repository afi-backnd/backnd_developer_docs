---
sidebar_label: 쪽지/우편 기능 핸들러
description: "쪽지/우편 기능 핸들러"
---

# 쪽지/우편 기능 핸들러

비동기로 작업이 진행되며 아래의 이벤트 핸들러로 쪽지/우편 기능 관련 이벤트들이 콜백 됩니다.  
실시간 알림이 발생하는 이벤트는 다음과 같습니다.  

- 새 쪽지 도착
- 새 유저 우편 도착

---

# OnReceivedMessage

public OnNotification **OnReceivedMessage**;

## 설명

새 쪽지가 도착했을 때 호출되는 핸들러입니다.  

## Example

```js
BackndBase.Notification.OnReceivedMessage = () => {
  Debug.Log("새 쪽지가 도착했습니다!");
};
```

---

# OnReceivedUserPost

public OnNotification **OnReceivedUserPost**;

## 설명

새 유저 우편이 도착했을 때 호출되는 핸들러입니다.  

## Example

```js
BackndBase.Notification.OnReceivedUserPost = () => {
  Debug.Log("새 유저 우편이 도착했습니다!");
};
```
