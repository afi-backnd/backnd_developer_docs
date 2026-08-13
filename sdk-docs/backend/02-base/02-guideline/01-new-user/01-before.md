---
sidebar_label: "Step 1. 사전 준비"
description: "사전 준비"
---

# 사전 준비

로그인/회원가입 기능을 구현하기 위해서는 다음과 같은 작업들이 사전에 준비되어있어야합니다.  

1. [뒤끝 초기화 로직 삽입](/sdk-docs/backend/base/start-up)
2. 뒤끝 로그인/회원가입 전용 스크립트 생성

## 1. 뒤끝 초기화 로직 스크립트 생성

해당 예제는 [뒤끝 개발자 문서 - 시작하기](/sdk-docs/backend/base/start-up)의 **Step 9번 이후의 로직**을 다루고 있습니다.  
따라서 해당 예제를 원활하게 테스트하기 위해서는 **시작하기의 초기화 로직**이 작성되어 있어야합니다.  

BackendManager.cs의 파일 위치는 테스트에 영향을 미치지 않습니다.  

<img src="https://developer.thebackend.io/static/img/unity/guide/install/5.9.0/default_unity.png" />

### BackendManager.cs 스크립트

시작하기의 초기화 로직에 이어 **동기 함수를 비동기에서 호출해주는 Test함수**를 추가합니다.  

**수정 전**  

```js
using System.Collections;
using System.Collections.Generic;
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
    }
}
```

**수정 후**  

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

    void Test()
    {
    }
}
```

## 2. 로그인 전용 스크립트 생성

새로운 스크립트를 생성하고 이름을 **BackendLogin**으로 수정합니다.  

### BackendLogin.cs

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/login/create_script.png" />

이후 BackendLogin.cs 스크립트를 열어 내용을 다음과 같이 수정합니다.  

```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

// 뒤끝 SDK namespace 추가
using BackEnd;

public class BackendLogin
{
    private static BackendLogin _instance = null;

    public static BackendLogin Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new BackendLogin();
            }

            return _instance;
        }
    }

    public void CustomSignUp(string id, string pw)
    {
        // Step 2. 회원가입 구현하기 로직
    }

    public void CustomLogin(string id, string pw)
    {
        // Step 3. 로그인 구현하기 로직
    }

    public void UpdateNickname(string nickname)
    {
        // Step 4. 닉네임 변경 구현하기 로직
    }
}
```

