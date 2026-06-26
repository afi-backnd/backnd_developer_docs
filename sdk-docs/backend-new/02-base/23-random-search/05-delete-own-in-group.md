---
sidebar_label: 랜덤 그룹에 나의 데이터 삭제
---

# DeleteRandomEntry

public Task< RequestResult > **DeleteRandomEntryAsync**(RandomType **randomType**, string **uuid**);

## 설명

랜덤 조회 그룹에 존재하는 내 데이터를 삭제합니다.  
길드의 경우, 모든 길드원들이 해당 함수를 실행할 수 있습니다.  

해당 그룹에 자신의 데이터가 존재하지 않아도, 삭제 시에는 에러없이 성공으로 리턴됩니다.  

## 파라미터

| Value      | Type       | Description                                                          |
| ---------- | ---------- | -------------------------------------------------------------------- |
| randomType | RandomType | 랜덤 조회 유형  
(RandomType.User는 유저, RandomType.Guild는 길드) |
| uuid       | string     | 랜덤 조회의 uuid                                                     |

## Example

### Task 방식

```js
var reqResult = await BackndRandom.Instance.DeleteRandomEntryAsync(RandomType.User, "9cd283c0-00c6-11ed-93ca-27f1ba8e440c");
```

### Callback 방식

```js
BackndRandom.Instance.DeleteRandomEntry(RandomType.User, "9cd283c0-00c6-11ed-93ca-27f1ba8e440c", (callback) =>
{
  // 이후 처리
});
```

## ReturnCase

### Success cases

**데이터 삭제가 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**입력한 uuid가 null 혹은 string.Empty일 경우**  
statusCode : 400  
errorCode : ValidationException  
message : uuid is null or empty

**랜덤 조회의 유형이 다를 경우(유저 유형인데 RandomType.Guild으로 지정했을 경우)**  
statusCode : 400  
errorCode : BadParameterException  
message : bad type is not match, 잘못된 type is not match 입니다

**(랜덤 조회 유형 : 길드) 길드에 가입되어 있지 않을 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guild not found, guild을(를) 찾을 수 없습니다

**존재하지 않는 uuid일 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : randomPool not found, randomPool을(를) 찾을 수 없습니다
