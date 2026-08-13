---
sidebar_label: "Step 1. 사전 준비"
description: "사전 준비"
---

# 사전 준비

쿠폰 기능을 구현하기 위해서는 다음과 같은 작업들이 사전에 준비되어 있어야 합니다.  

1. [우편 이용이 가능한 차트](/sdk-docs/backend/base/guideline/chart/insert)
2. [완성된 로그인 함수 로직](/sdk-docs/backend/base/guideline/new-user/all-code)
3. [완성된 게임 정보 관리 함수 로직](/sdk-docs/backend/base/guideline/game-information/all-code)
4. 쿠폰 전용 스크립트 생성

## 1. 우편 이용이 가능한 차트

우편을 수령한 후에 지급되는 아이템을 구현하기 위해서는 우편 이용이 가능한 차트의 정보가 필요합니다.  
뒤끝 콘솔 `뒤끝 베이스 > 차트 관리`에서 쿠폰 보상으로 지급할 아이템이 담긴 차트의 **우편 사용 여부를 사용**으로 변경해주세요.  
만약 등록된 차트가 없을 경우에는 **4. 차트 기능 구현하기**의 [Step 2. 콘솔에 차트 등록하기](/sdk-docs/backend/base/guideline/chart/insert)를 진행해 주세요.  

차트 우측의 **편집** 버튼을 클릭합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/post-console-usepost.png" />

우편 사용 여부를 **사용**으로 변경합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/post-console-usepost2.png" />

변경이 완료되면 우편 기능이 **사용**으로 표시됩니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/post-console-usepost3.png" />

## 2. 완성된 로그인 함수 로직

로그인/회원가입 외에 모든 뒤끝 기능은 로그인이 진행된 이후에 정상적으로 함수를 호출할 수 있습니다.  
만약 로그인 로직이 구현되지 않으셨을 경우 [1. 로그인/회원가입 구현하기](/sdk-docs/backend/base/guideline/new-user/before) 가이드에 따라 로그인 로직을 **모두** 구현해주시기 바랍니다.  

## 3. 완성된 게임 정보 관리 함수 로직

해당 예제에서는 쿠폰 사용 후, 반환되는 아이템을 저장하는 로직까지 구현되어있기 때문에 데이터를 저장하기 위한 **2. 게임 정보 기능 구현하기**가 필수로 구현되어 있어야 합니다.  
만약 게임 정보 기능 로직이 구현되지 않으셨을 경우 [2. 게임 정보 기능 구현하기](/sdk-docs/backend/base/guideline/game-information/before) 가이드에 따라 게임 정보 로직을 **모두** 구현해주시기 바랍니다.  

## 4. 쿠폰 전용 스크립트 생성

새로운 스크립트를 생성하고 이름을 **BackendCoupon**으로 수정합니다.  
이후 BackendCoupon.cs 스크립트를 열어 내용을 다음과 같이 수정합니다.  

### BackendCoupon.cs

```js
using System.Collections.Generic;
using System.Text;
using UnityEngine;

// 뒤끝 SDK namespace 추가
using BackEnd;

public class BackendCoupon
{
    private static BackendCoupon _instance = null;

    public static BackendCoupon Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new BackendCoupon();
            }

            return _instance;
        }
    }

    public void CouponUse(string couponNumber)
    {
        // Step 3. 쿠폰 사용하고 아이템 저장하기
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

        // 쿠폰 사용 로직 추가

        Debug.Log("테스트를 종료합니다.");
    }
}
```

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/coupon/insert-in-console">다음  Step으로</a>
</div>
