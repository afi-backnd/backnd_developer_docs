---
description: "전체 코드"
---



# 전체 코드

## BackendRank.cs
```js
using System.Text;
using System.Threading.Tasks;
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
        // [변경 필요] '복사한 UUID 값'을 '뒤끝 콘솔 > 리더보드'에서 생성한 랭킹의 UUID값으로 변경해주세요.  
        string rankUUID = "복사한 UUID 값"; // 예시 : "4088f640-693e-11ed-ad29-ad8f0c3d4c70"

        string tableName = "USER_DATA";
        string rowInDate = string.Empty;

        // 리더보드에 삽입하기 위해서는 게임 데이터에서 사용하는 데이터의 inDate값이 필요합니다.  
        // 따라서 데이터를 불러온 후, 해당 데이터의 inDate값을 추출하는 작업을 해야합니다.  
        Debug.Log("데이터 조회를 시도합니다.");
        var reqResult = await BackndUserData.Instance.GetDataAsync(tableName);
        if (reqResult.IsSuccess() == false)
        {
            Debug.LogError("데이터 조회 중 문제가 발생했습니다 : " + reqResult);
            return;
        }

        Debug.Log("데이터 조회에 성공했습니다 : " + reqResult);

        var dataRows = reqResult.GetRows();
        if (dataRows.Count > 0)
        {
            rowInDate = dataRows[0]["inDate"].ToString();
        }
        else
        {
            Debug.Log("데이터가 존재하지 않습니다. 데이터 삽입을 시도합니다.");
            var reqResult2 = await BackndUserData.Instance.CreateDataAsync(tableName);
            if (reqResult2.IsSuccess() == false)
            {
                Debug.LogError("데이터 삽입 중 문제가 발생했습니다 : " + reqResult2);
                return;
            }

            Debug.Log("데이터 삽입에 성공했습니다 : " + reqResult2);

            rowInDate = reqResult2.GetRowInDate();
        }

        Debug.Log("내 게임 정보의 rowInDate : " + rowInDate); // 추출된 rowIndate의 값은 다음과 같습니다.  

        Param param = new Param();
        param.Add("level", score);

        // 추출된 rowIndate를 가진 데이터에 param값으로 수정을 진행하고 리더보드에 데이터를 업데이트합니다.  
        Debug.Log("리더보드 삽입을 시도합니다.");
        var updateResult = await BackndLeaderboard.User.UpdateLeaderboardWithDataAsync(rankUUID, tableName, rowInDate, param);
        if (updateResult.IsSuccess() == false)
        {
            Debug.LogError("리더보드 등록 중 오류가 발생했습니다. : " + updateResult);
            return;
        }

        Debug.Log("리더보드 삽입에 성공했습니다. : " + updateResult);
    }

    public async Task RankGet()
    {
        string rankUUID = "<복사한 UUID 값>"; // 예시 : "4088f640-693e-11ed-ad29-ad8f0c3d4c70"
        var getResult = await BackndLeaderboard.User.GetRanksAsync(rankUUID);
        if (getResult.IsSuccess() == false)
        {
            Debug.LogError("랭킹 조회중 오류가 발생했습니다. : " + getResult);
            return;
        }

        Debug.Log("랭킹 조회에 성공했습니다. : " + getResult);
        Debug.Log("총 랭킹 등록 유저 수 : " + getResult.GetTotalCount());

        var rankInfoList = getResult.GetInfoList();
        foreach (var rankInfo in rankInfoList)
        {
            StringBuilder info = new StringBuilder();
            info.AppendLine("순위 : " + rankInfo.Rank);
            info.AppendLine("닉네임 : " + rankInfo.Nickname);
            info.AppendLine("점수 : " + rankInfo.Score);
            info.AppendLine("gamerInDate : " + rankInfo.UserInDate);
            info.AppendLine("정렬번호 : " + rankInfo.Index);
            info.AppendLine();
            Debug.Log(info);
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
        await BackendLogin.Instance.CustomSignIn("user1", "1234"); // 뒤끝 로그인

        await BackendRank.Instance.RankInsert(100); // [추가] 랭킹 등록하기 함수
        await BackendRank.Instance.RankGet(); // [추가] 랭킹 불러오기 함수

        Debug.Log("테스트를 종료합니다.");
    }
}
```

