# 전체코드
## BackendPost
```js
using System.Collections.Generic;
using System.Text;
using UnityEngine;

// 뒤끝 SDK namespace 추가
using BackEnd;

public class Post
{
    public bool isCanReceive = false;

    public string title; // 우편 제목
    public string content; // 우편 내용
    public string inDate; // 우편 inDate

    // string은 우편 아이템 이름, int는 갯수
    public Dictionary<string, int> postReward = new Dictionary<string, int>();

    public override string ToString()
    {
        string result = string.Empty;
        result += $"title : {title}\n";
        result += $"content : {content}\n";
        result += $"inDate : {inDate}\n";

        if (isCanReceive)
        {
            result += "우편 아이템\n";

            foreach (string itemKey in postReward.Keys)
            {
                result += $"| {itemKey} : {postReward[itemKey]}개\n";
            }
        }
        else
        {
            result += "지원하지 않는 우편 아이템입니다.";
        }

        return result;
    }
}

public class BackendPost
{
    private static BackendPost _instance = null;

    public static BackendPost Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new BackendPost();
            }

            return _instance;
        }
    }

    private List<Post> _postList = new List<Post>();

    public void SavePostToLocal(LitJson.JsonData item)
    {
        foreach (LitJson.JsonData itemJson in item)
        {
            if (itemJson["item"].ContainsKey("itemType"))
            {
                int itemId = int.Parse(itemJson["item"]["itemId"].ToString());
                string itemType = itemJson["item"]["itemType"].ToString();
                string itemName = itemJson["item"]["itemName"].ToString();
                int itemCount = int.Parse(itemJson["itemCount"].ToString());

                if (BackendGameData.userData.inventory.ContainsKey(itemName))
                {
                    BackendGameData.userData.inventory[itemName] += itemCount;
                }
                else
                {
                    BackendGameData.userData.inventory.Add(itemName, itemCount);
                }

                Debug.Log($"아이템을 수령했습니다. : {itemName} - {itemCount}개");
            }
            else
            {
                Debug.LogError("지원하지 않는 item입니다.");
            }
        }
    }

    public void PostListGet(PostType postType)
    {
        var bro = Backend.UPost.GetPostList(postType);

        if (bro.IsSuccess() == false)
        {
            Debug.LogError("우편 불러오기 중 에러가 발생했습니다.");
            return;
        }

        Debug.Log("우편 리스트 불러오기 요청에 성공했습니다. : " + bro);

        if (bro.GetFlattenJSON()["postList"].Count <= 0)
        {
            Debug.LogWarning("받을 우편이 존재하지 않습니다.");
            return;
        }

        foreach (LitJson.JsonData postListJson in bro.GetFlattenJSON()["postList"])
        {
            Post post = new Post();

            post.title = postListJson["title"].ToString();
            post.content = postListJson["content"].ToString();
            post.inDate = postListJson["inDate"].ToString();

            // 우편의 아이템
            if (postType == PostType.User)
            {
                if (postListJson["itemLocation"]["tableName"].ToString() == "USER_DATA")
                {
                    if (postListJson["itemLocation"]["column"].ToString() == "inventory")
                    {
                        foreach (string itemKey in postListJson["item"].Keys)
                        {
                            post.postReward.Add(itemKey, int.Parse(postListJson["item"][itemKey].ToString()));
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
                foreach (LitJson.JsonData itemJson in postListJson["items"])
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

    public void PostReceive(PostType postType, int index)
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

        var bro = Backend.UPost.ReceivePostItem(postType, _postList[index].inDate);

        if (bro.IsSuccess() == false)
        {
            Debug.LogError($"{postType.ToString()}의 {_postList[index].inDate} 우편수령 중 에러가 발생했습니다. : " + bro);
            return;
        }

        Debug.Log($"{postType.ToString()}의 {_postList[index].inDate} 우편수령에 성공했습니다. : " + bro);

        _postList.RemoveAt(index);

        if (bro.GetFlattenJSON()["postItems"].Count > 0)
        {
            SavePostToLocal(bro.GetFlattenJSON()["postItems"]);
        }
        else
        {
            Debug.LogWarning("수령 가능한 우편 아이템이 존재하지 않습니다.");
        }

        BackendGameData.Instance.GameDataUpdate();
    }

    public void PostReceiveAll(PostType postType)
    {
        if (_postList.Count <= 0)
        {
            Debug.LogWarning("받을 수 있는 우편이 존재하지 않습니다. 혹은 우편 리스트 불러오기를 먼저 호출해주세요.");
            return;
        }

        Debug.Log($"{postType.ToString()} 우편 모두 수령을 요청합니다.");

        var bro = Backend.UPost.ReceivePostItemAll(postType);

        if (bro.IsSuccess() == false)
        {
            Debug.LogError($"{postType.ToString()} 우편 모두 수령 중 에러가 발생했습니다 : " + bro);
            return;
        }

        Debug.Log("우편 모두 수령에 성공했습니다. : " + bro);

        _postList.Clear();

        foreach (LitJson.JsonData postItemsJson in bro.GetFlattenJSON()["postItems"])
        {
            SavePostToLocal(postItemsJson);
        }

        BackendGameData.Instance.GameDataUpdate();
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
        BackendLogin.Instance.CustomLogin("user1", "1234");

        // 게임데이터를 불러와 로컬에 저장합니다.(캐싱)
        BackendGameData.Instance.GameDataGet();

        // 우편 리스트를 불러와 우편의 정보와 inDate값들을 로컬에 저장합니다.  
        BackendPost.Instance.PostListGet(PostType.Admin);

        // 저장된 우편의 위치를 읽어 우편을 수령합니다. 여기서 index는 우편의 순서. 0이면 제일 윗 우편, 1이면 그 다음 우편
        BackendPost.Instance.PostReceive(PostType.Admin, 0);

        // 조회된 모든 우편을 수령합니다.  
        BackendPost.Instance.PostReceiveAll(PostType.Admin);

        Debug.Log("테스트를 종료합니다.");
    }
}
```

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/coupon/before">다음  챕터로</a>
</div>
