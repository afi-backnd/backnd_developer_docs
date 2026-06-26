---
sidebar_label: 굿즈로 리더보드 갱신
description: "굿즈로 리더보드 갱신"
---

# UpdateLeaderboardWithGuildGoods
public Task&lt;RequestResult&gt; **UpdateLeaderboardWithGuildGoodsAsync**(string **leaderboardUuid**, goodsType **goodsType**, GoodsUse **goodsUse**, int **value**);  
public Task&lt;RequestResult&gt; **UpdateLeaderboardWithGuildGoodsAsync**(string **leaderboardUuid**, goodsType **goodsType**, GoodsUse **goodsUse**, long **value**);  
public Task&lt;RequestResult&gt; **UpdateLeaderboardWithGuildGoodsAsync**(string **leaderboardUuid**, goodsType **goodsType**, GoodsUse **goodsUse**, float **value**);  
public Task&lt;RequestResult&gt; **UpdateLeaderboardWithGuildGoodsAsync**(string **leaderboardUuid**, goodsType **goodsType**, GoodsUse **goodsUse**, double **value**);


## 파라미터
| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| leaderboardUuid | string | 갱신할 리더보드의 uuid |
| goodsType | goodsType | 갱신할 길드 굿즈 타입 |
| goodsUse | GoodsUse | 굿즈를 기부/혹은 사용할 것인지에 대한 선택(Give / Spend) |
| value | int / long / float / double | 기부할 값(1 이상 가능) |

## 설명
길드에 굿즈를 기부함과 동시에 리더보드를 갱신합니다.  
* 해당 함수는 [BackndGuild.Instance.DonateGoods](/sdk-docs/backend/base/guild/goods/donate) 함수에 리더보드 갱신 기능이 추가된 함수입니다.  
* [DonateGoods](/sdk-docs/backend/base/guild/goods/donate) 함수 혹은 [UseGoods](/sdk-docs/backend/base/rank/guild/modify/by-using-goods)로 갱신된 굿즈 내역은 리더보드에 반영되지 않습니다.  
> 현재 리더보드에 10000점이 반영되어 있는 상태에서 BackndGuild.Instance.DonateGoods 함수를 이용하여 1000점을 기부한 경우
리더보드에는 10000점, 길드 굿즈에는 11000점이 반영되게 됩니다.  

  > 여기서 다시 UpdateLeaderboardWithGuildGoods 함수를 이용하여 1000점을 기부한 경우 리더보드에는 12000점, 길드 굿즈에도 12000점이 반영되게 됩니다.  

길드 리더보드는 추가 항목을 지원하지 않습니다.  


## Example

### Task 방식
```js
// 굿즈1에 100만큼 굿즈 기부
var reqResult = await BackndLeaderboard.Guild.UpdateLeaderboardWithGuildGoodsAsync
    ("leaderboardUuid", GoodsType.goods1, GoodsUse.Give, 100);

// 굿즈2에 10만큼 굿즈 소모
var reqResult = await BackndLeaderboard.Guild.UpdateLeaderboardWithGuildGoodsAsync
    ("leaderboardUuid", GoodsType.goods1, GoodsUse.Spend, 10);

```

### Callback 방식
```js
BackndLeaderboard.Guild.UpdateLeaderboardWithGuildGoods("leaderboardUuid", GoodsType.goods1, GoodsUse.Spend, 10, callback => {
    // 이후 처리
});
```

## ReturnCase

### Success cases

**갱신에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**uuid가 null 혹은 string.Empty인 경우**  
statusCode : 400  
errorCode : ValidationException  
message : leaderboardUuid is null or string.Empty

**갱신을 시도한 리더보드가 굿즈 리더보드가 아닌 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad table, 잘못된 table 입니다

**value가 0 이하인 경우**  
statusCode : 400  
errorCode : ValidationException  
message : value can't be negative

**[Spend] 길드장이 아닌 유저가 GoodsUse.Spend를 호출한 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden useGoods, 금지된 useGoods

**리더보드를 생성할 때 선택하지 않은 굿즈로 갱신을 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guild rank not found, guild rank을(를) 찾을 수 없습니다

**[Spend] 길드에 보유중인 굿즈의 양보다 많은 양의 굿즈 사용을 시도한 경우**  
statusCode : 412  
errorCode :  PreconditionFailed  
message : inadequateAmount 사전 조건을 만족하지 않습니다.  

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

