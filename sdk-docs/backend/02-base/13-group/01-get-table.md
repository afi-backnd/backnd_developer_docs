---
sidebar_label: 그룹 리스트 불러오기
---

# Table.Get
public BackendGroupReturnObject **Get**(**int** limit, **LastEvaluatedKey** lastEvaluatedKey);    
public BackendGroupReturnObject **Get**(**int** limit);  
public BackendGroupReturnObject **Get**();  

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| limit      | 100 | 불러올 최대 그룹의 수 |
| lastEvaluatedKey      | LastEvaluatedKey      | limit 보다 더 많은 그룹을 불러오고자 할 경우, bro.GetLastEvaluatedKey()를 통해 다음 그룹들을 불러올 수 있습니다. |

## 설명
콘솔에 등록된 그룹 리스트를 불러옵니다.
* 해당 함수는 SendQueue로 호출할 수 없습니다.

### BackendGroupReturnObject
```js
namespace BackEnd.Group
{
    public class GroupItem
    {
        public readonly string userCount = string.Empty;
        public readonly string groupUuid = string.Empty;
        public readonly string groupName = string.Empty;
    }

    public class BackendGroupReturnObject : BackendReturnObject
    {       
        public List<GroupItem> GetGroupList();
    }
}
```

## Example

### 동기
```js
BackEnd.Group.BackendGroupReturnObject bro = Backend.Group.Table.Get();

foreach(BackEnd.Group.GroupItem groupItem in bro.GetGroupList()) 
{
    string groupUUID = groupItem.groupUuid;
    string groupName = groupItem.groupName;
    Debug.Log(groupItem);
}
```

### 비동기
```js
Backend.Group.Table.Get(callback => {
    foreach(BackEnd.Group.GroupItem groupItem in bro.GetGroupList()) 
    {
        string groupUUID = groupItem.groupUuid;
        string groupName = groupItem.groupName;
        Debug.Log(groupItem);
    }
});
```

## ReturnCase

### Success cases

**블러오기에 성공한 경우**  
StatusCode : 200  
Message : Success  
ReturnValue : GetReturnValuetoJSON 참조

### Error cases

**그룹이 존재하지 않을 경우**  
StatusCode : 200  
ErrorCode : Success  
Message : {"rows":[]}  

## GetReturnValuetoJSON
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
