---
sidebar_label: 메타 정보로 리더보드 갱신
---

# UpdateLeaderboardWithGuildMeta
public Task&lt;RequestResult&gt; **UpdateLeaderboardWithGuildMetaAsync**(string **leaderboardUuid**, string **metaKey**, int **value**);  
public Task&lt;RequestResult&gt; **UpdateLeaderboardWithGuildMetaAsync**(string **leaderboardUuid**, string **metaKey**, long **value**);  
public Task&lt;RequestResult&gt; **UpdateLeaderboardWithGuildMetaAsync**(string **leaderboardUuid**, string **metaKey**, float **value**);  
public Task&lt;RequestResult&gt; **UpdateLeaderboardWithGuildMetaAsync**(string **leaderboardUuid**, string **metaKey**, double **value**);  


## 파라미터
| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| leaderboardUuid | string | 갱신할 리더보드의 uuid |
| metaKey | string | 갱신할 길드 메타 데이터 키 |
| value | int/long | 갱신할 값 |

## 설명
길드의 메타 데이터를 갱신함과 동시에 리더보드를 갱신합니다.  
* 갱신하려는 메타 데이터가 존재하지 않는 경우 해당 메타 데이터가 새로 생성되게 됩니다.  
* 해당 함수는 [BackndGuild.Instance.UpdateMetadata](/sdk-docs/backend/base/guild/metadata/change) 함수에 리더보드 갱신 기능이 추가된 함수입니다.  
* 해당 함수를 이용하지 않고 갱신된 데이터는 리더보드에 반영되지 않습니다.  
> **리더보드에 사용하는 메타 데이터의 값(점수로 사용할 값)의 범위는 아래와 같아야 합니다.  
해당 범위를 벗어나는 값은 반올림, 반내림 되는 등 정상적으로 저장되지 않을 수 있고, SDK에서 리더보드를 조회할 때 에러가 발생할 수 있습니다.**  
정수 : -9007199254740992 ~ 9007199254740992(-2^53 ~ 2^53)  
실수 : -3.40282347E+38F ~3.40282347E+38F(float.MinValue ~ float.MaxValue)

길드 리더보드는 추가 항목을 지원하지 않습니다.  

## Example

### Task 방식
```js
var reqResult = await BackndLeaderboard.Guild.UpdateLeaderboardWithGuildMetaAsync("leaderboardUuid", "갱신할 meta key", 1);
```

### Callback 방식
```js
BackndLeaderboard.Guild.UpdateLeaderboardWithGuildMeta("leaderboardUuid", "갱신할 meta key", 1, callback =>
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
message : leaderboardUuid is null or string.Empty

**갱신을 시도한 리더보드가 메타 리더보드가 아닌 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad table, 잘못된 table 입니다

**길드 리더보드를 생성할 때 등록한 메타 데이터가 아닌 다른 메타 데이터로 리더보드 갱신을 시도한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad table, 잘못된 table 입니다

**int, long double, float 외의 값으로 업데이트를 한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad value must number, 잘못된 value must number 입니다

**존재하지 않는 uuid로 리더보드 갱신을 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : leaderboard not found, leaderboard을(를) 찾을 수 없습니다

**가입한 길드가 없는 유저가 리더보드 갱신을 시도한 경우**  
statusCode : 412  
errorCode :  PreconditionFailed  
message : notGuildMember 사전 조건을 만족하지 않습니다.  

**초기화 시간에 리더보드 갱신을 시도한 경우**  
statusCode : 428  
errorCode : Precondition Required  
message : Precondition Required ranking is being counted

**기간이 끝난 일회성 리더보드의 갱신을 시도한 경우**  
statusCode : 428  
errorCode : Precondition Required  
message : Precondition Required ranking is being counted
