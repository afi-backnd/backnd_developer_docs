---
description: "회원가입 / 로그인"
---

# 회원가입 / 로그인

## 회원가입 방법
뒤끝에서는 게임 유저 관리를 위해 총 3가지 회원가입 방법이 존재합니다.  

| 회원가입 방법 | 인증에 사용되는 값 | 설명  |
| :------------ |:------------| :-----|
| [커스텀 계정](/sdk-docs/backend/base/user/custom/signup) | 아이디 / 비밀번호 | 유저가 직접 아이디 / 비밀번호를 이용하여 회원가입한 계정  |
| [페더레이션 계정](/sdk-docs/backend/base/user/federation/signup-and-login) | 연동 플랫폼의 토큰 정보 | 구글/애플/페이스북등 플랫폼이 제공하는 토큰 값을 이용하여 회원가입한 계정 |
| [게스트 계정](/sdk-docs/backend/base/user/guest/signup-and-login) (커스텀 계정) | 기기 내부에 저장된 게스트 계정 정보 | 기기 내부에서 생성된 계정 정보를 이용하여 회원가입한 계정 |

## 로그인 방법
뒤끝에서는 총 4가지 로그인 방법이 존재합니다.  
회원가입을 하는 경우 회원가입과 동시에 로그인이 진행됩니다.  
커스텀 계정, 페더레이션 계정, 게스트 계정 모두 로그인 시 accessToken과 refreshToken이 발급됩니다.  
발급된 accessToken과 refreshToken은 기기 내부에 저장되고, 이를 이용하여 토큰 로그인을 시도할 수 있습니다.  

| 로그인 방법 | 인증에 사용되는 값 | 설명  |
| :------------ |:------------| :-----|
| [커스텀 계정](/sdk-docs/backend/base/user/custom/signup) | 아이디 / 비밀번호 | 유저가 직접 아이디 / 비밀번호를 이용하여 로그인  |
| [페더레이션 계정](/sdk-docs/backend/base/user/federation/signup-and-login) | 연동 플랫폼의 토큰 정보 | 구글/애플/페이스북등 플랫폼이 제공하는 토큰 값을 이용하여 로그인 |
| [게스트 계정](/sdk-docs/backend/base/user/guest/signup-and-login) (커스텀 계정) | 기기 내부에 저장된 게스트 계정 정보 | 기기 내부에 생성/저장된 계정 정보를 이용하여 로그인 |
| [엑세스토큰 로그인](/sdk-docs/backend/base/user/token/accesstoken) | 기기 내부에 저장된 accessToken, refreshToken 정보 | 커스텀/페더레이션/게스트 로그인 시 생성된 엑세스 토큰 정보를 이용한 로그인  |

## 로그인 성공 시 제공되는 변수
로그인이 성공할 경우, 뒤끝 SDK 중 일부 변수에 값이 할당됩니다.  
할당되는 변수는 다음과 같습니다.  

| Value   | Description  |
| :-------- | :-----|
| Backend.UserNickName   | 유저의 닉네임 |
| Backend.UserInDate   | 유저의 inDate |
| Backend.UID   | 유저의 웹 쿠폰용 UID |

```js
var bro = Backend.BMember.CustomLogin("backendUser", "backendUser");

if(bro.IsSuccess()) {
    Debug.Log("로그인에 성공했습니다 : " + bro);
    Debug.Log($"유저 닉네임 : " + Backend.UserNickName);
    Debug.Log($"유저 인데이트 : " + Backend.UserInDate);
    Debug.Log($"유저 UID(쿠폰용) : " + Backend.UID);
}
else {
    Debug.LogError("로그인 중 에러가 발생했습니다.");
    // Backend.UserNickname, Backend.UserInDate, Backend.UID에 값이 할당되지 않습니다.  
}
```

<img src="https://developer.thebackend.io/static/img/unity/bmember/signup_login/value_log.png"/>


## 로그인 상태에서 다른 기기로 로그인 시도 시
뒤끝은 모든 통신에 로그인 시 발급되는 뒤끝 Access Token 유효성 검증을 합니다.  
* 한 명의 유저는 한 개의 Access Token만 유효합니다.  
* 다른 기기로 로그인 된 경우 이전의 Access Token 유효성 검증이 실패하여 이전의 로그인한 기기는 통신에 실패합니다.  
* 새로 로그인 한 기기의 경우 정상적으로 접속이 됩니다.  

Access Token 이 올바르지 않은 경우 다음과 같은 BackendReturnObject 가 리턴됩니다.  
```js
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad accessToken, 잘못된 accessToken 입니다
```

```js
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad bad,accessToken,,잘못된,accessToken,입니다, 잘못된 bad,accessToken,,잘못된,accessToken,입니다 입니다
```
