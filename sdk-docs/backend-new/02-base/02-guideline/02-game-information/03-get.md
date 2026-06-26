---
sidebar_label: Step 3. 게임 정보 불러오기 구현하기
description: "Step 3. 게임 정보 불러오기 구현하기"
---



# 게임 정보 불러오기 구현하기

## 1. 게임 정보 불러오기 함수 작성

사전 준비에서 작성한 [BackendGameData.cs](/sdk-docs/backend/base/guideline/game-information/before)의 GameDataGet 함수에 내용을 추가합니다.  

### BackendGameData.cs

**수정 전**  

```js
public async Task GameDataGet()
{
    // Step 3. 게임 정보 불러오기 구현하기
}
```

**수정 후**  

```js
public async Task GameDataGet()
{
    Debug.Log("게임 정보 조회 함수를 호출합니다.");
    
    var reqResult = await BackndUserData.Instance.GetDataAsync("USER_DATA");
    if (reqResult.IsSuccess())
    {
        Debug.Log("게임 정보 조회에 성공했습니다. : " + reqResult);

        // Json으로 리턴된 데이터를 받아옵니다.  
        var gameDataJson = reqResult.GetRows();

        // 받아온 데이터의 갯수가 0이라면 데이터가 존재하지 않는 것입니다.  
        if (gameDataJson.Count <= 0)
        {
            Debug.LogWarning("데이터가 존재하지 않습니다.");
        }
        else
        {
            gameDataRowInDate = gameDataJson[0]["inDate"].ToString(); //불러온 게임 정보의 고유값입니다.  

            userData = new UserData();
            userData.level = int.Parse(gameDataJson[0]["level"].ToString());
            userData.atk = float.Parse(gameDataJson[0]["atk"].ToString());
            userData.info = gameDataJson[0]["info"].ToString();
            
            var inventoryJson = gameDataJson[0]["inventory"] as JObject;
            foreach (var property in inventoryJson.Properties())
            {
                var itemKey = property.Name;
                var itemValue = property.Value;
                userData.inventory.Add(itemKey, int.Parse(itemValue.ToString()));
            }

            var equipmentJson = gameDataJson[0]["equipment"] as JArray;
            foreach (var equip in equipmentJson)
            {
                userData.equipment.Add(equip.ToString());
            }

            Debug.Log(userData.ToString());
        }
    }
    else
    {
        Debug.LogError("게임 정보 조회에 실패했습니다. : " + reqResult);
    }
}
```

## 2. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
**뒤끝 초기화와 뒤끝 로그인**이 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

### BackendManager.cs

**수정 전**(Step 2. 게임 정보 삽입 구현하기)

```js
async void Test()
{
    await BackendLogin.Instance.CustomSignIn("user1", "1234"); // 뒤끝 로그인
    await BackendGameData.Instance.GameDataInsert(); //[추가] 데이터 삽입 함수

    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
async void Test()
{        
    await BackendLogin.Instance.CustomSignIn("user1", "1234"); // 뒤끝 로그인
    await BackendGameData.Instance.GameDataGet(); //[추가] 데이터 불러오기 함수

    Debug.Log("테스트를 종료합니다.");
}
```

## 3. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  
각 데이터를 파싱한 로그들이 정상적으로 출력되는지 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/gamedata/gamedata-get-test.png" />

이때 로그에서 아이템 이름들이 정상적으로 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [GetData 에러케이스](/sdk-docs/backend/base/player-data/get/get-my-data)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

