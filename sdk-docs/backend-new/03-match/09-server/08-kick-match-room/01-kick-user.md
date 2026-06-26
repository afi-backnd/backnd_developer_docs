---
sidebar_label: 유저 강퇴 요청
---

# KickUser

public void **KickUser**(string **nickName**);

## 파라미터

| Value    | Type   | Description          |
| :------- | :----- | :------------------- |
| nickName | string | 강퇴할 유저의 닉네임 |

## 설명

유저를 방에서 강퇴합니다.  

- 방장만 호출할 수 있습니다.  

## Example

```js
BackndBase.Match.KickUser(nickName);
```
