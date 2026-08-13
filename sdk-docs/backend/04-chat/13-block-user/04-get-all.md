---
sidebar_label: "차단 목록 모두 불러오기"
draft: "true"
unlisted: "true"
description: "GetBlockUserList"
---

# GetBlockUserList

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public LitJson.JsonData**GetBlockUserList**();

## 설명

차단 목록 리스트를 조회합니다.  

해당 함수는 동기 형식만 제공하고 있습니다.  

## Example

```js
LitJson.JsonData blockList = Backend.Chat.GetBlockUserList();

for(int i=0; blockList.Count; i++)
{
    Debug.Log("닉네임 : "+blockList[i].ToString());
    //닉네임 : a1
    //닉네임 : a2
    //닉네임 : b1
}
```

## Return Cases

**조회에 성공한 경우**  
returnValue : Json 참조

**차단 유저 목록이 비어있을 경우**  
returnValue : `[ ]`

### Json

```js
["nickname1", "nickname2", "nickname3", "nickname4"];
```
