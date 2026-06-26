---
sidebar_label: 게스트 회원가입/로그인
description: "게스트 회원가입/로그인"
---

# SignInGuest

public Task&lt;RequestResult&gt; **SignInGuestAsync**();  
public Task&lt;RequestResult&gt; **SignInGuestAsync**(string **etc**);

## 파라미터

| Value | Type   | Description                              |
| :---- | :----- | :--------------------------------------- |
| etc   | string | (Optional) 부가적으로 저장하고 싶은 정보 |

## 설명

뒤끝 SDK는 커스텀(아이디/패스워드) 계정 기능을 응용한 게스트 로그인 기능을 제공합니다.

- 게스트 로그인을 한 계정은 [구글/애플/페이스북등 연동하려는 페더레이션 계정으로 변경](/sdk-docs/backend/base/user/federation/migrate-from-custom) 할 수 있습니다.
- 게스트 로그인한 계정도 토큰 로그인을 수행할 수 있습니다.
- 게스트 계정은 로컬에 저장된 계정 정보를 기반으로 로그인을 합니다.
- **로컬 데이터 삭제, 앱 재설치, 기기 변경, 로컬 데이터 손상이 발생하면 해당 계정으로 게스트 로그인을 시도할 수 없습니다.**
- 로컬에 저장된 계정 정보가 삭제되어도 서버에 저장된 사용자 데이터는 그대로 존재합니다.

:::danger 주의

- 로컬에 존재하는 계정 정보가 삭제되어 **로그인이 불가능해진 게스트 계정에 대한 별도의 로그인 방법은 제공하고 있지 않습니다.**
  - **게스트 로그인한 계정의 아이디/비밀번호를 콘솔에서 변경하는 기능은 제공하고 있지 않습니다.**
  - **게스트 계정의 데이터 복원, 데이터 이관 등의 기능은 제공하고 있지 않습니다.**
  - **로컬 데이터 삭제, 어플 재설치, 기기 변경, 로컬 데이터 손상 등의 상황이 발생하면 게스트 계정에 접근할 수 없다고 반드시 유저에게 충분한 설명을 고지해야 합니다.**
  - 게스트 계정에 이메일 정보를 등록한 경우 비밀번호 초기화 메일을 송신할 수 있습니다. 이것을 이용하여 비밀번호를 초기화할 수 있지만, 비밀번호를 초기화 한 경우 SDK 내부 게스트 계정의 비밀번호와 실제 계정의 비밀번호가 달라지게 됩니다.  
    이 경우 커스텀 로그인을 통해서만 로그인이 가능합니다.  
:::

## 게스트 로그인

게스트 로그인 함수를 호출하면 기기 로컬에 저장된 게스트 계정 정보가 있는지 확인합니다.  
로컬에 존재하는 게스트 계정 정보 유무에 따라 다른 방식으로 함수가 처리됩니다.

- 게스트 계정의 아이디는 **guest-0514db55-6bd3-46f3-a4dd-0cc5a57ddf53** 와 같은 형태로 id가 생성됩니다.
  - **guest-**는 게스트 계정임을 표시하는 값입니다.
  - 뒤에 붙는 문자열은 중복되지 않는 UUID 값입니다.
- 게스트 계정은 커스텀 회원가입 한 계정으로 취급됩니다. 뒤끝 콘솔에서도 **가입유형이 customSignUp**으로 표시됩니다.

### 기기 내부에 게스트 계정 정보가 존재하는 경우

저장된 게스트 계정을 이용하여 커스텀 로그인을 실시합니다.

### 기기 내부에 게스트 계정 정보가 존재하지 않는 경우

다른 유저와 중복되지 않는 아이디를 생성하여 커스텀 회원가입을 실시합니다.  
회원가입에 성공한 경우 [뒤끝 파일시스템 기능](/sdk-docs/backend/base/sdk-utils/filesystem)을 이용하여 기기 로컬에 게스트 계정 정보를 저장합니다.

## Example

### Task 방식
```js
var reqResult = await BackndAuth.Instance.SignInGuestAsync("게스트 로그인으로 로그인함");
if (reqResult.IsSuccess())
{
    Debug.Log("게스트 로그인에 성공했습니다");
}
```

### Callback 방식
```js
BackndAuth.Instance.SignInGuest("게스트 로그인으로 로그인함", (callback) =>
{
    if (callback.IsSuccess())
    {
        Debug.Log("게스트 로그인에 성공했습니다");
    }
});
```

## ReturnCase

### Success cases

**게스트 로그인에 성공한 경우**  
statusCode : 200  
errorCode : Success

**게스트 회원가입에 성공한 경우**  
statusCode : 201  
errorCode : Success

### Error cases

**디바이스 정보가 null일 경우**  
statusCode : 400  
errorCode : UndefinedParameterException  
message : undefined device_unique_id, device_unique_id을(를) 확인할 수 없습니다

**존재하지 않는 아이디의 경우(로컬데이터가 변조된 경우)**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad customId, 잘못된 customId 입니다

**게스트 계정을 페더레이션 계정으로 변경한 후 게스트 로그인을 시도한 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad customId, 잘못된 customId 입니다

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
var reqResult = await BackndAuth.Instance.SignInGuestAsync("a0", "a0");
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
var reqResult = await BackndAuth.Instance.SignUpCustomAsync("a0", "a0");
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

**탈퇴가 진행중일 경우(WithdrawAccount 함수 호출 이후)**  
statusCode : 410  
errorCode : GoneResourceException  
message : Gone user, 사라진 user 입니다
