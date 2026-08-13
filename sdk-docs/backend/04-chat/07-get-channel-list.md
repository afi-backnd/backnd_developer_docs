---
sidebar_label: "채널 리스트 조회"
draft: "true"
unlisted: "true"
description: "GetGroupChannelList"
---

# GetGroupChannelList

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public BackendReturnObject **GetGroupChannelList**(string **groupName**);

## 파라미터

| Value     | Type   | Description             |
| :-------- | :----- | :---------------------- |
| groupName | string | 검색할 채널 그룹의 이름 |

## 설명

그룹명을 통해 해당 그룹의 채팅 채널의 정보들을 불러옵니다.  
채팅 채널 그룹은 [**뒤끝 콘솔 > 뒤끝챗 > 채팅 그룹 관리**에서 미리 생성](/guide/console-guide/backnd-chat/groups)해야 하며 콘솔에서 생성한 채널 그룹을 불러올 수 있습니다.  

> 30초동안 유저가 존재하지 않는 채널은 자동으로 삭제가 됩니다.  
> 채널 리스트를 조회한 후, 유저가 없는 채널을 30초 이상의 시간이 지난 뒤에 접속을 시도할 경우 이미 삭제된 채널이기에 에러가 발생합니다.  
> 이 경우에는 다시 채널 리스트 조회 함수를 호출하여 접속 가능한 채널을 생성해주세요.  

## Example

### 동기

```js
Backend.Chat.GetGroupChannelList(string groupName);
```

### 비동기

```js
Backend.Chat.GetGroupChannelList(string groupName, (callback) =>
{
    // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Chat.GetGroupChannelList, string groupName, (callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

### Error cases

**groupName이 비어있을 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : Resource not found, Resource을(를) 찾을 수 없습니다

**해당 그룹명이 존재하지 않는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : Not found 그룹명 group. not found, Not found 그룹명 group.을(를) 찾을 수 없습니다

**채팅 서비스가 비활성화 상태인 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : chat server is not available 사전 조건을 만족하지 않습니다  

## GetReturnValuetoJSON

```js
{
 "groupName":"운영자전용",
	"rows": [{
		"groupName": "운영자전용",
		"alias": "버그피드백채널",
		"inDate": "2021-01-13T01:14:30.263Z",
		"maxUserCount": 200,
		"joinedUserCount": 10,
		"serverAddress": "ec2-3-36-74-178.ap-northeast-2.compute.amazonaws.com",
		"serverPort": 50000
	},
       {
		"groupName": string,
		"alias": string,
		"inDate": string,
		"maxUserCount": number,
		"joinedUserCount": number,
		"serverAddress": string,
		"serverPort": number
	}, ...]
}
```

## Sample Code

```js
public class ChatGroup
{
    public string groupName;
    public string alias;
    public string inDate;
    public int maxUserCount;
    public int joinedUserCount;
    public string serverAddress;
    public ushort serverPort;

    public override string ToString()
    {
        return $"groupName : {groupName}\n" +
        $"alias : {alias}\n" +
        $"inDate : {inDate}\n" +
        $"maxUserCount : {maxUserCount}\n" +
        $"joinedUserCount : {joinedUserCount}\n" +
        $"serverAddress : {serverAddress}\n" +
        $"serverPort : {serverPort}\n";
    }
}
```

```js
public void GetGroupChannelList()
{
    string publicGroupName = "PublicAuto";

    var callback = Backend.Chat.GetGroupChannelList(publicGroupName);

    if(!callback.IsSuccess())
    {
        Debug.LogError($"{publicGroupName} 채팅 채널 불러오기 실패 : {callback.ToString()}");
        return;
    }

    LitJson.JsonData groupListJson = callback.FlattenRows();

    List<ChatGroup> publicGroupsList = new List<ChatGroup>();

    for(int i = 0; i < groupListJson.Count; i++)
    {
        ChatGroup chatGroup = new ChatGroup();

        chatGroup.groupName = groupListJson[i]["groupName"].ToString();
        chatGroup.alias = groupListJson[i]["alias"].ToString();
        chatGroup.inDate = groupListJson[i]["inDate"].ToString();
        chatGroup.maxUserCount = int.Parse(groupListJson[i]["maxUserCount"].ToString());
        chatGroup.joinedUserCount = int.Parse(groupListJson[i]["joinedUserCount"].ToString());
        chatGroup.serverAddress = groupListJson[i]["serverAddress"].ToString();
        chatGroup.serverPort = ushort.Parse(groupListJson[i]["serverPort"].ToString());

        publicGroupsList.Add(chatGroup);
    }

    string str = "Backend.Chat.GetGroupChannelList : \n";
    foreach(var li in publicGroupsList)
    {
        str += li.ToString() + "\n";
    }
    Debug.Log(str);
}
```
