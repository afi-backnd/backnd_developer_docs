---
sidebar_label: 유저 리더보드 갱신
---

# UpdateLeaderboardWithData
public Task&lt;RequestResult&gt; **UpdateLeaderboardWithDataAsync**(string **leaderboardUuid**, string **tableName**, string **rowIndate**, Param **param**);

:::danger 안내
리더보드에 등록되어있는 테이블을 Public으로 변경할 경우, 400 bad public Table에러가 발생하며 갱신되지 않습니다.  

등록된 테이블을 Public으로 변경하는 것은 삼가해주시기 바랍니다.  
:::

## 파라미터
| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| leaderboardUuid | string | 갱신할 리더보드의 uuid |
| tableName | string | 갱신할 테이블의 이름 |
| rowIndate | string | 갱신할 row의 inDate |
| param | [Param](/sdk-docs/backend/base/knowhow/param/Param) | 갱신할 값 |

leaderboardUuid 값은 아래 방법을 통해 확인할 수 있습니다.  
* uuid 값은 뒤끝 콘솔에서 리더보드를 생성 후 해당 리더보드 정보에서 uuid 값 확인
* [모든 유저 리더보드 정보 조회](/sdk-docs/backend/base/leaderboard/user/get-leaderboard) 함수를 이용하여 uuid 값 확인

## 설명
uuid의 리더보드를 갱신함과 동시에 테이블의 row에 저장된 데이터를 갱신합니다.  
* 리더보드를 갱신하기 위해서는 먼저 리더보드를 갱신하려는 유저가 해당 테이블을 1회 Insert 하여 row를 생성해야 합니다.  
* rowIndate에는 Insert한 row의 inDate값을 기입해야 합니다.  
* UpdateLeaderboardWithData 함수는 [BackndLegacy.GameData.Update](/sdk-docs/backend/base/game-information/update/using-indate) 함수에 리더보드 갱신 기능이 추가된 함수 입니다. 
* UpdateLeaderboardWithData 함수를 이용하지 않고 갱신된 데이터는 리더보드 점수, 추가 항목 관계없이 모두 리더보드에 반영되지 않습니다.  

param은 아래의 조건을 만족해야 합니다.  
* 리더보드를 생성할 때 선택한 컬럼의 값이 포함되어 있어야 합니다.  
> **리더보드를 생성할 때 선택한 컬럼의 값(점수로 사용할 값)의 범위는 아래와 같아야 합니다.**
**해당 범위를 벗어나는 값은 반올림, 반내림 되는 등 정상적으로 저장되지 않을 수 있고, SDK에서 리더보드를 조회할 때 에러가 발생할 수 있습니다.**  
정수 : -9007199254740992 ~ 9007199254740992(-2^53 ~ 2^53)   
실수 : -3.40282347E+38F ~3.40282347E+38F(float.MinValue ~ float.MaxValue)  

### 추가 항목
* 리더보드를 생성할 때 선택한 추가 항목 컬럼은 포함되어 있지 않아도 무관합니다.  
* 기타 다른 컬럼이 포함되어 있어도 무관합니다.  
* 테이블에 업데이트되는 값은 각 테이블의 스키마 정의 / 미정의 규칙에 따릅니다.  
* 추가항목은 최대 **256byte**의 데이터까지 등록할 수 있으며 257byte이상 등록시 에러가 발생합니다.  
* 해당 param에 추가항목이 포함되어있지 않더라도, 해당 rowIndate의 데이터에 추가항목이 존재한다면 추가항목이 리더보드에 등록됩니다.  

param은 아래의 조건을 만족해야 합니다.  
* 리더보드를 생성할 때 선택한 컬럼의 값이 포함되어 있어야 합니다.  
> **리더보드를 생성할 때 선택한 컬럼의 값(점수로 사용할 값)의 범위는 아래와 같아야 합니다.  
해당 범위를 벗어나는 값은 반올림, 반내림 되는 등 정상적으로 저장되지 않을 수 있고, SDK에서 리더보드를 조회할 때 에러가 발생할 수 있습니다.**   
정수 : -9007199254740992 ~ 9007199254740992(-2^53 ~ 2^53)    
실수 : -3.40282347E+38F ~3.40282347E+38F(float.MinValue ~ float.MaxValue)  
* 리더보드를 생성할 때 선택한 추가 항목 컬럼은 포함되어 있지 않아도 무관합니다.  
* 기타 다른 컬럼이 포함되어 있어도 무관합니다.  
* 테이블에 업데이트되는 값은 각 테이블의 스키마 정의 / 미정의 규칙에 따릅니다.  

## Example

### Task 방식
```js
Param param = new Param();
param.Add("score", i);
param.Add("extraData", "추가 정보");

var reqResult = await BackndLeaderboard.User.UpdateLeaderboardWithDataAsync("리더보드 uuid", "테이블 이름", "갱신할 row inDate", param);
```

### Callback 방식
```js
Param param = new Param();
param.Add("score", i);
param.Add("extraData", "추가 정보");

BackndLeaderboard.User.UpdateLeaderboardWithData("리더보드 uuid", "테이블 이름", "갱신할 row inDate", param, callback => 
{
    // 이후 처리
});
```

## ReturnCase

### Success cases
**성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**(스키마) 스키마를 선언한 컬럼의 데이터 타입과 param에 삽입된 컬럼의 데이터 타입이 일치하지 않은 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad {컬럼의 데이터 타입} dataType, 잘못된 {컬럼의 데이터 타입} dataType 입니다

**uuid가 null 혹은 string.Empty인 경우**  
StatusCode : 400  
ErrorCode : ValidationException  
Message : leaderboardUuid is null or string.Empty  

**존재하지 않는 리더보드일 경우**  
StatusCode : 404  
ErrorCode : NotFoundException  
Message : leaderboard not found, leaderboard을(를) 찾을 수 없습니다  


**리더보드 생성 시 리더보드 항목으로 등록한 테이블이 아닌 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad table, 잘못된 table 입니다

**갱신을 시도한 리더보드가 유저 리더보드가 아닌 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad table, 잘못된 table 입니다

**inDate가 null 혹은 string.Empty인 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad inDate must be, 잘못된 inDate must be 입니다

**리더보드 생성 시 리더보드 항목으로 등록한 컬럼이 param에 존재하지 않을 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad rankData column, 잘못된 rankData column 입니다

**int, long double, float 외의 값으로 업데이트를 한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad value must number, 잘못된 value must number 입니다

**(스키마) 스키마를 선언하지 않은 컬럼이 param에 존재할 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : column not found, column을(를) 찾을 수 없습니다

**존재하지 않는 inDate를 기입한 경우 / 타인의 row Indate를 기입한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : data not found, data을(를) 찾을 수 없습니다

**초기화 시간 동안(초기화 기점 1시간) 호출을 시도한 경우**  
statusCode : 428  
errorCode : Precondition Required  
message : Precondition Required ranking is being counted
> 한국 시간으로 초기화 시간이 오후 2시일 경우, 오후 2시에서 오후 3시동안은 에러가 발생합니다.

**기간이 끝난 일회성 리더보드의 갱신을 시도한 경우**  
statusCode : 428  
errorCode : Precondition Required  
message : Precondition Required ranking is being counted

## Sample Code
```js
public async Task UpdateLeaderboardTest()
{
    string tableName = "score";
    string rowIndate = string.Empty;
    string leaderboardUuid = "fe8ffa20-1db6-11ec-83a0-77f4e266a1f3";

    Param param = new Param();
    param.Add("score", 10);

    var getResult = await BackndUserData.Instance.GetDataAsync("score");
    if (getResult.IsSuccess() == false)
    {
        Debug.LogError(getResult);
        return;
    }

    if (getResult.GetRows().Count > 0)
    {
        rowIndate = getResult.GetRows()[0]["inDate"].ToString();
    }
    else
    {
        var createResult = await BackndUserData.Instance.CreateDataAsync(tableName, param);
        if (createResult.IsSuccess())
        {
            rowIndate = createResult.GetInDate();
        }
        else
        {
            Debug.LogError(createResult);
            return;
        }
    }

    if (rowIndate == string.Empty)
    {
        return;
    }

    var rankResult = await BackndLeaderboard.User.UpdateLeaderboardWithDataAsync(leaderboardUuid, tableName, rowIndate, param);
    if (rankResult.IsSuccess())
    {
        Debug.Log("리더보드 등록 성공");
    }
    else
    {
        Debug.Log("리더보드 등록 실패 : " + rankResult);
    }
}
```
