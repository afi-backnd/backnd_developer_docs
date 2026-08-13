---
sidebar_label: "친구 접속 핸들러"
description: "친구 접속 핸들러"
---

# 친구 접속 핸들러

친구로 등록되어 있는 유저가 실시간 알림 서버에 접속/접속 해제했을 경우 호출되는 핸들러입니다.  
접속/접속 해제 시 닉네임과 gamerIndate를 획득하실 수 있습니다.  

> 실시간 알림 서버 접속은 Backend.Notification.Connect() 함수를 통해 하실 수 있습니다.  

실시간 알림이 발생하는 이벤트는 다음과 같습니다.  

- 친구로 등록된 유저 실시간 알림 서버 접속
- 친구로 등록된 유저 실시간 알림 서버 접속 해제

---

# OnFriendConnected

public OnFriendConnect **OnFriendConnected**;

## 설명

친구로 등록된 유저가 실시간 알림 서버에 접속했을 때 작동하는 핸들러입니다.  

## Example

```js
Backend.Notification.OnFriendConnected = (string inDate, string nickname) => {
        Debug.Log(nickname + "님이 연결되었습니다");
};

```

### Response Cases

| Value    | Type   | Description                                  |
| :------- | :----- | :------------------------------------------- |
| inDate   | string | 실시간 알림 서버에 접속한 친구의 gamerInDate |
| nickname | string | 실시간 알림 서버에 접속한 친구의 닉네임      |

---

# OnFriendDisconnected

public OnFriendConnect OnFriendDisconnected;

## 설명

친구로 등록된 유저가 실시간 알림 서버를 접속 해제했을 때 작동하는 핸들러입니다.  

## Example

```js
Backend.Notification.OnFriendDisconnected= (string inDate, string nickname) => {
        Debug.Log(nickname + "님이 종료했습니다");
};
```

### Response Cases

| Value    | Type   | Description                                       |
| :------- | :----- | :------------------------------------------------ |
| inDate   | string | 실시간 알림 서버 접속이 끊어진 친구의 gamerInDate |
| nickname | string | 실시간 알림 서버 접속이 끊어진 친구의 닉네임      |
