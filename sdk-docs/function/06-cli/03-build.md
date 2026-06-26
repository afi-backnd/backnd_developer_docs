---
sidebar_label: 빌드
description: "빌드"
---

# 빌드

개발사에서 생성한 프로젝트를 빌드하기 위한 방법입니다.  
프로젝트는 Backend CLI를 이용하여 빌드할 수 있습니다.  

> 뒤끝펑션은 리눅스 환경에서 동작합니다.  
> 빌드 시 뒤끝에서 제공하는 여러 가지 커스텀 빌드 설정이 필요합니다.  
> **반드시 Backend CLI를 이용하여 빌드 후 그 결과물을 서버로 배포해야 합니다.**

## build 명령어

config.json에 설정된 프로젝트를 빌드하거나 혹은 개발사가 직접 빌드할 프로젝트를 설정하여 빌드를 진행할 수 있습니다.  

**config.json에 설정된 프로젝트를 빌드하기**  
backend build

**임의의 프로젝트를 빌드하기**  
backend build **빌드할 프로젝트.csproj**  

> 이때 빌드할 프로젝트에는 해당 csproj가 저장된 절대 경로가 포함되어야 합니다.  

### Return cases

**빌드에 성공한 경우**  
아래 그림과 유사하게 해당 메시지가 표시되면 빌드에 성공한 것입니다.  

```js
Making publish.Zip
디렉터리 "C:\파일경로\BackendFunction\bin\Release\netcoreapp2.1\linux-x64\publish"의 압축을
"C:\파일경로\UserRank\BackendFunction\bin\Release\netcoreapp2.1\linux-x64\publish.zip"에
 푸는 중입니다.  
```

![image](https://developer.thebackend.io/static/img/bfunc/cli/build.PNG)

**프로젝트 경로가 올바르지 않거나, 특수문자/공백등으로 인식할 수 없을 때**  
MSBUILD : error MSB1003: 프로젝트 또는 솔루션 파일을 지정하세요. 현재 작업 디렉터리에 프로젝트 또는 솔루션 파일이 없습니다.  
MSBUILD : error MSB1008: 프로젝트를 하나만 지정할 수 있습니다.  
MSBUILD : error MSB1009: 프로젝트 파일이 없습니다.  
