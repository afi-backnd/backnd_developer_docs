---
sidebar_label: 공지사항 하나 조회
description: "공지사항 하나 조회"
sidebar_position: 2
---

# GetAnnouncementByInDate

public Task&lt;GetAnnouncementResult&gt; **GetAnnouncementByInDateAsync**(string **inDate**);

## 파라미터

| Value  | Type   | Description                                                                                          |
| ------ | ------ | ---------------------------------------------------------------------------------------------------- |
| inDate | string | 불러올 공지사항의 inDate  
GetAnnouncements의 GetInfoList() > GetInfoList()[i].InDate > value |

## 설명

뒤끝 콘솔에 등록한 공지사항 항목 중 특정 공지사항의 정보를 반환합니다.  

리스트의 특정 공지사항을 가져오기 위해서는 **inDate**값이 필요합니다.  

## Example

**공지의 inDate 불러오기**  

```js
var reqResult = await BackndAnnouncement.Instance.GetAnnouncementsAsync(10);
//공지 리스트중 첫번째 공지 데이터
var inDate = reqResult.GetInfoList()[0].InDate;
```

### Task 형식
```js
var reqResult = await BackndAnnouncement.Instance.GetAnnouncementByInDateAsync(inDate);
var content = reqResult.GetInfo().Content;
```

### Callback 형식
```js
BackndAnnouncement.Instance.GetAnnouncementByInDate(inDate, callback =>
{
   var content = callback.GetInfo().Content;
});
```

## ReturnCase

### Success cases

**불러오기에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue: GetReturnValuetoJSON 참조

### Error cases

**해당 공지사항이 없는 경우**  
statusCode : 404  
errorCode : NotFoundException  
returnValue: notice not found, notice을(를) 찾을 수 없습니다

## GetReturnValuetoJSON

<a name = "noticeone-json"></a>
성공 시, 등록된 공지사항들의 정보를 나타냅니다.  

```js
{
    row:
    {
        content: // 공지 내용
            { S : "2019년 1월 31일 게임 업데이트 예정입니다. 업데이트 내용은 다음과 같습니다. " },
        postingDate: // 공지 게시 일자
            { S : "2018-12-19T09:30:00.000Z" },
        imageKey: // 첨부한 이미지 url
            { S : "/upload/2147483648/notice/2018-12-27T02:55:36.845Z511886972.jpg" },
        inDate: // 공지 indate
            { S : "2018-12-19T09:26:53.382Z" },
        uuid: // 공지 uuid
            { S : "38b07660-0370-11e9-8d1d-7571a0570adf" },
        linkUrl: // 외부 링크 버튼 url
            { S : "http://thebackend.io" },
        isPublic: // 공개/비공개 여부
            { S : "y" },
        linkButtonName: // 외부 링크 버튼 이름
            { S : "buttonN" },
        author: // 작성자
            { S : "jake" },
        title: // 공지 제목
            { S : "게임 업데이트 공지" }
    }
}
```

## Sample Code

```js
public class Notice
{
    public string title;
    public string contents;
    public DateTime postingDate;
    public string imageKey;
    public string inDate;
    public string uuid;
    public string linkUrl;
    public bool isPublic;
    public string linkButtonName;
    public string author;

    public override string ToString()
    {
        return $"title : {title}\n" +
        $"contents : {contents}\n" +
        $"postingDate : {postingDate}\n" +
        $"imageKey : {imageKey}\n" +
        $"inDate : {inDate}\n" +
        $"uuid : {uuid}\n" +
        $"linkUrl : {linkUrl}\n" +
        $"isPublic : {isPublic}\n" +
        $"linkButtonName : {linkButtonName}\n" +
        $"author : {author}\n";
    }
}
```

```js
public async Task GetAnnouncementByInDate()
{
    var noticeList = new List<Notice>();    
    var reqResult = await BackndAnnouncement.Instance.GetAnnouncementsAsync(10);
    if (reqResult.IsSuccess())
    {
        var infoList = reqResult.GetInfoList();
        for (int i = 0; i < infoList.Count; i++)
        {
            var inDate = infoList[i].InDate;
            var reqResult2 = await BackndAnnouncement.Instance.GetAnnouncementByInDateAsync(inDate);
            if (reqResult2.IsSuccess())
            {
                var info = reqResult2.GetInfo();

                Notice notice = new Notice();
                notice.title = info.Title;
                notice.contents = info.Content;
                notice.postingDate = DateTime.Parse(info.PostingDate);
                notice.inDate = info.InDate;
                notice.uuid = info.Uuid;
                notice.isPublic = info.IsPublic;
                notice.author = info.Author;
                notice.imageKey = info.ImageKey;
                notice.linkUrl = info.LinkUrl;
                notice.linkButtonName = info.LinkButtonName;

                noticeList.Add(notice);
                Debug.Log(notice.ToString());
            }
        }
    }
}
```

## 공지사항에 등록한 이미지를 조회하는 방법

공지사항 등록 시 이미지를 등록했다면 reqResult.GetJObject() 값의 **imageKey** 값에서 이미지가 저장된 주소를 확인할 수 있습니다.  

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
