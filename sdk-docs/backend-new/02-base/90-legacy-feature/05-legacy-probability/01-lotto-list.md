---
sidebar_label: "[Deprecated] 뽑기 리스트 조회 V2"
description: "[Deprecated] 뽑기 리스트 조회 V2"
---

# [Deprecated] GetProbabilityCardListV2
public Task< RequestResult > **GetProbabilityCardListV2Async**();

## 설명
콘솔에 등록한 확률 카드 목록을 조회합니다.  

### GetProbabilityCardList와의 차이점
GetProbabilityCardList와 기능은 거의 동일하지만 다음과 같은 차이점이 존재합니다.  
* 파일이 적용되지 않는 차트는 리스트에서 제외
* Json 리턴값중 old 컬럼 제거

:::caution GetProbabilityCardList 마이그레이션 안내
GetProbabilityCardList() 함수에서 GetProbabilityCardListV2() 함수로 마이그레이션 시 다음과 같은 사항이 있는지 확인해주세요.  

- json["rows"][0]["S"]["old"].ToString()과 같이 JSON 파싱 중 old를 사용하는가.  

- 파일 적용이 되지 않은 차트에 대한 처리가 존재하는가.  
:::

## Example

### Task 방식
```js
var reqResult = await BackndLegacy.Probability.GetProbabilityCardListV2Async();
```

### Callback 방식
```js
BackndLegacy.Probability.GetProbabilityCardListV2((callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases
**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

## ReturnValueJson
``` js
{
    rows:
    [
        // version 2(new)
        // 적용된 확률 파일이 있는 경우
        {
            // 확률명
            probabilityName: { S: "랜덤 신발 뽑기" },
            // 확률 설명
            probabilityExplain: { NULL: true },
            // 적용된 차트 파일 id(있는 경우)
            selectedProbabilityFileId: { N: "8" }
        },
        {
            probabilityName: { S: "랜덤 무기 뽑기" },
            probabilityExplain: { S: "S급 이상의 무기를 선발합니다" },
            selectedProbabilityFileId: { N: "24945" }
        }
        ...  
    ]
}
```

## Sample Code
```js
public class ProbabilityCardV2
{
    public string probabilityName; // 차트이름
    public string probabilityExplain; // 차트 설명
    public string selectedProbabilityFileId;// 차트 파일 아이디

    public override string ToString()
    {
        return $"probabilityName: {probabilityName}\n" +
        $"probabilityExplain: {probabilityExplain}\n" +
        $"selectedProbabilityFileId: {selectedProbabilityFileId}\n";
    }
}
```

```js
public async Task GetProbabilityCardListTestV2()
{
    var reqResult = await BackndLegacy.Probability.GetProbabilityCardListV2Async();
    if (!reqResult.IsSuccess())
    {
        Debug.LogError("에러가 발생했습니다 : " + reqResult.ToString());
        return;
    }

    var probabilityCardList = new List<ProbabilityCardV2>();
    var json = reqResult.GetRows();

    for (int i = 0; i < json.Count; i++)
    {
        ProbabilityCardV2 probabilityCard = new ProbabilityCardV2();
        probabilityCard.probabilityName = json[i]["probabilityName"].ToString();
        probabilityCard.probabilityExplain = json[i]["probabilityExplain"].ToString();
        probabilityCard.selectedProbabilityFileId = json[i]["selectedProbabilityFileId"].ToString();

        probabilityCardList.Add(probabilityCard);
    }

    foreach (var probabilityCard in probabilityCardList)
    {
        Debug.Log(probabilityCard.ToString() + "\n");
    }
}
```
