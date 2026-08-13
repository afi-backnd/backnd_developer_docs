---
sidebar_label: "차단 목록에서 유저 조회"
draft: "true"
unlisted: "true"
description: "IsUserBlocked"
---

# IsUserBlocked

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public void **IsUserBlocked**(string **NickName**);

## 파라미터

| Value    | Type   | Description          |
| :------- | :----- | :------------------- |
| NickName | string | 조회할 유저의 닉네임 |

## 설명

차단 목록에서 해당 닉네임의 유저가 존재하는지 조회합니다.  

해당 함수는 동기 형식만 제공하고 있습니다.  

## Example

```js
string isUnblock = Backend.Chat.IsUserBlocked("Nickname");

if(isUnblock)
  Debug.Log("해당 유저는 차단된 상태입니다.");
else
  Debug.Log("해당 유저는 차단 목록에 존재하지 않습니다");
```

## Return Cases

| returnValue(bool) | Description           |
| :---------------- | :-------------------- |
| true              | 차단 목록에 있는 경우 |
| false             | 차단 목록에 없는 경우 |
