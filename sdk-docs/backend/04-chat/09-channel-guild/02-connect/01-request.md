---
sidebar_label: 길드 채널 접속 요청
draft: true
unlisted: true
---

# JoinChannel

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public bool **JoinChannel**(ChannelType **type**, string **serverAddress**, ushort **serverPort**, string **groupName**, string **inDate**, out ErrorInfo **errorInfo**);

## 파라미터

| Value         | Type               | Description                                                               |
| :------------ | :----------------- | :------------------------------------------------------------------------ |
| type          | ChannelType(enum) | 입장할 채널의 종류. ChannelType.Public(일반) 혹은 ChannelType.Guild(길드) |
| serverAddress | string             | 접속할 대상 서버 주소                                                     |
| serverPort    | string             | 접속할 대상 서버 포트                                                     |
| groupName     | string             | 접속할 채널의 그룹명                                                      |
| inDate        | string             | 접속할 채널의 inDate                                                      |
| errorInfo     | string             | 성공/실패 정보                                                            |

## 설명

채팅 채널에 입장을 시도합니다.  

- 채팅 채널 접속 함수의 리턴이 true인 것은 채팅 채널 접속에 성공한 것이 아닌 단순히 서버와 소켓이 연결된 것입니다.  
- 실제 서버 연결은 채팅 서버에서 유저 인증을 완료한 후 이루어지며, **유저의 길드 채팅 채널 접속 성공/실패 여부는 [OnJoinGuildChannel](/sdk-docs/backend/chat/channel-guild/connect/event) 이벤트를 통해 확인**할 수 있습니다.  

:::info 데이터 통신 환경의 IP 할당 문제 안내
**데이터 통신환경의 IP 할당 문제**로 인해 데이터 통신 환경(3G/LTE/5G)에서 간헐적으로 채팅 서버 접속이 이루어지지 않는 현상이 발생될 수 있습니다.  

해당 현상은 뒤끝챗 뿐 아니라 은행 앱 등 서버와 통신이 필요한 다양한 종류의 어플리케이션에서 동일하게 발생하는 상황으로  
아래 안내드리는 방법을 통해 스마트폰에 부여된 IP주소를 갱신하여 해결이 가능하니 확인하여 채팅 서버 접속 불가 현상을 문의하는 유저분들께 안내해 주세요.  

**채팅 서버 접속 불가 상황**  
JoinChannel 호출 시 자동으로 `OnJoinGuildChannel()`함수가 호출되며 채널에 접속되어야 하지만 `OnJoinGuildChannel()`함수가 호출되지 않고 반응이 없는 상태가 지속되는 상황
	
**대처 방안**
* **비행기 모드를 20~30초간 유지했다가 끄고 이용**
* **스마트폰 네트워크 재설정 후 이용**
  * 아이폰 : 설정 - 일반 - 재설정 - 네트워크 재설정
  * 안드로이드 : 설정 - 일반 - 초기화 - 네트워크 설정 초기화
* 단, 네트워크 재설정 진행 시 와이파이, 블루투스 등 모든 네트워크 설정이 초기화 됩니다.  
:::

### 일반 채널과 길드 채널 동시에 입장

일반 채팅 채널과 길드 채팅 채널 접속 요청을 동시에 송신하는 경우 두 개의 채널에 모두 접속에 실패하거나
하나의 채널에만 접속될 수 있습니다.  

채팅 채널 접속의 경우 각각의 접속 이벤트(OnJoinChannel / OnJoinGuildChannel)가 호출되어야 일렬의 접속 프로세스가 완료된 것으로,
하나의 채팅 채널 접속을 완료한 뒤 다른 채팅 채널에 접속 요청을 보내야 정상적으로 두 개의 채팅 채널에 모두 접속할 수 있습니다.  

## Example

### Simple Example

```js
LitJson.JsonData channel = Backend.Chat.GetGroupChannelList("groupName").Rows()[0];

string address = channel["serverAddress"].ToString();
string serverPort = channel["serverPort"].ToString();
string inDate = channel["inDate"].ToString();
string groupName = "groupName";

ErrorInfo errorInfo;
Backend.Chat.JoinChannel(ChannelType.Guild, address , serverPort, groupName, inDate, out errorInfo);
```

### Long Example

```js
string groupName = "Public";
string serverAddress = "";
string alias = "";
string inDate = "";
ushort serverPort = 50000;

// 그룹 정보 불러오기
var bro = Backend.Chat.GetGroupChannelList(groupName);

if(bro.IsSuccess())
{
    var channels = bro.Rows();
    for(int i = 0; i < channels.Count; ++i)
    {
       // 순서대로 190명 이상 들어가 있지 않는 채널을 찾기
        var count = channels[i]["joinedUserCount"].ToString();
        var num = int.Parse(count);
        if(num >= 190)
        {
            continue;
        }
        else
        {
            // 들어갈 채널을 찾았으면 정보를 받고 채널 탐색 종료(break)
            serverAddress = channels[i]["serverAddress"].ToString();
            alias = channels[i]["alias"].ToString();
            inDate = channels[i]["inDate"].ToString();
            serverPort = ushort.Parse(channels[i]["serverPort"].ToString());
            break;
        }
    }
    Debug.Log(string.Format("address  : {0} / groupName : {1} / inDate : {2} / alias : {3}", serverAddress, groupName, inDate, alias));

    // 얻은 인자 값으로 입장
    ErrorInfo errorInfo;
    Backend.Chat.JoinChannel(ChannelType.Guild, serverAddress, 50000, groupName, inDate, out errorInfo);
    Debug.Log(errorInfo);
}
else
{
    Debug.Log("Error to GetGroupChannelList: " + bro);
}
```

## ReturnCase

### Success cases

**채팅 서버와 소켓 연결에 성공한 경우**  
true  
errorInfo : ErrorInfo.Success  

### Error cases

**채팅 서버 접속에 실패한 경우**  
false  
errorInfo.Category : ErrorCode.Exception  
errorInfo.Reasion : "이유"  

**채팅 서버와 소켓 연결에 실패한 경우**  
false  
errorInfo.Category : ErrorCode.SocketOperationError  
errorInfo.SocketError : `발생한 소켓 에러 타입`  
errorInfo.Reason : "이유"  

**이미 일반 채널에 접속해 있는데 접속을 시도한 경우**  
false  
errorInfo.Category : ErrorCode.DuplicateConnection  
errorInfo.Reason : "Already Online(1)"  

### Exception Case

**채팅 채널에 접속을 시도한 상태에서 접속이 완료되기 전 다시 채팅 채널에도 접속을 시도하는 경우**  
TCP Client is working.  

> 동일한 채널에 접속을 시도할 때, 다른 채널에 접속을 시도할 때 모두 발생할 수 있습니다.  
> 채널에 접속되어 있지 않은데, 위 예외가 지속적으로 발생할 경우 [채팅 서버 접속 초기화](/sdk-docs/backend/chat/initialize-server) 함수를 호출하는 것을 추천합니다.  
