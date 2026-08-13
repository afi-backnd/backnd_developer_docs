---
sidebar_label: "게임방에 존재하는 유저 리스트 이벤트"
description: "OnSessionListInServer"
---

# OnSessionListInServer

public MatchInGameSessionListEventHandler **OnSessionListInServer**;

## 전달인자

| Value | Type                            | Description      |
| :---- | :------------------------------ | :--------------- |
| args  | MatchInGameSessionListEventArgs | 현재 게임방 정보 |

## MatchInGameSessionListEventArgs

| Value       | Type                                                   | Description                                                                |
| :---------- | :----------------------------------------------------- | :------------------------------------------------------------------------- |
| ErrInfo     | ErrorCode                                              | 에러 정보                                                                  |
| Reason      | string                                                 | 에러 정보                                                                  |
| RoomInfo    | [MatchInGameRoomInfo](/sdk-docs/backend/match/argument)        | 접속한 룸의 정보                                                           |
| GameRecords | List <[MatchUserGameRecord](/sdk-docs/backend/match/argument)> | 현재 게임방에 접속해 있는 유저들의 전적 리스트(각 세션들의 전적의 리스트) |

## 설명

유저가 게임방 접속에 성공했을 때 입장한 유저에게만 **최초 1회** 호출되는 이벤트 핸들러입니다.  
자신을 포함하여 **현재 게임방에 접속해 있는** 유저들의 세션 정보와 매칭 기록이 포함되어 있습니다.  
인게임 서버에 재접속했을 때도 호출됩니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

### 이벤트 호출 규칙

**현재 방에 접속해 있던 유저 + 방금 방에 접속한 유저**의 정보가 GameRecords에 포함되어 있습니다.  

- A, B, C 유저가 매칭되었습니다.  
- 게임방에 아무도 입장하지 않은 상황에서 A가 방에 접속하면 A의 GameRecord만 존재하는 OnSessionListInServer 이벤트가 A에게 호출됩니다.  
- 이후(A가 방에 있는 상황) B가 방에 접속하면 A, B의 GameRecord가 존재하는 OnSessionListInServer 이벤트가 B에게 호출됩니다.  
- 마지막으로 C가 방에 접속하면 A, B, C의 GameRecord가 존재하는 OnSessionListInServer 이벤트가 C에게 호출됩니다.  

## Example

```js
Backend.Match.OnSessionListInServer = (MatchInGameSessionListEventArgs args) => {
    // TODO
};
```

## ArgumentCase

** 유저가 게임방에 입장했을 때(입장한 유저에게만 호출됨) **  
ErrInfo : ErrorCode.Success
RoomInfo : "룸정보"
GameRecords : 현재 접속한 유저들의 정보

** 유저가 게임방 접속에 실패했을 때**  
호출되지 않습니다.  
