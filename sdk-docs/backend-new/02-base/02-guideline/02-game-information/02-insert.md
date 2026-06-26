---
sidebar_label: Step 2. 게임 정보 삽입 구현하기
description: "Step 2. 게임 정보 삽입 구현하기"
---



# 게임 정보 삽입 구현하기

## 1. 게임 정보 삽입 함수 작성

사전 준비에서 작성한 [BackendGameData.cs](/sdk-docs/backend/base/guideline/game-information/before)의 GameDataInsert 함수에 내용을 추가합니다.  

### BackendGameData.cs

**수정 전**  

```js
public async Task GameDataInsert()
{
    // Step 1. 게임 정보 삽입 구현하기
}
```

**수정 후**  

```js
public async Task GameDataInsert()
{
    if (userData == null)
    {
        userData = new UserData();
    }

    Debug.Log("데이터를 초기화합니다.");
    userData.level = 1;
    userData.atk = 3.5f;
    userData.info = "친추는 언제나 환영입니다.";

    userData.equipment.Add("전사의 투구");
    userData.equipment.Add("강철 갑옷");
    userData.equipment.Add("헤르메스의 군화");

    userData.inventory.Add("빨간포션", 1);
    userData.inventory.Add("하얀포션", 1);
    userData.inventory.Add("파란포션", 1);

    Debug.Log("뒤끝 업데이트 목록에 해당 데이터들을 추가합니다.");
    Param param = new Param();
    param.Add("level", userData.level);
    param.Add("atk", userData.atk);
    param.Add("info", userData.info);
    param.Add("equipment", userData.equipment);
    param.Add("inventory", userData.inventory);


    Debug.Log("게임 정보 데이터 삽입을 요청합니다.");
    var bro = Backend.GameData.Insert("USER_DATA", param);

    if (bro.IsSuccess())
    {
        Debug.Log("게임 정보 데이터 삽입에 성공했습니다. : " + bro);

        //삽입한 게임 정보의 고유값입니다.  
        gameDataRowInDate = bro.GetRowInDate();
    }
    else
    {
        Debug.LogError("게임 정보 데이터 삽입에 실패했습니다. : " + bro);
    }
}
```

## 2. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
**뒤끝 초기화와 뒤끝 로그인**이 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

### BackendManager.cs

**수정 전**  

```js
async void Test()
{
    await BackendLogin.Instance.CustomSignIn("user1", "1234"); // 뒤끝 로그인

    // 게임 정보 기능 구현 로직 추가

    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
async void Test()
{
    await BackendLogin.Instance.CustomSignIn("user1", "1234"); // 뒤끝 로그인
    await BackendGameData.Instance.GameDataInsert(); //[추가] 데이터 삽입 함수

    Debug.Log("테스트를 종료합니다.");
}
```

## 3. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/gamedata/gamedata-insert-test.png" />

이때 로그에서 **'게임 정보 데이터 삽입에 성공했습니다. : statusCode : 200'**이 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [Create 에러케이스](/sdk-docs/backend/base/player-data/insert)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

### statusCode : 404 에러가 발생할 경우

**게임 정보 데이터 삽입에 실패했습니다 : statusCode 404 table not found**가 발생할 경우, Step 0. 사전 준비에서 설정한 테이블의 이름과 아래 코드가 들어가는 "USER_DATA"가 일치하는지 확인해주세요. 다를 경우에는 두 값을 동일한 이름으로 통일해주세요.  

```js
var reqResult = await BackndUserData.Instance.CreateDataAsync("USER_DATA", param);
```

## 4. 콘솔에서 확인

유니티 콘솔에서 성공했다는 로그가 발생했다면 뒤끝 콘솔에 `게임 정보 관리 > 데이터` 메뉴를 통해 row가 생성되었는지 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/gamedata/gamedata-insert-console.png" />

