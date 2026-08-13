---
sidebar_label: "Step 3. 길드 찾아 가입 요청하기"
description: "길드 찾아 가입 요청하기"
---

# 길드 찾아 가입 요청하기

## 1. 길드 찾아 가입 요청하기 함수 작성

사전 준비에서 작성한 [BackendGuild.cs](/sdk-docs/backend/base/guideline/guild/before)의 RequestGuildJoin 함수에 내용을 추가합니다.  

### BackendGuild.cs

**수정 전**  

```js
public void RequestGuildJoin(string guildName)
{
    // Step 3. 길드 찾아 가입 요청하기
}
```

**수정 후**  

```js
public void RequestGuildJoin(string guildName)
{
    var bro = Backend.Guild.GetGuildIndateByGuildNameV3(guildName);

    if (bro.IsSuccess() == false)
    {
        Debug.LogError($"{guildName}을 검색하는 중 에러가 발생했습니다. : " + bro);
        return;
    }

    string guildInDate = bro.GetFlattenJSON()["guildInDate"].ToString();

    bro = Backend.Guild.ApplyGuildV3(guildInDate);

    if (bro.IsSuccess() == false)
    {
        Debug.LogError($"{guildName}({guildInDate})에게 가입 요청을 보내는 중 에러가 발생했습니다. : " + bro);
        return;
    }

    Debug.Log($"{guildName}({guildInDate})의 길드 가입이 정상적으로 요청되었습니다. : " + bro);
}
```

## 2. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
**뒤끝 초기화와 뒤끝 로그인**이 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

이때 로그인 되는 유저는 **Step 2. 길드 생성하기**를 호출한 유저가 아닌 다른 유저가 로그인해야합니다.  

### BackendManager.cs

**수정 전**(Step 2. 길드 생성하기)

```js
void Test()
{
    BackendLogin.Instance.CustomLogin("user1", "1234"); // user1으로 로그인
    // 길드 생성 중 412 에러가 발생할 경우, 이미 자신이 속한 길드가 있기 때문에 길드 생성이 불가능합니다.  
    // CustomSignUp("user3", "1234")로 다른 유저를 새로 생성하여 길드를 생성해주시기 바랍니다.  

    BackendGuild.Instance.CreateGuild("원하는_길드_이름"); // 길드 생성 함수

    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
void Test()
{
    // 처음 함수를 호출할 경우
    BackendLogin.Instance.CustomSignUp("guildUser", "1234"); // 길드용 유저 새로 회원가입
    BackendLogin.Instance.UpdateNickname("guildUser"); // 길드용 유저 닉네임 등록

    // 두번째로 함수를 호출할 경우
    //BackendLogin.Instance.CustomLogin("guildUser", "1234"); // 길드용 유저 새로 회원가입

    // Step 2에서 CreateGuild에 입력한 길드이름을 인자값으로 입력해주세요
    BackendGuild.Instance.RequestGuildJoin("원하는_길드_이름");

    Debug.Log("테스트를 종료합니다.");
}
```

## 4. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/guild/guild-request-success-log.png" />

이때 로그에서 **'ㅇㅇㅇ의 길드 가입이 정상적으로 요쳥되었습니다. : statusCode : 204'**이 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [GetGuildIndateByGuildNameV3
](/sdk-docs/backend/base/guild/search/get-guild-by-indate) 와 [ApplyGuildV3 에러케이스](/sdk-docs/backend/base/guild/create-and-join/request-join)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

### 에러케이스

회원가입에 실패했습니다. : statusCode : 409  
errorCode : DuplicatedParameterException  
message : Duplicated customId, 중복된 customId 입니다

> 회원가입하려는 아이디가 이미 존재할 경우입니다. 함수를 두번하게 된다면 이미 회원가입이 진행한 이후에 호출되므로 해당 에러가 발생할 수 있습니다. 아래 주석처리 된 BackendLogin.Instance.CustomLogin을 이용해주세요.  

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/guild/apply-request">다음  Step으로</a>
</div>
