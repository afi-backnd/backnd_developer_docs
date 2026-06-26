---
sidebar_label: Step 4. 닉네임 변경 구현하기
---



# 닉네임 변경 구현하기

## 1. 닉네임 변경 함수 작성

사전 준비에서 작성한 [BackendLogin.cs](/sdk-docs/backend/base/guideline/new-user/before)의 UpdateNickname 함수에 내용을 추가합니다.  

### BackendLogin.cs

**수정 전**  

```js
public void UpdateNickname(string nickname)
{
    // Step 4. 닉네임 변경 구현하기 로직
}
```

**수정 후**  

```js
public void UpdateNickname(string nickname)
{
    Debug.Log("닉네임 변경을 요청합니다.");

    var bro = Backend.BMember.UpdateNickname(nickname);

    if (bro.IsSuccess())
    {
        Debug.Log("닉네임 변경에 성공했습니다 : " + bro);
    }
    else
    {
        Debug.LogError("닉네임 변경에 실패했습니다 : " + bro);
    }
}
```

## 2. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
뒤끝 초기화가 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

**BackendManager.cs**  
**수정 전**  

```js
void Test()
{
    BackendLogin.Instance.CustomLogin("user1", "1234"); // [추가] 뒤끝 로그인
    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
void Test()
{
    BackendLogin.Instance.CustomLogin("user1", "1234"); // [추가] 뒤끝 로그인

    BackendLogin.Instance.UpdateNickname("원하는 이름"); // [추가] 닉네임 변겅
    Debug.Log("테스트를 종료합니다.");
}
```

## 3. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/login/nickname-test.png" />

이때 로그에서 **'닉네임 변경에 성공했습니다. : statusCode : 204'**가 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는[UpdateNickname 에러케이스](/sdk-docs/backend/base/user/nickname/update)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

### statusCode : 409 에러가 발생할 경우

로직을 변경하지 않고 닉네임을 2번 이상 한 경우, 이미 존재하는 닉네임으로 변경하려는 명령으로 인해 에러가 발생합니다.  
BackendManager.cs의 UpdateNickname에 들어가는 인자값을 기존 입력하던 닉네임과는 다른 값으로 해주세요.  

## 4. 콘솔에서 확인

뒤끝 콘솔 `뒤끝베이스 > 유저 관리`에서 로그인 한 유저의 닉네임 칸에 변경된 닉네임이 정상적으로 표시되는지 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/login/nickname-console.png" />

