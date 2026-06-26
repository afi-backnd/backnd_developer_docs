---
sidebar_label: Step 4. 우편 개별 수령 및 저장하기
description: "Step 4. 우편 개별 수령 및 저장하기"
---



# 우편 개별 수령 및 저장하기

## 1. 우편 수령하여 저장하기 함수 로직 작성

사전 준비에서 작성한 [BackendPost.cs](/sdk-docs/backend/base/guideline/post/before)의 PostReceive 함수에 내용을 추가합니다.  
해당 함수를 호출하기 위해서는 사전에 [PostListGet](/sdk-docs/backend/base/guideline/post/get) 함수가 구현되어 있어야 합니다.  

### BackendPost.cs

**수정 전**  

```js
public async Task PostReceive(MailType postType, int index)
{
    // Step 4. 우편 개별 수령 및 저장하기
}
```

**수정 후**  

```js
public async Task PostReceive(MailType postType, int index)
{
    if (_postList.Count <= 0)
    {
        Debug.LogWarning("받을 수 있는 우편이 존재하지 않습니다. 혹은 우편 리스트 불러오기를 먼저 호출해주세요.");
        return;
    }

    if (index >= _postList.Count)
    {
        Debug.LogError($"해당 우편은 존재하지 않습니다. : 요청 index{index} / 우편 최대 갯수 : {_postList.Count}");
        return;
    }

    Debug.Log($"{postType.ToString()}의 {_postList[index].inDate} 우편수령을 요청합니다.");

    var reqResult = await BackndMail.Instance.ReceiveMailAsync(postType, _postList[index].inDate);
    if (reqResult.IsSuccess() == false)
    {
        Debug.LogError($"{postType.ToString()}의 {_postList[index].inDate} 우편수령 중 에러가 발생했습니다. : " + reqResult);
        return;
    }

    Debug.Log($"{postType.ToString()}의 {_postList[index].inDate} 우편수령에 성공했습니다. : " + reqResult);

    _postList.RemoveAt(index);

    if (reqResult.GetRows("postItems").Count > 0)
    {
        SavePostToLocal(reqResult.GetRows("postItems"));
    }
    else
    {
        Debug.LogWarning("수령 가능한 우편 아이템이 존재하지 않습니다.");
    }

    await BackendGameData.Instance.GameDataUpdate();
}
```

## 2. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
**뒤끝 초기화와 뒤끝 로그인 그리고 게임 정보 불러오기와 우편 불러오기**가 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

### BackendManager.cs

**수정 전**(Step 3. 우편 불러오기 이후)

```js
async void Test()
{
    await BackendLogin.Instance.CustomLogin("user1", "1234"); // 뒤끝 로그인 함수

    await BackendPost.Instance.PostListGet(MailType.Admin);

    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
async void Test()
{
    await BackendLogin.Instance.CustomLogin("user1", "1234");

    // 게임데이터를 불러와 로컬에 저장합니다.(캐싱)
    await BackendGameData.Instance.GameDataGet();

    // 우편 리스트를 불러와 우편의 정보와 inDate값들을 로컬에 저장합니다.  
    await BackendPost.Instance.PostListGet(MailType.Admin);

    // 저장된 우편의 위치를 읽어 우편을 수령합니다. 여기서 index는 우편의 순서. 0이면 제일 윗 우편, 1이면 그 다음 우편
    await BackendPost.Instance.PostReceive(MailType.Admin, 0);

    Debug.Log("테스트를 종료합니다.");
}
```

## 3. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/post/post-receive-one-success-log.png" />

이때 로그에서 **'우편에 첨부한 아이템 이름 및 게임 정보 데이터 수정에 성공했습니다. : statusCode : 204'**가 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [ReceivePostItem 에러케이스](/sdk-docs/backend/base/post/get-one/console-post)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

## 4. 콘솔에서 우편 조회 여부 확인

해당 유저가 우편을 수령할 경우에는 수령일에 수령한 날짜가 표시됩니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/post/post-receive-one-console-success.png" />

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/post/receive">다음  Step으로</a>
</div>
