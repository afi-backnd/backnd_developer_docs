---
sidebar_label: Step 4. 게임 정보 수정 구현하기
---



# 게임 정보 수정 구현하기

## 1. 게임 정보 수정 함수 작성

사전 준비에서 작성한 [BackendGameData.cs](/sdk-docs/backend/base/guideline/game-information/before)의 LevelUp 함수와 GameDataUpdate 함수에 내용을 추가합니다.  

### BackendGameData.cs

**수정 전**  

```js
public void LevelUp()
{
    // Step 4. 게임 정보 수정 구현하기
}

public async Task GameDataUpdate()
{
    // Step 4. 게임 정보 수정 구현하기
}
```

**수정 후**  

```js
public void LevelUp()
{
    Debug.Log("레벨을 1 증가시킵니다.");
    userData.level += 1;
    userData.atk += 3.5f;
    userData.info = "내용을 변경합니다.";
}

// 게임 정보 수정하기
public async Task GameDataUpdate()
{
    if (userData == null)
    {
        Debug.LogError("서버에서 다운받거나 새로 삽입한 데이터가 존재하지 않습니다. Insert 혹은 Get을 통해 데이터를 생성해주세요.");
        return;
    }

    Param param = new Param();
    param.Add("level", userData.level);
    param.Add("atk", userData.atk);
    param.Add("info", userData.info);
    param.Add("equipment", userData.equipment);
    param.Add("inventory", userData.inventory);

    RequestResult reqResult = null;
    if (string.IsNullOrEmpty(gameDataRowInDate))
    {
        Debug.Log("내 제일 최신 게임 정보 데이터 수정을 요청합니다.");

        reqResult = await BackndUserData.Instance.UpdateLatestDataAsync("USER_DATA", param);
    }
    else
    {
        Debug.Log($"{gameDataRowInDate}의 게임 정보 데이터 수정을 요청합니다.");

        reqResult = await BackndUserData.Instance.UpdateDataAsync("USER_DATA", gameDataRowInDate, param);
    }

    if (reqResult.IsSuccess())
    {
        Debug.Log("게임 정보 데이터 수정에 성공했습니다. : " + reqResult);
    }
    else
    {
        Debug.LogError("게임 정보 데이터 수정에 실패했습니다. : " + reqResult);
    }
}
```

## 2. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
**뒤끝 초기화와 뒤끝 로그인**이 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

### BackendManager.cs

**수정 전**(Step 3. 게임 정보 불러오기 이후)

```js
async void Test()
{        
    await BackendLogin.Instance.CustomSignIn("user1", "1234"); // 뒤끝 로그인
    await BackendGameData.Instance.GameDataGet(); //[추가] 데이터 불러오기 함수

    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
void Test()
{
    BackendLogin.Instance.CustomLogin("user1", "1234"); // 뒤끝 로그인
    BackendGameData.Instance.GameDataGet(); // 데이터 삽입 함수

    // [추가] 서버에 불러온 데이터가 존재하지 않을 경우, 데이터를 새로 생성하여 삽입
    if (BackendGameData.userData == null)
    {
        BackendGameData.Instance.GameDataInsert();
    }

    BackendGameData.Instance.LevelUp(); // [추가] 로컬에 저장된 데이터를 변경
    BackendGameData.Instance.GameDataUpdate(); //[추가] 서버에 저장된 데이터를 덮어쓰기(변경된 부분만)

    Debug.Log("테스트를 종료합니다.");
}
```

## 3. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/gamedata/gamedata-update-test.png" />

이때 로그에서 **'내 제일 최신 게임 정보 데이터 수정을 요청합니다 : statusCode : 204'**이 발생하여야 정상적으로 진행된 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [Update 에러케이스](/sdk-docs/backend/base/player-data/update)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

## 4. 콘솔에서 확인

유니티 콘솔에서 성공했다는 로그가 발생했다면 뒤끝 콘솔 `뒤끝베이스 > 게임 정보 관리 > 데이터` 메뉴를 통해 row가 변경되었는지 확인합니다.  
함수를 호출할 때마다 값이 증가하므로 아래 예제 값하고는 다를 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/gamedata/gamedata-update-console.png" />

