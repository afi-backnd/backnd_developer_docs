---
sidebar_label: 전체 테이블 조회
---

# GetTables

public Task&lt;GetTablesResult&gt; **GetTablesAsync**();

## 설명

모든 테이블의 정보를 가져옵니다.  

## Example

### Task 형식
```js
var reqResult = await BackndUserData.Instance.GetTablesAsync();
```

### Callback 형식
```js
BackndUserData.Instance.GetTables((callback) =>
{
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
public async Task GetTableList()
{
    var reqResult = await BackndUserData.Instance.GetTablesAsync();
    if (!reqResult.IsSuccess())
    {
        Debug.LogError(reqResult.ToString());
        return;
    }

    var tableList = new List<TableItem>();
    var infoList = reqResult.GetInfoList();
    for (int i = 0; i < infoList.Count; i++)
    {
        var info = infoList[i];

        TableItem tableItem = new TableItem();
        tableItem.tableName = info.TableName;
        tableItem.tableExplaination = info.TableExplaination;
        tableItem.isChecked = info.IsChecked;
        tableItem.hasSchema = info.HasSchema;

        tableList.Add(tableItem);
        Debug.Log(tableItem.ToString());
    }
}
```
