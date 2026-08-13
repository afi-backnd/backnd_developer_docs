---
sidebar_position: "2"
description: "퍼널 분석"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 퍼널 분석

뒤끝 함수 [InsertLog](/sdk-docs/backend/base/game-log/insert)로 생성한 로그들의 데이터를 이용하여 퍼널을 분석할 수 있습니다.  

퍼널 분석은 유저가 특정 이벤트까지 도달하는 과정을 '깔때기(Funnel)'에 비유하여 도달률 및 전환율을 분석하는 방법론입니다.  
개발자는 유저가 특정 이벤트를 달성할 때마다 로그를 생성하고, 퍼널 분석 기능을 통해 이들을 집계하여 유저들이 이탈하는 주요 구간을 쉽게 발견할 수 있습니다.  

본 가이드에서는 게임 초기 튜토리얼에서의 퍼널 분석이라는 가상의 상황을 가정하여 퍼널 분석 기능의 사용법을 안내합니다.  

### 1. 로그 생성

1-1. 개발자는 `TutorialLog`라는 행동유형의 로그에서 `tutorial_complete`라는 데이터를 정의합니다.  

<ConsoleLinkButton text="로그 관리 바로가기" menu="baseLog/log" feature="로그" title="퍼널 분석" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/로그관리/뒤끝베이스--로그-관리---퍼널-분석.png" />

1-2. 게임 초기에 유저들은 "Tutorial #1"부터 "Tutorial #20"까지 총 20개의 튜토리얼을 진행합니다.  
N번째 튜토리얼을 완수할 때마다 `tutorial_complete`의 값이 "Tutorial #N"이라는 로그를 생성하며, 이때까지 해당 유저에 대한 로그는 총 N개입니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/로그관리/뒤끝베이스--로그-관리---퍼널-분석-2.png" />

해당 로그는 Unity 상에서 아래와 같이 등록할 수 있습니다.  

```js
public void InsertLogTutorialComplete(int stage)
{
    string logContent = "Tutorial #" + stage;

    Param param = new Param();
    param.Add("tutorial_complete", logContent);

    Backend.GameLog.InsertLog("TutorialLog", param);
}
```

이를 다운로드하여 CSV 형태로 확인하면 아래와 같습니다.  

<img src="https://developer.thebackend.io/static/img/console/gamelog/funnel/02-데이터-형태.png" />

:::danger 주의사항
데이터 형태가 본 문서에 설명된 형태와 일치하지 않으면 정확하게 집계가 되지 않습니다.  
데이터는 Integer 혹은 string 등 복합 데이터가 아닌 단일 데이터 형식이어야 합니다.  
:::
### 2. 퍼널 조회

2-1. 행동 유형을 `TutorialLog`로 설정한 뒤 컬럼명을 `tutorial_complete`로 설정합니다.  
같은 행동 유형에 다른 이름으로 데이터를 정의했다면 컬럼명을 바꾸어서 조회할 수 있습니다.  
기간은 현재로부터 최근 1일, 7일, 15일을 선택할 수 있으며, "직접 입력"을 통해 원하는 기간을 구체적으로 설정할 수 있습니다.  

<ConsoleLinkButton text="퍼널 분석 바로가기" menu="baseLog/funnel" feature="로그" title="퍼널 분석" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/로그관리/뒤끝베이스--로그-관리---퍼널-분석-3.png" />

2-2. 개발자는 총 20개의 튜토리얼 중 10번 튜토리얼까지 도달한 사람의 비율이 궁금할 수 있습니다.  
이 경우 첫 번째 퍼널을 "Tutorial #1"로, 두 번째 퍼널을 "Tutorial #10"으로 설정합니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/로그관리/뒤끝베이스--로그-관리---퍼널-분석---퍼널-경로.png" />


:::info
퍼널은 해당 데이터(컬럼)의 Unique Value들을 집계하며, 실시간으로 연동되지 않습니다.  
업데이트로 인해 새로운 값의 데이터가 로그로 생성될 경우 "새로 고침" 버튼으로 갱신해야 합니다. 이 경우 DB 요금이 발생할 수 있습니다.  
:::


### 3. 퍼널 분석

3-1. "분석하기" 버튼을 클릭하여 분석 결과를 확인할 수 있습니다.  

퍼널 이름 아래에 출력되는 수치는 **첫 번째 퍼널의 로그 개수 대비 해당 퍼널의 로그 개수 비율입니다.**  
"Tutorial #1" 로그를 생성한 유저가 10,000명일 경우 "Tutorial #10" 로그를 생성한 유저는 2,148명입니다.  

이 가상의 데이터에서는 처음 10개의 튜토리얼을 거치면서 약 21%의 유저만이 잔존했습니다. 구체적인 이탈 원인을 찾기 위해 퍼널을 변경하면서 분석할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/로그관리/뒤끝베이스--로그-관리---퍼널-분석---분석.png" />

3-2. 이번에는 퍼널을 "Tutorial #1"부터 "Tutorial #5"까지 5개를 순서대로 설정한 뒤 분석합니다.  

전환율은 **이전 단계의 퍼널 로그를 생성한 유저 중 다음 단계의 로그를 생성한 유저의 비율입니다.**  
2번 튜토리얼을 통과한 유저 중 3번 튜토리얼을 통과한 유저는 81%임을 확인할 수 있습니다.  
이로부터 3번 튜토리얼에 유저 이탈 원인이 있음을 발견할 수 있습니다.  

그러나 이것만으로는 10번 튜토리얼의 잔존율이 왜 21%밖에 안되는지 설명할 수 없습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/로그관리/뒤끝베이스--로그-관리---퍼널-분석---분석2.png" />

3-3. 퍼널의 범위를 확대하여 추가적인 분석을 진행합니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/로그관리/뒤끝베이스--로그-관리---퍼널-분석---분석3.png" />

"Tutorial #5"과 "Tutorial #6" 사이의 전환율이 39%임을 확인할 수 있습니다.  
개발자는 이로부터 6번 튜토리얼에 심각한 유저 이탈 원인이 있다고 추측합니다.  

일반적으로 유저들의 주된 이탈 원인은 버그나 지루한 기획 설계 등입니다.  
개발자는 업데이트 이후 퍼널 분석 결과의 변화 추이를 확인하면서 유저 경험을 개선할 수 있습니다.  

### 4. 퍼널 분석 조건 저장

분석한 퍼널 조건을 저장해 두고, 이후에 같은 조건으로 빠르게 분석할 수 있어요.

![퍼널저장1](/img/docs/guide/base/logs/funnel_save1.png)

분석 조건은 프로젝트별로 최대 30개까지 저장할 수 있어요.  
분석 기간 ‘최근 1일/7일/15일’은 조건을 불러오는 시점 기준으로 적용돼요.  
저장한 조건은 수정할 수 없어요.
