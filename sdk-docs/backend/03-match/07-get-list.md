---
sidebar_label: "매칭 리스트 조회"
description: "GetMatchList"
---

# GetMatchList

public BackendReturnObject **GetMatchList**();

## 설명

뒤끝콘솔에서 생성한 매칭 카드들의 정보를 조회합니다.  
매치 inDate, 매치 이름, 타입뿐만 아니라 설정한 모든 시간 값들이 리턴됩니다.  

## Example

### 동기

```js
Backend.Match.GetMatchList();
```

### 비동기

```js
Backend.Match.GetMatchList((callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Match.GetMatchList, (callback) => {
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
        {
          inDate: {"S" : "2020-08-04T09:24:14.807Z"},
          matchTitle : {"S", "매치타이틀"},
          enable_sandbox : {"BOOL" : false}
          matchType : {"S" : "mmr"},
          matchModeType : {"S" : "OneOnOne"},
          matchHeadCount : {"N" : "2"},
          enable_battle_royale : {"BOOL" : false},
          match_timeout_m : {"N" : "60"},
          transit_to_sandbox_timeout_ms : {"N" : "10000"},
          match_start_waiting_time_s : {"N" : "15"}
          match_increment_time_s : {"N" : "10"},
          maxMatchRange : {"N" : "5000"},
          increaseAndDecrease : {"N" : "1000"},
          initializeCycle : {"S" : "week"},
          defaultPoint : {"N" : "1000"},
          savingPoint :
            // 1:1, 팀전 점수 증감 값 예
            {"M":{"defeat":{"N":"-5"},"victory":{"N":"5"},"draw":{"N":"0"}}}
            // 개인전 점수 증감 값 예
            {"L":[{"M":{"1":{"S":"1"}}},{"M":{"2":{"S":"1"}}},{"M":{"3":{"S":"1"}}},{"M":{"4":{"S":"1"}}}]}
        },
        {
          inDate: [object], // 매칭 카드의 키값
          matchTitle : [object], // 콘솔에서 설정한 매치 이름
          enable_sandbox : [object], // 샌드박스 모드 활성화 여부
          matchType : [object],  // 매치 타입
          matchModeType : [object], // 매치 모드 타입
          matchHeadCount : [object], // 인원수
          enable_battle_royale : [object], //(optional. 개인전) 배틀로얄 활성화 여부
          match_timeout_m : [object], // 게임방 최대 유지 시간
          transit_to_sandbox_timeout_ms : [object], // 샌드박스 모드가 활성화된 경우 샌드박스 모드로 전환되는 시간
          match_start_waiting_time_s : [object], // 모든 유저가 게임방에 입장한 후 게임 시작 메시지가 리턴되기까지 걸리는 시간
          match_increment_time_s : [object], //(optional. 포인트, MMR) 매칭되지 않았을 때 매칭 범위를 늘리는 기준 시간
          maxMatchRange : [object], //(optional. 포인트, MMR) 최대 매칭 범위
          increaseAndDecrease : [object],  //(optional. 포인트, MMR) 최대 매칭 범위 증가 폭
          initializeCycle : [object], // 전적/점수 초기화 주기
          defaultPoint : [object], //(optional. 포인트) 최초 시작 포인트
          savingPoint : [object] //(optilnal. 포인트, MMR) 승/패/등수 달성 시 증감 포인트 설정값
        }
    ],
}
```

## Sample Code

```js
public class MatchCard
{
    public string inDate;
    public string matchTitle;
    public bool enable_sandbox;
    public string matchType;
    public string matchModeType;
    public int matchHeadCount;
    public bool enable_battle_royale;
    public int match_timeout_m;
    public int transit_to_sandbox_timeout_ms;
    public int match_start_waiting_time_s;
    public int match_increment_time_s;
    public int maxMatchRange;
    public int increaseAndDecrease;
    public string initializeCycle;
    public int defaultPoint;
    public int version;
    public string result_processing_type;
    public Dictionary<string, int> savingPoint = new Dictionary<string, int>(); // 팀전/개인전에 따라 키값이 달라질 수 있음.  
    public override string ToString()
    {
        string savingPointString = "savingPont : \n";
        foreach(var dic in savingPoint)
        {
            savingPointString += $"{dic.Key} : {dic.Value}\n";
        }
        savingPointString += "\n";
        return $"inDate : {inDate}\n" +
        $"matchTitle : {matchTitle}\n" +
        $"enable_sandbox : {enable_sandbox}\n" +
        $"matchType : {matchType}\n" +
        $"matchModeType : {matchModeType}\n" +
        $"matchHeadCount : {matchHeadCount}\n" +
        $"enable_battle_royale : {enable_battle_royale}\n" +
        $"match_timeout_m : {match_timeout_m}\n" +
        $"transit_to_sandbox_timeout_ms : {transit_to_sandbox_timeout_ms}\n" +
        $"match_start_waiting_time_s : {match_start_waiting_time_s}\n" +
        $"match_increment_time_s : {match_increment_time_s}\n" +
        $"maxMatchRange : {maxMatchRange}\n" +
        $"increaseAndDecrease : {increaseAndDecrease}\n" +
        $"initializeCycle : {initializeCycle}\n" +
        $"defaultPoint : {defaultPoint}\n" +
        $"version : {version}\n" +
        $"result_processing_type : {result_processing_type}\n" +
        savingPointString;
    }
}
```

```js
public void GetMatchList()
{
    var callback = Backend.Match.GetMatchList();

    if(!callback.IsSuccess())
    {
        Debug.LogError("Backend.Match.GetMatchList Error : " + callback);
        return;
    }

    List<MatchCard> matchCardList = new List<MatchCard>();

    LitJson.JsonData matchCardListJson = callback.FlattenRows();

    Debug.Log("Backend.Match.GetMatchList : " + callback);

    for(int i = 0; i < matchCardListJson.Count; i++)
    {
        MatchCard matchCard = new MatchCard();

        matchCard.inDate = matchCardListJson[i]["inDate"].ToString();
        matchCard.result_processing_type = matchCardListJson[i]["result_processing_type"].ToString();
        matchCard.version = int.Parse(matchCardListJson[i]["version"].ToString());
        matchCard.matchTitle = matchCardListJson[i]["matchTitle"].ToString();
        matchCard.enable_sandbox = matchCardListJson[i]["enable_sandbox"].ToString() == "true" ? true : false;
        matchCard.matchType = matchCardListJson[i]["matchType"].ToString();
        matchCard.matchModeType = matchCardListJson[i]["matchModeType"].ToString();
        matchCard.matchHeadCount = int.Parse(matchCardListJson[i]["matchHeadCount"].ToString());
        matchCard.enable_battle_royale = matchCardListJson[i]["enable_battle_royale"].ToString() == "true" ? true : false;
        matchCard.match_timeout_m = int.Parse(matchCardListJson[i]["match_timeout_m"].ToString());
        matchCard.transit_to_sandbox_timeout_ms = int.Parse(matchCardListJson[i]["transit_to_sandbox_timeout_ms"].ToString());
        matchCard.match_start_waiting_time_s = int.Parse(matchCardListJson[i]["match_start_waiting_time_s"].ToString());

        if(matchCardListJson[i].ContainsKey("match_increment_time_s"))
        {
            matchCard.match_increment_time_s = int.Parse(matchCardListJson[i]["match_increment_time_s"].ToString());
        }
        if(matchCardListJson[i].ContainsKey("maxMatchRange"))
        {
            matchCard.maxMatchRange = int.Parse(matchCardListJson[i]["maxMatchRange"].ToString());
        }
        if(matchCardListJson[i].ContainsKey("increaseAndDecrease"))
        {
            matchCard.increaseAndDecrease = int.Parse(matchCardListJson[i]["increaseAndDecrease"].ToString());
        }
        if(matchCardListJson[i].ContainsKey("initializeCycle"))
        {
            matchCard.initializeCycle = matchCardListJson[i]["initializeCycle"].ToString();
        }
        if(matchCardListJson[i].ContainsKey("defaultPoint"))
        {
            matchCard.defaultPoint = int.Parse(matchCardListJson[i]["defaultPoint"].ToString());
        }

        if(matchCardListJson[i].ContainsKey("savingPoint"))
        {
            if(matchCardListJson[i]["savingPoint"].IsArray)
            {
                for(int listNum = 0; listNum < matchCardListJson[i]["savingPoint"].Count; listNum++)
                {
                    var keyList = matchCardListJson[i]["savingPoint"][listNum].Keys;
                    foreach(var key in keyList)
                    {
                        matchCard.savingPoint.Add(key, int.Parse(matchCardListJson[i]["savingPoint"][listNum][key].ToString()));
                    }
                }
            }
            else
            {
                foreach(var key in matchCardListJson[i]["savingPoint"].Keys)
                {
                    matchCard.savingPoint.Add(key, int.Parse(matchCardListJson[i]["savingPoint"][key].ToString()));
                }
            }
        }
        matchCardList.Add(matchCard);
    }

    foreach(var matchCard in matchCardList)
    {
        Debug.Log(matchCard.ToString());
    }
}
```
