---
description: "전체 코드"
---



# 전체 코드

## BackendGuild.cs
```js
using Newtonsoft.Json.Linq;
using System.Collections.Generic;
using System.Threading.Tasks;
using UnityEngine;
// 뒤끝 SDK namespace 추가
using BACKND.Base;

public class BackendGuild
{
    private static BackendGuild _instance = null;

    public static BackendGuild Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new BackendGuild();
            }

            return _instance;
        }
    }

    public async Task CreateGuild(string guildName)
    {
        // 10으로 들어가는 goodsCount의 경우, 사용 가능한 굿즈 종류의 갯수이며 이후 수정이 불가능하다.  
        var reqRequest = await BackndGuild.Instance.CreateGuildAsync(guildName, 10);
        if (reqRequest.IsSuccess() == false)
        {
            Debug.LogError("길드를 생성하는중 오류가 발생했습니다. : " + reqRequest);
            return;
        }

        Debug.Log("길드가 생성되었습니다. : " + reqRequest);
    }

    public async Task RequestGuildJoin(string guildName)
    {
        var getInDateReult = await BackndGuild.Instance.GetGuildInDateAsync(guildName);
        if (getInDateReult.IsSuccess() == false)
        {
            Debug.LogError($"{guildName}을 검색하는 중 에러가 발생했습니다. : " + getInDateReult);
            return;
        }

        string guildInDate = getInDateReult.GetGuildInDate();
        var joinResult = await BackndGuild.Instance.RequestGuildJoinAsync(guildInDate);
        if (joinResult.IsSuccess() == false)
        {
            Debug.LogError($"{guildName}({guildInDate})에게 가입 요청을 보내는 중 에러가 발생했습니다. : " + joinResult);
            return;
        }

        Debug.Log($"{guildName}({guildInDate})의 길드 가입이 정상적으로 요청되었습니다. : " + joinResult);
    }

    public async Task AcceptGuildJoinRequest(int index)
    {
        var reqResult = await BackndGuild.Instance.GetApplicantsAsync();
        if (reqResult.IsSuccess() == false)
        {
            Debug.LogError("길드 가입 요청 유저 리스트을 불러오는 중 에러가 발생했습니니다. : " + reqResult);
            return;
        }

        Debug.Log("길드 가입 요청 유저 리스트를 성공적으로 불러왔습니다. : " + reqResult);


        var infoList = reqResult.GetInfoList();
        if (infoList.Count <= 0)
        {
            Debug.LogError("가입을 신청한 유저가 존재하지 않습니다. : " + reqResult);
            return;
        }

        List<Tuple<string, string>> requestUserList = new List<Tuple<string, string>>();
        foreach (var info in infoList)
        {
            requestUserList.Add(new Tuple<string, string>(info.Nickname, info.InDate));
        }

        string userString = "가입 요청 목록\n";

        for (int i = 0; i < requestUserList.Count; i++)
        {
            userString += $"{index}. {requestUserList[i].Item1}({requestUserList[i].Item2})\n";
        }

        Debug.Log(userString);

        var approveResult = await BackndGuild.Instance.ApproveApplicantAsync(requestUserList[index].Item2);
        if (approveResult.IsSuccess() == false)
        {
            Debug.LogError($"{requestUserList[index].Item1}({requestUserList[index].Item2})의 가입 요청을 수락하는 중 에러가 발생했습니다. : " +
                        approveResult);
            return;
        }

        Debug.Log($"{requestUserList[index].Item1}({requestUserList[index].Item2})의 가입 요청 요청 수락에 성공했습니다.: " + approveResult);
    }

    public async Task ContributeGoods()
    {
        var reqResult = await BackndGuild.Instance.DonateGoodsAsync(GoodsType.goods1, 100);
        if (reqResult.IsSuccess() == false)
        {
            Debug.LogError("길드 기부중 에러가 발생했습니다 . : " + reqResult);
        }

        Debug.Log("길드 굿즈 기부가 성공적으로 진행되었습니다. : " + reqResult);
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
        // #Step 1
        await BackendLogin.Instance.CustomLogin("user1", "1234"); // user1으로 로그인
        // 길드 생성 중 412 에러가 발생할 경우, 이미 자신이 속한 길드가 있기 때문에 길드 생성이 불가능합니다.  
        // CustomSignUp("user3", "1234")로 다른 유저를 새로 생성하여 길드를 생성해주시기 바랍니다.  

        await BackendGuild.Instance.CreateGuild("원하는_길드_이름"); // 길드 생성 함수
        
        // #Step 2
        await BackendLogin.Instance.CustomSignUp("guildUser", "1234"); // 길드용 유저 새로 회원가입
        await BackendLogin.Instance.UpdateNickname("guildUser"); // 길드용 유저 닉네임 등록

        // Step 1에서 CreateGuild에 입력한 길드이름을 인자값으로 입력해주세요
        await BackendGuild.Instance.RequestGuildJoin("원하는_길드_이름");
        
        // #Step 3
        await BackendLogin.Instance.CustomLogin("user1", "1234");
        await BackendGuild.Instance.AcceptGuildJoinRequest(0); // 가입 요청한 유저중 제일 최신에 신청한 유저를 수락합니다.  
        
        // #Step 4
        await BackendGuild.Instance.ContributeGoods(); // 길드 굿즈 기부(추가) 함수

        Debug.Log("테스트가 종료되었습니다.");
    }
}
```
