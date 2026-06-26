---
sidebar_label: 메타 정보 랭킹갱신
---

# UpdateGuildMetaData
public Task&lt;RequestResult&gt; **UpdateGuildMetaDataAsync**(string **rankUuid**, string **metaKey**, int **value**);  
public Task&lt;RequestResult&gt; **UpdateGuildMetaDataAsync**(string **rankUuid**, string **metaKey**, long **value**);  
public Task&lt;RequestResult&gt; **UpdateGuildMetaDataAsync**(string **rankUuid**, string **metaKey**, float **value**);  
public Task&lt;RequestResult&gt; **UpdateGuildMetaDataAsync**(string **rankUuid**, string **metaKey**, double **value**);  

:::info 기능 개선 안내
기존 랭킹의 성능과 기능을 개선한 **리더보드** 기능을 제공하고 있습니다.  
**콘솔에서 신규 생성은 리더보드만 제공**하므로 해당 기능을 사용해 주세요.
:::

## 파라미터
| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| rankUuid | string | 갱신할 랭킹의 uuid |
| metaKey | string | 갱신할 길드 메타 데이터 키 |
| value | int/long | 갱신할 값 |

rankUuid 값은 아래 방법을 통해 확인할 수 있습니다.  
* uuid 값은 뒤끝 콘솔에서 랭킹을 생성 후 해당 랭킹 정보에서 uuid 값 확인
* [모든 길드 랭킹 정보 조회](/sdk-docs/backend/base/rank/guild/get-all-settings) 함수를 이용하여 uuid 값 확인

## 설명
길드의 메타 데이터를 갱신함과 동시에 랭킹을 갱신합니다.  
* 갱신하려는 메타 데이터가 존재하지 않는 경우 해당 메타 데이터가 새로 생성되게 됩니다.  
* UpdateGuildMetaData 함수는 [BackndGuild.Instance.UpdateMetadata](/sdk-docs/backend/base/guild/metadata/change) 함수에 랭킹 갱신 기능이 추가된 함수입니다.  
* UpdateGuildMetaData 함수를 이용하지 않고 갱신된 데이터는 랭킹에 반영되지 않습니다.  
> **랭킹에 사용하는 메타 데이터의 값(점수로 사용할 값)의 범위는 아래와 같아야 합니다.  
해당 범위를 벗어나는 값은 반올림, 반내림 되는 등 정상적으로 저장되지 않을 수 있고, SDK에서 랭킹을 조회할 때 에러가 발생할 수 있습니다.**  
정수 : -9007199254740992 ~ 9007199254740992(-2^53 ~ 2^53)  
실수 : -3.40282347E+38F ~3.40282347E+38F(float.MinValue ~ float.MaxValue)

길드 랭킹은 추가 항목을 지원하지 않습니다.  

## Example

### Task 방식
```js
var reqResult = await BackndLegacy.URank.Guild.UpdateGuildMetaDataAsync("rankUuid", "갱신할 meta key", 1);
```

### Callback 방식
```js
BackndLegacy.URank.Guild.UpdateGuildMetaData("rankUuid", "갱신할 meta key", 1, callback =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**갱신에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**uuid가 null 혹은 string.empty인 경우**  
statusCode : 400  
errorCode : ValidationException  
message : rankUuid is null or empty

**갱신을 시도한 랭킹이 메타 랭킹이 아닌 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad table, 잘못된 table 입니다

**int, long double, float 외의 값으로 업데이트를 한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad value must number, 잘못된 value must number 입니다

**존재하지 않는 uuid로 랭킹 갱신을 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : rank not found, rank을(를) 찾을 수 없습니다

**길드 랭킹을 생성할 때 등록한 메타 데이터가 아닌 다른 메타 데이터로 랭킹 갱신을 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guild rank not found, guild rank을(를) 찾을 수 없습니다

**가입한 길드가 없는 유저가 랭킹 갱신을 시도한 경우**  
statusCode : 412  
errorCode :  PreconditionFailed  
message : notGuildMember 사전 조건을 만족하지 않습니다.  

**UTC+9 04:00 ~ 05:00 사이에 랭킹 갱신을 시도한 경우**  
statusCode : 428  
errorCode : Precondition Required  
message : Precondition Required ranking is being counted
> 한국 시간으로 새벽 4시 ~ 5시 사이 랭킹 갱신을 시도한 경우

**기간이 끝난 일회성 랭킹의 갱신을 시도한 경우**  
statusCode : 428  
errorCode : Precondition Required  
message : Precondition Required ranking is being counted
