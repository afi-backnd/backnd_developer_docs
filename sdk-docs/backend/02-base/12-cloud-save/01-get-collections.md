---
sidebar_label: "컬렉션 리스트 가져오기"
description: "GetCollectionNames"
---

# GetCollectionNames
public BackendCloudSaveCollectionReturnObject **GetCollectionNames**();    

:::tip 클라우드 세이브 안전하게 사용하기
클라우드 세이브는 **저비용 데이터 저장소**로, 일반적인 DB와는 동작 방식이 다릅니다.  
데이터 보호를 위한 백업과 요청 로그를 제공하지 않고 기존 데이터와 비교 없이 항상 덮어씁니다.

따라서 **중요한 데이터를 저장할 때는 아래 방식으로** 구성해 주세요.

1. 메인 컬렉션과 별도로 **백업 전용 컬렉션**을 두고, 주요 시점(스테이지 클리어, 결제 완료 등)에만 함께 저장합니다.
2. 빈 데이터(`{}`)나 신규 유저 기본값(초기화 상태)은 백업 컬렉션에 업로드하지 않도록 클라이언트에서 제외합니다.
3. 다운로드가 **오류로 실패한 경우**에는 신규 유저로 처리하지 말고, 저장을 보류한 뒤 재시도합니다.

자세한 구성 방법은 [세이브 업로드 하기](/sdk-docs/backend/base/cloud-save/upload)를 참고해 주세요.
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
