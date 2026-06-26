---
sidebar_label: 토큰 갱신
---

# RefreshTheBackendToken

public BackendReturnObject **RefreshTheBackendToken**();

## 설명

뒤끝 SDK는 기기 내부에 기한이 만료되지 않은 **access_token** 혹은 **refresh_token**이 필요합니다.  
재발급에 성공하면 access_token과 refresh_token을 새 값으로 업데이트를 합니다.

## Example

### 동기

```js
Backend.BMember.RefreshTheBackendToken();
```

### 비동기

```js
Backend.BMember.RefreshTheBackendToken((callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.BMember.RefreshTheBackendToken, (callback) => {
  // 이후 처리
});
```

## ReturnCase

### Success cases

**갱신에 성공한 경우**  
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

**다른 기기로 로그인하여 refresh_token이 만료된 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad refreshToken, 잘못된 refreshToken 입니다

**차단당한 유저인 경우**  
statusCode : 403  
errorCode : **콘솔에서 입력한 차단된 사유**  
message : Forbidden blocked user, 금지된 blocked user

**1년 뒤 refresh_token이 만료된 경우**  
statusCode: 410  
errorCode: GoneResourceException  
message: Gone expired refreshToken, 사라진 expired refreshToken 입니다.
