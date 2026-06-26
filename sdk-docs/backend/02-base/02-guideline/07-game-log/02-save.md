---
sidebar_label: Step 2. 게임 로그 저장하기
---



# 게임 로그 저장하기

## 1. 게임 로그 삽입 함수 작성

사전 준비에서 작성한 [BackendGameLog.cs](/sdk-docs/backend/base/guideline/game-log/before)의 GameLogInsert 함수에 내용을 추가합니다.  

### BackendGameLog.cs

**수정 전**  

```js
public void GameLogInsert()
{
    // Step 2. 게임 로그 저장하기 로직 추가
}
```

**수정 후**  

```js
public void GameLogInsert()
{
    Param param = new Param();

    param.Add("clearStage", 1);
    param.Add("currentMoney", 100000);

    Debug.Log("게임 로그 삽입을 시도합니다.");

    var bro = Backend.GameLog.InsertLog("ClearStage", param);

    if (bro.IsSuccess() == false)
    {
        Debug.LogError("게임 로그 삽입 중 에러가 발생했습니다. : " + bro);
        return;
    }

    Debug.Log("게임 로그 삽입에 성공했습니다. : " + bro);
}
```

## 2. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
**뒤끝 초기화와 뒤끝 로그인**이 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

### BackendManager.cs

**수정 전**  

```js
void Test()
{
    BackendLogin.Instance.CustomLogin("user1", "1234");

    // 게임 로그 저장 내용 추가

    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
void Test()
{
    BackendLogin.Instance.CustomLogin("user1", "1234");

    BackendGameLog.Instance.GameLogInsert(); // [추가] 게임 로그 저장 기능

    Debug.Log("테스트를 종료합니다.");
}
```

## 3. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/gamelog/gamelog-success-log.png" />

이때 로그에서 **'게임 로그 삽입에 성공했습니다. : statusCode : 204'**이 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [InsertLog 에러케이스](/sdk-docs/backend/base/game-log/old-insert)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

## 4. 콘솔에서 확인

게임 로그에서 성공하였다는 로그가 확인되었다면 뒤끝 콘솔 `뒤끝베이스 > 로그 관리`에서 행동 유형을 클릭하여 삽입하였던 행동 유형을 선택합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/gamelog/gamelog-success-console.png" />

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/friend/before">다음  챕터로</a>
</div>
