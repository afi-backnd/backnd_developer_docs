

# 전체 코드

## BackendChart.cs
```js
using System.Text;
using System.Threading.Tasks;
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

        await BackendChart.Instance.ChartGet("63584"); // [추가] chartId의 차트 정보 불러오기

        Debug.Log("테스트를 종료합니다.");
    }
}
```
