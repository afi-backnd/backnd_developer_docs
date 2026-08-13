---
sidebar_label: "Step 1. 사전 준비"
description: "사전 준비"
---

# 사전 준비

친구 기능을 구현하기 위해서는 다음과 같은 작업들이 사전에 준비되어 있어야 합니다.  

1. 친구 최대 보유수 설정
2. [완성된 로그인 로직 함수 작성](/sdk-docs/backend/base/guideline/new-user/login)
3. 친구기능 전용 스크립트 생성

## 1. 뒤끝 콘솔에서 친구 최대 보유수 설정

뒤끝 콘솔에서 친구의 최대 보유수를 설정해야합니다.  
뒤끝 콘솔 `서버 설정 - 소셜`에서 친구 최대 보유수를 100으로 지정합니다.(최대 수 : 100)

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/friend/friend-setting-console.png" />

## 2. 완성된 로그인 함수 로직

로그인/회원가입 외에 모든 뒤끝 기능은 로그인이 진행된 이후에 정상적으로 함수를 호출할 수 있습니다.  
만약 로그인 로직이 구현되지 않으셨을 경우 [1. 로그인/회원가입 구현하기](/sdk-docs/backend/base/guideline/new-user/login) 가이드에 따라 로그인 로직을 구현해주시기 바랍니다.  

## 3. 친구 기능 전용 스크립트 생성

새로운 스크립트를 생성하고 이름을 **BackendFriend**으로 수정합니다.  
이후 BackendFriend.cs 스크립트를 열어 내용을 다음과 같이 수정합니다.  

만약 Tuple<string, string>에서 에러가 발생할 경우에는 코드를 참고하며 BackendFriend.cs 최상단에 `using System.Collections.Generic` 코드를 추가해주세요.  

### BackendFriend.cs

```js
using System;
using System.Collections.Generic;
using System.Text;
using UnityEngine;

// 뒤끝 SDK namespace 추가
using BackEnd;

public class BackendFriend
{
    private static BackendFriend _instance = null;

    public static BackendFriend Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new BackendFriend();
            }

            return _instance;
        }
    }

    private List<Tuple<string, string>> _requestFriendList = new List<Tuple<string, string>>();

    public void SendFriendRequest(string nickName)
    {
        // Step 2. 친구 요청 보내기 로직 추가
    }

    public void GetReceivedRequestFriend()
    {
        // Step 3. 친구 요청 불러오기 및 수락하기 로직 추가(불러오기 부분)
    }

    public void ApplyFriend(int index)
    {
        // Step 3. 친구 요청 불러오기 및 수락하기 로직 추가(수락하기 부분)
    }

    public void GetFriendList()
    {
        // Step 4. 친구 리스트 불러오기
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
        BackendLogin.Instance.CustomLogin("user1", "1234");

        // 친구 기능 로직 추가

        Debug.Log("테스트를 종료합니다.");
    }
}
```

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/friend/request">다음  Step으로</a>
</div>
