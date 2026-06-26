---
sidebar_label: 예외 처리
draft: true
unlisted: true
---

# OnException

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public ExceptionEventHandler **OnException**;

## 전달인자

| Value | Type      | Description |
| :---- | :-------- | :---------- |
| e     | Exception | 발생한 예외 |

## 설명

뒤끝챗에서 예외가 발생하는 경우 호출되는 이벤트입니다.  
예외는 아래의 경우 발생할 수 있습니다.  

- 서버에서 송신한 메시지를 수신 및 처리하면서 예외가 발생하는 경우
- 개발사에서 작성한 이벤트 핸들러 내에서 예외가 발생하는 경우

:::caution Backend-5.0.3 이하 버전 Exception 발생
* Backend-5.0.3 이하 버전은 채널 입장시 OnException 이벤트 핸들러에서 해당 에러가 호출됩니다.  
* **System.NotSupportedException : Specified method is not supported.**
* 해당 Exception이 발생할 경우, 뒤끝 SDK 버전을 5.0.4 이상으로 올려주시기 바랍니다.  
:::

## Example

```js
Backend.Chat.OnException += (Exception e)  => {
    // TODO
};
```
