---
sidebar_label: "배드 엑세스토큰"
sidebar_position: "4"
description: "IsBadaccessTokenError"
---

# IsBadaccessTokenError  

public bool **IsBadAccessTokenError**();

## 설명

Bad AccessToken 에러 발생 시 true가 리턴됩니다.  
포함되는 에러케이스는 다음과 같습니다.  

- **유저의 Access Token이 올바르지 않거나 만료된 경우**  
  statusCode : 401  
  errorCode : BadUnauthorizedException  
  message : bad bad,accessToken,,잘못된,accessToken,입니다, 잘못된 bad,accessToken,,잘못된,accessToken,입니다 입니다

- **로그인 시, 유저의 Access Token이 올바르지 않거나 만료된 경우**  
  statusCode : 401  
  errorCode : BadUnauthorizedException  
  message : bad accessToken, 잘못된 accessToken입니다

각 경우에 따라 안내드리는 방법으로 해결이 가능합니다.  

- **로그인 이후 24시간이 지나 토큰이 만료된 경우**  
   Backend.BMember.RefreshTheBackendToken을 호출하여 토큰을 갱신

- **다른 기기에서 로그인을 하여 토큰이 만료된 경우**  
   커스텀/게스트 로그인 혹은 페더레이션 로그인 등 직접 로그인을 통해 토큰을 재발급

- **로그인 후 플레이 중인 상황에서 유저 제재(차단)로 인해 토큰이 만료된 경우**  
   제재가 이루어진 상황이기에, 재로그인 시도 시 리턴되는 사유를 안내하도록 구성  


## Example

```js
BackendReturnObject bro = Backend.GameData.Get("tableName", new Where());

if(bro.IsSuccess()) {

} else {
  if(bro.IsBadAccessTokenError()) {
    Debug.Log("액세스토큰이 만료되었을 경우");
    
    var bro2 = Backend.BMember.RefreshTheBackendToken();
    if(bro2.GetMessage() == "bad refreshToken") {
      Debug.Log("로그인 정보가 만료되었습니다.");
      }
    }
}
```
