---
sidebar_label: 친구 목록 조회
---

# GetFriends

public Task< GetFriendsResult > **GetFriendsAsync**();  
public Task< GetFriendsResult > **GetFriendsAsync**(int **limit**);  
public Task< GetFriendsResult > **GetFriendsAsync**(int **limit**, int **offset**);

## 파라미터

| Value  | Type | Description                            | default |
| ------ | ---- | -------------------------------------- | :-----: |
| limit  | int  | (Optional) 불러올 친구 리스트의 수     |   100   |
| offset | int  | (Optional) 불러올 친구 리스트의 시작점 |    0    |

## 설명

친구 목록을 조회합니다.  

> 친구는 **[콘솔 - 프로젝트 설정 - 소셜 설정]**에서 설정한 최대 보유 수 만큼만 맺을 수 있습니다. 친구 최대 보유 수의 default는 0으로, 콘솔에서 설정하신 이후에 사용 가능합니다.  

## Example

### Task 방식

```js
// 전체 친구 리스트 조회
var reqResult = await BackndFriend.Instance.GetFriendsAsync();
// limit, offset 사용하여 친구 리스트 조회
var reqResult2 = await BackndFriend.Instance.GetFriendsAsync(5); // 5명 친구 조회(1-5)
var reqResult3 = await BackndFriend.Instance.GetFriendsAsync(5, 5); // 처음 5명 이후의 5명 친구 조회(6-10)
```

### Callback 방식

```js
BackndFriend.Instance.GetFriends((callback) =>
{
  // 이후 처리
});
BackndFriend.Instance.GetFriends(5, (callback) =>
{
  // 5명 친구 조회(1-5)
  // 이후 처리
});
BackndFriend.Instance.GetFriends(5, 5, (callback) =>
{
  // 처음 5명 이후의 5명 친구 조회(6-10)
  // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

**친구가 존재하지 않을 경우**  
statusCode : 200  
message : Success  
returnValue : `{"rows":[]}`

## ReturnValueJson

```js
{
    rows:
    [
        // 닉네임이 없는 유저의 경우
        {
            "inDate": // 게임 유저 inDate
                { "S" : "2018-12-27T04:49:00.493Z"},
            "createdAt": // 친구가 된 시간
                { "S" : "2018-12-27T04:49:58.140Z"}
            "lastLogin": // 마지막으로 접속한 날짜
                {"S":"2021-07-23T09:13:14.186Z"}
        },
        // 닉네임이 있는 유저의 경우
        {
            "nickname": // 게임 유저 닉네임
                { "S" : "customid2"},
            "inDate": // 게임 유저 inDate
                { "S" : "2018-12-18T01:58:18.722Z"},
            "createdAt": // 친구가 된 시간
                { "S" : "2018-12-27T04:50:03.955Z"}
            "lastLogin": // 마지막으로 접속한 날짜
                {"S":"2021-07-23T09:13:14.186Z"}
        },
        {
            nickname: [Object],
            inDate: [Object]
        }
    ]
}
```

## Sample Code

```js
public class FriendItem
{
    public string nickname;
    public string inDate;
    public string lastLogin;
    public string createdAt;
    public override string ToString()
    {
        return $"nickname : {nickname}\ninDate : {inDate}\nlastLogin : {lastLogin}\ncreatedAt : {createdAt}\n";
    }
};
```

```js
public async Task GetFriendListTest()
{
    var reqResult = await BackndFriend.Instance.GetFriendsAsync();
    if (!reqResult.IsSuccess())
        return;

    var infoList = reqResult.GetInfoList();
    List<FriendItem> freindList = new List<FriendItem>();

    for (int i = 0; i < infoList.Count; i++)
    {
        var info = infoList[i];

        FriendItem friendItem = new FriendItem();
        friendItem.nickname = info.Nickname;
        friendItem.inDate = info.InDate;
        friendItem.lastLogin = info.LastSignIn;
        friendItem.createdAt = info.CreateAt;

        freindList.Add(friendItem);
        Debug.Log(friendItem.ToString());
    }
}
```
