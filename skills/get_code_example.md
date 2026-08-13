# get_code_example

기능명을 입력하면 관련 코드 예제와 문서 경로를 반환합니다.

## 설명

TheBackend SDK의 특정 기능에 대한 코드 예제와 참조 문서를 반환합니다.
코드 예제는 `llms-full.txt`에 수록되어 있으며, 상세 내용은 각 문서 경로를 참조하세요.

## 입력 파라미터

| 파라미터 | 타입 | 필수 | 설명 |
|---|---|---|---|
| `feature` | string | ✓ | 기능명 |
| `module` | string | - | SDK 모듈 한정 |

## 기능-경로 매핑

| feature | 문서 경로 |
|---|---|
| 초기화, initialize | sdk-docs/backend/02-base/05-sdk-initialize.md |
| 회원가입, signup | sdk-docs/backend/02-base/13-user/ |
| 로그인, login | sdk-docs/backend/02-base/13-user/ |
| 플레이어 데이터, game data | sdk-docs/backend/02-base/12-player-data/ |
| 랭킹, leaderboard, rank | sdk-docs/backend/02-base/14-leaderboard/ |
| 길드, guild | sdk-docs/backend/02-base/27-guild/ |
| 쿠폰, coupon | sdk-docs/backend/02-base/15-coupon/ |
| 푸시, push | sdk-docs/backend/02-base/16-push/ |
| 게임 로그, game log | sdk-docs/backend/02-base/17-game-log/ |
| 확률, probability, gacha | sdk-docs/backend/02-base/19-probability/ |
| 영수증, receipt | sdk-docs/backend/02-base/20-receipt/ |
| 친구, friend | sdk-docs/backend/02-base/24-friend/ |
| 우편, post, mail | sdk-docs/backend/02-base/25-post/ |
| 채팅 채널, chat channel | sdk-docs/chat/06-channel.md |
| 채팅 메시지, chat message | sdk-docs/chat/07-message.md |
| 펑션 호출, function call | sdk-docs/backend/02-base/31-function/ |
| DB 삽입, insert | sdk-docs/database/03-insert.md |
| DB 조회, query | sdk-docs/database/04-query.md |
| 트랜잭션, transaction | sdk-docs/database/06-transaction.md |

## 예시

**입력**: `{ "feature": "랭킹" }`

**참조**: `sdk-docs/backend/02-base/14-leaderboard/`

**코드 요약**:
```csharp
// 점수 저장
Backend.Rank.RegistRankScore("uuid", score, callback);

// 랭킹 조회
Backend.Rank.GetRankList("uuid", callback);
```
