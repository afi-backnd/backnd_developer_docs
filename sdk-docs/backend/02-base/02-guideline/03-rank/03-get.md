---
sidebar_label: "Step 3. 랭킹 불러오기"
description: "랭킹 불러오기"
---

# 랭킹 불러오기

## 1. 뒤끝 콘솔에서 랭킹 UUID 복사

뒤끝 콘솔 `뒤끝 베이스 > 랭킹 관리`에서 생성한 랭킹의 UUID를 복사합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/rank/insert-uuid-copy.png" />

## 2. 게임 정보 삽입 함수 작성

사전 준비에서 작성한 [BackendRank.cs](/sdk-docs/backend/base/guideline/rank/before)의 RankGet 함수에 내용을 추가합니다.  

이때 RankGet 함수의 rankUUID에는 1번에서 복사한 랭킹의 UUID를 붙여넣습니다.  

> 예시 : "<복사한 UUID 값>" -> "4088f640-693e-11ed-ad29-ad8f0c3d4c70"

### BackendRank.cs

**수정 전**  

```js
public void RankGet()
{
    // Step 3. 랭킹 불러오기 내용 추가
}
```

**수정 후**  

```js
public void RankGet()
{
    string rankUUID = "<복사한 UUID 값>"; // 예시 : "4088f640-693e-11ed-ad29-ad8f0c3d4c70"
    var bro = Backend.URank.User.GetRankList(rankUUID);

    if (bro.IsSuccess() == false)
    {
        Debug.LogError("랭킹 조회중 오류가 발생했습니다. : " + bro);
        return;
    }

    Debug.Log("랭킹 조회에 성공했습니다. : " + bro);

    Debug.Log("총 랭킹 등록 유저 수 : " + bro.GetFlattenJSON()["totalCount"].ToString());

    foreach (LitJson.JsonData jsonData in bro.FlattenRows())
    {
        StringBuilder info = new StringBuilder();

        info.AppendLine("순위 : " + jsonData["rank"].ToString());
        info.AppendLine("닉네임 : " + jsonData["nickname"].ToString());
        info.AppendLine("점수 : " + jsonData["score"].ToString());
        info.AppendLine("gamerInDate : " + jsonData["gamerInDate"].ToString());
        info.AppendLine("정렬번호 : " + jsonData["index"].ToString());
        info.AppendLine();
        Debug.Log(info);
    }
}
```

## 3. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
**뒤끝 초기화와 뒤끝 로그인**이 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

### BackendManager.cs

**수정 전**(Step 2. 랭킹 등록하기 이후)

```js
void Test()
{
    BackendLogin.Instance.CustomLogin("user1", "1234"); // 뒤끝 로그인 함수

    BackendRank.Instance.RankInsert(100); // [추가] 랭킹 등록하기 함수

    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
void Test()
{
    BackendLogin.Instance.CustomLogin("user1", "1234"); // 뒤끝 로그인 함수

    BackendRank.Instance.RankGet(); // [추가] 랭킹 불러오기 함수

    Debug.Log("테스트를 종료합니다.");
}
```

## 4. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/rank/get-success-log.png" />

이때 로그에서 **'랭킹에 업데이트된 유저의 정보'**가 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [GetRankList 에러케이스](/sdk-docs/backend/base/rank/user/get-list)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

### 에러케이스

랭킹 조회중 오류가 발생했습니다. : statusCode : 404  
errorCode : NotFoundException  
message : rank not found, rank을(를) 찾을 수 없습니다

> 함수 첫번째 줄, string rankUUID = "복사한 UUID 값";의 복사한 UUID 값을 실제 콘솔에서 생성한 랭킹의 uuid로 변경해주세요.  

