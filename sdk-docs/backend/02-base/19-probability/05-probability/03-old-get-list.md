---
sidebar_label: "[Deprecated]  뽑기 리스트 조회"
draft: "true"
unlisted: "true"
description: "[Deprecated] GetProbabilityCardList"
---

# [Deprecated] GetProbabilityCardList

public BackendReturnObject **GetProbabilityCardList**();

## 설명

콘솔에 등록한 확률 카드 목록을 조회합니다.  

## Example

### 동기

```js
Backend.Probability.GetProbabilityCardList();
```

### 비동기

```js
Backend.Probability.GetProbabilityCardList((callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Probability.GetProbabilityCardList, (callback) => {
  // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

## GetReturnValuetoJSON

```js
{
    rows:
    [
        // version 1(old)
        // 적용된 확률 파일이 없는 경우
        {
            // 확률 카드 설명
            probabilityCardExplain:{ S: "게임 아이템 가차 확률 카드" },
            // 확률 카드 indate
            inDate:{ S: "2018-10-22T08:21:11.508Z" },
            // 확률 카드 uuid
            uuid:{ S: "6f30b940-d5d3-11e8-9726-450436703261" },
            // 확률 카드명
            probabilityCardName:{ S: "아이템 확률 카드" },
            // version 정보(y: version1 , n: version2)
            old:{ S: "y" }
        },
        // version 1(old)
        // 적용된 확률 파일이 있는 경우
        {
            // 확률 카드 설명
            probabilityCardExplain:{ S: "ThisIsRandom" },
            // 확률 카드 indate
            inDate:{ S: "2018-10-16T02:57:20.237Z" },
            // 확률 카드 파일 정보
            selectedProbabilityCardFile:
            {
                M:
                {
                    // 확률 카드 파일명
                    probabilityCardFileName:{ S: "random00.xlsx" },
                    // 확률 경우의 수(row의 개수)
                    count:{ N: "15" },
                    // 확률 카드 파일 indate
                    inDate:{ S: "2018-10-16T09:12:24.804Z" },
                    // 확률 카드 파일 uuid
                    uuid:{ S: "9889fa40-d123-11e8-8344-cbf89aabaf9f" }
                }
            },
            // 확률 카드 uuid
            uuid:{ S: "32c591d0-d0ef-11e8-8375-11c8fed923ed" },
            // 확률 카드명
            probabilityCardName:{ S: "ThisIsRandom" }
            // version 정보(y: version1 , n: version2)
            old:{ S: "y"}
        },
        // version 2(new)
        // 적용된 확률 파일이 없는 경우
        {
            // 확률명
            probabilityName: { S: "랜덤 무기 뽑기" },
            // 확률 설명
            probabilityExplain: { NULL: true },
            // 적용된 차트 파일 id(없는 경우)
            selectedProbabilityFileId: { NULL: true },
            // version 정보(y: version1 , n: version2)
            old:{ S: "n" }
        },
        // version 2(new)
        // 적용된 확률 파일이 있는 경우
        {
            // 확률명
            probabilityName: { S: "랜덤 신발 뽑기" },
            // 확률 설명
            probabilityExplain: { NULL: true },
            // 적용된 차트 파일 id(있는 경우)
            selectedProbabilityFileId: { N: "8" },
            // version 정보(y: version1 , n: version2)
            old:{ S: "n" }
        }
    ]
}
```

## Sample Code

```js
public class ProbabilityCard
{
    public bool isChartUpload = true; //차트가 적용되어있는지(returnValue에는 없는 값)
    public string probabilityName; // 차트이름
    public string probabilityExplain; // 차트 설명
    public int selectedProbabilityFileId;// 차트 파일 아이디
    public string old; // 신규버전인지

    public override string ToString()
    {
        return $"probabilityName: {probabilityName}\n" +
        $"probabilityExplain: {probabilityExplain}\n" +
        $"isChartUpload: {isChartUpload}\n" +
        $"selectedProbabilityFileId: {selectedProbabilityFileId}\n" +
        $"old: {old}";
    }
}
```

```js
public void GetProbabilityCardListTest()
{
    var bro = Backend.Probability.GetProbabilityCardList();

    if(!bro.IsSuccess())
    {
        Debug.LogError("에러가 발생했습니다 : " + bro.ToString());
        return;
    }

    List<ProbabilityCard> probabilityCardList = new List<ProbabilityCard>();

    LitJson.JsonData json = bro.FlattenRows();

    for(int i = 0; i < json.Count; i++)
    {
        ProbabilityCard probabilityCard = new ProbabilityCard();

        probabilityCard.probabilityName = json[i]["probabilityName"].ToString();
        probabilityCard.probabilityExplain = json[i]["probabilityExplain"].ToString();

        int outNum = 0;
        if(int.TryParse(json[i]["selectedProbabilityFileId"].ToString(), out outNum))
        {
            probabilityCard.isChartUpload = true;
            probabilityCard.selectedProbabilityFileId = outNum;
        }
        else
        {
            probabilityCard.isChartUpload = false;
            probabilityCard.selectedProbabilityFileId = 0;
        }

        probabilityCard.old = json[i]["old"].ToString();

        probabilityCardList.Add(probabilityCard);
    }

    foreach(var probabilityCard in probabilityCardList)
    {
        Debug.Log(probabilityCard.ToString() + "\n");
    }
}
```
