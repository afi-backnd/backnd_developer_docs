---
sidebar_position: "1"
description: "공통 에러 처리 예제"
---

# 공통 에러 처리 예제

뒤끝에서 많이 발생하는 에러들에 대한 예외 처리입니다.  
게임 로직에 맞게 문제가 발생하면 팝업창을 띄우거나, 로그인 화면으로 이동시키거나, 유저가 수동으로 함수를 재 호출할 수 있게끔 구성해주시기 바랍니다.  

## Example

```js
    public void GetData(int maxRepeatCount)
    {
        if(maxRepeatCount <= 0)
        {
            //게임 문제 발생
            return;
        }

        Backend.GameData.Get("score", new Where(), callback =>
         {
             if(callback.IsSuccess())
             {

             }
             else
             {
                 if(callback.IsClientRequestFailError()) // 클라이언트의 일시적인 네트워크 끊김 시
                 {
                     GetData(maxRepeatCount - 1);
                 }
                 else if(callback.IsServerError()) // 서버의 이상 발생 시
                 {
                     GetData(maxRepeatCount - 1);
                 }
                 else if(callback.IsMaintenanceError()) // 서버 상태가 '점검'일 시
                 {
                     //점검 팝업창 + 로그인 화면으로 보내기
                     Debug.Log("게임 점검중입니다.");
                     return;
                 }
                 else if(callback.IsTooManyRequestError()) // 단기간에 많은 요청을 보낼 경우 발생하는 403 Forbbiden 발생 시
                 {
                     //단기간에 많은 요청을 보내면 발생합니다. 5분동안 뒤끝의 함수 요청을 중지해야합니다.  
                     return;
                 }
                 else if(callback.IsBadAccessTokenError())
                 {
                     bool isRefreshSuccess = RefreshTheBackendToken(3); // 최대 3번 리프레시 실행

                     if(isRefreshSuccess)
                     {
                         GetData(maxRepeatCount - 1);
                     }
                     else
                     {
                     }
                 }
             }
         });
    }

    //리프레시 토큰의 경우 로그인과 관련된 중요한 함수이므로 동기로 하는 것을 추천
    bool RefreshTheBackendToken(int maxRepeatCount)
    {
        if(maxRepeatCount <= 0)
        {
            return false;
        }
        var callback = Backend.BMember.RefreshTheBackendToken();
        if(callback.IsSuccess())
        {
            return true;
        }
        else
        {
            if(callback.IsClientRequestFailError()) // 클라이언트의 일시적인 네트워크 끊김 시
            {
                return RefreshTheBackendToken(maxRepeatCount - 1);
            }
            else if(callback.IsServerError()) // 서버의 이상 발생 시
            {
                return RefreshTheBackendToken(maxRepeatCount - 1);
            }
            else if(callback.IsMaintenanceError()) // 서버 상태가 '점검'일 시
            {
                //점검 팝업창 + 로그인 화면으로 보내기
                return false;
            }
            else if(callback.IsTooManyRequestError()) // 단기간에 많은 요청을 보낼 경우 발생하는 403 Forbbiden 발생 시
            {
                //너무 많은 요청을 보내는 중
                return false;
            }
            else
            {
                //재시도를 해도 액세스토큰 재발급이 불가능한 경우
                //커스텀 로그인 혹은 페데레이션 로그인을 통해 수동 로그인을 진행해야합니다.  
                //중복 로그인일 경우 401 bad refreshToken 에러와 함께 발생할 수 있습니다.  
                Debug.Log("게임 접속에 문제가 발생했습니다. 로그인 화면으로 돌아갑니다\n" + callback.ToString());
                return false;
            }
        }
    }
```
