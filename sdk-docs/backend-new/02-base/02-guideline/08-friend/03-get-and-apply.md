---
sidebar_label: Step 3. 친구 요청 불러오기 및 수락하기
---



# 친구 요청 불러오기 및 수락하기

## 1. 나에게 온 친구 요청 불러오기 및 수락하기

사전 준비에서 작성한 [BackendFriend.cs](/sdk-docs/backend/base/guideline/friend/before)의 GetReceivedRequestFriend 함수와ApplyFriend함수에 내용을 추가합니다.  

### BackendFriend.cs

**수정 전**  

```js
public async Task GetReceivedRequestFriend()
{
    // Step 3. 친구 요청 불러오기 및 수락하기 로직 추가(불러오기 부분)
}

public async Task ApplyFriend(int index)
{
    // Step 3. 친구 요청 수락하기 로직 추가(수락하기 부분)
}
```

**수정 후**  

```js
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
```

## 2. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
**뒤끝 초기화와 뒤끝 로그인**이 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

**로그인하는 유저는 Step2에서 로그인(회원가입)한 유저가 아닌 친구 요청을 보낸 유저로 로그인을 해야합니다.**  

### BackendManager.cs

**수정 전**(Step 2. 친구 요청 보내기 로직)

```js
async void Test()
{
    // user1에게 보낼 것이므로 user2로 회원가입
    string user2Id = "user2";

    // user2Id로 회원가입(409 에러 발생 시, 이미 user2Id로 아이디를 생성했으므로 CustomSignUp을 CustomLogin으로 변경)
    await BackendLogin.Instance.CustomSignUp(user2Id, "1234");
    await BackendLogin.Instance.UpdateNickname(user2Id); // 아이디와 동일하게 닉네임 변경

    string user1Nickname = "원하는 이름"; // 유저1의 닉네임(유저에 따라 다를 수 있습니다.)
    await BackendFriend.Instance.SendFriendRequest(user1Nickname); // 친구 요청 보내기 함수

    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
async void Test()
{
    // Step 2에서 user2가 친구 요청을 보낸 유저로 로그인해야합니다.  
    await BackendLogin.Instance.CustomLogin("user1", "1234");

    await BackendFriend.Instance.GetReceivedRequestFriend(); // 친구 요청 리스트 불러오기
    await BackendFriend.Instance.ApplyFriend(0); // 친구 요청 리스트 중 최신 요청 수락하기

    Debug.Log("테스트를 종료합니다.");
}
```

## 3. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/friend/friend-get-receive-success-log.png" />

이때 로그에서 **'ㅇㅇ이(가) 친구가 되었습니다. : statusCode : 204'**이 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [GetReceivedRequestList 에러케이스](/sdk-docs/backend/base/friend/request-to-another-user/list-received-request) 와 [AcceptFriend 에러케이스](/sdk-docs/backend/base/friend/request-to-another-user/apply-request)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/friend/get-all">다음  Step으로</a>
</div>
