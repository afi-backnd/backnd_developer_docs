---
sidebar_label: Step 3. 우편 불러오기
---



# 우편 불러오기

## 1. 우편 불러오기 함수 로직 작성

사전 준비에서 작성한 [BackendPost.cs](/sdk-docs/backend/base/guideline/post/before)의 PostListGet 함수에 내용을 추가합니다.  

> 만약 차트의 이름을'아이템 차트'로 생성하지 않으셨을 경우, PostListGet 함수의 chartName을 콘솔에서 설정한 차트 이름으로 변경해주세요.  

### BackendPost.cs

**수정 전**  

```js
public async Task PostListGet(MailType postType)
{
    // Step 3. 우편 불러오기
}
```

**수정 후**  

```js
public async Task PostListGet(MailType postType)
{
    var reqResult = await BackndMail.Instance.GetMailsAsync(postType);

    // 만약 차트 이름을 '아이템 차트'로 설정하지 않았을 경우,
    // 해당 값을 콘솔에서 설정한 차트 이름과 동일하게 설정해주세요
    string chartName = "아이템 차트";

    if (reqResult.IsSuccess() == false)
    {
        Debug.LogError("우편 불러오기 중 에러가 발생했습니다.");
        return;
    }

    Debug.Log("우편 리스트 불러오기 요청에 성공했습니다. : " + reqResult);

    var postList = reqResult.GetRows("postList");
    if (postList.Count <= 0)
    {
        Debug.LogWarning("받을 우편이 존재하지 않습니다.");
        return;
    }

    foreach (var postListJson in postList)
    {
        Post post = new Post();

        post.title = postListJson["title"].ToString();
        post.content = postListJson["content"].ToString();
        post.inDate = postListJson["inDate"].ToString();

        // 우편의 아이템
        if (postType == MailType.User)
        {
            if (postListJson["itemLocation"]["tableName"].ToString() == "USER_DATA")
            {
                if (postListJson["itemLocation"]["column"].ToString() == "inventory")
                {
                    var itemObj = postListJson["item"] as JObject;
                    foreach (var property in itemObj.Properties())
                    {
                        var itemKey = property.Name;
                        var itemValue = property.Value;
                        post.postReward.Add(itemKey, int.Parse(itemValue.ToString()));
                    }
                }
                else
                {
                    Debug.LogWarning("아직 지원되지 않는 컬럼 정보 입니다. : " +
                                    postListJson["itemLocation"]["column"].ToString());
                }
            }
            else
            {
                Debug.LogWarning("아직 지원되지 않는 테이블 정보 입니다. : " +
                                postListJson["itemLocation"]["tableName"].ToString());
            }
        }
        else
        {
            var items = postListJson["items"] as JArray;
            foreach (var itemJson in items)
            {
                if (itemJson["chartName"].ToString() == chartName)
                {
                    string itemName = itemJson["item"]["itemName"].ToString();
                    int itemCount = int.Parse(itemJson["itemCount"].ToString());

                    if (post.postReward.ContainsKey(itemName))
                    {
                        post.postReward[itemName] += itemCount;
                    }
                    else
                    {
                        post.postReward.Add(itemName, itemCount);
                    }

                    post.isCanReceive = true;
                }
                else
                {
                    Debug.LogWarning("아직 지원되지 않는 차트 정보 입니다. : " + itemJson["chartName"].ToString());
                    post.isCanReceive = false;
                }
            }
        }

        _postList.Add(post);
    }

    for (int i = 0; i < _postList.Count; i++)
    {
        Debug.Log($"{i}번 째 우편\n" + _postList[i].ToString());
    }
}
```

## 2. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
**뒤끝 초기화와 뒤끝 로그인**이 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

### BackendManager.cs

**수정 전**  

```js
async void Test()
{
    await BackendLogin.Instance.CustomLogin("user1", "1234"); // 뒤끝 로그인 함수

    // 우편 로직 추가

    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
async void Test()
{
    await BackendLogin.Instance.CustomLogin("user1", "1234"); // 뒤끝 로그인 함수

    await BackendPost.Instance.PostListGet(MailType.Admin); // [추가] 우편 리스트 불러오기

    Debug.Log("테스트를 종료합니다.");
}
```

## 3. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/post/post-get-success-log.png" />

이때 로그에서 **'콘솔에서 발송된 우편들의 목록'**이 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [GetPostList 에러케이스](/sdk-docs/backend/base/post/get-list/adminstrator)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

### 에러 정보

**'아직 지원되지 않는 차트 정보 입니다'라는 경고 로그가 발생할 경우**  
해당 코드에서 사용하는 차트 이름은 '아이템 차트'를 기본적으로 사용합니다. 만약 우편 발송 시에 등록한 차트의 제목이 '아이템 차트'가 아니라면 위 코드에서 chartName을 '아이템 차트'가 아닌 설정한 차트 이름으로 변경해주세요.  

## 4. 콘솔에서 우편 조회 여부 확인

유저가 우편을 조회할 경우, 해당 우편의 발송 대상칸에서는 **미수령**으로 표기되는 유저를 볼 수 있습니다.  
해당 유저가 우편을 수령할 경우에는 수령일에 수령한 날짜가 기입됩니다.  

**우편 조회 전**  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/post/post-get-success-console.png" />

**우편 조회 후**  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/post/post-get-success-console2.png" />

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/post/get-own-and-save">다음  Step으로</a>
</div>
