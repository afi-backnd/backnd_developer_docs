---
sidebar_label: "유저 차단"
draft: "true"
unlisted: "true"
description: "BlockUser"
---

# BlockUser

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

delegate void BlockCallback(bool **IsSuccess**);    
public void **BlockUser**(string **NickName**);

## 파라미터

| Value    | Type   | Description          |
| :------- | :----- | :------------------- |
| NickName | string | 차단할 유저의 닉네임 |

## 설명

괴롭힘이나 스팸 등의 방지를 위해 해당 닉네임의 유저를 차단합니다.  
차단된 유저 목록은 [뒤끝 파일시스템 기능](/sdk-docs/backend/base/sdk-utils/filesystem)을 이용하여 로컬에 저장됩니다.  

해당 함수는 비동기 형식만 제공하고 있습니다.  

## Example

```js
Backend.Chat.BlockUser("Nickname", (blockCallback) => {
  // 성공
  if(blockCallback) {
  }
  // 실패
  else {
  }
});
```

## Return Cases

| IsSuccess(blockCallback) | Description                        |
| :----------------------- | :--------------------------------- |
| true                     | 차단 목록에 성공적으로 추가한 경우 |
| false                    | 해당 닉네임이 존재하지 않는 경우   |
