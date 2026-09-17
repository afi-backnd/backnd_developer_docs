---
sidebar_label: "세이브 업로드 하기"
description: "Upload"
---

# Upload
public BackendReturnObject **Upload**(**string** collectionName, **string** jsonString);  
public BackendReturnObject **Upload**(**string** collectionName, **Param** param);  
  
:::tip 클라우드 세이브 안전하게 사용하기
클라우드 세이브는 **저비용 데이터 저장소**로, 일반적인 DB와는 동작 방식이 다릅니다.  
데이터 보호를 위한 백업과 요청 로그를 제공하지 않고 기존 데이터와 비교 없이 항상 덮어씁니다.  
따라서 **중요한 데이터를 저장할 때는 아래 방식으로** 구성해 주세요. 데이터 유실 위험을 낮추면서 비용 효율은 그대로 유지할 수 있습니다.

1. **메인 컬렉션과 백업 컬렉션을 분리하세요**  
   평소 저장/로드에 사용하는 메인 컬렉션과 별개로, 백업 전용 컬렉션을 하나 더 생성합니다.  
   컬렉션은 최대 20개까지 생성할 수 있으므로, 백업 컬렉션 1개를 미리 확보해 두시는 것을 권장합니다.

2. **주요 시점에만 백업 컬렉션에 기록하세요**  
   매 저장마다 백업할 필요는 없습니다. 스테이지 클리어, 결제 완료, 재화 대량 획득/소모 등  
   **되돌아갈 기준이 되는 시점**에만 백업 컬렉션에 함께 저장하세요.  
   백업 호출 횟수를 줄일 수 있어, 추가 비용을 최소화하면서 복구 지점을 확보할 수 있습니다.

3. **빈 데이터·초기 상태 데이터는 백업 컬렉션에 저장하지 마세요**  
   클라이언트에서 업로드 직전에 데이터를 검사하여, 빈 데이터(`{}`)이거나 신규 유저 기본값(초기화 상태)인 경우에는  
   **백업 컬렉션 업로드를 건너뛰도록** 처리하세요.  
   이 처리가 없으면 정상 백업이 초기 데이터로 덮어쓰여 복구 지점을 잃게 됩니다.

4. **로드에 실패하면 원인을 구분한 뒤 처리하세요**  
   메인 컬렉션 다운로드가 실패했을 때 곧바로 신규 유저로 판단하면,  
   직후 저장에서 기존 데이터가 초기값으로 덮어쓰일 수 있습니다. 아래와 같이 구분해 주세요.

   - **네트워크 오류·서버 오류로 실패한 경우**: 신규 유저로 처리하지 마세요. 저장을 보류하고 재시도해 주세요.
   - **정상 응답이지만 데이터가 없는 경우**: 백업 컬렉션을 먼저 조회하고, 백업에도 데이터가 없을 때만 신규 유저로 처리해 주세요.
:::

:::info 꼭 알아두세요
- 자동 백업과 데이터 버전 관리가 제공되지 않으므로, 복구가 필요한 데이터는 위 방식으로 백업 컬렉션을 구성해 주세요.
- 서버 요청에 대한 상세 로그가 저장되지 않습니다. 에러 상황 추적이 필요한 경우 클라이언트 측에서 로그를 관리해 주세요.
- 실시간 동기화, 데이터 이력 추적, 서버 측 검증이 필요한 정보는 [데이터베이스](/guide/console-guide/database/introduction/) 또는 [게임 정보](/guide/console-guide/backnd-base/game-information/table) 이용을 권장합니다.
:::  
  
## 파라미터
| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| collectionName  | string | 데이터가 업로드 될 컬렉션 이름 |
| jsonString      | string | JSON 문자열 형식의 저장 데이터 |
| param | [Param](/sdk-docs/backend/base/knowhow/param/param) | Param 형식의 저장 데이터 |

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

#### Case 3 - 백업 컬렉션에 함께 저장하기
```js
// 되돌아갈 기준이 되는 시점(스테이지 클리어, 결제 완료 등)에 호출
void SaveWithBackup(Param param)
{
    // 메인 컬렉션은 항상 저장
    var bro = Backend.CloudSave.Upload("mainCollection", param);
    if(bro.IsSuccess() == false)
    {
        return;
    }

    // 빈 데이터·초기 상태는 백업하지 않음 (복구 지점 보호)
    if(IsEmptyOrInitialData(param))
    {
        return;
    }

    var backupBro = Backend.CloudSave.Upload("backupCollection", param);
    if(backupBro.IsSuccess())
    {
        // 백업 성공 시, 처리 코드 작성.
    }
}

// 게임의 데이터 구조에 맞게 직접 구현해야 하는 함수입니다.
// 빈 데이터({})는 메인 업로드 단계에서 SDK가 이미 차단하므로, 여기서는 "초기 상태" 판정만 합니다.
bool IsEmptyOrInitialData(Param param)
{
    var value = param.GetValue();

    // 판정 예시: 진행도 키가 없거나 0이면 신규 유저 기본값으로 간주
    if(value.ContainsKey("stage_clear") == false)
    {
        return true;
    }

    return Convert.ToInt32(value["stage_clear"]) <= 0;
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
