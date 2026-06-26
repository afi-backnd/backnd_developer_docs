---
sidebar_label: 이벤트 하나 조회
sidebar_position: 2
---

# EventOneV2

public BackendReturnObject **EventOneV2**(string **inDate**);

## 파라미터

| Value  | Type   | Description                                                                                      |
| :----- | :----- | :----------------------------------------------------------------------------------------------- |
| inDate | string | 불러올 Event의 inDate (EventListV2의 GetReturnValuetoJSON()["rows"][i]["inDate"]) |

## 설명

뒤끝 콘솔에 등록한 이벤트 목록 중 특정 이벤트의 정보를 반환합니다.  

리스트의 특정 이벤트를 가져오기 위해서는 **inDate**값이 필요합니다.  

## Example

### 동기

```csharp
BackendReturnObject bro = Backend.Event.EventListV2(2);

string inDate = bro.GetReturnValuetoJSON()["rows"][0]["inDate"].ToString();

BackendReturnObject bro2 = Backend.Event.EventOneV2(inDate);

string content = bro2.GetReturnValuetoJSON()["row"]["content"]["content"].ToString();
```

### 비동기

```csharp
Backend.Event.EventListV2(2, callback => {
    string inDate = callback.GetReturnValuetoJSON()["rows"][0]["inDate"].ToString();

    Backend.Event.EventOneV2(inDate, callback2 => {
        string content = callback2.GetReturnValuetoJSON()["row"]["content"]["content"].ToString();
    });
});
```

### SendQueue

```csharp
SendQueue.Enqueue(Backend.Event.EventListV2, 2, callback => {
    string inDate = callback.GetReturnValuetoJSON()["rows"][0]["inDate"].ToString();

    SendQueue.Enqueue(Backend.Event.EventOneV2, inDate, callback2 => {
        string content = callback2.GetReturnValuetoJSON()["row"]["content"]["content"].ToString();
    });
});
```

## ReturnCase

### Success cases

**불러오기에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

### Error cases

**불러오기에 실패한 경우**  
statusCode : 404  
errorCode : NotFoundException  
returnValue: event not found, event을(를) 찾을 수 없습니다

## GetReturnValuetoJSON

성공 시, 해당 이벤트의 정보를 나타냅니다.

```json
{
    "row": {
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
    }
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
public void EventOne()
{
    List<EventItem> eventList = new List<EventItem>();
    List<string> eventIndateList = new List<string>();

    BackendReturnObject bro = Backend.Event.EventListV2(10);

    if(!bro.IsSuccess())
        return;

    LitJson.JsonData rows = bro.GetReturnValuetoJSON()["rows"];
    for(int i = 0; i < rows.Count; i++)
    {
        eventIndateList.Add(rows[i]["inDate"].ToString());
    }

    for(int i = 0; i < eventIndateList.Count; i++)
    {
        var eventOne = Backend.Event.EventOneV2(eventIndateList[i]);

        if(eventOne.IsSuccess())
        {
            var json = eventOne.GetReturnValuetoJSON()["row"];

            EventItem eventItem = new EventItem();

            eventItem.uuid = json["uuid"].ToString();
            eventItem.inDate = json["inDate"].ToString();
            eventItem.postingDate = DateTime.Parse(json["postingDate"].ToString());
            eventItem.startDate = DateTime.Parse(json["startDate"].ToString());
            eventItem.endDate = DateTime.Parse(json["endDate"].ToString());
            eventItem.isPublic = json["isPublic"].ToString() == "y";
            eventItem.eventTitle = json["eventTitle"].ToString();
            eventItem.author = json["author"].ToString();

            // content 객체 파싱
            LitJson.JsonData contentJson = json["content"];
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
        else
        {
            Debug.Log($"해당 이벤트를 불러올 수 없습니다 : {eventIndateList[i].ToString()}");
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
