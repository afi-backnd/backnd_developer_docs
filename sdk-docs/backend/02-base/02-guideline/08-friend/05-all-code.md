---
description: "전체 코드"
---

# 전체 코드

## BackendFriend.cs
```js
using System;
using System.Collections.Generic;
using System.Text;
using UnityEngine;

// 뒤끝 SDK namespace 추가
using BackEnd;

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

    public void SendFriendRequest(string nickName)
    {
        var inDateBro = Backend.Social.GetUserInfoByNickName(nickName);

        if (inDateBro.IsSuccess() == false)
        {
            Debug.LogError("유저 이름 검색 도중 에러가 발생했습니다. : " + inDateBro);
            return;
        }

        string inDate = inDateBro.GetReturnValuetoJSON()["row"]["inDate"].ToString();

        Debug.Log($"{nickName}의 inDate값은 {inDate} 입니다.");

        var friendBro = Backend.Friend.RequestFriend(inDate);

        if (friendBro.IsSuccess() == false)
        {
            Debug.LogError($"{inDate} 친구 요청 도중 에러가 발생했습니다. : " + friendBro);
            return;
        }

        Debug.Log("친구 요청에 성공했습니다." + friendBro);
    }

    public void GetReceivedRequestFriend()
    {
        var bro = Backend.Friend.GetReceivedRequestList();

        if (bro.IsSuccess() == false)
        {
            Debug.Log("친구 요청 받은 리스트를 불러오는 중 에러가 발생했습니다. : " + bro);
            return;
        }

        if (bro.FlattenRows().Count <= 0)
        {
            Debug.LogError("친구 요청이 온 내역이 존재하지 않습니다.");
            return;
        }

        Debug.Log("친구 요청 받은 리스트 불러오기에 성공했습니다. : " + bro);


        int index = 0;
        foreach (LitJson.JsonData friendJson in bro.FlattenRows())
        {
            string nickName = friendJson["nickname"]?.ToString();
            string inDate = friendJson["inDate"].ToString();

            _requestFriendList.Add(new Tuple<string, string>(nickName, inDate));

            Debug.Log($"{index}. {nickName} - {inDate}");
            index++;
        }
    }

    public void ApplyFriend(int index)
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

        var bro = Backend.Friend.AcceptFriend(_requestFriendList[index].Item2);

        if (bro.IsSuccess() == false)
        {
            Debug.LogError("친구 수락 중 에러가 발생했습니다. : " + bro);
            return;
        }

        Debug.Log($"{_requestFriendList[index].Item1}이(가) 친구가 되었습니다. : " + bro);
    }

    public void GetFriendList()
    {
        var bro = Backend.Friend.GetFriendList();

        if (bro.IsSuccess() == false)
        {
            Debug.LogError("친구 목록 불러오기 중 에러가 발생했습니다. : " + bro);
            return;
        }

        Debug.Log("친구 목록 불러오기에 성공했습니다. : " + bro);

        if (bro.FlattenRows().Count <= 0)
        {
            Debug.Log("친구가 존재하지 않습니다.");
            return;
        }

        int index = 0;

        string friendListString = "친구 목록\n";

        foreach (LitJson.JsonData friendJson in bro.FlattenRows())
        {
            string nickName = friendJson["nickname"]?.ToString();
            string inDate = friendJson["inDate"].ToString();

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
        // user1에게 보낼 것이므로 user2로 회원가입
        string user2Id = "user2";

        // user2Id로 회원가입(409 에러 발생 시, 이미 user2Id로 아이디를 생성했으므로 CustomSignUp을 CustomLogin으로 변경)
        BackendLogin.Instance.CustomSignUp(user2Id, "1234");
        BackendLogin.Instance.UpdateNickname(user2Id); // 아이디와 동일하게 닉네임 변경

        string user1Nickname = "원하는 이름"; // 유저1의 닉네임(유저에 따라 다를 수 있습니다.)
        BackendFriend.Instance.SendFriendRequest(user1Nickname); // 친구 요청 보내기 함수


        // Step 1에서 친구 요청을 보낸 대상자로 로그인해야합니다.  
        BackendLogin.Instance.CustomLogin("user1", "1234");

        BackendFriend.Instance.GetReceivedRequestFriend(); // 친구 요청 리스트 불러오기
        BackendFriend.Instance.ApplyFriend(0); // 친구 요청 리스트 중 최신 요청 수락하기

        BackendFriend.Instance.GetFriendList(); // 친구 리스트 불러오기

        Debug.Log("테스트를 종료합니다.");
    }
}
```

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/guild/before">다음   챕터로</a>
</div>
