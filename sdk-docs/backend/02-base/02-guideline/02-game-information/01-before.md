---
sidebar_label: "Step 1. 사전 준비"
description: "사전 준비"
---

# 사전 준비
게임 정보관리 기능을 구현하기 위해서는 다음과 같은 작업들이 사전에 준비되어 있어야 합니다.  
1. [완성된 로그인 함수 로직](/sdk-docs/backend/base/guideline/new-user/all-code) 
2. 뒤끝 콘솔 게임 정보 테이블 생성
3. 게임 정보 전용 스크립트 생성

## 1. 완성된 로그인 함수 로직
로그인/회원가입 외에 모든 뒤끝 기능은 로그인이 진행된 이후에 정상적으로 함수를 호출할 수 있습니다.  
만약 로그인 로직이 구현되지 않으셨을 경우 [1. 로그인/회원가입 구현하기](/sdk-docs/backend/base/guideline/new-user/login) 가이드에 따라 로그인 로직을 구현해주시기 바랍니다.  

## 2. 뒤끝 콘솔 게임 정보 테이블 생성
뒤끝 콘솔 `뒤끝베이스 > 게임 정보 관리 > 테이블`에서 상단 '테이블 생성' 버튼을 클릭합니다.  
<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/gamedata/gamedata-console1.png" />

분류는 **private**, 스키마 정의는 **미사용**인 테이블을 생성합니다.  
해당 예제에서는 테이블 명을 **USER_DATA**로 칭하였습니다.  

만약 테이블 명을 USER_DATA가 아닌 다른 이름으로 설정하였을 경우, 각 예제에 존재하는 "USER_DATA"라는 값을 설정하신 이름으로 변경해주세요.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/gamedata/gamedata-console2.png" />

생성된 테이블을 클릭하여 `게임 정보 관리 > 데이터`로 이동합니다.  
<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/gamedata/gamedata-console3.png" />

해당 테이블의 데이터는 테스트 이후에 생성될 예정입니다.  
<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/gamedata/gamedata-console4.png" />

## 3. 게임 정보 전용 스크립트 생성
새로운 스크립트를 생성하고 이름을 **BackendGameData**으로 수정합니다.  
이후 BackendGameData.cs 스크립트를 열어 내용을 다음과 같이 수정합니다.  

```js
using System.Collections.Generic;
using System.Text;
using UnityEngine;

// 뒤끝 SDK namespace 추가
using BackEnd;

public class UserData
{
    public int level = 1;
    public float atk = 3.5f;
    public string info = string.Empty;
    public Dictionary<string, int> inventory = new Dictionary<string, int>();
    public List<string> equipment = new List<string>();

    // 데이터를 디버깅하기 위한 함수입니다.(Debug.Log(UserData);)
    public override string ToString()
    {
        StringBuilder result = new StringBuilder();
        result.AppendLine($"level : {level}");
        result.AppendLine($"atk : {atk}");
        result.AppendLine($"info : {info}");

        result.AppendLine($"inventory");
        foreach (var itemKey in inventory.Keys)
        {
            result.AppendLine($"| {itemKey} : {inventory[itemKey]}개");
        }

        result.AppendLine($"equipment");
        foreach (var equip in equipment)
        {
            result.AppendLine($"| {equip}");
        }

        return result.ToString();
    }
}

public class BackendGameData
{
    private static BackendGameData _instance = null;

    public static BackendGameData Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new BackendGameData();
            }

            return _instance;
        }
    }

    public static UserData userData;

    private string gameDataRowInDate = string.Empty;

    public void GameDataInsert()
    {
        // Step 2. 게임 정보 삽입 구현하기 
    }

    public void GameDataGet()
    {
        // Step 3. 게임 정보 불러오기 구현하기
    }

    public void LevelUp()
    {
        // Step 4. 게임 정보 수정 구현하기
    }

    public void GameDataUpdate()
    {
        // Step 4. 게임 정보 수정 구현하기
    }
}
```

### BackendManager.cs
```js
using UnityEngine;


// 뒤끝 SDK namespace 추가
using BackEnd;

public class BackendManager : MonoBehaviour
{
    void Start()
    {
        var bro = Backend.Initialize(); // 뒤끝 초기화

        // 뒤끝 초기화에 대한 응답값
        if (bro.IsSuccess())
        {
            Debug.Log("초기화 성공 : " + bro); // 성공일 경우 statusCode 204 Success
        }
        else
        {
            Debug.LogError("초기화 실패 : " + bro); // 실패일 경우 statusCode 400대 에러 발생 
        }

        Test();
    }

    // 동기 함수를 비동기에서 호출하게 해주는 함수(유니티 UI 접근 불가)
    void Test()
    {
        BackendLogin.Instance.CustomLogin("user1", "1234"); // 뒤끝 로그인

        // 게임 정보 기능 구현 로직 추가

        Debug.Log("테스트를 종료합니다.");
    }
}
```


