---
sidebar_label: "Step 5. 길드 굿즈 기부하기"
description: "길드 굿즈 기부하기"
---

# 길드 굿즈 기부하기

## 1. 길드 굿즈 기부하기 함수 작성

사전 준비에서 작성한 [BackendGuild.cs](/sdk-docs/backend/base/guideline/guild/before)의 ContributeGoods 함수에 내용을 추가합니다.  

### BackendGuild.cs

**수정 전**  

```js
public void ContributeGoods()
{
    // Step 5. 길드 굿즈 기부하기
}
```

**수정 후**  

```js
public void ContributeGoods()
{
    var bro = Backend.Guild.ContributeGoodsV3(goodsType.goods1, 100);

    if (bro.IsSuccess() == false)
    {
        Debug.LogError("길드 기부중 에러가 발생했습니다 . : " + bro);
    }

    Debug.Log("길드 굿즈 기부가 성공적으로 진행되었습니다. : " + bro);
}
```

## 3. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
**뒤끝 초기화와 뒤끝 로그인**이 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

이때 로그인하는 유저는 길드에 속해있는 유저가 로그인해야합니다.(Step2, 4에서 호출한 길드 마스터 혹은 Step3에서 길드 가입을 요청한 길드원)

### BackendManager.cs

**수정 전**  

```js
void Test()
{
    BackendLogin.Instance.CustomLogin("user1", "1234"); // Step 3에서 길드를 생성한 유저로 로그인해야합니다.  

    BackendGuild.Instance.AcceptGuildJoinRequest(0); // 가입 요청한 유저중 제일 최신에 신청한 유저를 수락합니다.  

    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
void Test()
{
    // Step 2에서 길드를 생성한 유저 혹은 Step 3에서 가입 요청한 유저로 로그인해야합니다.  
    BackendLogin.Instance.CustomLogin("user1", "1234");

    BackendGuild.Instance.ContributeGoods(); // 길드 굿즈 기부(추가) 함수

    Debug.Log("테스트가 종료되었습니다.");
}
```

## 4. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/guild/guild-goods-success-log.png" />

이때 로그에서 **'길드 굿즈 기부가 성공적으로 진행되었습니다. : statusCode : 204'**이 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [ContributeGoodsV3 에러케이스](/sdk-docs/backend/base/guild/goods/donate-forbidden-during-reset-time)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

## 5. 콘솔에서 확인

뒤끝 콘솔 `운영 > 길드`에서 생성된 길드의 우측 **굿즈 수정 버튼**을 클릭하여 굿즈의 값이 증가하였는지 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/guild/guild-goods-success-console.png" />
