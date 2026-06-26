---
sidebar_label: Step 4. 길드 가입 요청 수락하기
---



# 길드 가입 요청 수락하기

## 1. 길드 가입 요청 승락하기 함수 작성

사전 준비에서 작성한 [BackendGuild.cs](/sdk-docs/backend/base/guideline/guild/before)의 AcceptGuildJoinRequest 함수에 내용을 추가합니다.  

### BackendGuild.cs

**수정 전**  

```js
public async Task AcceptGuildJoinRequest(int index)
{
    // Step 4. 길드 가입 요청 수락하기
}
```

**수정 후**  

```js
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
```

## 2. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
**뒤끝 초기화와 뒤끝 로그인**이 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

이때, 로그인하는 유저는 **Step 2. 길드 생성하기**를 호출한 유저가 로그인해야 합니다.  

### BackendManager.cs

**수정 전**  

```js
async void Test()
{
    await BackendLogin.Instance.CustomSignUp("guildUser", "1234"); // 길드용 유저 새로 회원가입
    await BackendLogin.Instance.UpdateNickname("guildUser"); // 길드용 유저 닉네임 등록

    await BackendGuild.Instance.RequestGuildJoin("원하는_길드_이름"); // Step 2에서 CreateGuild에 입력한 길드이름을 인자값으로 입력해주세요

    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
async void Test() {
        
    await BackendLogin.Instance.CustomLogin("user1", "1234"); // Step 2에서 길드를 생성한 유저로 로그인해야합니다.  

    await BackendGuild.Instance.AcceptGuildJoinRequest(0); // 가입 요청한 유저중 제일 최신에 신청한 유저를 수락합니다.  

    Debug.Log("테스트를 종료합니다.");
}
```

## 3. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/guild/guild-receive-success-log.png" />

이때 로그에서 **'ㅇㅇ의 가입 요청 요청 수락에 성공했습니다. : statusCode : 204'**가 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는[ApproveApplicantV3](/sdk-docs/backend/base/guild/operator/apply-join) 와 [GetApplicantsV3 에러케이스](/sdk-docs/backend/base/guild/operator/get-all-requested)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

## 4. 콘솔에서 확인

뒤끝 콘솔 `운영 > 길드`에서 생성된 길드를 클릭하여 유저가 증가했는지 확인합니다.    

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/guild/guild-receive-success-console.png" />

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/guild/donate-goods">다음  Step으로</a>
</div>
