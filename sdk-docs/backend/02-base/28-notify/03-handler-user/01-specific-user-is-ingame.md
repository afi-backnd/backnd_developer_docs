---
sidebar_label: "특정 유저 접속 핸들러"
description: "UserIsConnectByIndate"
---

# UserIsConnectByIndate

public void **UserIsConnectByIndate**(string **gamerIndate**);

## 파라미터

| Value       | Type   | Description                                            |
| ----------- | ------ | ------------------------------------------------------ |
| gamerIndate | string | 실시간 알림 서버 접속 여부를 확인할 유저의 gamerIndate |

## 설명

해당 gamerIndate를 가진 유저가 실시간 알림 서버에 접속해있는지 확인합니다.  
결과는 `OnIsConnectUser` 이벤트 핸들러를 통해 확인할 수 있습니다.  

> `Backend.Notification.Connect()` 함수 호출 후 사용하실 수 있습니다.  
> CheckUserIsConnect() 함수 / OnIsConnect 핸들러의 업그레이드된 버전입니다.  

:::danger 주의
* 확인할 유저가 실시간 알림 서버에 접속해 있어야 정상적으로 작동됩니다.  
* 실시간 알림 서버는 Backend.Notification.Connect()를 통해 접속하실 수 있습니다.  
:::

## Example

```js
//실시간 알림 서버에 연결합니다.  
Backend.Notification.Connect();

//"a1"의 닉네임을 가진 유저의 inDate를 찾는다
BackendReturnObject bro =  Backend.Social.GetUserInfoByNickName("a1");  
string gamerIndate = bro.GetFlattenJSON()["row"]["inDate"].ToString();

// UserIsConnectByIndate 함수 호출 시 반응하는 OnIsConnectUser 핸들러를 설정한다.  
Backend.Notification.OnIsConnectUser = (bool isConnect, string nickName, string gamerIndate) => {
       Debug.Log($"{nickName} / {gamerIndate} 접속 여부 확인 : " + isConnect);
 };

// 함수 호출 시, 위에서 설정한 OnIsConnectUser 핸들러가 호출된다.  
Backend.Notification.UserIsConnectByIndate(gamerIndate);
```

---

# OnIsConnectUser

public OnIsConnectUser **OnIsConnectUser**;

## 설명

`UserIsConnectByIndate` 함수 호출 시, 접속 여부를 bool 값을 통해 이벤트 핸들러로 전달해 줍니다.  

> 닉네임이 존재하지 않을 경우, string.Empty가 리턴됩니다.  
> 해당 inDate의 유저가 존재하지 않을 경우, 핸들러가 호출되지 않습니다.  

## Example

```js
 Backend.Notification.OnIsConnectUser = (bool isConnect, string nickName, string gamerIndate) => {
       Debug.Log($"{nickName} / {gamerIndate} 접속 여부 확인 : " + isConnect);
 };
```

### Response cases:

| Value       | Type   | Description                                 |
| ----------- | ------ | ------------------------------------------- |
| isConnect   | bool   | 해당 유저의 접속 여부(접속해있을 경우 true) |
| nickName    | string | 해당 유저의 닉네임                          |
| gamerIndate | string | 해당 유저의 inDate                          |

> 닉네임과 inDate는 해당 유저 접속 여부 상관없이 리턴됩니다.  
