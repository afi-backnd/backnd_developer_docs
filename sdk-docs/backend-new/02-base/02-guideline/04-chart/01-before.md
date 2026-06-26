---
sidebar_label: Step 1. 사전 준비
description: "Step 1. 사전 준비"
---



# 사전 준비

차트 기능을 구현하기 위해서는 다음과 같은 작업들이 사전에 준비되어 있어야 합니다.  

1. [완성된 로그인 함수 로직](/sdk-docs/backend/base/guideline/new-user/all-code)
2. 차트 전용 스크립트 생성

## 1. 완성된 로그인 함수 로직

로그인/회원가입 외에 모든 뒤끝 기능은 로그인이 진행된 이후에 정상적으로 함수를 호출할 수 있습니다.  
만약 로그인 로직이 구현되지 않으셨을 경우 [1. 로그인/회원가입 구현하기](/sdk-docs/backend/base/guideline/new-user/all-code) 가이드에 따라 로그인 로직을 구현해주시기 바랍니다.  

## 2. 차트 전용 스크립트 생성

새로운 스크립트를 생성하고 이름을 **BackendChart**으로 수정합니다.  
이후 BackendChart.cs 스크립트를 열어 내용을 다음과 같이 수정합니다.  

### BackendChart.cs

```js
using System.Collections.Generic;
using System.Text;
using UnityEngine;

// 뒤끝 SDK namespace 추가
using BACKND.Base;

public class BackendChart
{
    private static BackendChart _instance = null;

    public static BackendChart Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new BackendChart();
            }

            return _instance;
        }
    }

    public async Task ChartGet(string chartId)
    {
        // Step 3. 차트 정보 가져오기 내용 추가
    }
}
```

### BackendManager.cs

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
        await BackendLogin.Instance.CustomLogin("user1", "1234"); // 뒤끝 로그인 함수

        // 차트 정보 불러오기 로직 추가

        Debug.Log("테스트를 종료합니다.");
    }
}
```

