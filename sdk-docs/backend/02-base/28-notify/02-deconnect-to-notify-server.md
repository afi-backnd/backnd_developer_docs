---
sidebar_label: 실시간 알림 서버 해제
---

# Disconnect

public void **Disconnect**();

## 설명

실시간 알림 서버에 연결을 해제합니다.  
앱 종료 시, 필수로 호출해야 합니다.  

## Example

```js
//접속 해제 시 반응하는 핸들러를 설정.  
Backend.Notification.OnDisConnect = (string Reason) => {
    Debug.Log("해제 이유 : " + Reason);
};

//실시간 알림 서버 접속을 해제한다
Backend.Notification.DisConnect();
```

---

# OnDisConnect

public OnDisconnect OnDisConnect;

## 파라미터

| Value  | Type   | Description      |
| ------ | ------ | ---------------- |
| Reason | string | 연결 해제된 이유 |

## 설명

Disconncect 가 완료된 후 발생하는 이벤트의 핸들러입니다. Reason을 통해서 연결 해제의 원인이 반환됩니다.  
또한, 비정상적으로 접속이 끊길 시, 해당 핸들러가 호출됩니다.  

## Example

```js
Backend.Notification.OnDisConnect = (string Reason) => {
           Debug.Log("실시간 알림 서버 접속이 해제되었습니다!");
           Debug.Log("해제 이유 : " + Reason);
};
```

## ReturnCase

### Possible reasons:

| Reason                      | Side        | Description                                                                                           |
| :-------------------------- | :---------- | :---------------------------------------------------------------------------------------------------- |
| transport error             | Server Side | Transport error                                                                                       |
| server namespace disconnect | Server Side | Server performs a socket.disconnect()                                                                 |
| client namespace disconnect | Client Side | Got disconnect packet from client                                                                     |
| ping timeout                | Client Side | Client stopped responding to pings in the allowed amount of time(per the pingTimeout config setting) |
| transport close             | Client Side | Client stopped sending data                                                                           |
