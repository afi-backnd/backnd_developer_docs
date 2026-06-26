---
sidebar_label: 배포
---

# 배포

개발사에서 생성한 프로젝트를 서버로 배포하기 위한 방법입니다.  
프로젝트는 Backend CLI를 이용하여 서버로 배포할 수 있습니다.  
배포를 위해서는 [빌드 작업](/sdk-docs/function/cli/build)을 통해 프로젝트를 먼저 빌드해야 합니다.  

## deploy 명령어

config.json에 설정된 빌드 결과물 혹은 직접 파일을 설정하여 서버로 함수를 배포할 수 있습니다.  

**config.json에 설정된 빌드 결과물을 config.json에 설정된 함수명으로 배포하기**  
backend deploy

**config.json에 설정된 빌드 결과물을 원하는 함수명으로 배포하기**  
backend deploy **원하는 함수명**  

**직접 빌드 파일을 설정하여 원하는 함수명으로 배포하기**  
backend deploy **원하는 함수명** **빌드 파일 절대 경로**  

![image](https://developer.thebackend.io/static/img/bfunc/cli/deploy.PNG)

> description(함수 설명)이 긴 경우 뒷부분이 "..."로 표시되게 됩니다. 실제로 배포될 때는 잘린 부분까지 모두 정상적으로 업로드됩니다.  
> description은 `backend config` 명령어를 이용하여 설정할 수 있습니다.  

### Return cases

**함수 배포 성공했을 때**  
Success to deploy the function
StatusCode: 200  
message: OK

**config.json에 잘못된 authKey가 설정되어 있을 때**  
statusCode: 403  
message: The remote Server returned an error:(403) Forbidden

**함수명에 한글이 포함된 경우**  
statusCode: 400  
message: The remote server returned an error:(400) Bad Request.  
