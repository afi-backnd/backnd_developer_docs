---
sidebar_label: 인자/리턴 클래스 정보
draft: true
unlisted: true
---

# 인자/리턴 클래스 정보

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

뒤끝챗 이용 시 인자 값으로 주어지거나 리턴 값으로 리턴되는 클래스들에 대한 정보입니다.  

## ErrorInfo

에러 정보를 표현하는 클래스입니다.  

| Value           | Type                                                                                                            | Description                    |
| :-------------- | :-------------------------------------------------------------------------------------------------------------- | :----------------------------- |
| Category        | ErrorCode                                                                                                       | ErrorCode 카테고리             |
| Detail          | ErrorCode                                                                                                       | ErrorCode 상세내용             |
| SocketErrorCode | [SocketError](https://docs.microsoft.com/ko-kr/dotnet/api/system.net.sockets.socketerror?view=netframework-3.5) | Socket 클래스에 대한 오류 코드 |
| Reason          | string                                                                                                          | 성공/실패 사유                 |

## ErrorCode(enum)

<table>
    <thead>
        <tr>
            <th >Value</th>
            <th >Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td >Success</td>
            <td >요청 성공</td>
        </tr>
        <tr>
            <td >Exception</td>
            <td >내부 알 수 없는 예외 발생 에러 코드</td>
        </tr>
        <tr>
            <td >SocketOpertatonError</td>
            <td >소켓 작업이 실패한 경우 발생</td>
        </tr>
        <tr>
            <td >AuthenticationFailed</td>
            <td >채팅 서버에서 인증이 실패했을 경우 발생</td>
        </tr>
        <tr>
            <td >BrokenStream</td>
            <td >소켓 관련 송/수신 스트림에 문제가 생겼을 경우 발생</td>
        </tr>
        <tr>
            <td >NetworkTimeout</td>
            <td >채팅 서버와 비정상적인 이유로 연결이 끊어진 후 재접속에 실패한 경우 발생</td>
        </tr>
        <tr>
            <td >DisconnectFromLocal</td>
            <td >어떠한 이유로 SDK가 채팅 서버와 연결을 끊었을 경우 발생</td>
        </tr>
        <tr>
            <td >DisconnectFromRemote</td>
            <td >어떠한 이유로 채팅 서버가 연결을 끊었을 경우 발생</td>
        </tr>
        <tr>
            <td >InvalidMessage</td>
            <td >잘못된 메시지 송/수신 시 발생</td>
        </tr>
        <tr>
            <td >InvalidOperation</td>
            <td >잘못된 요청 시 발생</td>
        </tr>
        <tr>
            <td >InvalidSession</td>
            <td >채팅 서버에서 식별 불가능한 이유로 유효하지 않은 세션 에러</td>
        </tr>
        <tr>
            <td >ChannelTimeOut</td>
            <td >일정 시간 동안 채팅을 하지 않았을 경우 발생</td>
        </tr>
        <tr>
            <td >BannedChat</td>
            <td >일정 시간 동안 N회의 채팅 메시지를 전송할 경우 발생 & 어드민이 아닌 유저가 GlobalChat을 사용하는 경우 발생</td>
        </tr>
        <tr>
            <td >DuplicateConnection</td>
            <td >중복 로그인 시 발생</td>
        </tr>
        <tr>
            <td >NetworkOffline</td>
            <td >채팅 서버와 비정상적인 이유로 연결이 일시적으로 끊어지면 발생(자기 자신 / 다른 유저 모두 포함)</td>
        </tr>
        <tr>
            <td >NetworkOnline</td>
            <td >NetworkOffline 이후 채팅 서버와 연결이 회복되면 발생(자기자신 / 다른 유저 모두 포함)</td>
        </tr>
    </tbody>
</table>

## SessionInfo

채널에 있는 유저들의 정보에 대한 클래스입니다.  

| Value    | Type   | Description                                                   |
| :------- | :----- | :------------------------------------------------------------ |
| NickName | string | 해당 세션 유저의 닉네임                                       |
| IsRemote | bool   | remote 여부(나의 세션인 경우 false, 타인의 세션인 경우 true) |
