---
sidebar_label: 토큰 로그인
---

# SignInWithBackndToken

public Task&lt;RequestResult&gt; **SignInWithBackndTokenAsync**();

## 설명

기존에 로그인했던 유저가 [기기 로컬에 저장](/sdk-docs/backend/base/sdk-utils/filesystem) 된 뒤끝 AccessToken을 이용하여 로그인을 시도합니다.  
뒤끝 SDK의 로그인 함수는 기기 내부에 기한이 만료되지 않은 **access_token** 혹은 **refresh_token**이 필요합니다.  
Login에 성공하면 access_token과 refresh_token을 새 값으로 업데이트를 합니다.  

:::danger 토큰 로그인 구현 시 주의 사항  
다른 기기 접속, 업데이트로 인한 backend.dat 파일의 변경 등 다양한 이유로 토큰이 만료되는 상황이 발생할 수 있습니다.  
이 경우, 토큰 로그인이 불가해지므로 일반 로그인 과정이 필요합니다.

토큰 로그인을 사용하시는 경우 **반드시 토큰 로그인에 실패하였을 때, 일반 로그인을 진행할 수 있도록 코드를 구성**하여 이용해 주세요.
:::   

## Example

### Task 방식
```js
var reqResult = await BackndAuth.Instance.SignInWithBackndTokenAsync();
if (reqResult.IsSuccess())
{
    Debug.Log("자동 로그인에 성공했습니다");
}        
```

### Callback 방식
```js
BackndAuth.Instance.SignInWithBackndToken((callback) =>
{
    if (callback.IsSuccess())
    {
        Debug.Log("자동 로그인에 성공했습니다");
    }
});
```

## ReturnCase

### Success cases

**로그인에 성공한 경우**  
statusCode : 201  
message : Success

### Error cases

**기기 로컬에 액세스 토큰이 존재하지 않는데 토큰 로그인 시도를 한 경우**  
statusCode : 400  
errorCode : accessTokenError  
message : accessToken not exist

**로그인한적이 없거나, 로그아웃 후 재접속하는 등 토큰값이 존재하지 않는 상황에서 시도한 경우**  
statusCode : 400  
errorCode : UndefinedParameterException  
message : undefined refresh_token, refresh_token을(를) 확인할 수 없습니다

**다른 기기로 로그인 하여 refresh_token이 만료된 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad refreshToken, 잘못된 refreshToken 입니다

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
var reqResult = await BackndAuth.Instance.SignInCustomAsync("a0", "a0");
if (reqResult.Message.StartsWith("Forbidden blocked user"))
{
    StringBuilder returnString = new StringBuilder();
    returnString.Append("StatusCode : ").Append(reqResult.StatusCode).AppendLine();
    returnString.Append("ErrorCode : ").Append(reqResult.ErrorCode).AppendLine();
    returnString.Append("Message : ").Append(reqResult.Message).AppendLine();
    returnString.Append("Code : ").Append(reqResult.Code).AppendLine();
    returnString.Append("ErrorMessage : ").Append(reqResult.ErrorMessage).AppendLine();
    returnString.Append("errorData(uuid) : ").Append(reqResult.ErrorData["uuid"].ToString()).AppendLine();

    if (reqResult.ErrorData.ContainsKey("nickname"))
    {
        returnString.Append("errorData(nickname) : ").Append(reqResult.ErrorData?["nickname"].ToString()).AppendLine();
    }
    if (reqResult.ErrorData.ContainsKey("blockReason"))
    {
        returnString.Append("errorData(blockReason) : ").Append(reqResult.ErrorData?["blockReason"].ToString()).AppendLine();
    }

    returnString.Append("errorData(blockDate) : ").Append(reqResult.ErrorData["blockDate"].ToString()).AppendLine();

    Debug.Log(returnString.ToString());
}
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
var reqResult = await BackndAuth.Instance.SignInCustomAsync("a0", "a0");
if (reqResult.IsDeviceBlockError())
{
    StringBuilder returnString = new StringBuilder();
    returnString.Append("StatusCode : ").Append(reqResult.StatusCode).AppendLine();
    returnString.Append("ErrorCode : ").Append(reqResult.ErrorCode).AppendLine();
    returnString.Append("Message : ").Append(reqResult.Message).AppendLine();
    returnString.Append("Code : ").Append(reqResult.Code).AppendLine();
    returnString.Append("ErrorMessage : ").Append(reqResult.ErrorMessage).AppendLine();
    returnString.Append("errorData(deviceUniqueId)").Append(reqResult.ErrorData["deviceUniqueId"].ToString()).AppendLine();
    Debug.Log(returnString.ToString());
}
```

**1년 뒤 refresh_token이 만료된 경우**  
statusCode: 410  
errorCode: GoneResourceException  
message: Gone expired refreshToken, 사라진 expired refreshToken 입니다.
