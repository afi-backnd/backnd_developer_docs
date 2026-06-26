---
sidebar_label: 이벤트 하나 조회
sidebar_position: 2
---

# EventOne

public BackendReturnObject **EventOne**(string **inDate**);

## 파라미터

| Value  | Type   | Description                                                                                      |
| :----- | :----- | :----------------------------------------------------------------------------------------------- |
| inDate | string | 불러올 Event의 inDate (EventList의 BRO Return Value > JsonArray >(JsonObject) inDate > value) |

## 설명

뒤끝 콘솔에 등록한 이벤트 목록 중 특정 이벤트의 정보를 반환합니다.  

리스트의 특정 이벤트를 가져오기 위해서는 **inDate**값이 필요합니다.  

## Example

### 동기

```js
BackendReturnObject bro = Backend.Event.EventList(2);

string inDate = bro.Rows()[0]["inDate"]["S"].ToString();

BackendReturnObject bro2 = Backend.Event.EventOne(inDate);

string Content = bro2.GetReturnValuetoJSON()["row"]["content"]["S"].ToString();
```

### 비동기

```js
Backend.Event.EventList(2, callback => {
    string inDate = callback.Rows()[0]["inDate"]["S"].ToString();
    
    Backend.Event.EventOne(inDate,callback2 => {
        string Content = callback2.GetReturnValuetoJSON()["row"]["content"]["S"].ToString();
    });
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Event.EventList, 2, callback => {
    string inDate = callback.Rows()[0]["inDate"]["S"].ToString();
    
    SendQueue.Enqueue(Backend.Event.EventOne, inDate, callback2 => {
        string Content = callback2.GetReturnValuetoJSON()["row"]["content"]["S"].ToString();
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
returnValue: notice not found, notice을(를) 찾을 수 없습니다

## GetReturnValuetoJSON

성공 시, 해당 이벤트의 정보를 나타냅니다.  

```js
{
    row:
    {
        uuid: // 이벤트 uuid
            { S : "b33bc260-0981-11e9-bfd0-3bf848e9c52c" },
        content: // 이벤트 내용
            { S : "쿠폰번호를 입력하시면 아이템을 지급해드립니다~!" },
        contentImageKey: // 첨부한 컨텐츠 이미지 url
            { S : "/upload/2147483648/event/2018-12-27T02:46:57.391Z312652213.jpg" },
        popUpImageKey: // 첨부한 팝업 이미지 url
            { S : "/upload/2147483648/event/2018-12-27T02:46:58.945Z936251230.jpg" },
        postingDate: // 이벤트 게시 일자
            { S : "2018-12-27T02:00:00.000Z" },
        endDate: // 이벤트 종료일
            { S : "2018-12-28T03:00:00.000Z" },
        inDate: // 이벤트 indate
            { S : "2018-12-27T02:47:07.399Z" },
        startDate: // 이벤트 시작일
            { S : "2018-12-27T03:00:00.000Z" },
        linkUrl: // 외부 링크 버튼 url
            { S : "http://thebackend.io" },
        isPublic: // 공개/비공개 여부(y: 공개)
            { S : "y" },
        linkButtonName: // 외부 링크 버튼 이름
            { S : "buttonX" },
        author: // 작성자 정보(관리자 닉네임)
            { S : "jake" },
        title: // 이벤트 제목
            { S : "10만 다운로드 기념 이벤트!" }
    }
}
```

## Sample Code

```js
public class EventItem
{
    public string uuid;
    public string content;
    public string contentImageKey;
    public string popUpImageKey;
    public DateTime postingDate;
    public DateTime startDate;
    public DateTime endDate;
    public string inDate;
    public string linkUrl;
    public string author;
    public bool isPublic;
    public string linkButtonName;
    public string title;

    public override string ToString()
    {
        return $"uuid : {uuid}\n" +
        $"content : {content}\n" +
        $"contentImageKey : {contentImageKey}\n" +
        $"popUpImageKey : {popUpImageKey}\n" +
        $"postingDate : {postingDate}\n" +
        $"startDate : {startDate}\n" +
        $"endDate : {endDate}\n" +
        $"inDate : {inDate}\n" +
        $"linkUrl : {linkUrl}\n" +
        $"author : {author}\n" +
        $"isPublic : {isPublic}\n" +
        $"linkButtonName : {linkButtonName}\n" +
        $"title : {title}\n";
    }
}
```

```js
public void EventOne()
{
    List<EventItem> eventList = new List<EventItem>();
    List<string> eventIndateList = new List<string>();

    BackendReturnObject bro = Backend.Event.EventList(10);

    if(!bro.IsSuccess())
        return;

    for(int i = 0; i < bro.FlattenRows().Count; i++)
    {
        eventIndateList.Add(bro.FlattenRows()[i]["inDate"].ToString());
    }

    for(int i = 0; i < eventIndateList.Count; i++)
    {
        var eventOne = Backend.Event.EventOne(eventIndateList[i]);

        if(eventOne.IsSuccess())
        {
            var json = eventOne.GetFlattenJSON();

            EventItem eventItem = new EventItem();

            eventItem.title = json["row"]["title"].ToString();
            eventItem.content = json["row"]["content"].ToString();
            eventItem.postingDate = DateTime.Parse(json["row"]["postingDate"].ToString());
            eventItem.startDate = DateTime.Parse(json["row"]["startDate"].ToString());
            eventItem.endDate = DateTime.Parse(json["row"]["endDate"].ToString());
            eventItem.inDate = json["row"]["inDate"].ToString();
            eventItem.uuid = json["row"]["uuid"].ToString();
            eventItem.isPublic = json["row"]["isPublic"].ToString() == "y" ? true : false;
            eventItem.author = json["row"]["author"].ToString();

            if(json["row"].ContainsKey("contentImageKey"))
            {
                eventItem.contentImageKey = "http://upload-console.thebackend.io" + json["row"]["contentImageKey"].ToString();
            }
            if(json["row"].ContainsKey("popUpImageKey"))
            {
                eventItem.popUpImageKey= "http://upload-console.thebackend.io" + json["row"]["popUpImageKey"].ToString();
            }
            if(json["row"].ContainsKey("linkUrl"))
            {
                eventItem.linkUrl = json["row"]["linkUrl"].ToString();
            }
            if(json["row"].ContainsKey("linkButtonName"))
            {
                eventItem.linkButtonName = json["row"]["linkButtonName"].ToString();
            }

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

이벤트 등록 시 컨텐츠 이미지와 팝업 이미지를 등록했다면 위 GetReturnValuetoJSON() 값에서 각각 **contentImageKey**와 **popUpImageKey** 값을 통해 이미지가 저장된 주소를 확인할 수 있습니다.  

이 이미지 키값은 http://upload-console.thebackend.io 에 리턴된 이미지 키값을 더해 아래와 같은 주소로 요청하면 이미지를 조회할 수 있습니다.  

```js
http://upload-console.thebackend.io/upload/2147483648/event/2018-12-27T02:46:57.391Z312652213.jpg
```

:::caution iOS 주의
* iOS는 보안 정책에 따라 기본적으로 http의 접근을 제한하고 있습니다.  
      iOS 기기에서 공지/이벤트에 등록한 이미지를 가져오고 싶을 경우에는 아래와 같이 모든 http를 허용하거나 특정 도메인에 대한 예외 처리를 해주세요.  
:::

### 프로젝트 빌드 후 Info.lplist 열기 > Key중 App Transaport Security Settings Key 값을 찾거나 생성.  

### CASE-1. 모든 Http의 접근을 허용할 경우.  

App Transport Security Settings의 Allow Arblitray Loads를 YES로 변경합니다.  

<img src="https://developer.thebackend.io/static/img/unity/operation/http-info1.png" />

### CASE-2. 특정 도메인의 접근만 허용할 경우

- App Transport Security Settings의 Allow Arblitray Loads를 NO로 변경합니다.  
- App Transport Security Settings의 옵션 중 Exception Domains를 생성하고 하위에 **upload-console.thebackend**를 추가합니다.  

<img src="https://developer.thebackend.io/static/img/unity/operation/http-info2.png" />
