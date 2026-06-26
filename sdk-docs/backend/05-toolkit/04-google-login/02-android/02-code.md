---
sidebar_label: "구글 로그인 코드 예제"
---

# GoogleLogin  
public void **GoogleLogin**(GoogleLoginCallback **googleLoginCallback**);  
public void **GoogleLogin**(string **webClientId**, GoogleLoginCallback **googleLoginCallback**);  

public void **GoogleLoginCallback**(bool **isSuccess**, string **errorMessage**, string **token**)

## 파라미터

| Value        | Type           | Description  |  
| :------------ |:-------------| :----- |  
| webClientId      | string | (인스펙터 미설정 시) 구글 클라우드 플랫폼에서 설정한 웹 ClientID |  
| googleLoginCallback    | GoogleLoginCallback | 로그인 요청에 대한 성공여부, 에러메시지, 토큰값을 불러오는 콜백 함수 |  

## 설명
구글 로그인을 시도합니다. 결과는 **GoogleLoginCallback** 콜백 이벤트를 통해 확인할 수 있습니다.  
단말에 연동된 계정이 하나만 있으면 구글 로그인이 성공한 이 후에는 다시 구글 로그인을 호출할 때마다  
계정 선택 없이 이전에 선택한 아이디로 로그인이 됩니다.  
계정을 다시 선택하고 싶은 경우 아래 `TheBackend.ToolKit.GoogleLogin.Android.GoogleSignOut`을 호출해야합니다.

### Example
```js
public void StartGoogleLogin() {
    TheBackend.ToolKit.GoogleLogin.Android.GoogleLogin(GoogleLoginCallback);
}

private void GoogleLoginCallback(bool isSuccess, string errorMessage, string token) {
    if (isSuccess == false) {
        Debug.LogError(errorMessage);
        return;
    }
    
    Debug.Log("구글 토큰 : " + token);
    var bro = Backend.BMember.AuthorizeFederation(token, FederationType.Google);
    Debug.Log("페데레이션 로그인 결과 : " + bro);
}
```

## ReturnCase  

### Success cases  
**구글 로그인 호출에 성공한 경우**  
true  
errorMessage : ""
token : 
```
eyJhbGciOiJSUzI1NiIsImtpZCI6Ijg1ZTU1MTA3NDY2YjdlMjk4MzYxOTljNThjNzU4MWY1YjkyM2JlNDQiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2dsZS5jb20iLCJhenAiOiI0NTk2MDkyMzQ5MDAtMGZrdHBlbjlsNzYxZjFnc3RvdTI0a2Z0dXNlcGpvZGIuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJhdWQiOiI0NTk2MDkyMzQ5MDAtbnZzaW51bDllMnBvMGU4cGk2aWdqNmlvNGNnN3VmMG4uYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJzdWIiOiIxMTcxMTQ5NTAzOTIxNjIzMzA5MjAiLCJlbWFpbCI6InJsYWd1c3RtZDAyMTdAZ21haWwuY29tIiwiZW1haWxfdmVyDSDjksdajfaskljfkjksuYDtmITsirkiLCJwaWN0dXJlIjoiaHR0cHM6Ly9saDMuZ29vZ2xldXNlcmNvbnRlbnQuY29tL2EvQUNnOG9jSXprNWExOVpaQ3MyRmJ3ZG5FV0xGdDZIZUFMRUY4OUF6TF9rbFlaRXQxPXM5Ni1jIiwiZ2l2ZW5fbmFtZSI6Iu2YhOyKuSIsImZhbWlseV9uYW1lIjoi6rmAIiwibG9jYWxlIjoia28iLCJpYXQiOjE3MDY0NDA2MjMsImV4cCI6MTcwNjQ0NDIyM30.hJx5COWQWmMejDFz76b2NxAB3IcYP3obvQ6CGvwCF6oBQG7rqZzlA9YiQhQwpzd0SM6rt-aFPlrpO6yCF8V_sh2yfI784wf1VNzlhtxoajI_LpDmdny1bWlgpHGv3hwC3lL1Wi9tP2CkEfnlfTXnKXKtsPD1KiaQa8tYZnIXDR_MvnE9FtiNA-vGCclUcUdONgcgF8_tAieKVKJxfIGx2zNNCb19910abbMwkW8-lhWp6kgd
```


### Error cases  
**실행 기기가 Android가 아닐 경우**  
false  
errorMessage : "SDK Exception : not support os: \[ platform ]"  
token : ""(string.Empty)

**구글 클라우드 플랫폼에서 설정한 정보가 잘못되어있을 경우**  
bool : false  
errorMessage :  "GoogleLogin Failed. Reason : 10: "(10:는 구글 SDK에서 리턴되는 에러메시지입니다.)  
token : ""(string.Empty)

**올바르지 않은 webClientID를 사용하였을 경우**  
bool : false  
errorMessage :  "GoogleLogin Failed. Reason : 10: "(10:는 구글 SDK에서 리턴되는 에러메시지입니다.)  
token : ""(string.Empty)

**올바르지 않은 android ClientID를 GCP에 등록하였을 경우**  
bool : false  
errorMessage :  "GoogleLogin Failed. Reason : 10: "(10:는 구글 SDK에서 리턴되는 에러메시지입니다.)  
token : ""(string.Empty)

---
# GoogleSignOut  
public void **GoogleLogin**(GoogleSignOutCallback **googleSignOutCallback**);  

public void **GoogleSignOutCallback**(bool **isSuccess**, string **errorMessage**)

## 파라미터

| Value        | Type           | Description  |  
| :------------ |:-------------| :----- |  
| googleSignOutCallback    | GoogleSignOutCallback | 로그인 요청에 대한 성공여부, 에러메시지, 토큰값을 불러오는 콜백 함수 |  

## 설명
구글 아이디를 로그아웃합니다. 이후 구글 로그인 시, 계정을 다시 선택할 수 있습니다.


### Example
```js
public void SignOutGoogleLogin() {
    TheBackend.ToolKit.GoogleLogin.Android.GoogleSignOut(GoogleSignOutCallback);
}

private void GoogleSignOutCallback(bool isSuccess, string error) {
    if (isSuccess == false) {
        Debug.Log("구글 로그아웃 에러 응답 발생 : " + error);   
    } else {
        Debug.Log("로그아웃 성공");
    }
}
```
