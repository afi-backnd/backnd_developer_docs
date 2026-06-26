---
sidebar_label: 세이브 삭제 하기
description: "세이브 삭제 하기"
---

# DeleteSave
public Task&lt;RequestResult&gt; **DeleteSaveAsync**(**string** collectionName);  

## 파라미터
| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| collectionName  | string | 데이터가 위치한 컬렉션 이름 |

## 설명
저장 데이터를 클라우드 저장소에서 삭제 합니다.  

## Example
### 동기
```js
var reqResult = await BackndCloudSave.Instance.DeleteSaveAsync("collectionName");
if (reqResult.IsSuccess())
{   
    // 요청 성공 시, 처리 코드 작성.  
}
```

### 비동기
```js
BackndCloudSave.Instance.DeleteSave("collectionName", reqResult =>
{
    if (reqResult.IsSuccess())
    {   
        // 요청 성공 시, 처리 코드 작성.
    }
});
```

## ReturnCase

### Success cases

**삭제에 성공한 경우**  
StatusCode : 204   
Message : Success    

### Error cases

**컬렉션이 존재하지 않는 경우**  
StatusCode : 404  
Message : not exist folder  
Code : NotFound  

**데이터가 존재하지 않는 경우**  
StatusCode : 404  
Message : not exist file  
Code : NotFound  

