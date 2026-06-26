---
sidebar_label: 세이브 다운로드 하기
description: "세이브 다운로드 하기"
---

# DownloadSave
public Task&lt;RequestResult&gt; **DownloadSaveAsync**(**string** collectionName);  

## 파라미터
| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| collectionName  | string | 데이터가 위치한 컬렉션 이름 |

## 설명
저장 데이터를 클라우드 저장소에서 다운로드 합니다.  

## Example
### Task 형식
```js
// sample data class
public class SampleUnit
{
    public string className;
    public int level;        
    public double Power { get; set; }  
}
...

var reqResult = await BackndCloudSave.Instance.DownloadSaveAsync("collectionName");
if (reqResult.IsSuccess())
{   
    // 사용하고 있는 Json Parser로 데이터 파싱.
    var jsonString = reqResult.ReturnValue;

    // Example.(Newtonsoft Json 사용)
    var jsonObj = JObject.Parse(jsonString);
    var userName = jsonObj["user_name"].ToString();
    var stageClear = (int)jsonObj["stage_clear"];
    var archer = JsonConvert.DeserializeObject<SampleUnit>(jsonObj["archer"].ToString());
}
```

### Callback 형식
```js
// sample data class
public class SampleUnit
{
    public string className;
    public int level;        
    public double Power { get; set; }  
}
...

BackndCloudSave.Instance.DownloadSave("collectionName", reqResult =>
{
    if (reqResult.IsSuccess())
    {
        // 사용하고 있는 Json Parser로 데이터 파싱.
        var jsonString = reqResult.ReturnValue;

        // Example.(Newtonsoft Json 사용)
        var jsonObj = JObject.Parse(jsonString);
        var userName = jsonObj["user_name"].ToString();
        var stageClear = (int)jsonObj["stage_clear"];
        var archer = JsonConvert.DeserializeObject<SampleUnit>(jsonObj["archer"].ToString());
    }
});
```

## ReturnCase

### Success cases

**다운로드에 성공한 경우**  
StatusCode : 200  
Message : Success  
ReturnValue : ReturnValueJson 참조

### Error cases

**컬렉션이 존재하지 않는 경우**  
StatusCode : 404  
Message : not exist folder  
Code : NotFound  

**데이터가 존재하지 않는 경우**  
StatusCode : 404  
Message : not exist file  
Code : NotFound  

## ReturnValueJson
```js
{
    "archer": {
        "Power": 34534.59,
        "className": "archer",
        "level": 44
    },
    "stage_clear": 1024,
    "user_name": "backend"
}
```
