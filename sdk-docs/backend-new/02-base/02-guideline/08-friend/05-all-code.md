---
description: "전체 코드"
---



# 전체 코드

## BackendFriend.cs
```js
using Newtonsoft.Json.Linq;
using System.Collections.Generic;
using System.Threading.Tasks;
using UnityEngine;
// 뒤끝 SDK namespace 추가
using BACKND.Base;

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

    public async Task SendFriendRequest(string nickName)
    {
        var reqResult = await BackndSocial.Instance.GetUserInfoByNicknameAsync(nickName);
        if (reqResult.IsSuccess() == false)
        {
            Debug.LogError("유저 이름 검색 도중 에러가 발생했습니다. : " + reqResult);
            return;
        }

        string inDate = reqResult.GetInfo().InDate

        Debug.Log($"{nickName}의 inDate값은 {inDate} 입니다.");

        var reqFriendResult = await BackndFriend.Instance.SendFriendRequestAsync(inDate);
        if (reqFriendResult.IsSuccess() == false)
        {
            Debug.LogError($"{inDate} 친구 요청 도중 에러가 발생했습니다. : " + reqFriendResult);
            return;
        }

        Debug.Log("친구 요청에 성공했습니다." + reqFriendResult);
    }

    public async Task GetReceivedRequestFriend()
    {
        var getResult = await BackndFriend.Instance.GetReceivedFriendRequestsAsync();
        if (getResult.IsSuccess() == false)
        {
            Debug.Log("친구 요청 받은 리스트를 불러오는 중 에러가 발생했습니다. : " + getResult);
            return;
        }

        if (getResult.GetRows().Count <= 0)
        {
            Debug.LogError("친구 요청이 온 내역이 존재하지 않습니다.");
            return;
        }

        Debug.Log("친구 요청 받은 리스트 불러오기에 성공했습니다. : " + getResult);

        var infoList = getResult.GetInfoList();
        int index = 0;
        foreach (var info in infoList)
        {
            string nickName = info.Nickname;
            string inDate = info.InDate;

            _requestFriendList.Add(new Tuple<string, string>(nickName, inDate));

            Debug.Log($"{index}. {nickName} - {inDate}");
            index++;
        }
    }

    public async Task ApplyFriend(int index)
    {
        if (_requestFriendList.Count <= 0)
        {
            Debug.LogError("요청이 온 친구가 존재하지 않습니다.");
            return;
        }

        if (index >= _requestFriendList.Count)
        {
            Debug.LogError($"요청한 친구 요청 리스트의 범위를 벗어났습니다. 선택 : {index} / 리스트 최대 : {_requestFriendList.Count}");
            return;
        }

        var reqResult = await BackndFriend.Instance.AcceptFriendRequestAsync(_requestFriendList[index].Item2);
        if (reqResult.IsSuccess() == false)
        {
            Debug.LogError("친구 수락 중 에러가 발생했습니다. : " + reqResult);
            return;
        }

        Debug.Log($"{_requestFriendList[index].Item1}이(가) 친구가 되었습니다. : " + reqResult);
    }

    public async Task GetFriendList()
    {
        var reqResult = await BackndFriend.Instance.GetFriendsAsync();
        if (reqResult.IsSuccess() == false)
        {
            Debug.LogError("친구 목록 불러오기 중 에러가 발생했습니다. : " + reqResult);
            return;
        }

        Debug.Log("친구 목록 불러오기에 성공했습니다. : " + reqResult);

        if (reqResult.GetRows().Count <= 0)
        {
            Debug.Log("친구가 존재하지 않습니다.");
            return;
        }

        int index = 0;
        string friendListString = "친구 목록\n";
        var infoList = reqResult.GetInfoList();

        foreach (var info in infoList)
        {
            string nickName = info.Nickname;
            string inDate = info.InDate;

            friendListString += $"{index}. {nickName} - {inDate}\n";
            index++;
        }

        Debug.Log(friendListString);
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
        // user1에게 보낼 것이므로 user2로 회원가입
        string user2Id = "user2";

        // user2Id로 회원가입(409 에러 발생 시, 이미 user2Id로 아이디를 생성했으므로 CustomSignUp을 CustomLogin으로 변경)
        await BackendLogin.Instance.CustomSignUp(user2Id, "1234");
        await BackendLogin.Instance.UpdateNickname(user2Id); // 아이디와 동일하게 닉네임 변경

        string user1Nickname = "원하는 이름"; // 유저1의 닉네임(유저에 따라 다를 수 있습니다.)
        await BackendFriend.Instance.SendFriendRequest(user1Nickname); // 친구 요청 보내기 함수


        // Step 1에서 친구 요청을 보낸 대상자로 로그인해야합니다.  
        await BackendLogin.Instance.CustomLogin("user1", "1234");

        await BackendFriend.Instance.GetReceivedRequestFriend(); // 친구 요청 리스트 불러오기
        await BackendFriend.Instance.ApplyFriend(0); // 친구 요청 리스트 중 최신 요청 수락하기

        await BackendFriend.Instance.GetFriendList(); // 친구 리스트 불러오기

        Debug.Log("테스트를 종료합니다.");
    }
}
```

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/guild/before">다음   챕터로</a>
</div>
