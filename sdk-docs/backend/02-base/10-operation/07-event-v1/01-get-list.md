---
sidebar_label: "이벤트 리스트 조회"
sidebar_position: "1"
description: "EventList"
---

# EventList

public BackendReturnObject **EventList**();  
public BackendReturnObject **EventList**(int **limit**);  
public BackendReturnObject **EventList**(int **limit**, string **offset**);

## 파라미터

| Value  | Type   | Description                                                                                                    |
| ------ | ------ | -------------------------------------------------------------------------------------------------------------- |
| limit  | int    | (Optional) 불러올 이벤트의 수                                                                                  |
| offset | string | (Optional) 불러올 이벤트의 시작점  (BackendReturnObject.LastEvaluatedKeyString()를 통해 얻으실 수 있습니다.) |

## 설명

뒤끝 콘솔에 등록한 이벤트를 받아오는 기능을 제공합니다.  

- 유저의 국가 코드가 존재하고, 콘솔에 해당 국가 이벤트가 등록되어 있는 경우 → 해당 국가의 이벤트 제목과 내용을 리턴합니다.  
- 유저의 국가 코드가 존재하지 않거나, 콘솔에 해당 국가 이벤트가 등록되어 있지 않은 경우 → 기본 설정의 제목과 내용을 리턴합니다.  

## Example

### 동기

```js
//10개의 이벤트 리스트 불러오기
BackendReturnObject bro = Backend.Event.EventList(10);

if(bro.IsSuccess()) {
    //10개를 불러온 이후에 이벤트 10
    string offset = bro.LastEvaluatedKeyString();
    if(!string.IsNullOrEmpty(offset)) {
        // 2. 불러온 이벤트 2개 이후의 3개 불러오기
        Backend.Event.EventList(3, offset);
    }
}
```

### 비동기

```js
Backend.Event.EventList(2, (callback) => {
    // 이후 처리
    if(callback.IsSuccess()) {
        string offset = callback.LastEvaluatedKeyString();
        if(!string.IsNullOrEmpty(offset)) {
            // 2. 불러온 이벤트 2개 이후의 3개 불러오기
            Backend.Event.EventList(3, offset, (callback2) => {
                // 이후 처리
            });
        }
    }
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Event.EventList, 2, (callback) => {
    // 이후 처리
    if(callback.IsSuccess()) {
        string offset = callback.LastEvaluatedKeyString();
        if(!string.IsNullOrEmpty(offset)) {
            // 2. 불러온 이벤트 2개 이후의 3개 불러오기
            SendQueue.Enqueue(Backend.Event.EventList, 3, offset, (callback2) => {
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

```js
{
    rows:
    [
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
        },
        {
            uuid: [Object],
            content: [Object],
            contentImageKey: [Object],
            popUpImageKey: [Object],
            postingDate: [Object],
            endDate: [Object],
            inDate: [Object],
            startDate: [Object],
            linkUrl: [Object],
            isPublic: [Object],
            linkButtonName: [Object],
            author: [Object],
            title: [Object]
        }
    ],
    // 불러온 데이터 이후의 데이터가 존재하는 경우 리턴됨
    // 불러온 데이터 이후의 데이터가 존재하지 않는 경우 리턴되지 않음
    LastEvaluatedKey:{
        gamer_id: { S: "event" },
        inDate:{ S: "2019-08-19T06:45:07.816Z" },
        partition:{ S: "event" }
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
public void EventListTest()
{
    List<EventItem> eventList = new List<EventItem>();

    BackendReturnObject bro = Backend.Event.EventList(10);

    if(bro.IsSuccess())
    {
        Debug.Log("리턴값 : " + bro);
        LitJson.JsonData jsonList = bro.FlattenRows();
        for(int i = 0; i < jsonList.Count; i++)
        {
            EventItem eventItem = new EventItem();

            eventItem.title = jsonList[i]["title"].ToString();
            eventItem.content = jsonList[i]["content"].ToString();
            eventItem.postingDate = DateTime.Parse(jsonList[i]["postingDate"].ToString());
            eventItem.startDate = DateTime.Parse(jsonList[i]["startDate"].ToString());
            eventItem.endDate = DateTime.Parse(jsonList[i]["endDate"].ToString());
            eventItem.inDate = jsonList[i]["inDate"].ToString();
            eventItem.uuid = jsonList[i]["uuid"].ToString();
            eventItem.isPublic = jsonList[i]["isPublic"].ToString() == "y" ? true : false;
            eventItem.author = jsonList[i]["author"].ToString();

            if(jsonList[i].ContainsKey("contentImageKey"))
            {
                eventItem.contentImageKey = "http://upload-console.thebackend.io" + jsonList[i]["contentImageKey"].ToString();
            }
            if(jsonList[i].ContainsKey("popUpImageKey"))
            {
                eventItem.popUpImageKey = "http://upload-console.thebackend.io" + jsonList[i]["popUpImageKey"].ToString();
            }
            if(jsonList[i].ContainsKey("linkUrl"))
            {
                eventItem.linkUrl = jsonList[i]["linkUrl"].ToString();
            }
            if(jsonList[i].ContainsKey("linkButtonName"))
            {
                eventItem.linkButtonName = jsonList[i]["linkButtonName"].ToString();
            }

            eventList.Add(eventItem);
            Debug.Log(eventItem.ToString());
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

### 프로젝트 빌드 후 Info.lplist 열기 > Key중 App Transport Security Settings Key 값을 찾거나 생성.  

### CASE-1. 모든 Http의 접근을 허용할 경우.  

App Transport Security Settings의 Allow Arbitrary Loads를 YES로 변경합니다.  

<img src="https://developer.thebackend.io/static/img/unity/operation/http-info1.png" />

### CASE-2. 특정 도메인의 접근만 허용할 경우

- App Transport Security Settings의 Allow Arbitrary Loads를 NO로 변경합니다.  
- App Transport Security Settings의 옵션 중 Exception Domains를 생성하고 하위에 **upload-console.thebackend**를 추가합니다.  

<img src="https://developer.thebackend.io/static/img/unity/operation/http-info2.png" />
