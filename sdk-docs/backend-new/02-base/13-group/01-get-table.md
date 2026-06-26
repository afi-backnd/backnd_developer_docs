---
sidebar_label: 그룹 리스트 불러오기
description: "그룹 리스트 불러오기"
---

# GetGroups
public Task&lt;GetGroupsResult&gt; **GetGroupsAsync**();  
public Task&lt;GetGroupsResult&gt; **GetGroupsAsync**(int **limit**);  
public Task&lt;GetGroupsResult&gt; **GetGroupsAsync**(int **limit**, string **offsetKey**);    

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| limit      | 100 | 불러올 최대 그룹의 수 |
| offsetKey      | string      | limit 보다 더 많은 그룹을 불러오고자 할 경우, reqResult.GetOffsetKey()를 통해 다음 그룹들을 불러올 수 있습니다. |

## 설명
콘솔에 등록된 그룹 리스트를 불러옵니다.
* 해당 함수는 SendQueue로 호출할 수 없습니다.

## Example

### Task 형식
```js
var reqResult = await BackndGroup.Instance.GetGroupsAsync();        
foreach (var groupInfo in reqResult.GetInfoList())
{
    string groupUUID = groupInfo.GroupUuid;
    string groupName = groupInfo.GroupName;
    Debug.Log(groupInfo);
}
```

### Callback 형식
```js
BackndGroup.Instance.GetGroups(callback =>
{            
    foreach (var groupInfo in callback.GetInfoList())
    {
        string groupUUID = groupInfo.GroupUuid;
        string groupName = groupInfo.GroupName;
        Debug.Log(groupInfo);
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
StatusCode : 200  
ErrorCode : Success  
Message : {"rows":[]}  

## ReturnValueJson
```js
{
    "rows": [
        {
            "groupName": "USA",
            "groupUuid": "01920430-b9de-7865-a170-ec03ef6dceb3",
            "userCount": 100
        },
        {
            "groupName": "ASIAN",
            "groupUuid": "01920430-a2de-78cf-94fe-d106d73b86d4",
            "userCount": 10
        },
        {
            "groupName": "TEST_GROUP",
            "groupUuid": "01920430-d6d9-7493-afc6-9b49b6c6fef3",
            "userCount": 0
        }
    ]
}
```
