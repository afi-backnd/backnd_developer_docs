---
sidebar_label: "[Deprecated] 길드 메타 데이터 변경(4시~5시 갱신불가)"
sidebar_position: "1.1"
draft: "true"
unlisted: "true"
description: "ModifyGuildV4"
---

# ModifyGuildV4

public BackendReturnObject **ModifyGuildV4**(Param **param**);

:::info 안내
해당 함수는 랭킹 정산 에러를 방지하기 위해 매일 오전 4~5시외 시간에만 메타 정보를 변경하는 함수입니다.  
 메타 정보를 이용한 랭킹을 이용하지 않는 경우에는 제한이 존재하지 않는 ModifyGuildV3를 이용하시기 바랍니다.  
:::

## 파라미터

| Value | Type  | Description                        |
| ----- | ----- | ---------------------------------- |
| param | Param | 길드에 관하여 생성/수정할 메타 정보 |

## 설명

길드의 메타 정보를 변경합니다. 길드명은 변경 불가합니다.  
매일 오전 4~5시 사이에는 변경이 불가능하며, 요청 시 428에러가 리턴됩니다.  
ModifyGuildV3와 사용법은 동일합니다.  

> 메타 정보를 이용한 랭킹 갱신 중 해당 함수의 호출이 발생할 경우, 낮은 확률로 랭킹 정산이 실패하게 됩니다. 정산 시간에는 갱신이 불가능하도록 해당 함수로 마이그레이션을 권장드립니다.  

## Example

### 동기

```js
Param param = new Param();
param.Add("buf",2);

Backend.Guild.ModifyGuildV4(param);
```

### 비동기

```js
Param param = new Param();
param.Add("buf",2);

Backend.Guild.ModifyGuildV4(param, (callback) =>
{
    // 이후 처리
});
```

### SendQueue

```js
Param param = new Param();
param.Add("buf",2);

SendQueue.Enqueue(Backend.Guild.ModifyGuildV4, param, (callback) =>
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

**새벽 4~5시 사이에 호출한 경우**  
statusCode : 428  
errorCode : Precondition Required  
message : Precondition Required ranking is being counted
