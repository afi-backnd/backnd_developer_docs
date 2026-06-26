---
sidebar_label: 내 그룹 불러오기
---

# Get
public BackendMyGroupReturnObject **Get**;

## 설명
현재 속해있는 내 그룹의 정보를 불러옵니다.  
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

    public class BackendMyGroupReturnObject : BackendReturnObject
    {       
        public GroupItem GetGroup();
    }
}
```

## Example

### 동기
```js
BackEnd.Group.BackendMyGroupReturnObject bro = Backend.Group.Get();

if(bro.IsSuccess()) {
    BackEnd.Group.GroupItem groupItem = bro.GetGroup();
    string groupUuid = groupItem.groupUuid;
    string groupName = groupItem.groupName;
    Debug.Log(groupItem.ToString());
}
```

### 비동기
```js
Backend.Group.Get(callback => {
    if(callback.IsSuccess()) {
        BackEnd.Group.GroupItem groupItem = bro.GetGroup();
        string groupUuid = groupItem.groupUuid;
        string groupName = groupItem.groupName;
        Debug.Log(groupItem.ToString());
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
StatusCode : 404  
ErrorCode : NotFoundException  
Message : group not exist  

## GetReturnValuetoJSON
```js
{
    "row": {
        "userCount": 107,
        "groupUuid": "01912b69-0890-7fdc-bb12-1710833fb558",
        "groupName": "그룹이름"
    }
}
```
