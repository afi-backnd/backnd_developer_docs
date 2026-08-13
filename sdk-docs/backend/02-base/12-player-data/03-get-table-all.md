---
sidebar_label: "전체 테이블 조회"
description: "GetTableList"
---

# GetTableList

public BackendReturnObject **GetTableList**();

## 설명

모든 테이블의 정보를 가져옵니다.  

## Example

### 동기

```js
Backend.PlayerData.GetTableList();
```

### 비동기

```js
Backend.PlayerData.GetTableList((callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.PlayerData.GetTableList, (callback) => {
  // 이후 처리
});
```

## ReturnCase

### Success cases

**불러오기에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

## GetReturnValuetoJSON

```js
{
    // 요청을 처리한 서버 시간
    "serverTime":"2021-02-01T05:19:56.853Z",
    "tables":
    [
        {
            "tableName":"tableName", // 테이블 이름
            "tableExplaination":"테이블 설명", // 테이블 설명
            "isChecked":true, // 활성화 여부(콘솔의 테이블 관리에서 클릭을 통해 ON/OFF 가능)
            "hasSchema":false // 스키마 유무
        },
        {
            "tableName":"tableName2", // 테이블 이름
            "tableExplaination":"테이블 설명 2", // 테이블 설명
            "isChecked":true, // 활성화 여부(콘솔의 테이블 관리에서 클릭을 통해 ON/OFF 가능)
            "hasSchema":false // 스키마 유무
        },
        ...  
    ]
}
```

## Sample Code

```js
public class TableItem
{
    public string tableName;
    public string tableExplaination;
    public bool isChecked;
    public bool hasSchema;

    public override string ToString()
    {
        return $"tableName : {tableName}\n" +
        $"tableExplaination : {tableExplaination}\n" +
        $"isChecked : {isChecked}\n" +
        $"hasSchema : {hasSchema}\n";
    }
}
```

```js
public void GetTableList()
{
    var bro = Backend.PlayerData.GetTableList();

    if(!bro.IsSuccess())
    {
        Debug.LogError(bro.ToString());
        return;
    }

    List<TableItem> tableList = new List<TableItem>();
    LitJson.JsonData tableListJson = bro.GetReturnValuetoJSON()["tables"];

    for(int i = 0; i < tableListJson.Count; i++)
    {
        TableItem tableItem = new TableItem();

        tableItem.tableName = tableListJson[i]["tableName"].ToString();
        tableItem.tableExplaination = tableListJson[i]["tableExplaination"].ToString();
        tableItem.isChecked = tableListJson[i]["isChecked"].ToString() == "true" ? true : false;
        tableItem.hasSchema = tableListJson[i]["hasSchema"].ToString() == "true" ? true : false;

        tableList.Add(tableItem);
        Debug.Log(tableItem.ToString());
    }
}
```
