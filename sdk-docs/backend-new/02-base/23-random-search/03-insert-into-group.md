---
sidebar_label: 랜덤 그룹에서 데이터 삽입
description: "랜덤 그룹에서 데이터 삽입"
---

# SetRandomEntry

public Task< RequestResult > **SetRandomEntryAsync**(RandomType **randomType**, string **uuid**, int **value**);  
public Task< RequestResult > **SetRandomEntryAsync**(RandomType **randomType**, string **uuid**, long **value**);  
public Task< RequestResult > **SetRandomEntryAsync**(RandomType **randomType**, string **uuid**, float **value**);  
public Task< RequestResult > **SetRandomEntryAsync**(RandomType **randomType**, string **uuid**, double **value**);

## 설명

랜덤 조회 그룹에 데이터를 삽입합니다.  
하나의 랜덤 조회 그룹에서 각 유저당 하나의 데이터만 존재할 수 있습니다.  

스코어 외에 추가적인 값의 삽입은 불가능합니다.  

길드 유형 랜덤 조회의 경우, 길드에 가입된 유저만 삽입이 가능하며 미가입 유저가 삽입을 시도하면 에러가 발생합니다.  

## 파라미터

| Value      | Type                  | Description                                                                              |
| ---------- | --------------------- | ---------------------------------------------------------------------------------------- |
| randomType | RandomType            | 랜덤 조회 유형(RandomType.User는 유저, RandomType.Guild는 길드)                     |
| uuid       | string                | 랜덤 조회의 uuid                                                                         |
| value      | int/long/float/double | 삽입하고자 하는 값 (소수점, 음수도 가능)(long.MaxValue 이상일 경우 조회시 에러 발생) |

## Example

### Task 방식

```js
var reqRequest = await BackndRandom.Instance.SetRandomEntryAsync(RandomType.User, "9cd283c0-00c6-11ed-93ca-27f1ba8e440c", 10);
```

### Callback 방식

```js
BackndRandom.Instance.SetRandomEntry(RandomType.User, "9cd283c0-00c6-11ed-93ca-27f1ba8e440c", 10, (callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**데이터 삽입에 성공한 경우**  
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
