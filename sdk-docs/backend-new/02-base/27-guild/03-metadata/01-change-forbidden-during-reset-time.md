---
sidebar_label: "[New] 길드 메타 데이터 변경(리더 보드 초기화 시간 갱신 불가)"
description: "[New] 길드 메타 데이터 변경(리더 보드 초기화 시간 갱신 불가)"
---

# UpdateMetadata

public Task< RequestResult > **UpdateMetadataAsync**(Param **param**);

:::info 안내
해당 함수는 랭킹 정산 에러를 방지하기 위해 리더보드 정산 시간을 제외한 시간에만 길드 메타 정보를 변경하는 함수입니다.  
:::

## 파라미터

| Value | Type  | Description                        |
| ----- | ----- | ---------------------------------- |
| param | Param | 길드에 관하여 생성/수정할 메타 정보 |

## 설명

길드의 메타 정보를 변경합니다. 길드명은 변경 불가합니다.  
리더보드에 초기화 주기가 설정 되어 있다면 리더보드 초기화 시간에는 변경이 불가능하며, 요청 시 428에러가 리턴됩니다.  

## Example

### Task 방식

```js
Param param = new Param();
param.Add("buf", 2);

var reqResult = await BackndGuild.Instance.UpdateMetadataAsync(param);
```

### Callback 방식

```js
Param param = new Param();
param.Add("buf",2);

BackndGuild.Instance.UpdateMetadata(param, (callback) =>
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

**리더보드 정산 시간 사이에 호출한 경우**  
statusCode : 428  
errorCode : Precondition Required  
message : Precondition Required ranking is being counted
