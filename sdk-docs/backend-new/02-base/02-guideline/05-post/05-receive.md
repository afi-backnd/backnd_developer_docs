---
sidebar_label: Step 5. 우편 전체 수령 및 저장하기
description: "Step 5. 우편 전체 수령 및 저장하기"
---



# 우편 전체 수령 및 저장하기

## 1. 우편 새로 발송

이전에 `Step 2. 콘솔에서 우편 발송하기`에서 발송한 우편은 `Step 4.  우편 개별 수령 및 저장하기`에서 수령을 하였기 때문에 다시 우편을 불러올 수 없습니다.  

따라서 우편을 정상적으로 수령하려면 [Step 2. 콘솔에서 우편 발송하기](/sdk-docs/backend/base/guideline/post/post-in-console)를 한번 더 진행하여 새로운 우편을 한번 더 생성해야합니다.  

테스트에 필요한 우편은 `Step 2. 콘솔에서 우편 발송하기`에서 생성하는 우편과 동일합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/post/new-post-for-receive-all.png" />

## 2. 우편 전부 수령하여 저장하기 함수 로직 작성

사전 준비에서 작성한 [BackendPost.cs](/sdk-docs/backend/base/guideline/post/before)의 PostReceive 함수에 내용을 추가합니다.  
해당 함수를 호출하기 위해서는 사전에 [PostListGet](/sdk-docs/backend/base/guideline/post/get) 함수가 구현되어 있어야 합니다.  

### BackendPost.cs

**수정 전**  

```js
public async Task PostReceiveAll(MailType postType)
{
    // Step 5. 모든 우편 수령하여 저장하기
}
```

**수정 후**  

```js
public async Task PostReceiveAll(MailType postType)
{
    if (_postList.Count <= 0)
    {
        Debug.LogWarning("받을 수 있는 우편이 존재하지 않습니다. 혹은 우편 리스트 불러오기를 먼저 호출해주세요.");
        return;
    }

    Debug.Log($"{postType.ToString()} 우편 모두 수령을 요청합니다.");

    var reqResult = await BackndMail.Instance.ReceiveAllMailsAsync(postType);
    if (reqResult.IsSuccess() == false)
    {
        Debug.LogError($"{postType.ToString()} 우편 모두 수령 중 에러가 발생했습니다 : " + reqResult);
        return;
    }

    Debug.Log("우편 모두 수령에 성공했습니다. : " + reqResult);

    _postList.Clear();

    foreach (var postItemsJson in reqResult.GetRows("postItems"))
    {
        SavePostToLocal(postItemsJson);
    }

    await BackendGameData.Instance.GameDataUpdate();
}
```

## 3. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
**뒤끝 초기화와 뒤끝 로그인**이 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

### BackendManager.cs

**수정 전**(Step 4. 우편 개별 수령 및 저장하기 이후)

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

**수정 후**  

```js
async void Test()
{
    await BackendLogin.Instance.CustomLogin("user1", "1234");

    // 게임데이터를 불러와 로컬에 저장합니다.(캐싱)
    await BackendGameData.Instance.GameDataGet();

    // 우편 리스트를 불러와 우편의 정보와 inDate값들을 로컬에 저장합니다.  
    await BackendPost.Instance.PostListGet(MailType.Admin);

    // 조회된 모든 우편을 수령합니다.  
    await BackendPost.Instance.PostReceiveAll(MailType.Admin);

    Debug.Log("테스트를 종료합니다.");
}
```

## 4. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/post/post-receive-all-success-log.png" />

이때 로그에서 **'우편에 첨부한 아이템 이름 및 게임 정보 데이터 수정에 성공했습니다. : statusCode : 204'**가 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [ReceivePostItemAll 에러케이스](/sdk-docs/backend/base/post/get-all/console-post)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

## 5. 콘솔에서 우편 조회 여부 확인

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/post/post-console-receive1.png" />

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/coupon/before">다음  챕터로</a>
</div>
