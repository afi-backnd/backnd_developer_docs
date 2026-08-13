---
sidebar_label: "컬렉션 리스트 가져오기"
description: "GetCollectionNames"
---

# GetCollectionNames
public BackendCloudSaveCollectionReturnObject **GetCollectionNames**();    

:::warning 클라우드 세이브 이용 시 주의 사항
클라우드 세이브는 **저비용 데이터 저장**에 최적화된 기능입니다.  
이에 따라 다음 기능이 지원되지 않으니, 서비스 용도에 맞춰 도입을 결정해 주시기 바랍니다.

- **데이터 백업 미지원**: 자동 데이터 백업이 제공되지 않습니다.
- **서버 요청 로그 미저장**: 서버 요청에 대한 상세 로그가 저장되지 않습니다. 에러 발생 등의 상황 추적이 필요한 경우, 클라이언트 측에서 직접 로그를 관리해 주셔야 합니다.

데이터 유실 리스크가 있거나 정밀한 추적이 필요한 중요 정보는 **데이터베이스** 또는 **게임 정보** 이용을 권장합니다.
:::  
  
## 설명
콘솔에 등록된 컬렉션 리스트를 불러옵니다.
* 해당 함수는 SendQueue로 호출할 수 없습니다.  

### BackendCloudSaveCollectionReturnObject
```js
namespace BackEnd.Functions
{
    public sealed class BackendCloudSaveCollectionReturnObject : BackendReturnObject
    {   
        public List<string> GetCollectionNameList()
    }
}
```

## Example
### 동기
```js
var bro = Backend.CloudSave.GetCollectionNames();
if (bro.IsSuccess())
{
    foreach(var name in bro.GetCollectionNameList()) 
    {    
        Debug.Log(name);
    }
}
```

### 비동기
```js
Backend.CloudSave.GetCollectionNames(bro =>
{
    if (bro.IsSuccess())
    {
        foreach(var name in bro.GetCollectionNameList()) 
        {    
            Debug.Log(name);
        }
    }    
});
```

## ReturnCase

### Success cases

**컬렉션이 한개 이상 있는 경우**  
StatusCode : 200  
Message : Success  
ReturnValue : GetReturnValuetoJSON 참조

**컬렉션이 하나도 없는 경우**  
StatusCode : 200  
Message : Success  
ReturnValue : {"result":[]}  

## GetReturnValuetoJSON
```js
{
    "result": [
        {
            "name": "collection_01"            
        },
        {
            "name": "collection_02"           
        },
        {
            "name": "collection_03"                
        }
    ]
}
```
