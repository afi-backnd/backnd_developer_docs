---
sidebar_label: "사용 예제"
description: "유저의 재화 정보 서버에 저장하기"
---

# 유저의 재화 정보 서버에 저장하기

유저의 소중한 게임 재화를 안전하게 서버에 저장하세요.  
서버 저장소 설정부터 클라이언트에서 데이터 수정을 요청하기까지 모든 과정을 안내해드립니다.

## 1. 시작하기 전에

### 유저가 존재해야 합니다.  

'유저' 상단 메뉴에서 [게임 유저 생성]을 클릭해서 간단하게 유저를 생성할 수 있습니다.  
아이디 'user1', 비밀번호 '1234'로 유저를 생성하세요.  

![create user](/img/docs/guide/base/how-to/create-user.png)

## 2. 서버 저장소 설정하기

### 재화 정보를 저장할 테이블을 생성하세요.

뒤끝 콘솔의 '게임 정보' 메뉴 상단에서 [테이블 생성]을 클릭하세요.  
재화 정보는 보안이 중요하기 때문에, 유저 본인만 접근할 수 있는 Private 테이블로 설정하고, 스키마 미사용, 이름을 "GAME_MONEY"로 입력하고 생성합니다.  

![create table](/img/docs/guide/base/how-to/create-table.png)

## 3. 클라이언트에서 데이터 주고 받기

### 스크립트를 생성하세요.

새로운 스크립트를 생성하고, 이름을 'GameMoney.cs'로 설정합니다.  
해당 스크립트를 열어 내용을 다음과 같이 수정합니다.  

```js
using System.Collections.Generic;
using System.Text;
using UnityEngine;

// 뒤끝 SDK namespace 추가
using BackEnd;

public class GameMoney {
    public int gold = 100;
	public int diamond = 1;
	public int heart = 10;

    // 데이터를 디버깅하기 위한 함수입니다. (Debug.Log(GameMoney);)
    public override string ToString() {
        StringBuilder result = new StringBuilder();
        result.AppendLine($"gold : {gold}");
        result.AppendLine($"diamond : {diamond}");
        result.AppendLine($"heart : {heart}");
        return result.ToString();
    }
}

public class BackendGameData {
    private static BackendGameData _instance = null;

    public static BackendGameData Instance {
        get {
            if(_instance == null) {
                _instance = new BackendGameData();
            }

            return _instance;
        }
    }

    public static GameMoney gameMoney;

    public void InitializeGameMoney() {
        // Step 1. 게임 재화 초기화하기
    }
    
    public void GetGameMoney() {
        // Step 2. 게임 재화 불러오기
    }

    public void IncreaseHeart() {
        // heart 추가하기
    }
    
    public void UpdateGameMoney() {
        // Step 3. 게임 재화 수정하기
    }
}
```

### 게임 재화 초기화 함수를 작성하세요.

유저 계정이 새로 생성되었을 때, 해당 유저의 게임 재화를 초기값으로 설정하는 함수를 호출해야 합니다.  
함수를 아래와 같이 작성하세요.  

```js
public void InitializeGameMoney() {
    // Step 1. 게임 재화 초기화하기
    
    if(gameMoney == null) {
        gameMoney = new GameMoney();
    }

    Debug.Log("데이터를 초기화합니다.");
    gameMoney.gold = 100;
    gameMoney.diamond = 1;
    gameMoney.heart = 10;

    Param param = new Param();
    param.Add("gold", gameMoney.gold);
    param.Add("diamond", gameMoney.diamond);
    param.Add("heart", gameMoney.heart);

    Debug.Log("'GAME_MONEY' 테이블에 새로운 데이터 행을 추가합니다.");
    var bro = Backend.GameData.Insert("GAME_MONEY", param);

    if(bro.IsSuccess()) {
        Debug.Log("데이터를 추가하는데 성공했습니다. : " + bro);
    } else {
        Debug.LogError("데이터를 추가하는데 실패했습니다. : " + bro);
    }
}
```

### 데이터를 서버에서 불러오는 함수를 작성하세요.

```js
public void GetGameMoney() {
    // Step 2. 게임 재화 불러오기

    Debug.Log("'GAME_MONEY' 테이블의 데이터를 조회하는 함수를 호출합니다.");
    var bro = Backend.GameData.GetMyData("GAME_MONEY", new Where());
    if(bro.IsSuccess()) {
        Debug.Log("데이터 조회에 성공했습니다. : " + bro);
        
        // Json으로 리턴된 데이터를 받아옵니다.
        LitJson.JsonData gameDataJson = bro.FlattenRows(); 
        
        // 받아온 데이터의 갯수가 0이라면 데이터가 존재하지 않는 것입니다.
        if(gameDataJson.Count <= 0) {
            Debug.LogWarning("데이터가 존재하지 않습니다.");
        } else {
            gameMoney = new GameMoney();
            gameMoney.gold = int.Parse(gameDataJson[0]["gold"].ToString());
            gameMoney.diamond = float.Parse(gameDataJson[0]["diamond"].ToString());
            gameMoney.heart = gameDataJson[0]["heart"].ToString();
            Debug.Log(gameMoney.ToString());
        }
    } else {
        Debug.LogError("데이터 조회에 실패했습니다. : " + bro);
    }
}
```

### 데이터를 서버에 업데이트하는 함수를 작성하세요.

```js
public void IncreaseHeart() {
    // heart 추가하기

    Debug.Log("heart를 1 증가시킵니다.")
    gameMoney.heart += 1;
}

public void UpdateGameMoney() {
    // Step 3. 게임 재화 수정하기
    
    if(gameMoney == null) {
        Debug.LogError("데이터가 존재하지 않습니다. Initialize 혹은 Get을 통해 데이터를 생성해주세요.");
        return;
    }

    Param param = new Param();
    param.Add("gold", gameMoney.gold);
    param.Add("diamond", gameMoney.diamond);
    param.Add("heart", gameMoney.heart);

    BackendReturnObject bro = null;
    
    Debug.Log("나의 데이터를 수정합니다");
    bro = Backend.GameData.Update("GAME_MONEY", new Where(), param);
    
    if(bro.IsSuccess()) {
        Debug.Log("데이터 수정에 성공했습니다. : " + bro);
    } else {
        Debug.LogError("데이터 수정에 실패했습니다. : " + bro);
    }
}
```

### BackendManager.cs에서 함수를 호출하세요.

위에서 작성한 함수가 실행되려면, 게임을 실행했을 때 자동으로 실행되는 BackendManager에서 호출해야 합니다.  
뒤끝 초기화와 뒤끝 로그인이 이루어진 이후에 위 함수들을 호출하세요.  

```js
using UnityEngine;
using System.Threading.Tasks;

// 뒤끝 SDK namespace 추가
using BackEnd;

public class BackendManager : MonoBehaviour {
    void Start() {
        var bro = Backend.Initialize(); // 뒤끝 초기화

        // 뒤끝 초기화에 대한 응답값
        if(bro.IsSuccess()) {
            Debug.Log("초기화 성공 : " + bro); // 성공일 경우 statusCode 204 Success
        } else {
            Debug.LogError("초기화 실패 : " + bro); // 실패일 경우 statusCode 400대 에러 발생 
        }

        Test();
    }

    // 동기 함수를 비동기에서 호출하게 해주는 함수(유니티 UI 접근 불가)
    async void Test() {
        await Task.Run(() => {
            BackendLogin.Instance.CustomLogin("user1", "1234"); // 뒤끝 로그인

            // highlight-start
            BackendGameData.Instance.GetGameMoney(); // 데이터를 초기화 합니다

            // 불러오려는 데이터가 존재하지 않을 경우, 데이터를 새로 추가합니다.
            if(BackendGameData.gameMoney == null) {
                BackendGameData.Instance.InitializeGameMoney();
            }

            BackendGameData.Instance.IncreaseHeart(); // 로컬에 저장된 데이터를 변경합니다.

            BackendGameData.Instance.UpdateGameMoney(); //서버에 저장된 데이터를 업데이트합니다.
            // highlight-end

            Debug.Log("테스트를 종료합니다.");
        });
    }
}
```
