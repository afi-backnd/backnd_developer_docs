---
sidebar_label: "[Deprecated] 길드 메타 데이터 변경"
draft: true
unlisted: true
---

# ModifyGuildV3

public BackendReturnObject **ModifyGuildV3**(Param **param**);

:::danger 랭킹 정산 도중 함수 호출 시 랭킹 정산 실패 문제
메타 정보를 이용한 길드 랭킹을 이용 시, **랭킹 정산 시간에 해당 함수를 호출할 경우 랭킹 정산이 비정상적으로 종료**될 수 있습니다.  

따라서 메타 정보를 이용한 길드 랭킹을 이용하는 경우, 4~5시에 호출에는 갱신이 불가능한  **ModifyGuildV4** 기능을 이용해주시기 바랍니다.  
:::

## 파라미터

| Value | Type  | Description                        |
| ----- | ----- | ---------------------------------- |
| param | Param | 길드에 관하여 생성/수정할 메타 정보 |

## 설명

길드의 메타 정보를 변경합니다. 길드명은 변경 불가합니다.  

## Example

### 동기

```js
Param param = new Param();
param.Add("buf",2);

Backend.Guild.ModifyGuildV3(param);
```

### 비동기

```js
Param param = new Param();
param.Add("buf",2);

Backend.Guild.ModifyGuildV3(param, (callback) =>
{
    // 이후 처리
});
```

### SendQueue

```js
Param param = new Param();
param.Add("buf",2);

SendQueue.Enqueue(Backend.Guild.ModifyGuildV3, param, (callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**변경에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**길드명을 변경 시도한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad guildName, 잘못된 guildName 입니다

**길드에 가입하지 않은 회원이 시도한 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : subscribed guild 사전 조건을 만족하지 않습니다.  
