---
sidebar_label: 내 그룹 불러오기
description: "내 그룹 불러오기"
---

# GetUserGroup
public Task&lt;GetUserGroupResult&gt; **GetUserGroupAsync**;

## 설명
현재 속해있는 내 그룹의 정보를 불러옵니다.  
* 해당 함수는 SendQueue로 호출할 수 없습니다.

## Example

### Task 형식
```js
var reqResult = await BackndGroup.Instance.GetUserGroupAsync();
if (reqResult.IsSuccess())
{
    var groupInfo = reqResult.GetInfo();            

    string groupUuid = groupInfo.GroupUuid;
    string groupName = groupInfo.GroupName;
    Debug.Log(groupInfo.ToString());
}
```

### Callback 형식
```js
BackndGroup.Instance.GetUserGroup(callback =>
{
    if (callback.IsSuccess())
    {
        var groupInfo = reqResult.GetInfo();

        string groupUuid = groupInfo.GroupUuid;
        string groupName = groupInfo.GroupName;
        Debug.Log(groupInfo.ToString());
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

**그룹이 존재하지 않을 경우**  
StatusCode : 404  
ErrorCode : NotFoundException  
Message : group not exist  

## ReturnValueJson
```js
{
    "row": {
        "userCount": 107,
        "groupUuid": "01912b69-0890-7fdc-bb12-1710833fb558",
        "groupName": "그룹이름"
    }
}
```
