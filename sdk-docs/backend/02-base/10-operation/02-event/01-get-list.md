---
sidebar_label: 이벤트 리스트 조회
sidebar_position: 1
---

# EventListV2

public BackendReturnObject **EventListV2**();  
public BackendReturnObject **EventListV2**(int **limit**);  
public BackendReturnObject **EventListV2**(int **limit**, string **offset**);

## 파라미터

| Value  | Type   | Description                                                                                                    |
| ------ | ------ | -------------------------------------------------------------------------------------------------------------- |
| limit  | int    | (Optional) 불러올 이벤트의 수                                                                                  |
| offset | string | (Optional) 불러올 이벤트의 시작점  (BackendReturnObject.LastEvaluatedKeyString()를 통해 얻으실 수 있습니다.) |

## 설명

뒤끝 콘솔에 등록한 이벤트를 받아오는 기능을 제공합니다.  

- 유저의 언어정보가 존재하고, 콘솔에 해당 언어 이벤트가 등록되어 있는 경우 → 해당 언어의 이벤트 제목과 내용을 리턴합니다.
- 유저의 언어정보가 존재하지 않거나, 콘솔에 해당 언어 이벤트가 등록되어 있지 않은 경우 → 기본 설정의 제목과 내용을 리턴합니다.

## Example

### 동기

```csharp
//10개의 이벤트 리스트 불러오기
BackendReturnObject bro = Backend.Event.EventListV2(10);

if(bro.IsSuccess()) {
    //10개를 불러온 이후에 이벤트 10
    string offset = bro.LastEvaluatedKeyString();
    if(!string.IsNullOrEmpty(offset)) {
        // 2. 불러온 이벤트 2개 이후의 3개 불러오기
        Backend.Event.EventListV2(3, offset);
    }
}
```

### 비동기

```csharp
Backend.Event.EventListV2(2, (callback) => {
    if(callback.IsSuccess()) {
        string offset = callback.LastEvaluatedKeyString();
        if(!string.IsNullOrEmpty(offset)) {
            // 2. 불러온 이벤트 2개 이후의 3개 불러오기
            Backend.Event.EventListV2(3, offset, (callback2) => {
                // 이후 처리
            });
        }
    }
});
```

### SendQueue

```csharp
SendQueue.Enqueue(Backend.Event.EventListV2, 2, (callback) => {
    if(callback.IsSuccess()) {
        string offset = callback.LastEvaluatedKeyString();
        if(!string.IsNullOrEmpty(offset)) {
            // 2. 불러온 이벤트 2개 이후의 3개 불러오기
            SendQueue.Enqueue(Backend.Event.EventListV2, 3, offset, (callback2) => {
                // 이후 처리
            });
        }
    }
});
```

## ReturnCase

### Success cases

**불러오기에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

**이벤트가 0개일 경우**  
statusCode : 200  
message : Success  
returnValue : { "rows":[] }

## GetReturnValuetoJSON

성공 시, 등록된 이벤트들의 정보를 나타냅니다.

```json
{
    // 불러온 데이터 이후의 데이터가 존재하는 경우 리턴됨
    // 불러온 데이터 이후의 데이터가 존재하지 않는 경우 리턴되지 않음
    "LastEvaluatedKey":{
        "gamer_id": { "S": "_event" },
        "inDate":{ "S": "2019-08-19T06:45:07.816Z" },
    },
    "rows": [
        {
            "uuid": "7598c8e2-03ed-46ff-bbba-0be5eb742649", // 이벤트 uuid
            "inDate": "2026-02-03T01:49:19.613Z",           // 이벤트 inDate
            "postingDate": "2026-02-03",                    // 이벤트 게시 일자
            "startDate": "2026-02-03T02:00:34.828Z",        // 이벤트 시작일
            "endDate": "2026-02-23T02:00:34.828Z",          // 이벤트 종료일
            "isPublic": "y",                                // 공개/비공개 여부(y: 공개)
            "eventTitle": "이벤트 3",                         // 이벤트 제목
            "author": "운영자",                               // 작성자 정보(관리자 닉네임)
            "content": {                                    // 이벤트 컨텐츠 (다국어 지원)
                "title": "한국어",                            // 컨텐츠 제목
                "content": "한국어 이벤트 3",                   // 이벤트 내용
                "contentImageKey": "content-image",         // 첨부한 컨텐츠 이미지 키
                "popUpImageKey": "popup-image",             // 첨부한 팝업 이미지 키
                "linkUrl": "",                              // 외부 링크 버튼 url
                "linkButtonName": "",                       // 외부 링크 버튼 이름
                "language": "ko-KR",                        // 언어 코드
                "fallback": false                           // 기본 언어 폴백 여부
            }
        },
        {
            "uuid": "42b6889e-6eb1-4855-b547-7611ef4e1adb",
            "inDate": "2026-02-02T07:22:40.084Z",
            "postingDate": "2026-01-26",
            "startDate": "2026-02-03T02:00:34.828Z",
            "endDate": "2026-02-23T02:00:34.828Z",
            "isPublic": "y",
            "eventTitle": "이벤트 1",
            "author": "운영자",
            "content": {
                "title": "한국어",
                "content": "한국어 이벤트 1",
                "contentImageKey": "",
                "popUpImageKey": "",
                "linkUrl": "",
                "linkButtonName": "",
                "language": "kr",
                "fallback": true                            // 해당 국가 이벤트가 없어 기본 설정으로 폴백됨
            }
        }
    ]
}
```

## Sample Code

```csharp
// 이벤트 컨텐츠
public class EventContent
{
    public string title;
    public string content;
    public string contentImageKey;
    public string popUpImageKey;
    public string linkUrl;
    public string linkButtonName;
    public string language;
    public bool fallback;

    public override string ToString()
    {
        return $"  title : {title}\n" +
        $"  content : {content}\n" +
        $"  contentImageKey : {contentImageKey}\n" +
        $"  popUpImageKey : {popUpImageKey}\n" +
        $"  linkUrl : {linkUrl}\n" +
        $"  linkButtonName : {linkButtonName}\n" +
        $"  language : {language}\n" +
        $"  fallback : {fallback}\n";
    }
}

public class EventItem
{
    public string uuid;
    public string inDate;
    public DateTime postingDate;
    public DateTime startDate;
    public DateTime endDate;
    public bool isPublic;
    public string eventTitle;
    public string author;
    public EventContent content;

    public override string ToString()
    {
        return $"uuid : {uuid}\n" +
        $"inDate : {inDate}\n" +
        $"postingDate : {postingDate}\n" +
        $"startDate : {startDate}\n" +
        $"endDate : {endDate}\n" +
        $"isPublic : {isPublic}\n" +
        $"eventTitle : {eventTitle}\n" +
        $"author : {author}\n" +
        $"content :\n{content}";
    }
}
```

```csharp
public void EventListTest()
{
    List<EventItem> eventList = new List<EventItem>();

    BackendReturnObject bro = Backend.Event.EventListV2(10);

    if(bro.IsSuccess())
    {
        Debug.Log("리턴값 : " + bro);
        LitJson.JsonData jsonList = bro.GetReturnValuetoJSON()["rows"];
        for(int i = 0; i < jsonList.Count; i++)
        {
            EventItem eventItem = new EventItem();

            eventItem.uuid = jsonList[i]["uuid"].ToString();
            eventItem.inDate = jsonList[i]["inDate"].ToString();
            eventItem.postingDate = DateTime.Parse(jsonList[i]["postingDate"].ToString());
            eventItem.startDate = DateTime.Parse(jsonList[i]["startDate"].ToString());
            eventItem.endDate = DateTime.Parse(jsonList[i]["endDate"].ToString());
            eventItem.isPublic = jsonList[i]["isPublic"].ToString() == "y";
            eventItem.eventTitle = jsonList[i]["eventTitle"].ToString();
            eventItem.author = jsonList[i]["author"].ToString();

            // content 객체 파싱
            LitJson.JsonData contentJson = jsonList[i]["content"];
            eventItem.content = new EventContent
            {
                title = contentJson["title"].ToString(),
                content = contentJson["content"].ToString(),
                contentImageKey = contentJson["contentImageKey"].ToString(),
                popUpImageKey = contentJson["popUpImageKey"].ToString(),
                linkUrl = contentJson["linkUrl"].ToString(),
                linkButtonName = contentJson["linkButtonName"].ToString(),
                language = contentJson["language"].ToString(),
                fallback = (bool)contentJson["fallback"]
            };

            eventList.Add(eventItem);
            Debug.Log(eventItem.ToString());
        }
    }
}
```

## 이벤트에 등록한 이미지를 조회하는 방법

이벤트 등록 시 컨텐츠 이미지와 팝업 이미지를 등록했다면 위 GetReturnValuetoJSON() 값에서 **content** 객체 내의 **contentImageKey**와 **popUpImageKey** 값을 통해 이미지가 저장된 주소를 확인할 수 있습니다.

이 이미지 키값은 http://upload-console.thebackend.io 에 리턴된 이미지 키값을 더해 아래와 같은 주소로 요청하면 이미지를 조회할 수 있습니다.

```
http://upload-console.thebackend.io/{contentImageKey 또는 popUpImageKey 값}
```

:::caution iOS 주의
* iOS는 보안 정책에 따라 기본적으로 http의 접근을 제한하고 있습니다.  
      iOS 기기에서 공지/이벤트에 등록한 이미지를 가져오고 싶을 경우에는 아래와 같이 모든 http를 허용하거나 특정 도메인에 대한 예외 처리를 해주세요.  
:::

### 프로젝트 빌드 후 Info.plist의 App Transport Security Settings 설정

### CASE-1. 모든 Http의 접근을 허용할 경우.  

App Transport Security Settings의 Allow Arbitrary Loads를 YES로 변경합니다.  

<img src="https://developer.thebackend.io/static/img/unity/operation/http-info1.png" />

### CASE-2. 특정 도메인의 접근만 허용할 경우

- App Transport Security Settings의 Allow Arbitrary Loads를 NO로 변경합니다.  
- App Transport Security Settings의 옵션 중 Exception Domains를 생성하고 하위에 **upload-console.thebackend**를 추가합니다.  

<img src="https://developer.thebackend.io/static/img/unity/operation/http-info2.png" />
