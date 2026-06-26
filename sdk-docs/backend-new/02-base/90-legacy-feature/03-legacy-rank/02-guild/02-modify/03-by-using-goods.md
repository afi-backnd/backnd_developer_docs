---
sidebar_label: 굿즈 사용을 통한 랭킹갱신
---

# UseGuildGoods
public Task&lt;RequestResult&gt; **UseGuildGoodsAsync**(string **rankUuid**, goodsType **goodsType**, int **value**);

:::info 기능 개선 안내
기존 랭킹의 성능과 기능을 개선한 **리더보드** 기능을 제공하고 있습니다.  
**콘솔에서 신규 생성은 리더보드만 제공**하므로 해당 기능을 사용해 주세요.
:::

## 파라미터
| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| rankUuid | string | 갱신할 랭킹의 uuid |
| goodsType | goodsType | 갱신할 길드 굿즈 타입 |
| value | int | 사용할 값 |

rankUuid 값은 아래 방법을 통해 확인할 수 있습니다.  
* uuid 값은 뒤끝 콘솔에서 랭킹을 생성 후 해당 랭킹 정보에서 uuid 값 확인
* [모든 길드 랭킹 정보 조회](/sdk-docs/backend/base/rank/guild/get-all-settings) 함수를 이용하여 uuid 값 확인

## 설명
길드의 굿즈를 사용함과 동시에 랭킹을 갱신합니다.  
* UseGuildGoods 함수는 [BackndGuild.Instance.UseGoods](/sdk-docs/backend/base/guild/guild-master/use-goods) 함수에 랭킹 갱신 기능이 추가된 함수입니다.  
* [DonateGoods](/sdk-docs/backend/base/rank/guild/modify/by-donating-goods) 함수 혹은 UseGuildGoods 함수를 이용하지 않고 갱신된 굿즈 내역은 랭킹에 반영되지 않습니다.  
> 현재 랭킹에 10000점이 반영되어 있는 상태에서 BackndGuild.Instance.UseGoods 함수를 이용하여 1000점을 사용한 경우
랭킹에는 10000점, 길드 굿즈에는 9000점이 반영되게 됩니다.  
  

> 여기서 다시 UseGuildGoods 함수를 이용하여 1000점을 사용한 경우 랭킹에는 8000점, 길드 굿즈에도 8000점이 반영되게 됩니다.  

길드 굿즈의 사용은 길드장만이 할 수 있습니다.  

길드 랭킹은 추가항목을 지원하지 않습니다.  

## Example

### Task 방식
```js
var reqResult = await BackndLegacy.URank.Guild.UseGuildGoodsAsync("rankUuid", GoodsType.goods1, 1);
```

### Callback 방식
```js
BackndLegacy.URank.Guild.UseGuildGoods("rankUuid", GoodsType.goods1, 1, callback =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**랭킹 갱신에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**uuid가 null 혹은 string.Empty인 경우**  
statusCode : 400  
errorCode : ValidationException  
message : rankUuid is null or empty

**갱신을 시도한 랭킹이 굿즈 랭킹이 아닌 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad table, 잘못된 table 입니다

**value가 0 이하인 경우**  
statusCode : 400  
errorCode : PreconditionFailed  
message : value only allow more than 0

**길드장이 아닌 유저가 UseGuildGoods 함수를 호출한 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden useGoods, 금지된 useGoods

**랭킹을 생성할 때 선택하지 않은 굿즈로 갱신을 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guild rank not found, guild rank을(를) 찾을 수 없습니다

**길드에 보유중인 굿즈의 양보다 많은 양의 굿즈 사용을 시도한 경우**  
statusCode : 412  
errorCode :  PreconditionFailed  
message : inadequateAmount 사전 조건을 만족하지 않습니다.  

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
