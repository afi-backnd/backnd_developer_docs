---
sidebar_label: "길드 생성"
description: "CreateGuildV4"
---

# CreateGuildV4

public BackendReturnObject **CreateGuildV4**(string **guildName**, int **goodsCount**);  
public BackendReturnObject **CreateGuildV4**(string **guildName**, int **goodsCount**, CountryCode **countryCode**);  
public BackendReturnObject **CreateGuildV4**(string **guildName**, int **goodsCount**, Param **param**);  
public BackendReturnObject **CreateGuildV4**(string **guildName**, int **goodsCount**, Param **param**, CountryCode **countryCode**);  
public BackendReturnObject **CreateGuildV4**(string **guildName**, int **goodsCount**, bool **isOpenGuild**);  
public BackendReturnObject **CreateGuildV4**(string **guildName**, int **goodsCount**, Param **param**, bool **isOpenGuild**);  
public BackendReturnObject **CreateGuildV4**(string **guildName**, int **goodsCount**, Param **param**, bool **isOpenGuild**, CountryCode **countryCode**);

## 파라미터

| Value       | Type        | Description                                               |     default      |
| :---------- | :---------- | :-------------------------------------------------------- | :--------------: |
| guildName   | string      | 길드 이름                                                 |        -         |
| goodsCount  | int         | 사용할 길드 재화의 개수(1~10)(길드 생성 이후 수정 불가) |        -         |
| param       | Param       | (Optional) 길드에 관한 메타 정보                          |        -         |
| isOpenGuild | bool        | (Optional) 오픈 길드 생성 여부                            |      false       |
| countryCode | CountryCode | (Optional) 국가 정보                                      | CountryCode.NONE |

## 설명

길드를 생성합니다.  

길드 생성은 뒤끝 콘솔에 설정된 길드 생성 조건을 만족하는 회원만 사용할 수 있습니다. 길드 생성 조건은 **뒤끝 콘솔 > 뒤끝베이스 > 길드 관리 > 설정**에 지정된 테이블과 파티션의 최신 값을 기준으로 판단합니다. 콘솔에 등록된 길드 생성 조건이 없으면 모든 회원이 길드를 생성할 수 있습니다.  

:::caution 주의
* 길드명은 2자 이상 16자 이하로 입력할 수 있으며, 한글/영문/숫자만 사용할 수 있습니다. 특수문자는 사용할 수 없습니다.  
* 길드명은 숫자로 시작할 수 없으며, 공백은 사용할 수 있지만 연속으로 입력할 수 없습니다. 또한 공백으로 시작하거나 끝날 수 없습니다.  
* 길드 굿즈 종류 수는 생성 시에만 지정할 수 있으며, 생성 이후에는 변경할 수 없습니다.  
* 메타 정보는 원하는 값을 저장할 수 있으며, `ModifyGuild(Param)`을 통해 추가/수정할 수 있습니다.  
* 국가 코드는 길드 생성 이후 길드 마스터가 <a href="/sdk-docs/backend/base/guild/guild-master/change-country-code" target="_blank">UpdateCountryCode</a>를 사용하여 추가/수정할 수 있습니다.  
* CountryCode에 대한 정보는 <a href="/sdk-docs/backend/base/country-code/all-codes" target="_blank">이곳</a>을 참고해 주세요.  
* 길드 국가 코드에는 유저들의 국가 코드 풀에 포함된 국가만 등록해야 합니다.  
* 유저들의 국가 코드 풀에 포함되지 않은 길드 국가 코드는 콘솔에 표시되지 않습니다.  
:::

## Example

### 동기

```js
// 메타 정보가 없는 경우
Backend.Guild.CreateGuildV4("guildName", 2);

Backend.Guild.CreateGuildV4("guildName", 2, CountryCode.SouthKorea);

// 오픈 길드로 생성
Backend.Guild.CreateGuildV4("guildName", 2, true);

// param에 원하는 메타 정보를 입력(Optional)
Param param = new Param();
param.Add("level",0);
param.Add("buf",1);

Backend.Guild.CreateGuildV4("guildName", 2, param);

Backend.Guild.CreateGuildV4("guildName", 2, param, true);

Backend.Guild.CreateGuildV4("guildName", 2, param, true, CountryCode.SouthKorea);
```

### 비동기

```js
// 메타 정보가 없는 경우
Backend.Guild.CreateGuildV4("guildName", 2, (callback) =>
{
    // 이후 처리
});

Backend.Guild.CreateGuildV4("guildName", 2, true, (callback) =>
{
    // 이후 처리
});

Backend.Guild.CreateGuildV4("guildName", 2, CountryCode.SouthKorea, (callback) =>
{
    // 이후 처리
});

// 메타 정보가 있는 경우
Param param = new Param();
param.Add("level",0);
param.Add("buf",1);

Backend.Guild.CreateGuildV4("guildName", 2, param, (callback) =>
{
    // 이후 처리
});

Backend.Guild.CreateGuildV4("guildName", 2, param, true, (callback) =>
{
    // 이후 처리
});

Backend.Guild.CreateGuildV4("guildName", 2, param, true, CountryCode.SouthKorea, (callback) =>
{
    // 이후 처리
});
```

### SendQueue

```js
// 메타 정보가 없는 경우
SendQueue.Enqueue(Backend.Guild.CreateGuildV4, "guildName", 2, (callback) =>
{
    // 이후 처리
});

SendQueue.Enqueue(Backend.Guild.CreateGuildV4, "guildName", 2, true, (callback) =>
{
    // 이후 처리
});

// 메타 정보가 있는 경우
Param param = new Param();
param.Add("level",0);
param.Add("buf",1);

SendQueue.Enqueue(Backend.Guild.CreateGuildV4, "guildName", 2, param, true, CountryCode.SouthKorea, (callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**생성에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**goodsCount가 10 이상인 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad goodsCount is too big, 잘못된 goodsCount is too big 입니다

**goodsCount가 0이하인 경우**  
statusCode : 400  
errorCode : UndefinedParameterException  
message : undefined goodsCount must be more then 1, goodsCount must be more then 1을(를) 확인할 수 없습니다

**콘솔 설정 조건에 맞지 않는 유저가 길드 생성 시도한 경우**  
statusCode : 403  
errorCode : ForbiddenError  
message : Forbidden createGuild, 금지된 createGuild

**중복된 길드명으로 생성 시도한 경우**  
statusCode : 409  
errorCode : DuplicatedParameterException  
message : Duplicated guildName, 중복된 guildName 입니다

**길드명 조건이 맞지 않는 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : guildName 사전 조건을 만족하지 않습니다  

**이미 속해있는 길드가 존재하는 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : JoinedGamer 사전 조건을 만족하지 않습니다  
