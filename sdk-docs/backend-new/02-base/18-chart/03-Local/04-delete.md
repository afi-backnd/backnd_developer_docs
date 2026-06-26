---
sidebar_label: "로컬 삭제"
description: "로컬 삭제"
---

# Local.Delete

public bool **Delete**(List< string > **tableIdList**, out Exception **e**);  
public void **Delete**(List< string > **tableIdList**,  Action< bool, Exception > **callback**);  
public Task< (bool, Exception) > **DeleteAsync**(List< string > **tableIdList**,  out Exception **exception**);

## 파라미터
| Value| Type | Description |
| :--- | :--- | :--- |
| tableIdList | List< string > | 로컬에 저장된 테이블을 삭제합니다. |


## 설명
로컬에 저장된 테이블을 삭제합니다.  
* 해당 함수는 SendQueue로 호출할 수 없습니다.

## Example
### Task 방식
```js
var chartIdList = new List<string>()
{
    "12038", "546", "7419"
};
var deleteResult = await BackndGameTable.Instance.Local.DeleteAsync(chartIdList);
if (deleteResult.Item1 == true)
{
    // 요청 성공 시, 처리 코드 작성.
} 
```

### Callback 방식
```js
var chartIdList = new List<string>()
{
    "12038", "546", "7419"
};
BackndGameTable.Instance.Local.Delete(chartIdList, (isSuccess, exception) =>
{
    if (isSuccess)
    {
        // 요청 성공 시, 처리 코드 작성.
    }
});
```
