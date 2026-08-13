---
sidebar_label: "최근 채팅 내역 조회"
draft: "true"
unlisted: "true"
description: "GetRecentChat"
---

# GetRecentChat

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public BackendReturnObject**GetRecentChat**(ChannelType **channelType**, string **channelinDate**);  
public BackendReturnObject**GetRecentChat**(ChannelType **channelType**, string **channelinDate**, int **limit**);

## 파라미터

| Value         | Type        | Description                                                             | Default |
| :------------ | :---------- | :---------------------------------------------------------------------- | :-----: |
| channelType   | ChannelType | 해당 채널의 종류. ChannelType.Public(일반) 혹은 ChannelType.Guild(길드) |    -    |
| channelIndate | string      | 해당 채널의 inDate                                                      |    -    |
| limit         | int         | (Optional) 불러올 게임 정보 row 수. 최대 30. 0일 경우, 30개 반환        |    0    |

## 설명

최근 내역 불러오기 이벤트와는 별개의 함수로, 원하는 시점에 호출할 수 있는 비동기 함수입니다.  
채널에 입장하기 전에도 해당 함수를 사용하여 최근 채팅 내역을 볼 수 있습니다.  

> 리턴 시의 key 값에서 해당 채널의 inDate는 channel_uuid, 메시지를 보낸 시간은 inDate입니다.  

- 일반/길드 채널의 내역만 불러오며, 귓속말이나 공지/전체 채팅은 포함되지 않습니다.  
- 메시지 내역은 최대 30개까지 불러올 수 있습니다.  
- 최근 1~2분 사이의 보낸 메시지들은 불러오지 못할 수 있습니다.  
- 메시지의 필터링은 적용되나, 차단은 적용되지 않습니다.  
- 발송 시간부터 48시간이 지난 메시지들은 불러올 수 없습니다.(콘솔의 뒤끝챗 내역 보기와 동일)

## Example

### 동기

```js
// 채팅 채널 리스트 가져오기
BackendReturnObject bro = Backend.Chat.GetGroupChannelList("그룹명");

//채팅 채널 uuid 받아오기
string channelIndate = bro.GetReturnValuetoJSON()["rows"][0]["inDate"].ToString();

//uuid를 이용하여 해당 일반 채널의 최근 채팅 내역 가져오기(25개만)
BackendReturnObject result = Backend.Chat.GetRecentChat(ChannelType.Guild , channelIndate , 25);

for(int i=0; i< result.Rows().Count; i++)
{
string nickname = result.Rows()[i]["nickname"].ToString();
string message = result.Rows()[i]["message"].ToString();

Debug.Log(nickname + " : " + message);
}
```

### 비동기

```js
// 채팅 채널 리스트 가져오기
Backend.Chat.GetGroupChannelList("그룹 명", callback =>
{
    if(callback.IsSuccess())
    {
        //채팅 채널 uuid 받아오기
        string channelIndate = callback.GetReturnValuetoJSON()["rows"][0]["inDate"].ToString();

        //uuid를 이용하여 해당 일반 채널의 최근 채팅 내역 가져오기(25개만)
        Backend.Chat.GetRecentChat(ChannelType.Guild , channelIndate , 25, callback2 =>
        {
            if(callback2.IsSuccess())
            {
                //성공 시...  
            }
        });
    }
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Chat.GetGroupChannelList, "그룹 명", callback =>
{
    if(callback.IsSuccess())
    {
        //채팅 채널 uuid 받아오기
        string channelIndate = callback.GetReturnValuetoJSON()["rows"][0]["inDate"].ToString();

        //uuid를 이용하여 해당 일반 채널의 최근 채팅 내역 가져오기(25개만)
        SendQueue.Enqueue(Backend.Chat.GetRecentChat,ChannelType.Guild , channelIndate , 25, callback2 =>
        {
            if(callback2.IsSuccess())
            {
                //성공 시...  
            }
        });
    }
});
```

## ReturnCase

### Success cases

**불러오기에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

### Error cases

**존재하지 않는 channel일 경우**  
statusCode : 400  
errorCode : UndefinedParameterException  
message : undefined channel, channel을 찾을 수 없습니다.  

**존재하지 않는 channelinDate일 경우**  
statusCode : 400  
errorCode : NotFoundException  
message : log not found, log를 찾을 수 없습니다.  

**채팅 내역이 존재하지 않을 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : log not found, log를 찾을 수 없습니다.  

## GetReturnValuetoJSON

```js
{
    "rows":
    [
        {
            "channel":0,
            "channel_uuid":"2021-01-12T08:42:39.086Z", // 해당 채널의 inDate
            "inDate":"2020-10-27T02:40:42.580549Z", // 메시지를 보낸 시간
            "message":"안녕하세요",
            "nickname":"a1"
        },
        {
            "channel":0,
            "channel_uuid":"2021-01-12T08:42:39.086Z",
            "inDate":"2020-10-27T02:40:43.780942Z",
            "message":" 잘부탁드립니다",
            "nickname":"b1"
        }
    ]
}
```
