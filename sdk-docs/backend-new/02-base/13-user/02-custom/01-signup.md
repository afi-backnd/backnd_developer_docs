---
sidebar_label: 커스텀 회원가입
description: "커스텀 회원가입"
---

# SignUpCustom 
public Task&lt;RequestResult&gt; **SignUpCustomAsync**(string **id**, string **password**);  
public Task&lt;RequestResult&gt; **SignUpCustomAsync**(string **id**, string **password**, string **etc**);


## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| id      | string | 커스텀 가입을 진행할 때 필요한 아이디 |
| password      | string      | 커스텀 가입을 진행할 때 필요한 비밀번호 |
| etc | string      | (Optional) 부가적으로 저장하고 싶은 정보  |

## 설명
유저가 직접 아이디, 비밀번호를 지정해 회원가입을 합니다.   
* 커스텀 회원가입을 한 계정은 [구글/애플/페이스북등 연동하려는 페더레이션 계정으로 변경](/sdk-docs/backend/base/user/federation/migrate-from-custom)할 수 있습니다.  

## Example

### Task 방식
```js
var reqResult = await BackndAuth.Instance.SignUpCustomAsync("id", "password");
if (reqResult.IsSuccess())
{
    Debug.Log("회원가입에 성공했습니다");
}
```

### Callback 방식
```js
BackndAuth.Instance.SignUpCustomAsync("id", "password", reqResult =>
{
    if (reqResult.IsSuccess())
    {
        Debug.Log("회원가입에 성공했습니다");
    }
});
```

## ReturnCase

### Success cases

**회원가입에 성공한 경우**  
statusCode : 201  
message : Success  

### Error cases

**디바이스 정보가 null일 경우**  
statusCode : 400  
errorCode : UndefinedParameterException  
message : undefined device_unique_id, device_unique_id을(를) 확인할 수 없습니다

**프로젝트 상태가 '점검'일 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad serverStatus: maintenance, 잘못된 serverStatus: maintenance 입니다

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
var reqResult = await BackndAuth.Instance.SignUpCustomAsync("id", "password");
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


**중복된 customId 가 존재하는 경우**  
statusCode : 409  
errorCode : DuplicatedParameterException  
message : Duplicated customId, 중복된 customId 입니다

