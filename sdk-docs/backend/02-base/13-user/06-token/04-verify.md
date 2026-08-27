---
sidebar_label: "토큰 유효성 체크"
description: "IsAccessTokenAlive"
---

# IsAccessTokenAlive
public BackendReturnObject **IsAccessTokenAlive**();  
public BackendReturnObject **VerifyToken**();

## 설명
현재 유저가 갖고 있는 AccessToken이 뒤끝 통신을 위해 유효한지 확인합니다.  

멀티 캐릭터 프로젝트에서는 캐릭터를 선택하지 않은 계정 로그인 상태에서도 사용할 수 있습니다. 계정 로그인 상태에서는 계정 토큰을, 캐릭터 로그인 상태에서는 캐릭터 토큰을 검증합니다.  

`Backend.BMember.VerifyToken`은 `IsAccessTokenAlive`와 동일하게 동작하는 별칭입니다. 둘 중 어느 것을 사용해도 결과가 같습니다.  

## Example

### 동기
```js
BackendReturnObject bro = Backend.BMember.IsAccessTokenAlive();
if(bro.IsSuccess())
{
   Debug.Log("액세스 토큰이 살아있습니다");
}
```

### 비동기
```js
Backend.BMember.IsAccessTokenAlive((callback) =>
{
  if(callback.IsSuccess())
      Debug.Log("액세스 토큰이 살아있습니다");
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.BMember.IsAccessTokenAlive, (callback) =>
{
  if(callback.IsSuccess())
      Debug.Log("액세스 토큰이 살아있습니다");
});
```

## ReturnCase

### Success cases

**싱글 캐릭터 게이머의 토큰이 유효한 경우**  
statusCode : 200  
message : Success  
returnValue : 게이머 정보(`gamer_id`, `inDate`, `gamerType`, `locationProperties` 등)

**멀티 캐릭터 계정 또는 캐릭터 토큰이 유효한 경우**  
statusCode : 204  
message : Success  

:::caution 5.18.14에서 변경된 점
토큰 유효성 검증 엔드포인트가 통합되면서 싱글 캐릭터 게이머의 성공 응답 statusCode가 **204에서 200으로 변경**되었습니다. 반면 멀티 캐릭터의 계정·캐릭터 컨텍스트는 **204**를 그대로 반환하므로, 성공 여부는 statusCode 비교 대신 `IsSuccess()`로 판단해 주세요.
:::

### Error cases

**로그인하지 않았거나 토큰이 유효하지 않은 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad accessToken, 잘못된 accessToken 입니다
