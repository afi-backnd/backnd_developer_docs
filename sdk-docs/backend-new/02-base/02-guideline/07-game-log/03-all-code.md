---
description: "전체 코드"
---



# 전체 코드

## BackendGameLog.cs
```js
using Newtonsoft.Json.Linq;
using System.Collections.Generic;
using System.Threading.Tasks;
using UnityEngine;
// 뒤끝 SDK namespace 추가
using BACKND.Base;

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

    public async Task GameLogInsert()
    {
        Param param = new Param();

        param.Add("clearStage", 1);
        param.Add("currentMoney", 100000);

        Debug.Log("게임 로그 삽입을 시도합니다.");

        var reqResult = await BackndLog.Instance.SendLogAsync("ClearStage", param);
        if (reqResult.IsSuccess() == false)
        {
            Debug.LogError("게임 로그 삽입 중 에러가 발생했습니다. : " + reqResult);
            return;
        }

        Debug.Log("게임 로그 삽입에 성공했습니다. : " + reqResult);
    }
}
```

## BackendManager.cs
```js
using UnityEngine;
// 뒤끝 SDK namespace 추가
using BACKND.Base;

public class BackendManager : MonoBehaviour
{
    async void Start()
    {
        // 뒤끝 초기화
        var reqResult = await BackndBase.InitializeAsync();
        // 뒤끝 초기화에 대한 응답값
        if (reqResult.IsSuccess())
        {
            Debug.Log("초기화 성공 : " + reqResult); // 성공일 경우 statusCode 204 Success
        }
        else
        {
            Debug.LogError("초기화 실패 : " + reqResult); // 실패일 경우 statusCode 400대 에러 발생
        }

        Test();
    }
    
    async void Test()
    {
        await BackendLogin.Instance.CustomLogin("user1", "1234");

        await BackendGameLog.Instance.GameLogInsert(); // [추가] 게임 로그 저장 기능

        Debug.Log("테스트를 종료합니다.");
    }
}
```

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/friend/before">다음  챕터로</a>
</div>
