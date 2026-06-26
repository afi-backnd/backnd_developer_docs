import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 확률 관리
확률은 뒤끝 콘솔에 각 행(row)의 퍼센트를 지정한 확률 파일을 업로드하고, 뒤끝 SDK를 통해 해당하는 확률로 도출된 결과를 받아와 활용하는 기능입니다.  
예를 들어, 아이템 별 확률 테이블을 만들어서 게임 내 확률형 아이템 기능에 사용할 수 있습니다.  

<ConsoleLinkButton text="확률 바로가기" menu="baseProbability" feature="확률" title="확률 관리" />

## 확률 파일 업로드 조건
- "id" 열(column)은 예약된 키이므로 사용할 수 없습니다.  
- 확률 파일의 시트(sheet)는 하나여야 합니다.  
- 확률 파일에 열(column) 이름은 영문 대소문자, 숫자만 가능하며, 공백을 포함할 수 없습니다.  
- 업로드한 확률 파일에 "percent" 열(column)이 존재해야 합니다.  
- 확률 파일 내 "percent" 열(column) 확률 값 총합이 100%가 되어야 합니다.  
- 확률 파일 내 "percent" 값에 소수점 입력이 가능합니다.  

:::info 다운로드
<a href="https://developer.thebackend.io/files/console/chart/probabilityExample.xlsx"><b>[확률 예시 파일 다운로드]</b></a>
:::

## 확률 및 확률 파일
현재 생성된 확률 수와, 업로드된 확률 파일 수를 확인할 수 있습니다.  
확률은 최대 200개까지 생성이 가능하며, 확률 파일은 최대 600개까지 생성 및 업로드 가능합니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/base/probability/probability01.png" />

## 확률 파일 적용하기
확률 파일을 업로드 했더라도 페이지 상단의 명령 버튼 중 **확률 파일 적용** 버튼을 누르지 않으면 SDK 내에서 가져올 수 없습니다.  
반드시 사용할 확률 파일을 선택하신 후 **확률 파일 적용** 버튼을 눌러 업로드된 파일을 적용하시기 바랍니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/base/probability/probability02.png" />

## 확률 파일 수정하기
확률 파일은 뒤끝 콘솔에서 수정할 수 없습니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/base/probability/probability03.png" />
