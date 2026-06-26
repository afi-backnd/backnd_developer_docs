---
sidebar_label: 친구 요청 핸들러
---

# 친구 요청 핸들러

비동기로 작업이 진행되며 아래의 이벤트 핸들러로 친구기능 관련 이벤트들이 콜백 됩니다.  
실시간 알림이 발생하는 이벤트는 다음과 같습니다.  

- 친구 요청 도착
- 친구 요청 수락
- 친구 요청 거절

---

# OnReceivedFriendRequest

public OnNotification **OnReceivedFriendRequest**;

## 설명

친구 요청이 도착했을 때 호출되는 핸들러입니다.  

## Example

```js
Backend.Notification.OnReceivedFriendRequest = () => {
  Debug.Log("친구 요청이 도착했습니다!");
};
```

---

# OnAcceptedFriendRequest

public OnNotification **OnAcceptedFriendRequest**;

## 설명

친구 요청이 수락되었을 때 호출되는 핸들러입니다.  

## Example

```js
Backend.Notification.OnAcceptedFriendRequest = () => {
  Debug.Log("친구 요청이 수락되었습니다!");
};
```

---

# OnRejectedFriendRequest

public OnNotification **OnRejectedFriendRequest**;

## 설명

친구 요청이 거절당했을 때 호출되는 핸들러입니다.  

## Example

```js
Backend.Notification.OnRejectedFriendRequest = () => {
  Debug.Log("친구 요청이 거절당했습니다..");
};
```
