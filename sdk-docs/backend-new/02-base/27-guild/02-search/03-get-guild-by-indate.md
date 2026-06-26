---
sidebar_label: 길드 inDate 조회
---

# GetGuildInDate

public Task< GetGuildInDateResult > **GetGuildInDateAsync**(string **guildName**);  
public Task< GetGuildInDateResult > **GetGuildInDateAsync**(string **guildName**, bool **allGroup**);

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

### Task 방식

```js
var reqResult = await BackndGuild.Instance.GetGuildInDateAsync("guildName", true);
string guildIndate = reqResult.GetGuildInDate();
```

### Callback 방식

```js
BackndGuild.Instance.GetGuildInDate("guildName" , true, (callback) =>
{
    string guildIndate = reqResult.GetGuildInDate();
    //이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

### Error cases

**존재하지 않는 길드명일 경우**, **길드가 같은 그룹이 아닐 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guild not found, guild을(를) 찾을 수 없습니다

**길드명을 입력하지 않은 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : Resource not found, Resource을(를) 찾을 수 없습니다

## ReturnValueJson

```js
{
  guildInDate: {
    S: "2019-03-04T00:29:41.084Z";
  }
}
```
