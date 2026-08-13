---
sidebar_label: "유저를 대기방에 초대 요청"
description: "InviteUser"
---

# InviteUser

public void **InviteUser**(string **nickName**);

## 파라미터

| Value    | Type   | Description        |
| :------- | :----- | :----------------- |
| nickName | string | 초대할 유저 닉네임 |

## 설명

대기방에 유저를 초대합니다.  

## Example

```js
Backend.Match.InviteUser(nickName);
```
