---
sidebar_label: 게임 로그 저장
---

# SendLog
public Task&lt;RequestResult&gt; **SendLogAsync**(string **logType**, Param **param**);  

:::info 게임로그 저장 기능은 DB 쓰기 처리량이 발생하지 않습니다.  
SendLog는 호출 비용만 발생하며, DB 쓰기 처리량이 발생하지 않아 DB 쓰기 요금에 대한 부담 없이 로그를 저장할 수 있습니다.  
:::

## 파라미터

| Value |  Type | Description |
| --- |  --- | --- | 
| logType | string | 로그를 구분하기 위한 type (행동 유형) | 
| param | string | 로그에 기록하고자 하는 내용을 담은 Param | 

## 설명
- 게임 로그를 저장합니다.  
- 생성된 로그는 뒤끝 콘솔에서 확인 가능합니다.  
- 콘솔에 등록되기까지 최대 5분이 걸릴 수 있습니다.  
- logType 값에는 -(하이픈), _(언더바), (띄어쓰기)를 제외한 특수문자의 사용이 불가능합니다.  
- 콘솔에서 스키마 정의를 통해 행동 유형의 보관 기간 및 저장할 필드(Param의 key값)를 설정/관리 할 수 있습니다.
- 콘솔에서 스키마 정의를 통해 설정되지 않은 필드는 unknown_fields에 저장됩니다.
- Param으로 등록되는 데이터의 최대 수는 100개로 제한됩니다.  
- Param의 key값은 숫자 혹은 _(언더바)로 시작할 수 없습니다.  
- Param의 key값에는 -(하이픈), _(언더바), (띄어쓰기)를 제외한 특수문자의 사용이 불가능합니다.  
- Param의 key값에는 nickname, gamer_id, indate, unknown_fields를 대소문자 상관없이 사용할 수 없습니다.  
- 게임 로그로 저장 가능한 데이터의 최대 크기는 1MB입니다.
## Example

### Task 방식
```js
Param param = new Param();
param.Add("n_n", "tableName");
var reqResult = await BackndLog.Instance.SendLogAsync("logType", param);
```

### Callback 방식
```js
Param param = new Param();
param.Add("n_n", "tableName");
BackndLog.Instance.SendLog("logType", param, (callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**로그 생성에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**param에 대소문자 상관없이 사용할 수 없는 key값이 존재할 경우**  
statusCode : 405  
errorCode : MethodNotAllowedParameterException  
message : MethodNotAllowed {key}, 이용할 수 없는 {key}입니다

**param에 _ 혹은 숫자로 시작되는 key값이 존재할 경우**  
statusCode : 405  
errorCode : MethodNotAllowedParameterException  
message : MethodNotAllowed {key}, 이용할 수 없는 {key}입니다

**param에 대소문자 상관없이 중복되는 key값이 존재할 경우**  
statusCode : 405  
errorCode : MethodNotAllowedParameterException  
message : MethodNotAllowed {중복된key대문자}, 이용할 수 없는 {중복된key대문자}입니다

**param에 기타 조건에 맞지 않는 key값이 존재할 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad reserved word, 잘못된 reserved word 입니다

**param에 Add된 데이터가 100개 초과일 경우**  
statusCode : 428  
errorCode : Precondition Required  
message : Precondition Required Allowed the maximum length of key is 100

**1MB 이상 데이터를 저장하려 할 경우**  
statusCode : 428  
errorCode : Precondition Required  
message : Precondition Required Allowed the maximum body size is 1MB  

## Sample Code
```js
public async Task SendLogTest()
{
    long money = 12345678;
    int level = 100;
    double hp = 123;
    Dictionary<string, int> items = new Dictionary<string, int> { { "hpPotion", 12 }, { "mpPotion", 20 }, { "cook", 1 }, { "bomb", 20 } };
    List<string> equip = new List<string>() { "hat12", "hat10", "shoes1", "costume20" };

    Param param = new Param();
    param.Add("money", money);
    param.Add("level", level);
    param.Add("hp", hp);

    Param param2 = new Param();
    param2.Add("items", items);
    param2.Add("equip", equip);

    var updateResult1 = await BackndUserData.Instance.UpdateLatestDataAsync("stats", param);
    var updateResult2 = await BackndUserData.Instance.UpdateLatestDataAsync("items", param2);

    // 에러 수집용 log
    Param logParam = new Param();

    if (!updateResult1.IsSuccess())
    {
        logParam.Add("statsUpdateError", updateResult1.ToString());
    }
    if (!updateResult2.IsSuccess())
    {
        logParam.Add("itemsUpdateError", updateResult2.ToString());
    }

    logParam.Add("statsParam", param);
    logParam.Add("itemsParam", param2);

    BackndLog.Instance.SendLog("updateLog", logParam, (reqResult) =>
    {
        Debug.Log(reqResult.ToString());
    });
}
```
