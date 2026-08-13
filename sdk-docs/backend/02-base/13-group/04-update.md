---
sidebar_label: "내 유저그룹 변경하기"
description: "Update"
---

# Update
public BackendReturnObject **Update**(string **groupUuid**, string **groupName**);  
public BackendReturnObject **Update**(string **groupUuid**, string **groupName**, LeaderboardValuePolicy **leaderboardValuePolicy**);  

## 파라미터

| Value                | Type                 | Description |
| :------------------- | :------------------- | :---------- |
| groupUuid            | string               | 변경할 그룹의 UUID |
| groupName            | string               | 변경할 그룹의 이름 |
| leaderboardValuePolicy | LeaderboardValuePolicy | (Optional) 그룹 변경 후 리더보드 연동 게임정보 처리 방식 |

## 설명
자신이 속한 그룹을 변경합니다.
* 길드마스터일 경우 그룹을 변경할 수 없으며, 길드를 탈퇴한 뒤 변경할 수 있습니다.  
* 길드원일 경우 그룹 변경 시 길드에서 탈퇴됩니다.  
* 현재 속한 그룹으로 다시 변경을 시도하면 204 Success가 반환됩니다.  
* 그룹이 변경되면 이전 그룹의 리더보드에 등록된 데이터는 삭제됩니다.  
* `leaderboardValuePolicy`는 그룹 변경 후 리더보드에 사용되는 집계 필드 데이터를 유지할지, 초기화할지, 리더보드에 다시 등록할지를 결정하는 옵션입니다.

## LeaderboardValuePolicy

```csharp
public enum LeaderboardValuePolicy
{
    None = 0,        // 리더보드 순위, 점수 및 집계 필드 데이터를 초기화합니다.
    Reset = 1,       // 리더보드 순위, 점수를 초기화합니다. (집계 필드 데이터는 유지)
    Recalculate = 2  // 대상 그룹의 리더보드로 통합됩니다.
}
```

세 번째 파라미터를 생략하거나 `LeaderboardValuePolicy.None`을 사용하면 리더보드 순위, 점수 및 집계 필드 데이터는 초기화됩니다.

## Example

### 동기
```csharp
BackEnd.Group.BackendGroupReturnObject tableBro = Backend.Group.Table.Get();
BackEnd.Group.GroupItem group = tableBro.GetGroupList()[0];

var bro = Backend.Group.Update(group.groupUuid, group.groupName, LeaderboardValuePolicy.Recalculate);
if(bro.IsSuccess() == false)
{
    Debug.LogError(bro);
}
```

### 비동기
```csharp
Backend.Group.GetGroupList(bro => {
    BackEnd.Group.GroupItem group = bro.GetGroupList()[0];

    Backend.Group.Update(group.groupUuid, group.groupName, LeaderboardValuePolicy.Reset, callback => {
        if(callback.IsSuccess() == false)
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
