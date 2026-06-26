---
draft: true
unlisted: true
---

# 랭킹 추가 항목 사용법

:::info 기능 개선 안내
기존 랭킹의 성능과 기능을 개선한 **리더보드** 기능을 제공하고 있습니다.  
**콘솔에서 신규 생성은 리더보드만 제공**하므로 해당 기능을 사용해 주세요.
:::

## 개요

뒤끝 랭킹에서는 랭킹에서 기본적으로 제공되는 score이외에도 데이터를 추가할 수 있는 추가 항목값이 존재합니다.  

해당 값에는 number나 string등 게임 정보 관리에 필요한 모든 데이터 타입이 들어갈 수 있지만 해당 데이터의 크기는 256byte 미만으로 제한이 되어 있어 실제 개발시에는 주의가 필요합니다.  

연산되는 byte수는 다음과 같습니다.  

- 한글을 포함한 외국 문자 : 2byte(추가 항목이 한글로만 구성되어 있을 경우 최대 32자까지 입력가능합니다.)
- 영어, 숫자, 특수문자등 컴퓨터 지원 문자 : 1byte

> 추가 항목에 '마법사'라는 데이터가 들어갈 경우 byte는 6byte입니다.  
> 추가 항목에 '10000'라는 데이터가 들어갈 경우 byte는 5byte입니다.  
> 추가 항목에 '2022-11-10T09:39:56.850Z'라는 데이터가 들어갈 경우 byte는 24byte입니다.  

따라서 추가 항목에 나타낼 데이터의 갯수 및 데이터의 크기에 따라 데이터 저장 방식이 변경될 수 있습니다.  

- 추가 항목에 표현할 데이터의 갯수가 1개일 경우
- 추가 항목에 표현할 데이터의 갯수가 2개 이상일 경우

## 추가 항목에 표현할 데이터의 갯수가 1개일 경우

데이터의 값이 256byte미만이고 하나일 경우에는 필요한 데이터를 변경없이 그대로 사용할 수 있습니다.  
랭킹에서 기본적으로 제공하는 스코어외에도 레벨, 전투력등 **하나**만 표시하고 싶을 경우 구현할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/unity/rank/extravalue/rank-extra1.png"/>

아래 코드는 레벨 데이터 하나만 추가 항목에 저장하는 코드 입니다.  

### 데이터 저장 형식

```js
{
  extraData: 15;
}
```

### 데이터 등록 코드

```js
Param rankParam = new Param();

// userData는 임의의 클래스이며 userData.Score는 number형 데이터입니다.  
rankParam.Add("score", userData.Score);

// userData.Level은 number형 데이터입니다.  
rankParam.Add("extraData", userData.Level);

Backend.URank.User.UpdateUserScore("aa6609c0-****-****-****-ffd79d523912", "DAILY_USER_RANK", rankDataIndate, rankParam);

```

### 데이터 조회

```js
var bro = Backend.URank.User.GetRankList("aa6609c0-****-****-****-ffd79d523912");

foreach(LitJson.JsonData json in bro.FlattenRows()) {
    int level = int.Parse(json["extraData"].ToString());
}

```

## 추가항목에 저장할 데이터가 2개 이상일 경우

랭킹에서 기본으로 제공하는 스코어 외에도 레벨, 전투력등을 **모두** 표시하고 싶을 때 사용합니다.  

추가항목에는 string 값에 두 데이터를 임의의 구별자(쉼표, 슬래쉬, 특수문자등)로 나누어 Param에 저장합니다.  
데이터를 나열하여 저장하는 순서와 서버에서 데이터를 불러와 파싱하는 순서는 클라이언트내에서 구현 되어 있어야합니다.  

다만 아래 예시와 같이 **클래스 형태 혹은 Dictionary 형태의 데이터를 그대로 추가항목에 등록**하여 사용할 경우, 컬럼명이 자동으로 추가되며 2~3개의 데이터여도 256byte 기준을 초과할 가능성이 높습니다.  

### 잘못된 데이터 저장 형식 1 - class를 Json으로 변환한 형태

```js
{
	"extraData": "{\"atk\":99999,\"def\":5,\"currentStage\":0,\"currentQuestClear\":0,\"clearTime\":0.0,\"nickname\":\"유저1\"}"
}
```

### 잘못된 데이터 저장 형식 2 - class를 저장한 형태

```js
{
	"userinfo": {
		"atk": 99999,
		"def": 5,
		"currentStage": 0,
		"currentQuestClear": 0,
		"clearTime": 0.0,
		"nickname": "유저1"
	}
}
```

### 잘못된 데이터 저장 함수

```js
public class UserInfo {
	public int atk = 99999;
	public int def = 5;
	public int currentStage = 0;
	public int currentQuestClear = 0;
	public int clearTime = 0;
}
public void InsertRank() {
	Param param = new Param();
	param.Add("score",1);

	UserInfo userInfo = new UserInfo();

	// case 1. 클래스 형태로 등록
	param.Add("extraData", userInfo);

	// case 2. json으로 파싱하여 등록
	var userInfoString = JsonMapper.ToJson(userInfo);
	param.Add("extraData", userInfoString);

	Backend.URank.User.UpdateUserScore("aa6609c0****-ffd79d523912", "tableName", "rowInDate", param);
}
```

### 추천 데이터 저장 방식

따라서 추가항목에 여러 데이터를 등록하고자 할 경우에는 컬럼명을 제외한 데이터로만 등록이 되어야 하며,
해당 데이터들은 클라이언트에서 데이터 등록 시 추가되는 순서와 데이터 조회 시 파싱되는 순서가 동일하도록 로직을 구성해야합니다.  

아래 방식은 위 데이터의 컬럼명을 제외하고 임의의 구분자(| 특수문자)로 구분하며 저장한 방식입니다.  

### 데이터 표기 예시

```js
{
  // 레벨|스테이지 정보
  extraData: "99999|5|0|0|0.0|유저1";
}
```

### 데이터 저장

```js
Param rankParam = new Param();

rankParam.Add("score", userData.Score);

// userData.Level은 int형, userData.StageInfo는 string형입니다.  
// 두 값은 | 로 인해 구분되어 있습니다.  
// 순서는 level, stageInfo순입니다.  
rankParam.Add("extraData", $"{userData.atk}|{userData.def}|{userData.currentStage}|{userData.currentQuestClear}|{userData.clearTime}|{userData.nickname}");

Backend.URank.User.UpdateUserScore("aa6609c0-****-****-****-ffd79d523912", "DAILY_USER_RANK", rankDataIndate, rankParam);

```

### 데이터 조회

```js
var bro = Backend.URank.User.GetRankList("aa6609c0-****-****-****-ffd79d523912");

foreach(LitJson.JsonData json in bro.FlattenRows()) {
    // 데이터 등록 시 구분자였던 | 를 통해 string 데이터를 분리합니다.  
    string[] extraData = json["extraData"].ToString().Split("|");

    UserInfo userInfo = new UserInfo();

    userInfo.atk = int.Parse(extraData[0].ToString());
    userInfo.def = int.Parse(extraData[1].ToString());
    userInfo.currentStage = int.Parse(extraData[2].ToString());
    userInfo.currentQuestClear = int.Parse(extraData[3].ToString());
    userInfo.clearTime= float.Parse(extraData[4].ToString());
    userInfo.nickname = extraData[5].ToString();

}

```

**아래는 데이터를 2개 이상 사용한 랭킹 UI 표기 예시입니다.**  

<img src="https://developer.thebackend.io/static/img/unity/rank/extravalue/rank-extra2.png"/>
