---
description: "전체 코드"
---

# 전체 코드

## BackendGuild.cs
```js
using System;
using System.Collections.Generic;
using System.Text;
using UnityEngine;

// 뒤끝 SDK namespace 추가
using BackEnd;

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

    public void CreateGuild(string guildName)
    {
        // 10으로 들어가는 goodsCount의 경우, 사용 가능한 굿즈 죵류의 갯수이며 이후 수정이 불가능하다.  
        var bro = Backend.Guild.CreateGuildV4(guildName, 10); // 오픈 길드로 생성하려면 세 번째 파라미터에 true 전달

        if (bro.IsSuccess() == false)
        {
            Debug.LogError("길드를 생성하는중 오류가 발생했습니다. : " + bro);
            return;
        }

        Debug.Log("길드가 생성되었습니다. : " + bro);
    }

    public void RequestGuildJoin(string guildName)
    {
        var bro = Backend.Guild.GetGuildIndateByGuildNameV3(guildName);

        if (bro.IsSuccess() == false)
        {
            Debug.LogError($"{guildName}을 검색하는 중 에러가 발생했습니다. : " + bro);
            return;
        }

        string guildInDate = bro.GetFlattenJSON()["guildInDate"].ToString();

        bro = Backend.Guild.ApplyGuildV3(guildInDate);

        if (bro.IsSuccess() == false)
        {
            Debug.LogError($"{guildName}({guildInDate})에게 가입 요청을 보내는 중 에러가 발생했습니다. : " + bro);
            return;
        }

        Debug.Log($"{guildName}({guildInDate})의 길드 가입이 정상적으로 요청되었습니다. : " + bro);
    }

    public void AcceptGuildJoinRequest(int index)
    {
        var bro = Backend.Guild.GetApplicantsV3();

        if (bro.IsSuccess() == false)
        {
            Debug.LogError("길드 가입 요청 유저 리스트을 불러오는 중 에러가 발생했습니니다. : " + bro);
            return;
        }

        Debug.Log("길드 가입 요청 유저 리스트를 성공적으로 불러왔습니다. : " + bro);


        if (bro.FlattenRows().Count <= 0)
        {
            Debug.LogError("가입을 신청한 유저가 존재하지 않습니다. : " + bro);
            return;
        }

        List<Tuple<string, string>> requestUserList = new List<Tuple<string, string>>();

        foreach (LitJson.JsonData requestJson in bro.FlattenRows())
        {
            requestUserList.Add(new Tuple<string, string>(requestJson["nickname"].ToString(),
                requestJson["inDate"].ToString()));
        }

        string userString = "가입 요청 목록\n";

        for (int i = 0; i < requestUserList.Count; i++)
        {
            userString += $"{index}. {requestUserList[i].Item1}({requestUserList[i].Item2})\n";
        }

        Debug.Log(userString);

        bro = Backend.Guild.ApproveApplicantV3(requestUserList[index].Item2);
        if (bro.IsSuccess() == false)
        {
            Debug.LogError(
                $"{requestUserList[index].Item1}({requestUserList[index].Item2})의 가입 요청을 수락하는 중 에러가 발생했습니다. : " + bro);
            return;
        }

        Debug.Log($"{requestUserList[index].Item1}({requestUserList[index].Item2})의 가입 요청 요청 수락에 성공했습니다.: " + bro);
    }

    public void ContributeGoods()
    {
        var bro = Backend.Guild.ContributeGoodsV3(goodsType.goods1, 100);

        if (bro.IsSuccess() == false)
        {
            Debug.LogError("길드 기부중 에러가 발생했습니다 . : " + bro);
        }

        Debug.Log("길드 굿즈 기부가 성공적으로 진행되었습니다. : " + bro);
    }
}
```

## BackendManager.cs
```js
using UnityEngine;


// 뒤끝 SDK namespace 추가
using BackEnd;

public class BackendManager : MonoBehaviour {
    void Start() {
        var bro = Backend.Initialize(); // 뒤끝 초기화

        // 뒤끝 초기화에 대한 응답값
        if(bro.IsSuccess()) {
            Debug.Log("초기화 성공 : " + bro); // 성공일 경우 statusCode 204 Success
        } else {
            Debug.LogError("초기화 실패 : " + bro); // 실패일 경우 statusCode 400대 에러 발생 
        }

        Test();
    }

    // 동기 함수를 비동기에서 호출하게 해주는 함수(유니티 UI 접근 불가)
    void Test() {
        
        
        // #Step 1
        BackendLogin.Instance.CustomLogin("user1", "1234"); // user1으로 로그인
        // 길드 생성 중 412 에러가 발생할 경우, 이미 자신이 속한 길드가 있기 때문에 길드 생성이 불가능합니다.  
        // CustomSignUp("user3", "1234")로 다른 유저를 새로 생성하여 길드를 생성해주시기 바랍니다.  

        BackendGuild.Instance.CreateGuild("원하는_길드_이름"); // 길드 생성 함수
        
        // #Step 2
        BackendLogin.Instance.CustomSignUp("guildUser", "1234"); // 길드용 유저 새로 회원가입
        BackendLogin.Instance.UpdateNickname("guildUser"); // 길드용 유저 닉네임 등록

        // Step 1에서 CreateGuild에 입력한 길드이름을 인자값으로 입력해주세요
        BackendGuild.Instance.RequestGuildJoin("원하는_길드_이름");
        
        // #Step 3
        BackendLogin.Instance.CustomLogin("user1", "1234");
        BackendGuild.Instance.AcceptGuildJoinRequest(0); // 가입 요청한 유저중 제일 최신에 신청한 유저를 수락합니다.  
        
        // #Step 4
        BackendGuild.Instance.ContributeGoods(); // 길드 굿즈 기부(추가) 함수

        Debug.Log("테스트가 종료되었습니다.");
        }
}
```
