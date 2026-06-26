---
sidebar_label: 그룹 내 유저 리스트 불러오기
description: "그룹 내 유저 리스트 불러오기"
---

# GetGroupMembers
public Task&lt;GetGroupMembersResult&gt; **GetGroupMembersAsync**(string **groupName**);  
public Task&lt;GetGroupMembersResult&gt; **GetGroupMembersAsync**(string **groupName**, int **limit**);  
public Task&lt;GetGroupMembersResult&gt; **GetGroupMembersAsync**(string **groupName**, int **limit**, string **offsetKey**);  

## 파라미터
| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| groupUuid      | string | 불러올 그룹의 이름 |
| limit      | int      | 불러올 유저의 수(default = 10) |
| offsetKey      | string      | limit 보다 더 많은 그룹을 불러오고자 할 경우, reqResult.GetOffsetKey()를 통해 다음 그룹들을 불러올 수 있습니다. |

## 설명
현재 속해있는 내 그룹의 정보를 불러옵니다.  
* 해당 함수는 SendQueue로 호출할 수 없습니다.

## Example

### Task 형식
```js
var groupListResult = await BackndGroup.Instance.GetGroupsAsync();
var groupName = groupListResult.GetInfoList()[0].GroupName;

var memberResult = await BackndGroup.Instance.GetGroupMembersAsync(groupName);
if (memberResult.IsSuccess())
{
    Debug.Log(memberResult.GetGroupName());
    Debug.Log(memberResult.GetUserCount());

    var memberInfoList = memberResult.GetInfoList();
    foreach (var memberInfo in memberInfoList)
    {
        Debug.Log(memberInfo.ToString());
    }
}

// 유저를 더 불러오고자 할 경우
if (memberResult.HasOffsetKey())
{
    var memberResult2 = await BackndGroup.Instance.GetGroupMembersAsync(groupName, 10, memberResult.GetOffsetKey());
    if (memberResult2.IsSuccess())
    {
        foreach (var memberInfo in memberResult2.GetInfoList())
        {
            Debug.Log(memberInfo.ToString());
        }
    }
}
```

### Callback 형식
```js
BackndGroup.Instance.GetGroupMembers("groupName", 10, callback =>
{
    if (callback.HasOffsetKey())
    {
        BackndGroup.Instance.GetGroupMembers("groupName", 10, callback.GetOffsetKey(), callback2 =>
        {
            Debug.Log(callback2);
        });
    }
});
```

## ReturnCase

### Success cases

**블러오기에 성공한 경우**  
StatusCode : 200  
Message : Success  
ReturnValue : ReturnValueJson 참조

### Error cases

**그룹 이름이 null이거나 string.Empty일 경우**  
StatusCode : 400   
ErrorCode : ValidationException   
Message : groupName is null or string.Empty   

**그룹이 존재하지 않을 경우**  
StatusCode : 404  
ErrorCode : NotFoundException  
Message : group not exist  

## ReturnValueJson
```js
{
  "rows": [
    {
      "lastLoginDate": "2024-08-14T06:35:03.871Z",
      "inDate": "2024-08-14T06:17:53.290Z",
      "nickname": ""
    },
    {
      "lastLoginDate": "2024-08-08T07:24:01.850Z",
      "inDate": "2024-08-08T07:24:01.850Z",
      "nickname": "user1"
    },
    {
      "lastLoginDate": "2024-08-08T05:19:02.793Z",
      "inDate": "2024-08-08T05:19:02.793Z",
      "nickname": "user2"
    }
  ],
  "summary": {
    "userCount" : 100,
    "groupName" : "그룹이름"
  },
  "LastEvaluatedKey": {
    "gamer_id": {
      "S": "01912b69-0890-7fdc-bb12-1710833fb558"
    },
    "inDate": {
      "S": "그룹이름#user#2024-06-18T09:26:03.850Z#c8a47aa0-2d54-11ef-9eb4-01759d9a8cf5"
    }
  }
}
```
