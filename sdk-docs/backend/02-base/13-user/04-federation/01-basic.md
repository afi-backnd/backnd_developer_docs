---
description: "페더레이션"
---

# 페더레이션
뒤끝은 구글 및 페이스북 인증 그리고 Apple로 로그인을 통한 회원가입 및 로그인이 가능합니다.  
* 해당 프로젝트에 가입하지 않은 계정으로 페더레이션 인증을 수행하면 회원가입이 됩니다.  
* 해당 프로젝트에 가입된 계정으로 페더레이션 인증을 수행하면 로그인이 됩니다.  

## 권한 요구
뒤끝은 구글, 페이스북에서 각각 다음과 같은 권한 요구를 하고 페더레이션 인증을 한 경우, 해당 페더레이션의 토큰에서 유저의 이메일 정보를 얻을 수 있습니다.  
애플은 코드상에서 별도의 권한 설정이 필요하지 않습니다.  
* 이메일 정보 권한을 요구한 경우 유저의 아이디를 이메일로 표시합니다.  
* 이메일 정보 조회 권한을 요구하지 않은 경우에도 페더레이션 인증을 수행할 수 있습니다.  
* 이 경우 뒤끝 콘솔에서 유저의 아이디는 표시되지 않습니다.  

### 구글
```js
PlayGamesClientConfiguration config = new PlayGamesClientConfiguration
        .Builder()
        .RequestServerAuthCode(false)
        .RequestEmail() // 이메일 권한을 요구하기 위해 추가해야 합니다.  
        .RequestIdToken()
        .Build();
```
### 페이스북

```js
var perms = new List<string>() { "public_profile", "email" }; // 이메일 정보를 요구하기 위한 값입니다.  
```