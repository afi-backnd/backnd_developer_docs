---
sidebar_label: "Step 3. 로그인 구현하기"
description: "로그인 구현하기"
---

# 로그인 구현하기

## 1. 로그인 스크립트 작성
사전 준비에서 작성한 [BackendLogin.cs](/sdk-docs/backend/base/guideline/new-user/before)의 CustomLogin 함수에 내용을 추가합니다.  

**BackendLogin.cs**  
**수정 전**  
```js
public void CustomLogin(string id, string pw)
{
    // Step 3. 로그인 구현하기 로직
}
```

**수정 후**  
```js
public void CustomLogin(string id, string pw)
{
    Debug.Log("로그인을 요청합니다.");

    var bro = Backend.BMember.CustomLogin(id, pw);

    if (bro.IsSuccess())
    {
        Debug.Log("로그인이 성공했습니다. : " + bro);
    }
    else
    {
        Debug.LogError("로그인이 실패했습니다. : " + bro);
    }
}
```

## 2. BackendManager.cs에 함수 호출 추가
해당 함수가 호출되기 위해서는 게임 실행시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
뒤끝 초기화가 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

### BackendManager.cs
**수정 전**(Step2. 회원가입 구현하기 이후)
```js
void Test()
{
    BackendLogin.Instance.CustomSignUp("user1", "1234"); // [추가] 뒤끝 회원가입
    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  
```js
void Test()
{
    BackendLogin.Instance.CustomLogin("user1", "1234"); // [추가] 뒤끝 로그인
    Debug.Log("테스트를 종료합니다.");
}
```

### 3. 유니티에서 테스트
스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  
<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/login/login-test.png" />

이때 로그에서 **'로그인에 성공했습니다. : statusCode : 200''**이 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [CustomLogin 에러케이스](/sdk-docs/backend/base/user/custom/login)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

### 4. 콘솔에서 확인
 뒤끝 콘솔 `뒤끝베이스 > 유저 관리`에서 가입일은 유저의 회원가입이며, 최종 접속일은 해당 유저가 최신에 로그인한 날짜를 갱신해 줍니다.  
따라서 회원가입을 할 경우에는 두 일자가 동일하지만 회원가입/로그인은 일자가 다르므로 로그인 시 최종 접속일이 계속 바뀌게 됩니다.  
<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/login/login-console.png" />




