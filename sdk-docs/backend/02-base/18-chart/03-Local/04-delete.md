---
sidebar_label: "로컬 삭제"
description: "Backend.CDN.Content.Local.Delete"
---

# Backend.CDN.Content.Local.Delete
public bool **Backend.CDN.Content.Local.Delete**(List< string > **chartIdList**,  out Exception **exception**);
public void **Backend.CDN.Content.Local.Delete**(List< string > **chartIdList**,  Action< bool, Exception > **callback**);  

## 파라미터
| Value| Type | Description |
| :--- | :--- | :--- |
| chartIdList | List< string > | 로컬에 저장된 차트를 삭제합니다. |


## 설명
로컬에 저장된 차트를 삭제합니다.  
* 해당 함수는 SendQueue로 호출할 수 없습니다.

## Example
### 동기
```js
var chartIdList = new List<string>()
{
    "12038", "546", "7419"
};
var isSuccess = Backend.CDN.Content.Local.Delete(chartIdList, out var exception);
if (isSuccess)
{
    // 요청 성공 시, 처리 코드 작성.
}   
```

### 비동기
```js
var chartIdList = new List<string>()
{
    "12038", "546", "7419"
};
Backend.CDN.Content.Local.Delete(chartIdList, (isSuccess, exception) =>
{
    if (isSuccess)
    {
        // 요청 성공 시, 처리 코드 작성.
    }
});   
```
