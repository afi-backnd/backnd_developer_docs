---
sidebar_label: 재접속 가능 여부 확인
---

# IsGameRoomActivate

public BackendReturnObject **IsGameRoomActivate**();

## 설명

유저가 진행 중이던 게임이 있는지 체크하는 함수입니다.  
재접속이 가능한 게임이 존재할 때 해당 게임의 서버 주소, 서버 포트, 매치 타입, 매치 모드 타입, 룸 토큰이 반환됩니다.  

## Example

### 동기

```js
Backend.Match.IsGameRoomActivate();
```

### 비동기

```js
Backend.Match.IsGameRoomActivate((callback) => {
  //todo
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Match.IsGameRoomActivate, (callback) => {
  //todo
});
```

## ReturnCase

### Success cases

**재접속 가능한 게임이 있을 때**  
statusCode : 200  
ReturnValue : GetReturnValuetoJSON 참조

### Error cases

**재접속 가능한 게임이 없을 때**  
statusCode : 404  
ErrorCode : NotFoundError  
Message : room token을 찾을 수 없을 때

## GetReturnValuetoJSON

```js
{
    {
        serverPublicHostName  : { "인게임 서버 주소" },
        serverPort : { "인게임 서버 포트" },
        roomToken : { "룸 토큰" }
        matchType : { "1" }, // 해당 string 값을 byte로 타입캐스팅 후 MatchType으로 타입캐스팅 후 사용해야 합니다.  
        matchModeType : { "1" }, // 해당 string 값을 byte로 타입캐스팅 후 MatchModeType으로 타입캐스팅 후 사용해야 합니다.  
        enable_sandbox_mode : { true }, // 성사된 매칭의 샌드박스 모드 여부
        match_card_inDate : { "매칭카드Indate" }  // 성사된 매칭의 매칭 카드 inDate
    },
    {
        serverPublicHostName: [string],
        serverPort: [string],
        roomToken : [string],
        matchType: [string],
        matchModeType: [string],
        enable_sandbox_mode: [bool],
        match_card_inDate: [string]
    },
}
```
