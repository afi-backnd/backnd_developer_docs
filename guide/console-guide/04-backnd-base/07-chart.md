import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 차트 관리

차트는 게임 정보와 달리 모든 유저가 공통적으로 조회할 수 있는 데이터입니다.  
개발사에서 차트 파일을 뒤끝 콘솔에 업로드하고 적용하면 유저는 서버를 통해서 이에 접근할 수 있습니다.  
예를 들어, 아이템 차트 파일을 만들어 업로드 후, 게임 내 상점에서 아이템 리스트를 불러와 이를 출력할 수 있습니다.  
차트는 최대 200개까지 생성 가능하며, 차트 파일은 최대 600개까지 업로드 가능합니다.

<ConsoleLinkButton text="차트 바로가기" menu="baseChart" feature="차트" title="차트 관리" />

## 폴더 또는 차트 생성

- 폴더 외부에서 생성한 차트는 이후 원하는 폴더에 추가할 수도 있습니다.  
  > <img src="https://developer.thebackend.io/static/img/newconsole/base/chart/chart01.png" />

## 폴더 생성  


- 폴더를 생성하고 폴더 내에서 차트를 별도 관리할 수 있습니다.  

  > <img src="https://developer.thebackend.io/static/img/newconsole/base/chart/chart02.png" />

- 폴더 생성 후 폴더 내에서도 차트를 직접 생성할 수 있습니다.  

  > <img src="https://developer.thebackend.io/static/img/newconsole/base/chart/chart03.png" />
  >   


- 폴더 내 차트 생성

  > <img src="https://developer.thebackend.io/static/img/newconsole/base/chart/chart04.png" />
  >   


- 폴더 내에 생성된 차트 목록

  > <img src="https://developer.thebackend.io/static/img/newconsole/base/chart/chart05.png" />

- 폴더 내 차트 관리

  > <img src="https://developer.thebackend.io/static/img/newconsole/base/chart/chart06.png" />

- 폴더 해제 : 선택된 차트를 폴더 밖으로 꺼낼 수 있습니다.  
- 삭제 : 선택된 차트를 삭제할 수 있습니다.  

  


**폴더로 진입하지 않고 차트 생성**  

- 폴더에 추가 : 선택된 차트를 폴더 내로 추가할 수 있습니다.  
- 삭제 : 선택된 차트를 삭제할 수 있습니다.  
  > <img src="https://developer.thebackend.io/static/img/newconsole/base/chart/chart07.png" />

## 차트 파일 업로드

생성한 차트 내에 차트 파일을 업로드합니다.  

- "id" 열(column)은 예약된 키이므로 차트 파일에 사용할 수 없습니다.  
- 차트 파일의 시트(sheet)는 하나여야 합니다.  
- 차트 파일에 열(column) 이름은 영문 대소문자, 숫자만 가능하며, 공백을 포함할 수 없습니다.  
- 차트 파일에 역슬래시(&#92;)를 사용할 수 없습니다.  

:::info
<a href="https://developer.thebackend.io/files/console/chart/chartExample.xlsx"><b>[차트 예시 파일 다운로드]</b></a>
:::
><img src="https://developer.thebackend.io/static/img/newconsole/base/chart/chart08.png" />

차트 파일 업로드 후 **"차트 파일 적용" / "CSV 다운로드" / "삭제"** 하려면 원하는 차트 파일을 선택해주세요.  
><img src="https://developer.thebackend.io/static/img/newconsole/base/chart/chart09.png" />

#### 차트 파일 적용하기

차트 파일을 업로드했더라도, 페이지 상단의 명령 버튼 중 **"차트 파일 적용"** 버튼을 누르지 않으면 SDK 내에서 GetChartList 함수를 통해 가져올 수 없습니다.  
반드시 사용할 차트를 선택한 후 **"차트 파일 적용"** 버튼을 눌러 업로드된 파일을 적용하시기 바랍니다.  

## 차트 상세 화면

차트 파일을 뒤끝 콘솔에서 간단히 수정할 수 있습니다.  
편집 모드를 ON으로 변경 후, 셀을 선택하고 엔터 혹은 더블클릭을 통해서 해당 셀의 내용을 수정할 수 있습니다.  
><img src="https://developer.thebackend.io/static/img/newconsole/base/chart/chart10.png" />
