---
sidebar_label: 매칭 신청
---

# RequestMatchMaking

public void **RequestMatchMaking**(MatchType **matchType**, MatchModeType **matchModeType**, string **matchCardIndate**);

## 파라미터

| Value           | Type          | Description                                                                                                                     |
| :-------------- | :------------ | :------------------------------------------------------------------------------------------------------------------------------ |
| matchType       | MatchType     | 신청할 매치 타입                                                                                                                |
| modeType        | MatchModeType | 신청할 매치 모드 타입                                                                                                           |
| matchCardIndate | string        | [GetMatchList 함수](/sdk-docs/backend/match/get-list)에서 리턴 받은 inDate 혹은  
뒤끝 콘솔에서 확인할 수 있는 해당 매칭의 inDate |

## 설명

뒤끝 콘솔에서 생성한 매칭을 신청합니다.  

- 대기방에 참여하고 있을 때만 매칭을 신청할 수 있습니다.  
- 대기방 안에 **자기 자신만 존재할 경우 일대일, 개인전, 팀전** 모두 신청할 수 있습니다.  
- 대기방 안에 **다른 유저와 함께 있을 경우 팀전**만 신청할 수 있습니다.  
- **매칭 신청은 방장(방을 만든 유저)만** 할 수 있습니다.  

## Example

```js
BackndBase.Match.RequestMatchMaking(matchType, modeType, inDate);
```
