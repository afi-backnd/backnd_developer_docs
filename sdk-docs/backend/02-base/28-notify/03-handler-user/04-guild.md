---
sidebar_label: "길드 기능 핸들러"
description: "길드 기능 핸들러"
---

# 길드 기능 핸들러

비동기로 작업이 진행되며 아래의 이벤트 핸들러로 길드 기능 관련 이벤트들이 콜백 됩니다.  
실시간 알림이 발생하는 이벤트는 다음과 같습니다.  

- 길드 가입 신청 도착(길드 마스터, 부 길드 마스터)
- 길드 가입 신청 수락
- 길드 가입 신청 거절
- 길드에서 추방

> 길드 가입신청 수락/거절 알림은 즉시 가입 방식 길드에는 해당되지 않습니다.  

---

# OnReceivedGuildApplicant

public OnNotification **OnReceivedGuildApplicant**;

## 설명

새 길드 가입 신청이 도착했을 때 호출되는 핸들러입니다.  

## Example

```js
Backend.Notification.OnReceivedGuildApplicant = () => {
  Debug.Log("새 길드 가입 신청이 도착했습니다!");
};
```

---

# OnApprovedGuildJoin

public OnNotification **OnApprovedGuildJoin**;

## 설명

길드 가입 신청이 수락됐을 때 호출되는 핸들러입니다.  

## Example

```js
Backend.Notification.OnApprovedGuildJoin = () => {
  Debug.Log("길드 가입 신청이 수락되었습니다!");
};
```

---

# OnRejectedGuildJoin

public OnNotification **OnRejectedGuildJoin**;

## 설명

길드 가입 신청이 거절되었을 때 호출되는 핸들러입니다.  

## Example

```js
Backend.Notification.OnRejectedGuildJoin = () => {
  Debug.Log("길드 가입 신청이 거절당했습니다..");
};
```

---

# OnGuildMemberExpelled

public OnNotification **OnGuildMemberExpelled**;

## 설명

길드에서 추방됐을 때 호출되는 핸들러입니다.

## Example

```js
Backend.Notification.OnGuildMemberExpelled = () => {
  Debug.Log("길드에서 추방당했습니다..");
};
```
