---
sidebar_label: "유저 랭킹 갱신"
draft: "true"
unlisted: "true"
description: "UpdateUserScore"
---

# UpdateUserScore
public BackendReturnObject **UpdateUserScore**(string **rankUuid**, string **tableName**, string **rowIndate**, Param **param**);

:::info 기능 개선 안내
기존 랭킹의 성능과 기능을 개선한 **리더보드** 기능을 제공하고 있습니다.  
**콘솔에서 신규 생성은 리더보드만 제공**하므로 해당 기능을 사용해 주세요.
:::

:::warning 그룹 구분된 리더보드 사용 불가
URank 랭킹 함수는 그룹이 구분된 리더보드에서 NULL그룹만 등록할 수 있습니다.  
그룹에 따른 리더보드를 등록하고자 할 경우에는 Leaderboard함수를 이용해주세요. 
:::

:::danger 안내
랭킹에 등록되어있는 테이블을 Public으로 변경할 경우, 400 bad public Table에러가 발생하며 갱신되지 않습니다.  

등록된 테이블을 Public으로 변경하는 것은 삼가해주시기 바랍니다.  
:::

## 파라미터
| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| rankUuid | string | 갱신할 랭킹의 uuid |
| tableName | string | 갱신할 테이블의 이름 |
| rowIndate | string | 갱신할 row의 inDate |
| param | [Param](/sdk-docs/backend/base/knowhow/param/Param) | 갱신할 값 |

rankUuid 값은 아래 방법을 통해 확인할 수 있습니다.  
* uuid 값은 뒤끝 콘솔에서 랭킹을 생성 후 해당 랭킹 정보에서 uuid 값 확인
* [모든 유저 랭킹 정보 조회](/sdk-docs/backend/base/rank/user/get-all-sertting) 함수를 이용하여 uuid 값 확인

## 설명
uuid의 랭킹을 갱신함과 동시에 테이블의 row에 저장된 데이터를 갱신합니다.  
* 랭킹을 갱신하기 위해서는 먼저 랭킹을 갱신하려는 유저가 해당 테이블을 1회 Insert 하여 row를 생성해야 합니다.  
* rowIndate에는 Insert한 row의 inDate값을 기입해야 합니다.  
* UpdateUserScore 함수는 [Backend.GameData.Update](/sdk-docs/backend/base/game-information/update/using-indate) 함수에 랭킹 갱신 기능이 추가된 함수 입니다. 
* UpdateUserScore 함수를 이용하지 않고 갱신된 데이터는 랭킹 점수, 추가 항목 관계없이 모두 랭킹에 반영되지 않습니다.  

param은 아래의 조건을 만족해야 합니다.  
* 랭킹을 생성할 때 선택한 컬럼의 값이 포함되어 있어야 합니다.  
> **랭킹을 생성할 때 선택한 컬럼의 값(점수로 사용할 값)의 범위는 아래와 같아야 합니다.**
**해당 범위를 벗어나는 값은 반올림, 반내림 되는 등 정상적으로 저장되지 않을 수 있고, SDK에서 랭킹을 조회할 때 에러가 발생할 수 있습니다.**  
정수 : -9007199254740992 ~ 9007199254740992(-2^53 ~ 2^53)   
실수 : -3.40282347E+38F ~3.40282347E+38F(float.MinValue ~ float.MaxValue)  

### 추가 항목
* 랭킹을 생성할 때 선택한 추가 항목 컬럼은 포함되어 있지 않아도 무관합니다.  
* 기타 다른 컬럼이 포함되어 있어도 무관합니다.  
* 테이블에 업데이트되는 값은 각 테이블의 스키마 정의 / 미정의 규칙에 따릅니다.  
* 추가항목은 최대 **256byte**의 데이터까지 등록할 수 있으며 257byte이상 등록시 에러가 발생합니다.  
* 해당 param에 추가항목이 포함되어있지 않더라도, 해당 rowIndate의 데이터에 추가항목이 존재한다면 추가항목이 랭킹에 등록됩니다.  

param은 아래의 조건을 만족해야 합니다.  
* 랭킹을 생성할 때 선택한 컬럼의 값이 포함되어 있어야 합니다.  
> **랭킹을 생성할 때 선택한 컬럼의 값(점수로 사용할 값)의 범위는 아래와 같아야 합니다.  
해당 범위를 벗어나는 값은 반올림, 반내림 되는 등 정상적으로 저장되지 않을 수 있고, SDK에서 랭킹을 조회할 때 에러가 발생할 수 있습니다.**   
정수 : -9007199254740992 ~ 9007199254740992(-2^53 ~ 2^53)    
실수 : -3.40282347E+38F ~3.40282347E+38F(float.MinValue ~ float.MaxValue)  
* 랭킹을 생성할 때 선택한 추가 항목 컬럼은 포함되어 있지 않아도 무관합니다.  
* 기타 다른 컬럼이 포함되어 있어도 무관합니다.  
* 테이블에 업데이트되는 값은 각 테이블의 스키마 정의 / 미정의 규칙에 따릅니다.  

## Example

### 동기
```js
Param param = new Param();
param.Add("score", i);
param.Add("extraData", "추가 정보");

Backend.URank.User.UpdateUserScore("랭킹 uuid", "테이블 이름", "갱신할 row inDate", param);
```

### 비동기
```js
Param param = new Param();
param.Add("score", i);
param.Add("extraData", "추가 정보");

Backend.URank.User.UpdateUserScore("랭킹 uuid", "테이블 이름", "갱신할 row inDate", param, callback => 
{
    // 이후 처리
});
```

### SendQueue
```js
Param param = new Param();
param.Add("score", i);
param.Add("extraData", "추가 정보");

SendQueue.Enqueue(Backend.URank.User.UpdateUserScore, "랭킹 uuid", "테이블 이름", "갱신할 row inDate", param, callback => 
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
statusCode : 400  
errorCode : ValidationException  
message : rankUuid is null or empty

**랭킹 생성 시 랭킹 항목으로 등록한 테이블이 아닌 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad table, 잘못된 table 입니다

**갱신을 시도한 랭킹이 유저 랭킹이 아닌 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad table, 잘못된 table 입니다

**inDate가 null 혹은 string.Empty인 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad inDate must be, 잘못된 inDate must be 입니다

**랭킹 생성 시 랭킹 항목으로 등록한 컬럼이 param에 존재하지 않을 경우**  
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

**UTC+9 04:00 ~ 05:00 사이에 랭킹 갱신을 시도한 경우**  
statusCode : 428  
errorCode : Precondition Required  
message : Precondition Required ranking is being counted
> 한국 시간으로 새벽 4시 ~ 5시 사이 랭킹 갱신을 시도한 경우

**기간이 끝난 일회성 랭킹의 갱신을 시도한 경우**  
statusCode : 428  
errorCode : Precondition Required  
message : Precondition Required ranking is being counted

## Sample Code
```js
public void GetUpdateUserRankTest()
{
    string tableName = "score";
    string rowIndate = string.Empty;
    string rankingUUID = "fe8ffa20-1db6-11ec-83a0-77f4e266a1f3";

    Param param = new Param();
    param.Add("score", 10);

    var bro = Backend.GameData.Get("score", new Where());

    if(bro.IsSuccess())
    {
        if(bro.FlattenRows().Count > 0)
        {
            rowIndate = bro.FlattenRows()[0]["inDate"].ToString();
        }
        else
        {
            var bro2 = Backend.GameData.Insert(tableName, param);

            if(bro2.IsSuccess())
            {
                rowIndate = bro2.GetInDate();
            }
            else
            {
                return;
            }

        }
    }
    else
    {
        return;
    }

    if(rowIndate == string.Empty)
    {
        return;
    }

    var rankBro = Backend.URank.User.UpdateUserScore(rankingUUID, tableName, rowIndate, param);
    if(rankBro.IsSuccess())
    {
        Debug.Log("랭킹 등록 성공");
    }
    else
    {
        Debug.Log("랭킹 등록 실패 : " + rankBro);
    }
}
```
