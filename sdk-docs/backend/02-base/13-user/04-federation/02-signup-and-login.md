---
sidebar_label: "페더레이션 회원가입/로그인"
description: "AuthorizeFederation"
---

# AuthorizeFederation

public BackendReturnObject **AuthorizeFederation**(string **federationToken**, FederationType **type**);  
public BackendReturnObject **AuthorizeFederation**(string **federationToken**, FederationType **type**, string **ect**);

## 파라미터

| Value           | Type                 | Description                                                                                                               |
| :-------------- | :------------------- | :------------------------------------------------------------------------------------------------------------------------ |
| federationToken | string               | 페더레이션 token                                                                                                          |
| type            | FederationType(enum) | 페더레이션의 종류.<br />`FederationType.Google`<br />`FederationType.Facebook`<br/>`FederationType.Apple`<br />`FederationType.Steam`<br />`FederationType.GPGS2`<br />`FederationType.Line` |
| etc             | string               | (Optional) 부가적으로 나오는 정보들 중에 저장하고 싶은 정보                                                               |

## 설명

구글/애플/페이스북등 연동하려는 플랫폼의 회원정보 토큰 값을 이용하여 회원가입/로그인을 시도합니다.

- 해당 토큰으로 회원가입을 한 내역이 없을 경우 회원가입과 동시에 로그인을 시도합니다.
- 해당 토큰으로 회원가입을 한 내역이 존재할 경우 로그인을 시도합니다.

## Example

### 동기

```js
Backend.BMember.AuthorizeFederation(
  "federationToken",
  FederationType.Google,
  "GPGS로 가입함"
);
```

### 비동기

```js
Backend.BMember.AuthorizeFederation(
  "federationToken",
  FederationType.Google,
  "GPGS로 가입함",
  (callback) => {
    // 페더레이션 인증 이후 처리
  }
);
```

### SendQueue

```js
SendQueue.Enqueue(
  Backend.BMember.AuthorizeFederation,
  "federationToken",
  FederationType.Google,
  "GPGS로 가입함",
  (callback) => {
    // 페더레이션 인증 이후 처리
  }
);
```

## ReturnCase

### Success cases

**로그인에 성공한 경우**  
statusCode : 200  
message : Success

**신규 회원가입에 성공한 경우**  
statusCode : 201  
message : Success

### Error cases

**디바이스 정보가 null일 경우**  
statusCode : 400  
errorCode : UndefinedParameterException  
message : undefined device_unique_id, device_unique_id을(를) 확인할 수 없습니다

**차단당한 유저인 경우**  
statusCode : 403  
errorCode : 콘솔에 입력된 차단 사유  
message : Forbidden blocked user, 금지된 blocked user  
code : 403100  
errorMessage : 757fe1d0-03af-11ef-bda5-fd24a5f141c1 blocked until 2024-11-29T12:41:19.465Z  
errorData : {  
    uuid : 757fe1d0-03af-11ef-bda5-fd24a5f141c1  
    nickname : 닉네임  
    blockReason : 콘솔에 입력된 차단 사유  
    blockDate : 2024-11-29T12:41:19.465Z  
}  


> 차단당한 유저일 경우, 다음과 같은 에러 데이터를 확인할 수 있습니다.
* Code : 해당 에러 케이스의 고유 ID
* ErrorMessage : 해당 에러 세부 사항
* ErrorData : 에러에 관련된 세부 데이터(nickname과 blockReason은 존재하지 않을 경우, 데이터를 파싱할 수 없습니다.)


**에러 사용 예제**
```js
var callback = Backend.BMember.CustomLogin("a0", "a0");

Debug.Log(
    if (callback.GetMessage().StartsWith("Forbidden blocked user"))
    {
        StringBuilder returnString = new StringBuilder();
        returnString.Append("StatusCode : ").Append(callback.GetStatusCode()).AppendLine();
        returnString.Append("ErrorCode : ").Append(callback.GetErrorCode()).AppendLine();
        returnString.Append("Message : ").Append(callback.GetMessage()).AppendLine();

        returnString.Append("Code : ").Append(callback.GetCode()).AppendLine();
        returnString.Append("ErrorMessage : ").Append(callback.GetErrorMessage()).AppendLine();    

        returnString.Append("errorData(uuid) : ").Append(callback.GetErrorData()["uuid"].ToString()).AppendLine();          

        if (callback.GetErrorData().ContainsKey("nickname"))
        {
            returnString.Append("errorData(nickname) : ").Append(callback.GetErrorData()?["nickname"].ToString()).AppendLine();
        }    

        if (callback.GetErrorData().ContainsKey("blockReason"))
        {
            returnString.Append("errorData(blockReason) : ").Append(callback.GetErrorData()?["blockReason"].ToString()).AppendLine();
        }    

        returnString.Append("errorData(blockDate) : ").Append(callback.GetErrorData()["blockDate"].ToString()).AppendLine();
        
        Debug.Log(returnString.ToString());
    }
)
```


**차단당한 디바이스일 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden blocked device, 금지된 blocked device  
code : 403100   
errorMessage : 20240426092307fc5ac478c is blocked  
errorData : {  
    deviceUniqueId : 20240426092307fc5ac478c  
}  

> 차단당한 디바이스일 경우, 다음과 같은 에러 데이터를 확인할 수 있습니다.
* Code : 해당 에러 케이스의 고유 ID
* ErrorMessage : 해당 에러 세부 사항
* ErrorData : 에러에 관련된 세부 데이터


**에러 사용 예제**
```js
var callback = Backend.BMember.CustomSignUp("a0", "a0");

Debug.Log(
    if (callback.IsDeviceBlockError())
    {
        StringBuilder returnString = new StringBuilder();
        returnString.Append("StatusCode : ").Append(callback.GetStatusCode()).AppendLine();
        returnString.Append("ErrorCode : ").Append(callback.GetErrorCode()).AppendLine();
        returnString.Append("Message : ").Append(callback.GetMessage()).AppendLine();
        returnString.Append("Code : ").Append(callback.GetCode()).AppendLine();
        returnString.Append("ErrorMessage : ").Append(callback.GetErrorMessage()).AppendLine();
        returnString.Append("errorData(deviceUniqueId)").Append(callback.GetErrorData()["deviceUniqueId"].ToString()).AppendLine();
        Debug.Log(returnString.ToString());
    }
)
```

**탈퇴가 진행중일 경우(WithdrawAccount 함수 호출 이후)**  
statusCode : 410  
errorCode : GoneResourceException  
message : Gone user, 사라진 user 입니다

> 탈퇴가 완료되면 201로 신규 회원가입이 진행됩니다.

---

# 페더레이션 계정 로그아웃

## 기존에 로그인 된 페더레이션 계정을 로그아웃 후, 다른 페더레이션 유저로 변경하고 싶을 경우

뒤끝 계정을 로그아웃 한 후 페더레이션 계정을 로그아웃해야 정상적으로 다른 페더레이션 계정으로 로그인 할 수 있습니다.  
다른 페더레이션 계정으로 로그인 한 후에는 다시 뒤끝 계정으로 `AuthorizeFederation` 함수를 호출해야 새로운 계정으로 게임을 진행할 수 있습니다.

### GPGS 로그아웃 방법

```js
public void GoogleSignout(){
    GoogleSignIn.DefaultInstance.SignOut();
}
```

### 페이스북 로그아웃 방법

```js
public void FacebookLogOut(){
    FB.LogOut();
}
```
