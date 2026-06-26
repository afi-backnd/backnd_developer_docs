---
sidebar_label: 세이브 업로드 하기
description: "세이브 업로드 하기"
---

# UploadSave
public Task&lt;RequestResult&gt; **UploadSaveAsync**(**string** collectionName, **string** jsonString);  
public Task&lt;RequestResult&gt; **UploadSaveAsync**(**string** collectionName, **Param** param);  

## 파라미터
| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| collectionName  | string | 데이터가 업로드 될 컬렉션 이름 |
| jsonString      | string | JSON 문자열 형식의 저장 데이터 |
| param | [Param](/sdk-docs/backend/base/knowhow/param/Param) | Param 형식의 저장 데이터 |

## 설명
저장 데이터를 클라우드 저장소로 업로드 합니다.  
* **컬렉션은 콘솔을 통해 미리 생성되어 있어야 합니다.**  
* **JSON 형태로 구성된 문자열만 저장 가능합니다.**  
* **컬렉션마다 각각 한개의 데이터만 저장 가능합니다. 기존 데이터가 있다면 새로운 데이터로 덮어씁니다.**  
* **각 데이터의 저장 가능한 최대 크기는 1MB입니다.**  
* **일반적인 응답 시간은 평균 800ms이지만 처리량이 많은 경우, 2초 이상 응답 지연이 발생할 수 있습니다.**  

## Example

### Task 형식
#### Case 1
```js
var reqResult = await BackndCloudSave.Instance.UploadSaveAsync("collectionName", jsonString);
if (reqResult.IsSuccess())
{
    // 요청 성공 시, 처리 코드 작성.
}
```
#### Case 2
```js
// sample data class
public class SampleUnit
{
    public string className;
    public int level;        
    public double Power { get; set; }  
}
...

var archer = new SampleUnit()
{
    className = "archer", level = 44, Power = 34534.59
};
var param = new Param();
param.Add("user_name", "backend");
param.Add("stage_clear", 1024);
param.Add("archer", archer);

var reqResult = await BackndCloudSave.Instance.UploadSaveAsync("collectionName", param);
if (reqResult.IsSuccess())
{
    // 요청 성공 시, 처리 코드 작성.
}
```

### Callback 형식
#### Case 1
```js
BackndCloudSave.Instance.UploadSaveAsync("collectionName", jsonString, reqResult =>
{
    if (reqResult.IsSuccess())
    {
        // 요청 성공 시, 처리 코드 작성.
    }
});
```
#### Case 2
```js
// sample data class
public class SampleUnit
{
    public string className;
    public int level;        
    public double Power { get; set; }  
}
...

var archer = new SampleUnit()
{
    className = "archer", level = 44, Power = 34534.59
};
var param = new Param();
param.Add("user_name", "backend");
param.Add("stage_clear", 1024);
param.Add("archer", archer);

BackndCloudSave.Instance.UploadSaveAsync("collectionName", param, reqResult =>
{
    if (reqResult.IsSuccess())
    {
        // 요청 성공 시, 처리 코드 작성.
    }
});
```

## ReturnCase

### Success cases

**업로드에 성공한 경우**  
StatusCode : 204  
Message : Success  

### Error cases

**JSON 형태의 데이터가 아닌 경우**  
StatusCode : 400  
ErrorCode : ValidationException  
Message : Failed to parse the string into JSON.  

**데이터 크기가 제한값을 초과하는 경우**  
StatusCode : 400  
ErrorCode : ValidationException  
Message : The string size is too big.  

**컬렉션이 존재하지 않는 경우**  
StatusCode : 404  
Message : not exist folder  
Code : NotFound  
