---
sidebar_label: 친구 요청 보낸 리스트 조회
---

# GetSentFriendRequests

public Task< GetSentFriendRequestsResult > **GetSentFriendRequestsAsync**();  
public Task< GetSentFriendRequestsResult > **GetSentFriendRequestsAsync**(int **limit**);  
public Task< GetSentFriendRequestsResult > **GetSentFriendRequestsAsync**(int **limit**, int **offset**);

## 파라미터

| Value  | Type | Description                                  | default |
| ------ | ---- | -------------------------------------------- | :-----: |
| limit  | int  | (Optional) 불러올 친구 요청 보낸 리스트의 수 |   100   |
| offset | int  | (Optional) 불러올 친구 요청 보낸 리스트의 시작점 |    -    |

## 설명

친구 요청을 보낸 리스트를 조회합니다.  
친구 요청받은 유저가 수락/거절하면 해당 리스트에서 삭제됩니다.  

## Example

### Task 방식

```js
// 친구 요청을 보낸 리스트 전체 조회
var reqResult = await BackndFriend.Instance.GetSentFriendRequestsAsync();
// limit, offset 사용하여 친구 요청을 보낸 리스트 조회
var reqResult2 = await BackndFriend.Instance.GetSentFriendRequestsAsync(5); // 친구 요청을 보낸 5명 리스트 조회(1-5)
var reqResult3 = await BackndFriend.Instance.GetSentFriendRequestsAsync(5, 5); // 친구 요청을 보낸 처음 5명 이후의 5명 리스트 조회(6-10)
```

### Callback 방식

```js
BackndFriend.Instance.GetSentFriendRequests((callback) =>
{
  // 이후 처리
});
// limit, offset 사용하여 친구 요청을 보낸 리스트 조회
BackndFriend.Instance.GetSentFriendRequests(5, (callback) =>
{
  // 친구 요청을 보낸 5명 리스트 조회(1-5)
  // 이후 처리
});
BackndFriend.Instance.GetSentFriendRequests(5, 5, (callback) =>
{
  // 친구 요청을 보낸 처음 5명 이후의 5명 리스트 조회(6-10)
  // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

## ReturnValueJson

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
public async Task GetSentRequestListTest()
{
    var reqResult = await BackndFriend.Instance.GetSentFriendRequestsAsync();
    if (!reqResult.IsSuccess())
        return;

    var infoList = reqResult.GetInfoList();
    List<FriendReqeustItem> freindList = new List<FriendReqeustItem>();

    for (int i = 0; i < infoList.Count; i++)
    {
        var info = infoList[i];

        FriendReqeustItem friendItem = new FriendReqeustItem();
        friendItem.nickname = info.Nickname;
        friendItem.inDate = info.InDate;
        friendItem.createdAt = info.CreateAt;

        freindList.Add(friendItem);
        Debug.Log(friendItem.ToString());
    }
}
```
