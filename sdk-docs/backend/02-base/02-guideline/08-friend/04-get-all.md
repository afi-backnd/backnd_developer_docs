---
sidebar_label: "Step 4. 친구 리스트 불러오기"
description: "친구 리스트 불러오기"
---

# 친구 리스트 불러오기

## 1. 친구 리스트 불러오기 함수 작성

사전 준비에서 작성한 [BackendFriend.cs](/sdk-docs/backend/base/guideline/friend/before)의 GetFriendList함수에 내용을 추가합니다.  

### BackendFriend.cs

**수정 전**  

```js
public void GetFriendList()
{
    // Step 4. 친구 리스트 불러오기
}
```

**수정 후**  

```js
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
```

## 2. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
**뒤끝 초기화와 뒤끝 로그인**이 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

### BackendManager.cs

**수정 전**(Step 3. 친구 요청 불러오기 및 수령하기)

```js
void Test()
{
    // Step 2에서 user2가 친구 요청을 보낸 유저로 로그인해야합니다.  
    BackendLogin.Instance.CustomLogin("user1", "1234");

    BackendFriend.Instance.GetReceivedRequestFriend(); // 친구 요청 리스트 불러오기
    BackendFriend.Instance.ApplyFriend(0); // 친구 요청 리스트 중 최신 요청 수락하기

    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
void Test()
{
    // Step 2에서 회원가입 한 유저(친구 요청 발송인)혹은 Step 3에서 로그인한 유저(친구 요청 수신인)중 하나로 로그인해야합니다.  
    BackendLogin.Instance.CustomLogin("user1", "1234");

    BackendFriend.Instance.GetFriendList(); // 친구 리스트 불러오기

    Debug.Log("테스트를 종료합니다.");
}
```

## 3. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/friend/friend-get-success-log.png" />

이때 로그에서 **'친구 목록'**이 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [GetFriendList 에러케이스](/sdk-docs/backend/base/friend/list-friends)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/guild/before">다음   챕터로</a>
</div>
