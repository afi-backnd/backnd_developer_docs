---
sidebar_label: "[Deprecated] 특정 폴더의 모든 차트 저장"
description: "[Deprecated] 특정 폴더의 모든 차트 저장"
---

# [Deprecated] GetChartByFolderAndSave
public Task< RequestResult > **GetChartByFolderAndSaveAsync**(int folderId, bool isChartKeyIsName);    

## 파라미터

| Value|  Type | Description |
| --- | --- | --- |
| folderId | int | 차트 폴더의 ID |
| isChartKeyIsName | bool | 저장할 차트의 키값으로 차트의 이름을 사용할지 여부  
**true** : 뒤끝 콘솔에서 지정한 차트의 이름을 키 값으로 사용  
**false** : 차트의 { uuid / id }를 키값으로 사용 |

## 설명
뒤끝 콘솔의 차트 관리에서 생성한 **특정 폴더에 존재하는** 모든 차트 내용을 불러와 저장합니다.  
- 입력한 파라미터에 따라 차트의 ID 혹은 뒤끝 콘솔에서 설정한 차트의 이름이 키로 적용됩니다.  
- 현재 적용된 차트 파일의 내용이 그 값으로 저장됩니다. 
- 현재 적용된 차트가 없다면 저장되지 않습니다.  


## Example

### Task 방식
```js
var reqResult = await BackndLegacy.Chart.GetChartByFolderAndSaveAsync(1024, true);
```

### Callback 방식
```js
BackndLegacy.Chart.GetChartByFolderAndSave(1024, true, callback =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases
**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : RetunrValueJson 참조

## RetunrValueJson
```js
{
    rows:
    [
        // version 1(old)
        // selectedChartFile 이 없는 경우
        {
            // 차트 uuid
            uuid: { S: "538b3a20-7b7a-11e8-8002-f31a1dd37719" },
            // 차트 indate
            inDate: { S: "2018-06-29T08:56:35.266Z" },
            // 차트 설명
            chartExplain: { S: "2" },
            // 차트명
            chartName: { S: "v1" },
            // version 정보(y: version1 , n: version2)
            old: { S: "y" }
        },
        // version 1(old)
        // selectedChartFile 이 있는 경우
        {
            // 차트에 적용한 파일 정보
            selectedChartFile:
            {
                M:
                {
                    // 차트의 row 수
                    count: { N: "1000" },
                    // 차트 파일 uuid
                    uuid: { S: "780932f0-75fb-11e8-bf7a-cbcc37090d69" },
                    // 차트 파일 indate
                    inDate: { S: "2018-06-22T09:05:54.591Z" },
                    // 차트 파일 명
                    chartFileName: { S: "222222.xlsx" }
                }
            },
            // 차트 indate
            inDate: { S: "2018-06-22T09:05:38.562Z" },
            // 차트 uuid
            uuid: { S: "6e7b5e20-75fb-11e8-bf7a-cbcc37090d69" },
            // 차트 설명
            chartExplain: { S: "v1" },
            // 차트명(이 이름으로 PlayerPrefs에 selectedChartFile에 대한 차트 내용이 저장됨)
            chartName: { S: "23" },
            // version 정보(y: version1 , n: version2)
            old: { S: "y" }
        },
        // version 2(new)
        // selectedChartFile 이 없는 경우
        {
            // 차트명
            chartName: { S: "gggg" },
            // 차트 설명
            chartExplain: { NULL: true },
            // 적용된 차트 파일 id(없는 경우)
            selectedChartFileId: { NULL: true },
            // version 정보(y: version1 , n: version2)
            old: { S: "n" }
        },
        // version 2(new)
        // selectedChartFile 이 있는 경우
        {
            // 차트명
            chartName: { S: "ㅎㅇㅎㅇ" },
            // 차트 설명
            chartExplain: { NULL: true },
            // 적용된 차트 파일 id(있는 경우)
            selectedChartFileId: { N: "47" },
            // version 정보(y: version1 , n: version2)
            old: { S: "n" }
        }
    ]
}
```
