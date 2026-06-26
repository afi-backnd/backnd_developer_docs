---
sidebar_label: Step 1. 사전 준비
description: "Step 1. 사전 준비"
---



# 사전 준비

매일 갱신되는 리더보드로 유저들의 경쟁심을 자극하세요.  
이 문서에서는 자동으로 갱신되는 리더보드를 설정하는 방법과  
서버로부터 리더보드의 정보를 클라이언트로 불러오는 방법을 상세히 설명합니다.

**랭킹 기능을 구현하기 위해서는 다음과 같은 작업들이 사전에 준비되어 있어야 합니다.**

1. [완성된 로그인 함수 로직](/sdk-docs/backend/base/guideline/new-user/all-code)
2. [랭킹 데이터를 저장하기 위한 private 테이블](/sdk-docs/backend/base/guideline/game-information/before)
3. [숫자형 데이터를 가진 row](/sdk-docs/backend/base/guideline/game-information/insert)
4. 뒤끝 콘솔 - 랭킹 관리에서 1번의 데이터를 이용한 랭킹 생성
5. 랭킹 전용 스크립트 생성

## 1. 로그인 함수 로직

로그인/회원가입 외에 모든 뒤끝 기능은 로그인이 진행된 이후에 정상적으로 함수를 호출할 수 있습니다.  
만약 로그인 로직이 구현되지 않으셨을 경우 [1. 로그인/회원가입 구현하기](/sdk-docs/backend/base/guideline/new-user/all-code) 가이드에 따라 로그인 로직을 구현해주시기 바랍니다.  

## 2. 랭킹 데이터를 저장하기 위한 private 테이블 생성

랭킹은 게임 정보 관리의 데이터를 기반으로 동작합니다.  
따라서 게임 정보관리에서 키값으로 설정이 가능한 숫자형 데이터가 존재해야만 랭킹을 생성할 수 있습니다.  

뒤끝 가이드라인 `2. 게임 정보 기능 구현하기 > Step 1. 사전 준비`를 완료하시지 않으셨다면 [사전 준비](/sdk-docs/backend/base/guideline/new-user/before)를 통해 테이블 생성을 진행해주세요.  

해당 가이드라인을 진행하려면 다음과 같은 테이블이 등록되어 있어야 합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/rank/gamedata-exist.png" />

## 3. 숫자형 데이터를 가진 row 생성

게임 정보 관리에서 랭킹에 점수로 사용될 숫자형 데이터(row)를 생성합니다.  

뒤끝 가이드라인 `2. 게임 정보 기능 구현하기 > Step 2. 게임 정보 삽입 구현하기`를 완료하시지 않으셨다면 [게임 정보 삽입 구현하기](/sdk-docs/backend/base/guideline/new-user/before)를 통해 게임 정보 데이터를 생성해주세요.  

해당 가이드라인을 진행하려면 다음과 같은 데이터가 등록되어 있어야 합니다.**(atk, level등 숫자형 데이터가 하나라도 존재해야합니다.)**  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/gamedata/gamedata-insert-console.png" />

## 4. 게임 정보관리 데이터를 이용한 랭킹 생성

게임 정보에 데이터가 정상적으로 삽입이 되었다면 해당 데이터를 이용하여 랭킹을 생성합니다.  

### 1. 뒤끝 콘솔 - 랭킹 관리에서 랭킹 생성 클릭

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/rank/ready-console-rank-create.png" />

### 2. 랭킹 정보 입력

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/rank/ready-console-rank-column2.png" />

- 유형 : 유저 랭킹
- 랭킹명 : USER_RANK(원하는 이름으로)
- 초기화 기간 : 일간
- 랭킹 종료 시 컬럼 초기화 : 적용
- 랭킹 항목 테이블 : USER_DATA(2번에서 생성한 테이블)
- 랭킹 항목 컬럼 : level
- 추가 항목 : 없음
- 정렬 기준 : 내림차순
- 랭킹 보상 : 없음
- 보상 우편 제목 :(비활성화, 랭킹 보상 선택 시 활성화됨)

### 생성된 랭킹 확인

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/rank/ready-console-rank-create-end.png" />

## 5. 랭킹 전용 스크립트 생성

새로운 스크립트를 생성하고 이름을 **BackendRank**으로 수정합니다.  
이후 BackendRank.cs 스크립트를 열어 내용을 다음과 같이 수정합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/rank/ready-create-script.png" />

### BackendRank.cs

```js
using System.Collections.Generic;
using System.Text;
using UnityEngine;

// 뒤끝 SDK namespace 추가
using BACKND.Base;

public class BackendRank
{
    private static BackendRank _instance = null;

    public static BackendRank Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new BackendRank();
            }

            return _instance;
        }
    }

    public async Task RankInsert(int score)
    {
        // Step 2. 랭킹 등록하기 내용 추가
    }

    public async Task RankGet()
    {
        // Step 3. 랭킹 불러오기 내용 추가
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
        await BackendLogin.Instance.CustomSignIn("user1", "1234"); // 뒤끝 로그인
        
        // 랭킹 기능 구현 로직 추가

        Debug.Log("테스트를 종료합니다.");
    }
}
```

