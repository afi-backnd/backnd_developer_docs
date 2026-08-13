---
sidebar_label: "유저 차단 해제"
draft: "true"
unlisted: "true"
description: "UnblockUser"
---

# UnblockUser

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public void **UnblockUser**(string **NickName**);

## 파라미터

| Value    | Type   | Description            |
| :------- | :----- | :--------------------- |
| NickName | string | 차단해제 유저의 닉네임 |

## 설명

차단한 유저를 차단 목록에서 해제시킵니다.  

해당 함수는 동기 형식만 제공하고 있습니다.  

## Example

```js
string isUnblock = Backend.Chat.UnblockUser("Nickname");

if(isUnblock)
  Debug.Log("해제에 성공했습니다");
else
  Debug.Log("해당 유저는 차단 목록에 존재하지 않습니다");
```

## Return Cases

| returnValue(bool) | Description                      |
| :---------------- | :------------------------------- |
| true              | 차단 해제된 경우                 |
| false             | 해당 닉네임이 존재하지 않는 경우 |
