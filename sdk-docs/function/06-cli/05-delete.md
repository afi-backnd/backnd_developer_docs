---
sidebar_label: 삭제
---

# 삭제

서버로 배포된 함수는 Backend CLI를 이용하여 삭제할 수 있습니다.  

## delete 명령어

삭제할 함수명을 입력하여 배포된 함수를 삭제할 수 있습니다.  
해당 명령어를 사용하면 **배포된 함수가 즉각적으로 삭제**되기 때문에 주의해서 사용해야 합니다.  

**함수명을 지정하여 삭제**  
backend delete **삭제할 함수 명**

![image](https://developer.thebackend.io/static/img/bfunc/cli/delete.PNG)

### Return cases

**함수 삭제에 성공했을 때**  
Success to delete the function
StatusCode: 200  
message: OK

**config.json에 잘못된 authKey가 설정되어 있을 때**  
Fail to delete the function: "삭제를 시도한 함수 명"
Show Detail
StatusCode: 403  
message: The remote Server returned an error:(403) Forbidden

**존재하지 않는 함수의 삭제를 시도했을 때**  
Fail to delete the function: "삭제를 시도한 함수 명"
Show Detail
StatusCode: 404  
message: The remote server returned an error:(404) Not Found.  
