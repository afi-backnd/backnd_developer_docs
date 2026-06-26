---
sidebar_label: 계정 로그인
---

# LoginAccount
public Task&lt;RequestResult&gt; **LoginAccountAsync**(string **id**, string **password**);


## 파라미터
| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| id      | string | 가입된 고유한 유저 아이디 |
| password      | string      | 로그인에 사용된 비밀번호 |

## 설명
기존에 생성된 유저 계정에 로그인합니다.  
* BackndAuth.Instance.SignUpCustom 을 통해 생성된 아이디로 로그인할 수 없습니다.  
* 계정 로그인이 완료된 상태에서는 캐릭터 관련 함수를 제외하고는 뒤끝 기능을 이용하실 수 없습니다.  
> 계정(유저) 로그인이 완료된 상태에서 캐릭터 로그인을 하지 않고 뒤끝베이스 기능 호출 시 The client not login yet. Please login first(0) 라는 Exception이 발생합니다.  

## Example

### Task 방식
```js
var reqResult = await BackndMultiCharacter.Account.LoginAccountAsync("id", "password");
if (reqResult.IsSuccess())
{
    Debug.Log("계정 로그인에 성공했습니다.");
}        
```

### Callback 방식
```js
BackndMultiCharacter.Account.LoginAccountAsync("id", "password", (callback) =>
{
    if (callback.IsSuccess())
    {
        Debug.Log("계정 로그인에 성공했습니다.");
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

**존재하지 않는 아이디일 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad customId, 잘못된 customId 입니다

**비밀번호가 일치하지 않는 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad customPassword, 잘못된 customPassword 입니다

**프로젝트 상태가 '점검'일 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad serverStatus: maintenance, 잘못된 serverStatus: maintenance 입니다

**차단당한 디바이스일 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden blocked device, 금지된 blocked device입니다.  

**커스텀 회원가입으로 생성한 아이디로 로그인한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : user not found, user을(를) 찾을 수 없습니다

