---
sidebar_label: "길드 inDate 조회"
description: "GetGuildIndateByGuildNameV3"
---

# GetGuildIndateByGuildNameV3

public BackendReturnObject **GetGuildIndateByGuildNameV3**(string **guildName**);  
public BackendReturnObject **GetGuildIndateByGuildNameV3**(string **guildName**, bool **allGroup**);

## 파라미터

| Value     | Type   | Description                                      | default |
| --------- | ------ | -------------------------------------------------|---------|
| guildName | string | 찾고자 하는 길드의 이름                            |    -    |
| allGroup  | bool   | 길드 검색 범위(false : 같은 그룹/ true : 모든 그룹) | false   |

## 설명

길드명을 통해 해당 길드의 indate를 조회합니다.  
**allGroup** 값에 **false** 를 전달하면 같은 그룹 내에서만 길드 indate 조회가 가능하며,  
**true** 를 전달하면 전체 그룹에서 길드조회가 가능합니다.  
**allGroup** 파라미터의 값을 넣지 않고 사용할 시 기본 false로 동작합니다.

## Example

### 동기

```js
BackendReturnObject bro = Backend.Guild.GetGuildIndateByGuildNameV3("guildName", true);

string guildIndate = bro.GetReturnValuetoJSON()["guildInDate"]["S"].ToString();
```

### 비동기

```js
Backend.Guild.GetGuildIndateByGuildNameV3("guildName" , true, (callback) =>
{
    string guildIndate = callback.GetReturnValuetoJSON()["guildInDate"]["S"].ToString();
    //이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.GetGuildIndateByGuildNameV3, "guildName" , true, (callback) =>
{
    string guildIndate = callback.GetReturnValuetoJSON()["guildInDate"]["S"].ToString();
    //이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

### Error cases

**존재하지 않는 길드명일 경우**, **길드가 같은 그룹이 아닐 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guild not found, guild을(를) 찾을 수 없습니다

**길드명을 입력하지 않은 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : Resource not found, Resource을(를) 찾을 수 없습니다

## GetReturnValuetoJSON

```js
{
  guildInDate: {
    S: "2019-03-04T00:29:41.084Z";
  }
}
```
