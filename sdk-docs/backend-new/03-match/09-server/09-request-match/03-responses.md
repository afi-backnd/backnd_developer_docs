---
sidebar_label: 매칭 신청/취소/성사 이벤트
---

# OnMatchMakingResponse
public MatchMakingResponseEventHandler OnMatchMakingResponse;

## 파라미터
| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| args| MatchMakingResponseEventArgs | 매칭 신청 성공 / 신청 취소 / 매칭 성사 정보 |

## args 타입 명
| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| ErrInfo | ErrorCode | 에러 정보 |
| Reason | string | 에러 정보 |
| MatchCardIndate | string | 성사된 매칭 정보(매치 카드 inDate) |
| RoomInfo | [MatchInGameRoomInfo](/sdk-docs/backend/match/argument) | 접속할 게임방의 정보 |

## 설명
유저가 **매칭을 신청, 취소** 했을 때 그리고 **매칭이 성사**되었을 때 호출되는 이벤트 핸들러입니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  


## Example
```js
BackndBase.Match.OnMatchMakingResponse = (MatchMakingResponseEventArgs args) => {
    // TODO
};
```

## ArgumentCase

** 매칭 신청에 성공했을 때 **  
ErrInfo : ErrorCode.Match_InProgress

** 매칭이 성사되었을 때 **  
ErrInfo : ErrorCode.Success
MatchCardIndate : 성사된 매칭 카드 inDate  
RoomInfo : 인게임 서버 및 입장할 방의 정보

** 매칭 신청을 취소했을 때 **  
ErrInfo : ErrorCode.Match_MatchMakingCanceled

** 잘못된 매치 타입으로 매칭을 신청했을 때 **  
ErrInfo : ErrorCode.Match_InvalidMatchType

** 잘못된 매치 모드 타입으로 매칭을 신청했을 때  **  
ErrInfo : ErrorCode.Match_InvalidModeType

** 잘못된 요청을 했을 때 **  
ErrInfo : ErrorCode.InvalidOperation  
Reason : 에러 상황에 대한 이유

** 매칭 신청 요청을 2번 이상 보낸 경우 **  
ErrInfo : ErrorCode.InvalidOperation  
Reason : Already matchmaking in-progress.  

** 방장이 아닌 유저가 매칭을 신청했을 때 **  
ErrInfo : ErrorCode.InvalidOperation   
Reason : You are not owner of room.  

** 방장이 아닌 유저가 매칭을 취소했을 때 **  
ErrInfo : ErrorCode.InvalidOperation   
Reason : You are not owner of room.  

** 대기방에 허용된 팀 인원보다 많은 인원이 존재할 경우(ex. 2:2 팀전에서 대기방에 3명 이상의 유저가 존재) **  
ErrInfo : ErrorCode.Match_Making_InvalidRoom   
Reason : There are currently more squad members than are supported.  

** 대기방에 2명 이상의 유저가 존재하는데 1:1, 개인전 매칭을 신청한 경우 **  
ErrInfo : ErrorCode.InvalidOperation   
Reason : Squad does not support OneOnOne or Melee mode.  

** 매칭이 성사되었으나 서버에서 인게임 룸 생성에 실패했을 경우(다시 매칭을 신청해야 합니다.) **  
ErrInfo : ErrorCode.Match_MatchMakingCanceled  
Reason : FDM 코드 리턴

