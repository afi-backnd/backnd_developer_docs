---
sidebar_label: "Step 2. 길드 생성하기"
description: "길드 생성하기"
---

# 길드 생성하기

## 1. 길드 생성하기 함수 작성

사전 준비에서 작성한 [BackendGuild.cs](/sdk-docs/backend/base/guideline/guild/before)의 CreateGuild 함수에 내용을 추가합니다.  

### BackendGuild.cs

**수정 전**  

```js
public void CreateGuild(string guildName)
{
    // Step 2. 길드 생성하기
}
```

**수정 후**  

```js
public void CreateGuild(string guildName)
{
    // 10으로 들어가는 goodsCount의 경우, 사용 가능한 굿즈 종류의 갯수이며 이후 수정이 불가능하다.  
    var bro = Backend.Guild.CreateGuildV4(guildName, 10);

    if (bro.IsSuccess() == false)
    {
        Debug.LogError("길드를 생성하는중 오류가 발생했습니다. : " + bro);
        return;
    }

    Debug.Log("길드가 생성되었습니다. : " + bro);
}
```

## 2. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
**뒤끝 초기화와 뒤끝 로그인**이 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

BackendGuild.Instance.CreateGuild의 인자값에는 생성할 길드의 이름을 입력해줍니다.  

### BackendManager.cs

**수정 전**  

```js
void Test()
{
    BackendLogin.Instance.CustomLogin("user1", "1234"); // user1으로 로그인

    // 길드 로직 추가

    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
void Test()
{
    BackendLogin.Instance.CustomLogin("user1", "1234"); // user1으로 로그인
    // 길드 생성 중 412 에러가 발생할 경우, 이미 자신이 속한 길드가 있기 때문에 길드 생성이 불가능합니다.  
    // CustomSignUp("user3", "1234")로 다른 유저를 새로 생성하여 길드를 생성해주시기 바랍니다.  

    BackendGuild.Instance.CreateGuild("원하는_길드_이름"); // 길드 생성 함수
    // 오픈 길드로 생성하려면 CreateGuildV4 호출 시 세 번째 파라미터로 true를 전달할 수 있습니다.

    Debug.Log("테스트를 종료합니다.");
}
```

## 3. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/guild/guild-create-success-log.png" />

이때 로그에서 **'길드가 생성되었습니다. : statusCode : 204'**이 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [CreateGuildV4 에러케이스](/sdk-docs/backend/base/guild/create-and-join/create)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

## 4. 콘솔에서 확인

뒤끝 콘솔 `뒤끝 베이스 > 길드 관리`에서 생성한 길드가 존재하는지 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/guild/guild-create-success-console.png" />

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/guild/find-and-apply">다음  Step으로</a>
</div>
