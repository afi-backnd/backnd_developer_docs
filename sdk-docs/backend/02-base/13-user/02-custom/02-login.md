---
sidebar_label: 커스텀 로그인
---

# CustomLogin

public BackendReturnObject **CustomLogin**(string **id**, string **password**);  
public BackendReturnObject **CustomLogin**(string **id**, string **password**, string **etc**);

## 파라미터

| Value    | Type   | Description                              |
| :------- | :----- | :--------------------------------------- |
| id       | string | 아이디                                   |
| password | string | 비밀번호                                 |
| etc      | string | (Optional) 부가적으로 저장하고 싶은 정보 |

## 설명

커스텀 회원가입을 성공한 계정은 커스텀 로그인 기능을 이용하여 로그인을 수행할 수 있습니다.

## Example

### 동기

```js
BackendReturnObject bro = Backend.BMember.CustomLogin("id" , "password");
if(bro.IsSuccess())
{
  Debug.Log("로그인에 성공했습니다");
}
```

### 비동기

```js
Backend.BMember.CustomLogin("id", "password", (callback) => {
  if (callback.IsSuccess()) {
    Debug.Log("로그인에 성공했습니다");
  }
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.BMember.CustomLogin, "id", "password", (callback) => {
  if (callback.IsSuccess()) {
    Debug.Log("로그인에 성공했습니다");
  }
});
```

## ReturnCase

### Success cases

**로그인에 성공한 경우**  
statusCode : 200  
message : Success

### Error cases

**디바이스 정보가 null일 경우**  
statusCode : 400  
errorCode : UndefinedParameterException  
message : undefined device_unique_id, device_unique_id을(를) 확인할 수 없습니다

**존재하지 않는 아이디의 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad customId, 잘못된 customId 입니다

**비밀번호가 틀린 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad customPassword, 잘못된 customPassword 입니다

**프로젝트 상태가 '점검'일 경우(접근 허용 유저 제외)**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad serverStatus: maintenance, 잘못된 serverStatus: maintenance 입니다

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
