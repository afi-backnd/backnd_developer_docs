---
sidebar_label: 길드 가입 요청
description: "길드 가입 요청"
---

# RequestGuildJoin

public Task< RequestResult > **RequestGuildJoinAsync**(string **guildIndate**);

## 파라미터

| Value       | Type   | Description              |
| ----------- | ------ | ------------------------ |
| guildIndate | string | 가입신청할 길드의 indate |

## 설명

특정 길드에 가입을 요청합니다.  
**[콘솔 - 운영 - 길드 - 설정]**에서 설정한 가입조건을 만족해야 가입 요청이 가능합니다.
가입 조건이 없는 경우, 모든 유저가 가입을 요청할 수 있습니다. 

- 길드에 즉시 가입 설정이 되어있을 경우 해당 길드에 즉시 가입이 됩니다.  
- 길드에 즉시 가입 설정이 off 되어 있을 경우 길드 마스터 혹은 부 길드 마스터 승인 이후 길드에 가입됩니다.  

## Example

### Task 방식

```js
var reqResult = await BackndGuild.Instance.RequestGuildJoinAsync("guildIndate");
```

### Callback 방식

```js
BackndGuild.Instance.RequestGuildJoin("guildIndate", (callback) =>
{
  // 이후 처리
});
```

## ReturnCase

### Success cases

**요청에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**콘솔 설정 조건에 맞지 않는 유저가 길드 가입 요청 시도한 경우**  
statusCode : 403  
errorCode : ForbiddenError  
message : Forbidden applyGuild, 금지된 applyGuild

**존재하지 않는 길드명일 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guild not found, guild을(를) 찾을 수 없습니다  

**이미 가입 요청한 길드에 다시 가입 요청 한 경우**  
statusCode : 409  
errorCode : DuplicatedParameterException  
message : Duplicated alreadyRequestGamer, 중복된 alreadyRequestGamer 입니다

**이미 속해있는 길드가 존재하는 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : JoinedGamer 사전 조건을 만족하지 않습니다.  

**길드원이 이미 100명 이상인 경우**  
statusCode : 429  
errorCode : Too Many Request  
message : guild member count 요청 횟수를 초과하였습니다.  
