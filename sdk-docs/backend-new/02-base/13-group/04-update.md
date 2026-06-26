---
sidebar_label: 내 유저그룹 변경하기
description: "내 유저그룹 변경하기"
---

# ChangeUserGroup 
public Task&lt;RequestResult&gt; **ChangeUserGroupAsync**(string **groupUuid**, string **groupName**);  

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| groupUuid      | string | 변경할 그룹의 UUID |
| groupName      | string      | 변경할 그룹의 이름 |

## 설명
자신이 속한 그룹을 변경합니다.
* 길드마스터일 경우, 그룹 변경이 불가능하며, 길드를 탈퇴해야 가능합니다.  
* 길드원일 경우, 길드에서 탈퇴됩니다.
* 현재 속한 그룹에 동일하게 그룹 변경을 시도할 경우, 204 Success가 호출됩니다.

## Example

### Task 형식
```js
var groupListResult = await BackndGroup.Instance.GetGroupsAsync();
var groupInfo = groupListResult.GetInfoList()[0];

var changeResult = await BackndGroup.Instance.ChangeUserGroupAsync(groupInfo.GroupUuid, groupInfo.GroupName);
if (changeResult.IsSuccess() == false)
{
    Debug.LogError(changeResult);
}
```

### Callback 형식
```js
BackndGroup.Instance.GetGroups(groupListResult =>
{
    var groupInfo = groupListResult.GetInfoList()[0];
    BackndGroup.Instance.ChangeUserGroup(groupInfo.GroupUuid, groupInfo.GroupName, callback =>
    {
        if (callback.IsSuccess() == false)
        {
            Debug.LogError(callback);
        }

    });
});
```

## ReturnCase

### Success cases

**그룹 변경에 성공한 경우**  
statusCode : 204   
message : Success    

### Error cases

**입력한 그룹의 정보가 일치하지 않을 경우**  
StatusCode : 400  
ErrorCode : BadParameterException  
Message : group not exist  

**groupUuid가 string.Empty이거나 null일 경우**  
StatusCode : 400  
ErrorCode : ValidationException  
Message : groupUuid is null or string.Empty  

**길드마스터인 유저가 변경을 시도할 경우**  
StatusCode : 400  
ErrorCode : BadParameterException  
Message : guild master can not change group

**통합 진행 중 상태인 그룹으로 변경을 시도 할 경우**  
StatusCode : 403  
ErrorCode : ForbiddenException  
Message : task is running {from} {to}  
