---
sidebar_label: 필터링 여부 설정
draft: true
unlisted: true
---

# SetFilterUse

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public bool **SetFilterUse**(bool **use**);

## 파라미터

| Value | Type | Description                                               | default |
| :---- | :--- | :-------------------------------------------------------- | :-----: |
| use   | bool | 필터링 설정 여부. true(필터링 사용), false(필터링 미사용) |  false  |

## 설명

콘솔에서 설정한 단어들을 채팅 수신 시 필터링합니다.  

해당 함수는 채팅 필터링 여부를 결정하고 성공 여부를 리턴합니다.  

## Example

```js
Backend.Chat.SetFilterUse(true);
```
