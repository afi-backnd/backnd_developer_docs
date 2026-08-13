---
description: "전체 코드"
---

# 전체 코드

## BackendGameLog.cs
```js
using System.Collections.Generic;
using System.Text;
using UnityEngine;

// 뒤끝 SDK namespace 추가
using BackEnd;

public class BackendGameLog
{
    private static BackendGameLog _instance = null;

    public static BackendGameLog Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new BackendGameLog();
            }

            return _instance;
        }
    }

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
}
```

## BackendManager.cs
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
        BackendLogin.Instance.CustomLogin("user1", "1234");

        BackendGameLog.Instance.GameLogInsert(); // [추가] 게임 로그 저장 기능

        Debug.Log("테스트를 종료합니다.");
    }
}
```

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/friend/before">다음  챕터로</a>
</div>
