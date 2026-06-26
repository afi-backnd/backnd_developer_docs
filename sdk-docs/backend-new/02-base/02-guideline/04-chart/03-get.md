---
sidebar_label: Step 3. 차트 정보 가져오기
description: "Step 3. 차트 정보 가져오기"
---



# 차트 정보 가져오기

## 1. 뒤끝 콘솔에서 차트의 파일 ID 복사

뒤끝 콘솔에 `뒤끝베이스 > 차트 관리 > 아이템 차트`에서 업로드한 차트의 파일 ID를 복사합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/chart/chart-fileid-before.png" />
<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/chart/chart-fileid.png" />

## 2. 차트 불러오기 함수 작성

사전 준비에서 작성한 [BackendChart.cs](/sdk-docs/backend/base/guideline/chart/before)의 Chart 함수에 내용을 추가합니다.  

### BackendChart.cs

**수정 전**  

```js
public async Task ChartGet(string chartId)
{
    // Step 3. 차트 정보 가져오기 내용 추가
}
```

**수정 후**  

```js
public async Task ChartGet(string chartId)
{
    Debug.Log($"{chartId}의 차트 불러오기를 요청합니다.");

    // 테이블 불러오기.
    var tableResult = await BackndGameTable.Instance.GetTablesAsync();
    if (tableResult.IsSuccess() == false)
    {
        Debug.LogError(tableResult);
        return;
    }

    // 불러온 테이블의 내용 조회하기
    var contentResult = await BackndGameTable.Instance.GetContentsAsync(tableResult.GetInfoList(), null);
    if (contentResult.IsSuccess() == false)
    {
        Debug.LogError("GetContents : Fail : " + contentResult);
        return;
    }

    Debug.Log("차트 불러오기에 성공했습니다. : " + contentResult);        
    var contentsJson = contentResult.GetInfoById()[chartId].GetContentsJson();
    foreach (var gameData in contentsJson)
    {
        StringBuilder content = new StringBuilder();
        content.AppendLine("itemID : " + int.Parse(gameData["itemId"].ToString()));
        content.AppendLine("itemName : " + gameData["itemName"].ToString());
        content.AppendLine("itemType : " + gameData["itemType"].ToString());
        content.AppendLine("itemID : " + long.Parse(gameData["itemPower"].ToString()));
        content.AppendLine("itemInfo : " + gameData["itemInfo"].ToString());

        Debug.Log(content.ToString());
    }
}
```

## 3. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
**뒤끝 초기화와 뒤끝 로그인**이 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

이때 BackendChart.Instance.ChartGet의 인자값에는 1번에서 복사한 차트의 파일 ID를 붙여넣습니다.  

> 예시 : "<파일 ID>" -> "64584"

### BackendManager.cs

**수정 전**  

```js
async void Test()
{
    await BackendLogin.Instance.CustomLogin("user1", "1234"); // 뒤끝 로그인 함수

    // 차트 정보 불러오기 로직 추가

    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
async void Test()
{
    await BackendLogin.Instance.CustomLogin("user1", "1234"); // 뒤끝 로그인 함수

    // [추가] chartId의 차트 정보 불러오기
    // [변경 필요] <파일 ID>을 뒤끝 콘솔 > 차트 관리 > 아이템차트에서 등록한 차트의 파일 ID값으로 변경해주세요.  
    await BackendChart.Instance.ChartGet("<파일 ID>"); // 예시 : "64584"

    Debug.Log("테스트를 종료합니다.");
}
```

## 4. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/chart/chart-success-log.png" />

이때 로그에서 **'차트에 등록된 데이터'**들이 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [GetChartContents 에러케이스](/sdk-docs/backend/base/chart/search-chart/load-chart-list)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

