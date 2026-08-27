---
sidebar_label: "길드 마스터 자동 교체 신청"
description: "ClaimGuildMaster"
---

# ClaimGuildMaster

public BackendReturnObject **ClaimGuildMaster**();

## 설명

길드 마스터가 장기 미접속 상태일 때, 길드원이 길드 마스터 권한을 직접 가져오는 함수입니다.  
**함수를 호출한 게이머가 새 길드 마스터가 되며**, 기존 길드 마스터는 길드원으로 강등됩니다. 위임 대상을 지정하는 파라미터는 없습니다.

호출자가 부 길드 마스터였던 경우 부 길드 마스터 목록에서 제거되고 길드 마스터로 승격됩니다.

:::danger 사용 전 확인
* 이 기능은 **뒤끝 콘솔 > 뒤끝베이스 > 길드 관리 > 설정**에서 **길드 마스터 자동 교체 기준 일수**(1~365일)를 설정한 프로젝트에서만 동작합니다. 설정하지 않은 프로젝트에서 호출하면 statusCode 412가 반환됩니다.
* 기준 일수를 설정하지 않은 프로젝트에서는 길드 조회 응답에 `masterLastLogin`, `inactivedMaster` 키가 포함되지 않습니다.
* 길드 마스터가 직접 위임 대상을 지정하는 [NominateMasterV3](./02-grant-master-role.md)와는 별개의 기능이며, 두 기능은 함께 사용할 수 있습니다.
:::

## 호출 조건

호출 전 [GetMyGuildInfoV4](../02-search/02-get-my-guild.md)로 현재 길드 마스터가 비활성 상태인지 먼저 확인할 수 있습니다.

| 조건 | 확인 방법 |
| --- | --- |
| 호출자가 길드에 소속되어 있어야 합니다. | 미소속이면 statusCode 412 |
| 호출자가 현재 길드 마스터가 아니어야 합니다. | 이미 길드 마스터면 statusCode 403 |
| 프로젝트에 길드 마스터 자동 교체 기준 일수가 설정되어 있어야 합니다. | 미설정이면 statusCode 412 |
| 현재 길드 마스터의 미접속 기간이 기준 일수를 초과해야 합니다. | `inactivedMaster`가 `true` |

`inactivedMaster` 값의 의미는 아래와 같습니다. 응답 원문(JSON)의 키 유무와 SDK 파싱 결과가 서로 다르게 보이므로 함께 확인해 주세요.

| 응답 원문 | 의미 | `GetReturnValueByGuildItem()` 결과 |
| --- | --- | --- |
| `inactivedMaster: true` | 길드 마스터가 기준 일수를 초과해 접속하지 않았습니다. 로그인 기록이 없는 구 계정도 비활성으로 처리됩니다. | `hasInactivedMaster == true`, `inactivedMaster == true` |
| `inactivedMaster: false` | 길드 마스터가 기준 일수 내에 접속했습니다. | `hasInactivedMaster == true`, `inactivedMaster == false` |
| `inactivedMaster: null` | 서버가 길드 마스터의 접속 기록 조회에 실패해 판단할 수 없습니다.(길드 리스트 조회에서만 발생) | `inactivedMaster == null` |
| 키 없음 | 프로젝트에 길드 마스터 자동 교체 기준 일수가 설정되지 않았습니다. | `hasInactivedMaster == false`, `inactivedMaster == null` |

`inactivedMaster`는 `bool?`이므로 **기준 일수 미설정과 조회 실패가 SDK에서는 모두 `null`로 보입니다.** 두 경우를 구분하려면 `hasInactivedMaster`를 함께 확인해야 합니다. `masterLastLogin`도 마찬가지로 `hasMasterLastLogin`으로 구분합니다.

## Example

### 동기

```js
Backend.Guild.ClaimGuildMaster();
```

### 비동기

```js
Backend.Guild.ClaimGuildMaster((callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.ClaimGuildMaster, (callback) => {
  // 이후 처리
});
```

`Backend.NewFunctions.Guild.ClaimGuildMaster`로도 동일하게 호출할 수 있습니다.

```js
Backend.NewFunctions.Guild.ClaimGuildMaster();

Backend.NewFunctions.Guild.ClaimGuildMaster((callback) => {
  // 이후 처리
});
```

## ReturnCase

### Success cases

**길드 마스터 교체에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**호출자가 이미 길드 마스터인 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden alreadyMaster, 금지된 alreadyMaster

**호출자의 길드원 정보가 존재하지 않는 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden guildMember, 금지된 guildMember

**기존 길드 마스터의 길드원 정보가 존재하지 않는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guildMember not found, guildMember을(를) 찾을 수 없습니다

**다른 길드원이 먼저 길드 마스터 교체를 완료한 경우**  
statusCode : 409  
errorCode : DuplicatedParameterException  
message : Duplicated alreadyClaimed, 중복된 alreadyClaimed 입니다

**길드에 속하지 않은 유저가 호출한 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : subscribed guild 사전 조건을 만족하지 않습니다.

**프로젝트에 길드 마스터 자동 교체 기준 일수가 설정되지 않은 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : masterInactivePeriod not configured 사전 조건을 만족하지 않습니다.

**현재 길드 마스터가 활성 상태인 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : master is active 사전 조건을 만족하지 않습니다.

## Sample Code

```js
public void ClaimGuildMaster()
{
    var guildInfo = Backend.NewFunctions.Guild.GetMyGuildInfoV4();

    if(!guildInfo.IsSuccess())
        return;

    var guildItem = guildInfo.GetReturnValueByGuildItem();

    // 기준 일수가 설정되지 않은 프로젝트에서는 키 자체가 내려오지 않습니다.
    if(!guildItem.hasInactivedMaster || guildItem.inactivedMaster != true)
    {
        Debug.Log("길드 마스터가 활성 상태이므로 길드 마스터 교체를 신청할 수 없습니다.");
        return;
    }

    var bro = Backend.Guild.ClaimGuildMaster();

    if(!bro.IsSuccess())
    {
        Debug.LogError(bro.ToString());
        return;
    }

    Debug.Log("길드 마스터 교체에 성공했습니다.");
}
```
