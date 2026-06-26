---
sidebar_label: 매칭 서버 접속 요청
---

# JoinMatchMakingServer

public bool **JoinMatchMakingServer**(out ErrorInfo **errorInfo**);

## 파라미터

| Value     | Type      | Description    |
| :-------- | :-------- | :------------- |
| errorInfo | ErrorInfo | 성공/실패 정보 |

## 설명

매칭 서버에 접속을 시도합니다.  

- 매칭 서버 접속 함수의 리턴이 true인 것은 서버 접속에 성공한 것이 아닌 단순히 서버와 소켓이 연결된 것입니다.  
- 실제 서버 연결은 매칭 서버에서 유저 인증을 완료한 후 이루어지며, **매칭 서버의 접속 성공/실패 여부는 [OnJoinMatchMakingServer](/sdk-docs/backend/match/server/join-match-making-sever/join-server-event) 이벤트**를 통해 확인할 수 있습니다.  

## Example

```js
ErrorInfo errorInfo;
Backend.Match.JoinMatchMakingServer(out errorInfo);
```

## ReturnCase

### Success cases

**매칭 서버와 소켓 연결에 성공한 경우**  
true  
errorInfo : ErrorInfo.Success

### Error cases

**매칭 서버 접속에 실패한 경우**  
false  
errorInfo.Category : ErrorCode.Exception  
errorInfo.Reason : "이유"

**매칭 서버와 소켓 연결에 실패한 경우**  
false  
errorInfo.Category : ErrorCode.SocketOperationError  
errorInfo.SocketError : `발생한 소켓 에러 타입`  
errorInfo.Reason : "이유"
