---
sidebar_label: 유저
description: "유저"
---

# 유저

유저 정보를 관리 하실 수 있는 기능을 제공 합니다. 아바타, 닉네임, 언어 정보, 메타 정보에 대해서 수정이 가능하며, 유저간의 차단 기능을 제공하고 있습니다.

## 설명

유저 정보에 대한 콜백 및 호출 함수, 정보 클래스입니다.

### 유저 정보

#### 제약 사항

##### 유저 정보 길이 제한

- **닉네임**: 최대 45바이트까지 가능합니다.
- **아바타**: 최대 45바이트까지 가능합니다.
- **메타데이터**: 총 500바이트까지 저장할 수 있습니다.

##### 언어 코드 형식

- **언어 코드**는 ISO 639-1 언어 코드 형식을 따라야 합니다.
- 선택적으로 국가 코드를 포함할 수 있습니다.

자세한 언어 코드 목록은 [이 링크](https://www.localeplanet.com/dotnet/index.html)를 참고하세요.

> **참고**: 한글, 이모지 등 다국어 문자의 경우 1자가 여러 바이트를 차지할 수 있으므로, 바이트 기준으로 길이를 계산해야 합니다.

#### 콜백 함수

```csharp
// 채널에 접속 되어 있는 유저들 중 특정 유저의 정보가 변경 될 경우 오는 콜백 함수 입니다. 변경 된 유저 정보를 넘겨 줍니다.
void OnUpdatePlayerInfo(string channelGroup, string channelName, uint64_t channelNumber, BackndChat::PlayerInfo player) { }

// 채팅 서버에 접속 되어 있는 유저들 중 특정 유저의 닉네임이 변경 시 오는 콜백 함수 입니다. 변경 전, 후의 유저 닉네임을 넘겨 줍니다.
void OnChangeGamerName(string oldGamerName, string newGamerName) { }
```

#### 호출 함수

```csharp
// 자신의 아바타 정보를 변경 하는 함수 입니다. 변경 할 아바타 정보를 넣어서 보내 줍니다.
BackndChat::BDChatMain::UpdateAvatar(string avatar)

// 자신의 닉네임을 변경 하는 함수 입니다. 변경 할 닉네임을 넣어서 보내 줍니다.
BackndChat::BDChatMain::UpdateNickname(string nickname)

// 자신의 언어 정보를 변경 하는 함수 입니다. 변경 할 언어 정보를 넣어서 보내 줍니다.
BackndChat::BDChatMain::UpdateLanguage(string language)

// 자신의 메타 정보를 변경 하는 함수 입니다. 모든 메타 정보를 넣어서 보내 줍니다. (정보를 덮어쓰기를 합니다.)
BackndChat::BDChatMain::UpdateMetadata(map<string, string> metadata)
```

## 설명

유저간의 차단 기능에 대한 호출 함수 입니다.

### 유저 간 차단

#### 호출 함수

```csharp
// 현재 차단 한 유저의 닉네임 리스트를 얻어 옵니다.
vector<string> blockedPlayers = BackndChat::BDChatMain::GetBlockGamers();
```

```csharp
// 유저 차단 함수 입니다. 차단 할 유저의 닉네임을 넣어서 보내 줍니다.
BackndChat::BDChatMain::SendAddBlockGamer(string gamerName)

// 유저 차단 해제 함수 입니다. 해제 할 유저의 닉네임을 넣어서 보내 줍니다.
BackndChat::BDChatMain::SendRemoveBlockGamer(string gamerName);
```


### 정보 클래스

```csharp
struct PlayerInfo
{
  // 플레이어 이름
  string GamerName;

  // 플레이어 아바타 이름
  string Avatar;

  // 플레이어 언어
  string Language;

  // 플레이어 메타 정보
  map<string, string> Metadata;
};
```

### 커스텀 인증 사용 시 유저 간 차단

커스텀 인증을 사용하고 계시다면 연결하고자 하는 유저 간 차단 / 해제 엔드 포인트를 뒤끝 콘솔에서 설정하여야 합니다.

#### 커스텀 인증 사용 시 유저 간 차단 API 설정

콘솔의 채팅 > 설정 메뉴에서 유저 간 차단 / 해제 API 엔드 포인트를 설정할 수 있습니다.  

![custom auth](/img/docs/guide/chat/console/settings-customauth.png)

채팅 서버는 아래의 방식으로 HTTP 요청을 합니다.

#### 차단 API

##### HTTP request

```js
POST https://auth.mygame.com/block
```

##### Headers

```js
Content-Type: application/json; charset=utf8
token: USER_TOKEN
```

##### Request body

| Property name  | Type           | Value           | Description           |
| :------------ | :--------------- |:--------------- |:--------------- |
| source     | string                | thebackend-chat-api               | 요청한 서버 이름 (뒤끝의 채팅 서버)               |
| nickname     | string                | nickname              | 차단 할 유저 닉네임            |

```json
{
  "source": "thebackend-chat-api",
  "nickname" : "nickname",
}
```

##### Responses

형식에 맞는 응답을 반환하도록 구성하세요.  
유저 정보는 string 형식의 `nickname`과 `uid`를 필수로 포함해야 합니다.  
`uid`를 통해 유저를 구분하므로, `uid`는 각 유저에게 부여된 고유 값이어야 합니다.

다음은 유저 차단 API에서 채팅 서버로 보내는 유저 정보 예시입니다.
```json title="Status: 200 OK"
{
  "nickname": "john doe",
  "uid": "1234"
}
```

#### 해제 API

##### HTTP request

```js
DELETE https://auth.mygame.com/block
```

##### Headers

```js
Content-Type: application/json; charset=utf8
token: USER_TOKEN
```

##### Request body

| Property name  | Type           | Value           | Description           |
| :------------ | :--------------- |:--------------- |:--------------- |
| source     | string                | thebackend-chat-api               | 요청한 서버 이름 (뒤끝의 채팅 서버)               |
| uid     | string                | uid              | 해제 할 유저 고유 값            |
| nickname     | string                | nickname              | 해제 할 유저 닉네임            |

```json
{
  "source": "thebackend-chat-api",
  "nickname": "john doe",
  "uid": "1234"
}
```

##### Responses

형식에 맞는 응답을 반환하도록 구성하세요.  
유저 정보는 string 형식의 `nickname`과 `uid`를 필수로 포함해야 합니다.  
`uid`를 통해 유저를 구분하므로, `uid`는 각 유저에게 부여된 고유 값이어야 합니다.

다음은 유저 해제 API에서 채팅 서버로 보내는 유저 정보 예시입니다.
```json title="Status: 200 OK"
{
  "nickname": "john doe",
  "uid": "1234"
}
```
