---
sidebar_label: "공지사항 리스트 조회"
sidebar_position: "1"
description: "NoticeListV2"
---

# NoticeListV2

public BackendReturnObject **NoticeListV2**();  
public BackendReturnObject **NoticeListV2**(int **limit**);  
public BackendReturnObject **NoticeListV2**(int **limit**, string **offset**);

## 파라미터

| Value  | Type   | Description                                                                                                      |
| ------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| limit  | int    | (Optional) 불러올 공지사항의 수                                                                                  |
| offset | string | (Optional) 불러올 공지사항의 시작점  (BackendReturnObject.LastEvaluatedKeyString()를 통해 얻으실 수 있습니다.) |

## 설명

뒤끝 콘솔에 등록한 공지사항을 받아오는 기능을 제공합니다.

- 유저의 언어정보가 존재하고, 콘솔에 해당 언어 공지가 등록되어 있는 경우 → 해당 언어의 공지 제목과 내용을 리턴합니다.
- 유저의 언어정보가 존재하지 않거나, 콘솔에 해당 언어 공지가 등록되어 있지 않은 경우 → 기본 설정의 제목과 내용을 리턴합니다.

## Example

### 동기

```csharp
//10개의 공지사항 리스트 불러오기
BackendReturnObject bro = Backend.Notice.NoticeListV2(10);

if(bro.IsSuccess()) {
    //10개를 불러온 이후에 공지사항 10
    string offset = bro.LastEvaluatedKeyString();
    if(!string.IsNullOrEmpty(offset)) {
        // 2. 불러온 공지사항 2개 이후의 3개 불러오기
        Backend.Notice.NoticeListV2(3, offset);
    }
}
```

### 비동기

```csharp
Backend.Notice.NoticeListV2(2, (callback) => {
    if(callback.IsSuccess()) {
        string offset = callback.LastEvaluatedKeyString();
        if(!string.IsNullOrEmpty(offset)) {
            // 2. 불러온 공지사항 2개 이후의 3개 불러오기
            Backend.Notice.NoticeListV2(3, offset, (callback2) => {
                // 이후 처리
            });
        }
    }
});
```

### SendQueue

```csharp
SendQueue.Enqueue(Backend.Notice.NoticeListV2, 2, (callback) => {
    if(callback.IsSuccess()) {
        string offset = callback.LastEvaluatedKeyString();
        if(!string.IsNullOrEmpty(offset)) {
            // 2. 불러온 공지사항 2개 이후의 3개 불러오기
            SendQueue.Enqueue(Backend.Notice.NoticeListV2, 3, offset, (callback2) => {
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

**공지사항이 0개일 경우**  
statusCode : 200  
message : Success  
returnValue : { "rows":[] }

## GetReturnValuetoJSON

<a name = "noticelist-json"></a>
성공 시, 등록된 공지사항들의 정보를 나타냅니다.

```json
{
    // 불러온 데이터 이후의 데이터가 존재하는 경우 리턴됨
    // 불러온 데이터 이후의 데이터가 존재하지 않는 경우 리턴되지 않음
    "LastEvaluatedKey":{
        "gamer_id": { "S": "_notice" },
        "inDate":{ "S": "2019-08-19T06:45:07.816Z" },
    },
    "rows": [
        {
            "uuid": "f3df1f9d-52eb-4685-8d35-f75528789292",  // 공지 uuid
            "inDate": "2026-02-03T04:21:16.627Z",            // 공지 inDate
            "postingDate": "2025-07-20",                     // 공지 게시 일자
            "isPublic": "y",                                 // 공개/비공개 여부 (y: 공개)
            "noticeTitle": "공지 제목",                        // 공지 제목
            "author": "운영자",                               // 작성자 정보 (관리자 닉네임)
            "content": {                                    // 공지 컨텐츠 (다국어 지원)
                "title": "한국어",                            // 컨텐츠 제목
                "content": "한국어 공지 내용",                  // 공지 내용
                "imageKey": "",                             // 첨부한 이미지 키
                "linkUrl": "",                              // 외부 링크 버튼 url
                "language": "kr",                           // 언어 코드
                "fallback": true                            // 기본 언어 폴백 여부
            }
        },
        {
            "uuid": "a1b2c3d4-5678-90ab-cdef-1234567890ab",
            "inDate": "2026-02-02T10:15:30.123Z",
            "postingDate": "2025-07-15",
            "isPublic": "y",
            "noticeTitle": "게임 업데이트 공지",
            "author": "운영자",
            "content": {
                "title": "한국어",
                "content": "게임 업데이트 내용입니다.",
                "imageKey": "/upload/2147483648/notice/2026-02-02T10:15:30.123Z.jpg",
                "linkUrl": "http://thebackend.io",
                "language": "kr",
                "fallback": false
            }
        }
    ]
}
```

## Sample Code

```csharp
// 공지 컨텐츠
public class NoticeContent
{
    public string title;
    public string content;
    public string imageKey;
    public string linkUrl;
    public string language;
    public bool fallback;

    public override string ToString()
    {
        return $"  title : {title}\n" +
        $"  content : {content}\n" +
        $"  imageKey : {imageKey}\n" +
        $"  linkUrl : {linkUrl}\n" +
        $"  language : {language}\n" +
        $"  fallback : {fallback}\n";
    }
}

public class NoticeItem
{
    public string uuid;
    public string inDate;
    public DateTime postingDate;
    public bool isPublic;
    public string noticeTitle;
    public string author;
    public NoticeContent content;

    public override string ToString()
    {
        return $"uuid : {uuid}\n" +
        $"inDate : {inDate}\n" +
        $"postingDate : {postingDate}\n" +
        $"isPublic : {isPublic}\n" +
        $"noticeTitle : {noticeTitle}\n" +
        $"author : {author}\n" +
        $"content :\n{content}";
    }
}
```

```csharp
public void GetNoticeList()
{
    List<NoticeItem> noticeList = new List<NoticeItem>();

    BackendReturnObject bro = Backend.Notice.NoticeListV2(10);
    if(bro.IsSuccess())
    {
        Debug.Log("리턴값 : " + bro);

        LitJson.JsonData jsonList = bro.GetReturnValuetoJSON()["rows"];
        for(int i = 0; i < jsonList.Count; i++)
        {
            NoticeItem notice = new NoticeItem();

            notice.uuid = jsonList[i]["uuid"].ToString();
            notice.inDate = jsonList[i]["inDate"].ToString();
            notice.postingDate = DateTime.Parse(jsonList[i]["postingDate"].ToString());
            notice.isPublic = jsonList[i]["isPublic"].ToString() == "y";
            notice.noticeTitle = jsonList[i]["noticeTitle"].ToString();
            notice.author = jsonList[i]["author"].ToString();

            // content 객체 파싱
            LitJson.JsonData contentJson = jsonList[i]["content"];
            notice.content = new NoticeContent
            {
                title = contentJson["title"].ToString(),
                content = contentJson["content"].ToString(),
                imageKey = contentJson["imageKey"].ToString(),
                linkUrl = contentJson["linkUrl"].ToString(),
                language = contentJson["language"].ToString(),
                fallback = (bool)contentJson["fallback"]
            };

            noticeList.Add(notice);
            Debug.Log(notice.ToString());
        }
    }
}
```

## 공지사항에 등록한 이미지를 조회하는 방법

공지사항 등록 시 이미지를 등록했다면 위 GetReturnValuetoJSON() 값에서 **content** 객체 내의 **imageKey** 값을 통해 이미지가 저장된 주소를 확인할 수 있습니다.

이 이미지 키값은 http://upload-console.thebackend.io 에 리턴된 이미지 키값을 더해 아래와 같은 주소로 요청하면 이미지를 조회할 수 있습니다.

```
http://upload-console.thebackend.io/{imageKey 값}
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
