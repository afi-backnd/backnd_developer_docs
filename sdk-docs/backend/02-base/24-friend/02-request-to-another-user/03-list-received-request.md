---
sidebar_label: 친구 요청 받은 리스트 조회
---

# GetReceivedRequestList

public BackendReturnObject **GetReceivedRequestList**();  
public BackendReturnObject **GetReceivedRequestList**(int **limit**);  
public BackendReturnObject **GetReceivedRequestList**(int **limit**, int **offset**);

## 파라미터

| Value  | Type | Description                                 | default |
| ------ | ---- | ------------------------------------------- | :-----: |
| limit  | int  | (Optional) 불러올 친구 요청받은 리스트의 수 |   100   |
| offset | int  | (Optional) 불러올 친구 요청받은 리스트의 시작점 |    -    |

## 설명

친구 요청을 받은 리스트를 조회합니다.  

## Example

### 동기

```js
// 친구 요청을 받은 리스트 전체 조회
Backend.Friend.GetReceivedRequestList();
// limit, offset 사용하여 친구 요청을 받은 리스트 조회
Backend.Friend.GetReceivedRequestList(5); // 친구 요청을 받은 5명 리스트 조회(1-5)
Backend.Friend.GetReceivedRequestList(5, 5); // 친구 요청을 받은 처음 5명 이후의 5명 리스트 조회(6-10)
```

### 비동기

```js
// 친구 요청을 받은 리스트 전체 조회
Backend.Friend.GetReceivedRequestList((callback) => {
  // 이후 처리
});
// limit, offset 사용하여 친구 요청을 받은 리스트 조회
Backend.Friend.GetReceivedRequestList(5, (callback) => {
  // 친구 요청을 받은 5명 리스트 조회(1-5)
  // 이후 처리
});
Backend.Friend.GetReceivedRequestList(5, 5, (callback) => {
  // 친구 요청을 받은 처음 5명 이후의 5명 리스트 조회(6-10)
  // 이후 처리
});
```

### SendQueue

```js
// 친구 요청을 받은 리스트 전체 조회
SendQueue.Enqueue(Backend.Friend.GetReceivedRequestList, (callback) => {
  // 이후 처리
});
// limit, offset 사용하여 친구 요청을 받은 리스트 조회
SendQueue.Enqueue(Backend.Friend.GetReceivedRequestList, 5, (callback) => {
  // 친구 요청을 받은 5명 리스트 조회(1-5)
  // 이후 처리
});
SendQueue.Enqueue(Backend.Friend.GetReceivedRequestList, 5, 5, (callback) => {
  // 친구 요청을 받은 처음 5명 이후의 5명 리스트 조회(6-10)
  // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

## GetReturnValuetoJSON

```js
{
  rows: [
    // 닉네임이 없는 유저의 경우
    {
      // 게임 유저 inDate
      inDate: { S: "2018-12-27T04:49:00.493Z" },
      // 친구 요청을 보낸 시간
      createdAt: { S: "2018-12-27T04:49:04.407Z" },
    },
    // 닉네임이 있는 유저의 경우
    {
      // 게임 유저 닉네임
      nickname: { S: "customid2" },
      // 게임 유저 inDate
      inDate: { S: "2018-12-18T01:58:18.722Z" },
      // 친구 요청을 보낸 시간
      createdAt: { S: "2018-12-27T04:43:18.715Z" },
    },
    {
      nickname: [Object],
      inDate: [Object],
      createdAt: [Object],
    },
  ];
}
```

## Sample Code

```js
public class FriendReqeustItem
{
    public string nickname;
    public string inDate;
    public string createdAt;
    public override string ToString()
    {
        return $"nickname : {nickname}\ninDate : {inDate}\ncreatedAt : {createdAt}\n";
    }
};
```

```js
public void GetReceivedRequestListTest()
{
    var bro = Backend.Friend.GetReceivedRequestList();

    if(!bro.IsSuccess())
        return;

    LitJson.JsonData json = bro.FlattenRows();
    List<FriendReqeustItem> freindList = new List<FriendReqeustItem>();

    for(int i = 0; i < json.Count; i++)
    {
        FriendReqeustItem friendItem = new FriendReqeustItem();

        if(json[i].ContainsKey("nickname"))
        {
            friendItem.nickname = json[i]["nickname"].ToString();
        }
        friendItem.inDate = json[i]["inDate"].ToString();
        friendItem.createdAt = json[i]["createdAt"].ToString();

        freindList.Add(friendItem);
        Debug.Log(friendItem.ToString());
    }
}
```
