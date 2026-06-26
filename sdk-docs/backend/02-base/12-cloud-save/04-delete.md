---
sidebar_label: 세이브 삭제 하기
---

# Delete
public BackendReturnObject **Delete**(**string** collectionName);  
  
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
| collectionName  | string | 데이터가 위치한 컬렉션 이름 |

## 설명
저장 데이터를 클라우드 저장소에서 삭제 합니다.  

## Example
### 동기
```js
var bro = Backend.CloudSave.Delete("collectionName");
if(bro.IsSuccess())
{   
    // 요청 성공 시, 처리 코드 작성.  
}
```

### 비동기
```js
Backend.CloudSave.Delete("collectionName", bro =>
{
    if(bro.IsSuccess())
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

