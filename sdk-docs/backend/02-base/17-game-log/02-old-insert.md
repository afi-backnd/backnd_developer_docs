---
sidebar_label: "[Deprecated] 게임 로그 저장"
draft: true
unlisted: true
---

# InsertLog
public BackendReturnObject **InsertLog**(string **logType**, Param **param**);  
public BackendReturnObject **InsertLog**(string **logType**, Param **param**, int **graceDays**);  

:::info 개선된 신규 게임로그 저장 기능(V2)을 이용해 주세요.
기존의 InsertLog는 로그 저장 시 호출 비용과 함께 DB 쓰기 처리량이 발생하였으나,  
개선된 InsertLogV2는 호출 비용만 발생하며, DB 쓰기 처리량이 발생하지 않아 DB 쓰기 요금에 대한 부담 없이 로그를 저장할 수 있습니다.  
:::

## 파라미터

| Value | Type | Description | 
| --- | --- | --- | 
| logType | string | 로그를 구분하기 위한 type | 
| param | string | 로그에 기록하고자 하는 내용을 담은 Param | 
| graceDays | int | 삭제 예정 날짜(graceDays가 10일 경우, 10일 뒤 자동 삭제) | 

## 설명

게임 로그를 저장합니다.  
생성된 로그는 뒤끝 콘솔 및 게임 클라이언트에서 확인 가능합니다.  

### graceDays
graceDays는 유예 기간입니다.  
graceDays를 10으로 입력할 경우, 삽입 시간으로부터 10일후에 로그가 자동으로 삭제됩니다.  
graceDays를 0 이하로 입력하거나 입력하지 않을 경우 90(3개월)으로 지정됩니다.  


## Example

### 동기
```js
Param param = new Param();
param.Add("n_n", "tableName");
Backend.GameLog.InsertLog("logType", param);

//로그 보관 기간을 최대 10일로 지정하고 싶을 경우
Backend.GameLog.InsertLog("logType", param, 10);

```

### 비동기
```js
Param param = new Param();
param.Add("n_n", "tableName");
Backend.GameLog.InsertLog("logType", param, (callback) => 
{
    // 이후 처리
});

//로그 보관 기간을 최대 10일로 지정하고 싶을 경우
Backend.GameLog.InsertLog("logType", param, 10, (callback) => 
{
    // 이후 처리
});

```

### SendQueue
```js
Param param = new Param();
param.Add("n_n", "tableName");
SendQueue.Enqueue(Backend.GameLog.InsertLog, "logType", param, (callback) => 
{
    // 이후 처리
});

//로그 보관 기간을 최대 10일로 지정하고 싶을 경우
SendQueue.Enqueue(Backend.GameLog.InsertLog, "logType", param, 10, (callback) => 
{
    // 이후 처리
});
```

## ReturnCase

### Success cases
**로그 생성에 성공한 경우**  
statusCode : 204  
message : Success  

## Sample Code
```js
public void InsertLogTest()
{
    long money = 12345678;
    int level = 100;

    Dictionary<string, int> items = new Dictionary<string, int> { { "hpPotion", 12 }, { "mpPotion", 20 }, { "cook", 1 }, { "bomb", 20 } };
    List<string> equip = new List<string>() { "hat12", "hat10", "shoes1", "costume20" };

    Param param = new Param();
    param.Add("money", money);
    param.Add("level", level);

    Param param2 = new Param();
    param2.Add("items", items);
    param2.Add("equip", equip);

    var bro1 = Backend.GameData.Update("stats", new Where(), param);
    var bro2 = Backend.GameData.Update("items", new Where(), param2);

    Param logParam = new Param();

    if(!bro1.IsSuccess())
    {
        logParam.Add("statsUpdateError", bro1.ToString());
    }
    if(!bro2.IsSuccess())
    {
        logParam.Add("itemsUpdateError", bro2.ToString());
    }

    logParam.Add("statsParam", param);
    logParam.Add("itemsParam", param2);

    Backend.GameLog.InsertLog("updateLog", logParam);
}
```
