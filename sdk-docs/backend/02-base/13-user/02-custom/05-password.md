---
sidebar_label: "비밀번호 관리"
---

# ConfirmCustomPassword
public BackendReturnObject **ConfirmCustomPassword**(string **CustomGamerPassword**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| CustomGamerPassword      | string | 커스텀 비밀번호 | 

## 설명
현재 accessToken의 유저의 커스텀 비밀번호 일치 여부를 확인합니다.  
유저의 정보 수정을 하기 전 보안상의 문제로 비밀번호를 재확인하는 용도 등으로 사용될 수 있습니다.  

## Example

### 동기
```js
BackendReturnObject bro = Backend.BMember.ConfirmCustomPassword("1234");
if(bro.IsSuccess())
{
  Debug.Log("비밀번호가 일치합니다");
}
```

### 비동기
```js
Backend.BMember.ConfirmCustomPassword("1234", (callback) =>
{
    if(callback.IsSuccess())
    {
      Debug.Log("비밀번호가 일치합니다");
    }
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.BMember.ConfirmCustomPassword, "1234", (callback) =>
{
    if(callback.IsSuccess())
    {
      Debug.Log("비밀번호가 일치합니다");
    }
});
```

## ReturnCase

### Success cases

**비밀번호가 일치한 경우**  
statusCode : 200  
message : Success  

### Error cases

**잘못된 비밀번호를 입력한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad password, 잘못된 password 입니다.  

----------------------------------------

# ResetPassword
public BackendReturnObject **ResetPassword**(string **CustomGamerId**, string **EmailAddress**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| CustomGamerId  | string | 비밀번호 초기화를 진행할 CustomId |
| EmailAddress   | string | 미리 등록해둔 초기화된 비밀번호를 받을 이메일 주소 |

## 설명
커스텀 아이디와 등록한 이메일 정보를 입력 시, 해당 이메일로 초기화된 비밀번호를 전송합니다.  

* 유저의 국가 정보가 등록되어 있을 경우, 해당 국가정보로 작성된 비밀번호 초기화 양식을 발송합니다.  
* 유저의 국가 정보가 존재하지 않을 경우에는 기본 비밀번호 초기화 양식으로 발송합니다. 

:::danger 게스트 로그인한 계정은 비밀번호 초기화를 사용하면 안 됩니다.  
게스트 로그인한 계정이 비밀번호 초기화를 한 경우 정상적인 방법으로 게스트 로그인을 할 수 없습니다.  
:::


## Example

### 동기
```js
Backend.BMember.ResetPassword("CustomGamerId" , "help@backnd.com");

```

### 비동기
```js
Backend.BMember.ResetPassword("CustomGamerId" , "help@backnd.com", (callback) =>
{
  // 이후 처리
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.BMember.ResetPassword, "CustomGamerId" , "help@backnd.com", (callback) =>
{
  // 이후 처리
});
```

## ReturnCase

### Success cases

**비밀번호 초기화 이메일 송신에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**잘못된 이메일을 입력한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad email is not match, 잘못된 email is not match 입니다

**잘못된 CustomId를 입력한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : gamer not found, gamer을(를) 찾을 수 없습니다

**등록한 이메일이 없는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : enrolled email not found, enrolled email을(를) 찾을 수 없습니다

**24시간 이내에 5회 이상 같은 이메일 정보로 비밀번호 초기화를 시도한 경우**  
statusCode : 429  
errorCode : Too Many Request  
message : 정보보호를 위해 아이디 찾기/비밀번호 초기화는 하루 5회로 제한됩니다. 내일 다시 시도해 주세요. 요청 횟수를 초과하였습니다.  

**프로젝트 명에 특수문자가 추가된 경우
(비밀번호는 초기화되지만 안내 메일 미발송 및 에러 발생)**  
statusCode : 400  
errorCode : InvalidParameterValue  
message : User name is missing: 고유값 no-reply@backnd.com;

----------------------------------------------------------------------------------------------------

# UpdatePassword
public BackendReturnObject **UpdatePassword**(string **OldPassword**, string **NewPassword**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| OldPasword   | string | 현재 비밀번호 |
| NewPassword  | string | 변경할 비밀번호 |

## 설명
현재 계정의 비밀번호를 새로운 비밀번호로 변경합니다.  

:::danger 게스트 로그인한 계정은 비밀번호 업데이트를 사용하면 안 됩니다.  
게스트 로그인한 계정이 비밀번호 업데이트를 한 경우 정상적인 방법으로 게스트 로그인을 할 수 없습니다.  
:::


## Example

### 동기
```js
Backend.BMember.UpdatePassword("OldPassword", "NewPassword");
```

### 비동기
```js
Backend.BMember.UpdatePassword("OldPassword", "NewPassword", (callback) =>
{
    // 이후 처리
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.BMember.UpdatePassword, "OldPassword", "NewPassword", (callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**업데이트에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**잘못된 OldPasword를 입력한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad password is not match, 잘못된 password is not match 입니다
