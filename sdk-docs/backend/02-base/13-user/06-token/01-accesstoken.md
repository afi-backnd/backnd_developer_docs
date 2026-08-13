---
description: "AccessToken"
---

# AccessToken
**AccessToken은 뒤끝에서 유저를 인증하기 위한 수단으로 사용되는 값**입니다.  
뒤끝의 다양한 기능들을 호출할 때 이를 이용하여 유저를 인증/식별합니다.  
AccessToken은 유저가 회원가입/로그인을 할 때 발급되고, 기기 로컬에 저장됩니다.  

AccessToken에는 access_token과 refresh_token 2가지가 존재합니다.  

| 토큰 종류 | 설명 |
|---| ---|
|access_token | 1일 동안 이용 가능한 토큰입니다. 1일이 지나면 만료되어 사용하지 못합니다.|
|refresh_token | 1년 동안 이용 가능한 토큰입니다. 1년이 지나면 만료되어 사용하지 못합니다.|

* AccessToken은 게임을 종료해도 만료되지 않습니다.  
* AccessToken은 Logout을 한 경우 만료됩니다.  
* AccessToken은 로그인을 할 때마다 새로 발급됩니다.  

