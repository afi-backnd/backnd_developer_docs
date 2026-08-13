---
sidebar_label: "Step 2. 랭킹 등록하기"
description: "랭킹 등록하기"
---

# 랭킹 등록하기

## 1. 뒤끝 콘솔에서 랭킹 UUID 복사

뒤끝 콘솔 `뒤끝 베이스 > 랭킹 관리`에서 생성한 랭킹의 UUID를 복사합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/rank/insert-uuid-copy.png" />

## 2. 게임 정보 삽입 함수 작성

사전 준비에서 작성한 [BackendRank.cs](/sdk-docs/backend/base/guideline/rank/before)의 RankInsert 함수에 내용을 추가합니다.  

이때 RankInsert 함수의 string rankUUID에는 1번에서 복사한 랭킹의 UUID를 붙여넣습니다.  

> 예시 : "복사한 UUID 값" -> "4088f640-693e-11ed-ad29-ad8f0c3d4c70"

### BackendRank.cs

**수정 전**  

```js
public void RankInsert(int score)
{
    // Step 2. 랭킹 삽입하기 내용 추가
}
```

**수정 후**  

```js
    public void RankInsert(int score)
{
    // [변경 필요] '복사한 UUID 값'을 '뒤끝 콘솔 > 랭킹 관리'에서 생성한 랭킹의 UUID값으로 변경해주세요.  
    string rankUUID = "복사한 UUID 값"; // 예시 : "4088f640-693e-11ed-ad29-ad8f0c3d4c70"

    string tableName = "USER_DATA";
    string rowInDate = string.Empty;

    // 랭킹을 삽입하기 위해서는 게임 데이터에서 사용하는 데이터의 inDate값이 필요합니다.  
    // 따라서 데이터를 불러온 후, 해당 데이터의 inDate값을 추출하는 작업을 해야합니다.  
    Debug.Log("데이터 조회를 시도합니다.");
    var bro = Backend.GameData.GetMyData(tableName, new Where());

    if (bro.IsSuccess() == false)
    {
        Debug.LogError("데이터 조회 중 문제가 발생했습니다 : " + bro);
        return;
    }

    Debug.Log("데이터 조회에 성공했습니다 : " + bro);

    if (bro.FlattenRows().Count > 0)
    {
        rowInDate = bro.FlattenRows()[0]["inDate"].ToString();
    }
    else
    {
        Debug.Log("데이터가 존재하지 않습니다. 데이터 삽입을 시도합니다.");
        var bro2 = Backend.GameData.Insert(tableName);

        if (bro2.IsSuccess() == false)
        {
            Debug.LogError("데이터 삽입 중 문제가 발생했습니다 : " + bro2);
            return;
        }

        Debug.Log("데이터 삽입에 성공했습니다 : " + bro2);

        rowInDate = bro2.GetInDate();
    }

    Debug.Log("내 게임 정보의 rowInDate : " + rowInDate); // 추출된 rowIndate의 값은 다음과 같습니다.  

    Param param = new Param();
    param.Add("level", score);

    // 추출된 rowIndate를 가진 데이터에 param값으로 수정을 진행하고 랭킹에 데이터를 업데이트합니다.  
    Debug.Log("랭킹 삽입을 시도합니다.");
    var rankBro = Backend.URank.User.UpdateUserScore(rankUUID, tableName, rowInDate, param);

    if (rankBro.IsSuccess() == false)
    {
        Debug.LogError("랭킹 등록 중 오류가 발생했습니다. : " + rankBro);
        return;
    }

    Debug.Log("랭킹 삽입에 성공했습니다. : " + rankBro);
}
```

## 3. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
**뒤끝 초기화와 뒤끝 로그인**이 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

### BackendManager.cs

**수정 전**  

```js
void Test()
{
    BackendLogin.Instance.CustomLogin("user1", "1234"); // 뒤끝 로그인

    // 랭킹 기능 구현 로직 추가

    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
void Test()
{
    BackendLogin.Instance.CustomLogin("user1", "1234"); // 뒤끝 로그인 함수

    BackendRank.Instance.RankInsert(100); // [추가] 랭킹 등록하기 함수

    Debug.Log("테스트를 종료합니다.");
}
```

## 4. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/rank/insert-success-log.png" />

이때 로그에서 **'랭킹 삽입에 성공했습니다. : statusCode : 204'**이 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [UpdateUserScore 에러케이스](/sdk-docs/backend/base/rank/user/update)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

### 에러케이스

랭킹 조회중 오류가 발생했습니다. : statusCode : 404  
errorCode : NotFoundException  
message : rank not found, rank을(를) 찾을 수 없습니다

> 함수 첫번째 줄, string rankUUID = "복사한 UUID 값";의 복사한 UUID 값을 실제 콘솔에서 생성한 랭킹의 uuid로 변경해주세요.  

## 5. 콘솔에서 확인

뒤끝 콘솔 `뒤끝 베이스 > 랭킹 관리`에서 생성한 랭킹을 클릭하여 유저의 순위를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/rank/insert-success-console.png" />

