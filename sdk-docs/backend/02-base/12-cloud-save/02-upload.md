---
sidebar_label: "세이브 업로드 하기"
description: "Upload"
---

# Upload
public BackendReturnObject **Upload**(**string** collectionName, **string** jsonString);  
public BackendReturnObject **Upload**(**string** collectionName, **Param** param);  
  
:::warning 클라우드 세이브 이용 시 주의 사항
클라우드 세이브는 **저비용 데이터 저장**에 최적화된 기능입니다.  
이에 따라 다음 기능이 지원되지 않으니, 서비스 용도에 맞춰 도입을 결정해 주시기 바랍니다.

- **데이터 백업 미지원**: 자동 데이터 백업이 제공되지 않습니다.
- **서버 요청 로그 미저장**: 서버 요청에 대한 상세 로그가 저장되지 않습니다. 에러 발생 등의 상황 추적이 필요한 경우, 클라이언트 측에서 직접 로그를 관리해 주셔야 합니다.

데이터 유실 리스크가 있거나 정밀한 추적이 필요한 중요 정보는 **데이터베이스** 또는 **게임 정보** 이용을 권장합니다.
:::  
  
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
### 동기
#### Case 1
```js
var bro = Backend.CloudSave.Upload("collectionName", jsonString);
if(bro.IsSuccess())
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

var bro = Backend.CloudSave.Upload("collectionName", param);
if(bro.IsSuccess())
{    
    // 요청 성공 시, 처리 코드 작성.
}
```

### 비동기
#### Case 1
```js
Backend.CloudSave.Upload("collectionName", jsonString, bro =>
{
    if(bro.IsSuccess())
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

Backend.CloudSave.Upload("collectionName", param, bro =>
{
    if(bro.IsSuccess())
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
