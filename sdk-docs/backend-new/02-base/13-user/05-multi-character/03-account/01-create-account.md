---
sidebar_label: 계정 생성
---

# CreateAccount
public Task&lt;RequestResult&gt; **CreateAccountAsync**(string **id**, string **password**);


## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| id      | string | 유저 계정 가입, 로그인 시 필요한 고유한 아이디 |
| password      | string      | 유저 계정 가입, 로그인 시 필요한 비밀번호 |

## 설명
고유한 아이디와 그에 따른 비밀번호를 입력하여 새로운 유저 계정을  생성합니다.  
* BackndAuth.Instance.SignInCustom 을 통해 로그인할 수 없습니다.  
* CustomSignUp을 통해 생성된 아이디와 동일한 이름으로 생성할 수 없습니다.  
* 생성에 성공할 경우 해당 계정으로 로그인이 진행되며, LoginAccount를 호출할 필요가 없습니다.  
* 계정 로그인이 완료된 상태에서는 캐릭터 관련 함수를 제외하고는 뒤끝 기능을 이용하실 수 없습니다.  
> 계정(유저) 로그인이 완료된 상태에서 캐릭터 로그인을 하지 않고 뒤끝베이스 기능 호출 시 The client not login yet. Please login first(0) 라는 Exception이 발생합니다.  

## Example

### Task 방식
```js
var reqResult = await BackndMultiCharacter.Account.CreateAccountAsync("id", "password");
if (reqResult.IsSuccess())
{
    Debug.Log("계정 생성에 성공했습니다.");
}
```

### Callback 방식
```js
BackndMultiCharacter.Account.CreateAccount("id", "password", (callback) =>
{
    if (callback.IsSuccess())
    {
        Debug.Log("계정 생성에 성공했습니다.");
    }
});
```

## ReturnCase

### Success cases

**회원가입에 성공한 경우**  
statusCode : 200  
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
message : Forbidden blocked device, 금지된 blocked device입니다.  

**중복된 id가 존재하는 경우**  
statusCode : 409  
errorCode : DuplicatedParameterException  
message : Duplicated customId, 중복된 customId 입니다

