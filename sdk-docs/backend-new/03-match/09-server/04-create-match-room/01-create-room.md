---
sidebar_label: 대기방 생성요청
---

# CreateMatchRoom

public void**CreateMatchRoom**();

## 설명

대기방을 생성합니다.  

- 대기방 생성과 동시에 해당 방에 입장하게 됩니다.  
- 대기방을 생성한 유저가 **[방장](/sdk-docs/backend/match/supervisor-and-superuser)**이 됩니다.  

## Example

```js
BackndBase.Match.CreateMatchRoom();
```
