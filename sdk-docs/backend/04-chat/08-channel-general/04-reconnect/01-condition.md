---
sidebar_label: 재접속 조건
draft: true
unlisted: true
---

# 재접속 조건

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

뒤끝챗은 클라이언트와 채팅 서버가 1분 30초 이상 연결이 끊어지는 경우 채팅 서버와의 연결이 끊어지게 됩니다.  
SDK 5.4.5 이하 버전에서는 일시적으로 서버와 연결이 끊어졌을 때 재접속을 시도하지만,
SDK 5.5.0 이상 버전에서는 채팅 서버와의 연결을 완전히 끊어버리며 재접속을 시도하지 않습니다.  

## SDK 5.5.0 이상 버전

뒤끝챗에서는 **클라이언트와 채팅 서버가 연결이 끊어지는 경우** 재접속을 하지 않고 채팅 채널을 나가게 됩니다.  

해당 경우에는 [채널 접속 종료 이벤트](/sdk-docs/backend/chat/channel-general/deconnect/event)가 발생하며, 다시 [채널에 접속을 요청](/sdk-docs/backend/chat/channel-general/connect/request)해야 합니다.  

> 나가진 채널에 다시 접속을 하기 위해 JoinChannel을 호출할 경우, 방의 인원이 꽉 차있다면 채널에 접속이 되지 않을 수 있습니다.  

## SDK 5.4.5 이하 버전

뒤끝챗에서는 [클라이언트와 채팅 서버가 연결이 끊어지는 경우](/sdk-docs/backend/chat/architecture) 자동으로 재접속을 시도합니다.  

채널에 재접속을 하기 위해서는 아래 조건이 만족되어야 합니다.  

- 클라이언트가 네트워크에 연결된 상태여야 합니다.  
- 주기적으로 Poll 함수를 호출하고 있어야 합니다.  

재접속한 주체가 자기 자신인지, 다른 유저인지에 따라 다른 이벤트가 호출됩니다.  

### 자기 자신이 재접속한 경우

**일반 채널 재접속 성공 시**  
[일반 채널 접속 이벤트](/sdk-docs/backend/chat/channel-general/connect/event)가 호출됩니다.  
 
**일반 채널 재접속 실패 시**  
[일반 채널 접속 종료 이벤트](/sdk-docs/backend/chat/channel-general/deconnect/event)가 호출됩니다.  

### 다른 유저가 재접속한 경우

**일반 채널 재접속 성공 시**  
[일반 채널 재접속 성공 이벤트](/sdk-docs/backend/chat/channel-general/reconnect/old-event)가 호출됩니다.  

**일반 채널 재접속 실패 시**  
[일반 채널 접속 종료 이벤트](/sdk-docs/backend/chat/channel-general/deconnect/event)가 호출됩니다.  
