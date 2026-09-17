---
sidebar_label: "세이브 다운로드 하기"
description: "Download"
---

# Download
public BackendReturnObject **Download**(**string** collectionName);  
  
:::tip 클라우드 세이브 안전하게 사용하기
클라우드 세이브는 **저비용 데이터 저장소**로, 일반적인 DB와는 동작 방식이 다릅니다.  
데이터 보호를 위한 백업과 요청 로그를 제공하지 않고 기존 데이터와 비교 없이 항상 덮어씁니다.

따라서 **중요한 데이터를 저장할 때는 아래 방식으로** 구성해 주세요.

1. 메인 컬렉션과 별도로 **백업 전용 컬렉션**을 두고, 주요 시점(스테이지 클리어, 결제 완료 등)에만 함께 저장합니다.
2. 빈 데이터(`{}`)나 신규 유저 기본값(초기화 상태)은 백업 컬렉션에 업로드하지 않도록 클라이언트에서 제외합니다.
3. 다운로드가 **오류로 실패한 경우**에는 신규 유저로 처리하지 말고, 저장을 보류한 뒤 재시도합니다.

자세한 구성 방법은 [세이브 업로드 하기](/sdk-docs/backend/base/cloud-save/upload)를 참고해 주세요.
:::  
  
## 파라미터
| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| collectionName  | string | 데이터가 위치한 컬렉션 이름 |

## 설명
저장 데이터를 클라우드 저장소에서 다운로드 합니다.  

## Example
### 동기
```js
// sample data class
public class SampleUnit
{
    public string className;
    public int level;        
    public double Power { get; set; }  
}
...

var bro = Backend.CloudSave.Download("collectionName");
if(bro.IsSuccess())
{   
    // 사용하고 있는 Json Parser로 데이터 파싱.
    var jsonString = bro.ReturnValue;

    // Example.(Newtonsoft Json 사용)
    var jsonObj = JObject.Parse(jsonString);
    var userName = jsonObj["user_name"].ToString();
    var stageClear = (int)jsonObj["stage_clear"];
    var archer = JsonConvert.DeserializeObject<SampleUnit>(jsonObj["archer"].ToString());
}
```

### 비동기
```js
// sample data class
public class SampleUnit
{
    public string className;
    public int level;        
    public double Power { get; set; }  
}
...

Backend.CloudSave.Download("collectionName", bro =>
{
    if(bro.IsSuccess())
    {   
        // 사용하고 있는 Json Parser로 데이터 파싱.
        var jsonString = bro.ReturnValue;

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
ReturnValue : GetReturnValuetoJSON 참조

### Error cases

**컬렉션이 존재하지 않는 경우**  
StatusCode : 404  
Message : not exist folder  
Code : NotFound  

**데이터가 존재하지 않는 경우**  
StatusCode : 404  
Message : not exist file  
Code : NotFound  

## GetReturnValuetoJSON
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
