---
sidebar_label: 게임 결과 메시지 전송
---

# MatchEnd

public void **MatchEnd**(MatchGameResult **matchGameResult**);

## 파라미터

| Value           | Type                                        | Description |
| :-------------- | :------------------------------------------ | :---------- |
| matchGameResult | [MatchGameResult](/sdk-docs/backend/chat/basic) | 게임의 결과 |

## 설명

게임의 결과를 서버로 전송합니다.  
게임 결과 메시지를 서버로 보내고 서버에서 결과 종합을 완료한 경우 **[게임 결과 이벤트](/sdk-docs/backend/match/ingame-server/result/match-result)와 [인게임 서버 접속 종료 이벤트](/sdk-docs/backend/match/ingame-server/leave-game-server/leave-server-event)가 함께 호출**됩니다.  

### 결과 처리 방법

**콘솔에서 설정한 결과 처리 방법**에 따라 게임 결과를 처리하는 방법이 다릅니다.  

| 결과 처리 방법 | 설명 |
| ---------------------------------- | -------------------------------------------------------------------------------------------- |
| 기본 | **게임에 참여한 모든 유저**가 게임 결과를 전송해야 게임 결과가 정상적으로 서버에 저장됩니다. |
| 슈퍼 게이머 | **SuperGamer 유저만** 게임 결과를 전송하면 게임 결과가 정상적으로 서버에 저장됩니다. |

> 기본의 경우 게임 진행 중 게임에서 나간 유저가 있으면 게임에 참여한 모든 유저가 결과 메시지를 서버로 보낼 수 없으므로 결과 종합을 할 수 없습니다.  

## Example

```js
// 1:1
MatchGameResult matchGameResult = new MatchGameResult();
matchGameResult.m_winners = new List<SessionId>();
matchGameResult.m_losers = new List<SessionId>();

matchGameResult.m_winners.Add("승자 세션 ID");
matchGameResult.m_losers.Add("패자 세션 ID");
// --------------------

// 팀전
MatchGameResult matchGameResult = new  MatchGameResult();
matchGameResult.m_winners = new List<SessionId>();
matchGameResult.m_losers = new List<SessionId>();

foreach(var session in winnerTeam)
{
    // 순서는 무관합니다.  
    matchGameResult.m_winners.Add(session);
}

foreach(var session in loserTeam)
{
    // 순서는 무관합니다.  
    matchGameResult.m_losers .Add(session);
}
// --------------------

// 개인전
MatchGameResult matchGameResult = new MatchGameResult();
foreach(var session in rank)
{
    // 1등부터 차례대로 추가해야 합니다.  
    matchGameResult.m_winners.Add(session);
}
// --------------------

// 서버로 결과 전송
BackndBase.Match.MatchEnd(matchGameResult);
```
