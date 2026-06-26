---
sidebar_label: Step 2. 회원가입 구현하기
description: "Step 2. 회원가입 구현하기"
---



# 회원가입 구현하기

## 1. 회원가입 함수 작성

사전 준비에서 작성한 [BackendLogin.cs](/sdk-docs/backend/base/guideline/new-user/before)의 CustomSignUp 함수에 내용을 추가합니다.  

### BackendLogin.cs

**수정 전**  

```js
public Task CustomSignUp(string id, string pw)
{
    // Step 2. 회원가입 구현하기 로직
}
```

**수정 후**  

```js
public async Task CustomSignUp(string id, string pw)
{
    Debug.Log("회원가입을 요청합니다.");

    var reqResult = await BackndAuth.Instance.SignUpCustomAsync(id, pw);
    if (reqResult.IsSuccess())
    {
        Debug.Log("회원가입에 성공했습니다. : " + reqResult);
    }
    else
    {
        Debug.LogError("회원가입에 실패했습니다. : " + reqResult);
    }
}
```

## 2. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
뒤끝 초기화가 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

### BackendManager.cs

**수정 전**  

```js
async void Test()
{
    // 추후 테스트 케이스 추가
    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
async void Test()
{
    await BackendLogin.Instance.CustomSignUp("user1", "1234"); // [추가] 뒤끝 회원가입 함수
    Debug.Log("테스트를 종료합니다.");
}
```

## 3. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/login/signup-test.png" />

이때 로그에서 **'회원가입이 성공했습니다. : statusCode : 201'**이 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [CustomSignUp 에러케이스](/sdk-docs/backend/base/user/custom/signup)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

### statusCode : 409 에러가 발생할 경우

로직을 변경하지 않고 회원가입을 2번 이상 한 경우, 이미 회원가입된 유저를 한번 더 회원가입하는 명령으로 인해 에러가 발생합니다.  
BackendManager.cs의 BackendLogin.Instance.CustomSignUp에 들어가는 user1을 다른 아이디로 변경해주세요.  

## 4. 콘솔에서 확인

유니티 콘솔에서 회원가입에 성공했다는 로그가 발생했다면 뒤끝 콘솔에 `뒤끝베이스 > 유저 관리` 메뉴를 통해 유저가 생성되었는지 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/login/signup-console.png" />

