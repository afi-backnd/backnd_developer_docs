---
sidebar_label: 타임아웃
---

# 타임아웃

뒤끝펑션에는 실행 제한 시간이 존재합니다.  
함수 1회 실행 당 제한 시간은 **10000ms(10초)**입니다.  
개발사에서 작성한 로직을 수행하는 데 10000ms 이상 걸리게 되면 서버에서 처리하지 않고 에러를 리턴합니다.  

## 디버깅할 때 타임아웃 확인하기

디버깅할 때 타임아웃을 활성화하여 개발사에서 작성한 함수가 제한 시간 내에 실행되는지 확인할 수 있습니다.  
디버깅 환경에서 타임아웃을 활성화하기 위해서는 **debugConfig.json**파일에서 **timeoutEnable**을 아래와 같이 설정해 줍니다.  

### debugConfig.json

```js
{
  ...  
  "timeoutEnable": "true", // string 문자열입니다. "true = 활성화"
  ...  
}
```

> 중단점 등을 통해 함수 동작 중 멈추는 경우에도 타임아웃 시간이 지나게 되면 실패로 리턴됩니다.  

### Error case

**로직을 수행하는 데 10초 이상 걸리는 경우**  
Backend Function timeout: Exceeded 10 seconds

![image](https://developer.thebackend.io/static/img/bfunc/timeout/debug.PNG)

## 서버 배포 후 타임아웃

개발사의 개발 환경과 실제 실행환경이 다르기 때문에
디버깅할 때는 타임아웃에 걸리지 않았지만, 서버 배포 후 타임아웃이 걸리는 경우가 존재합니다.  
서버 배포 후에도 정상적으로 실행되는지 반드시 확인이 필요합니다.  

또한 [ColdStart](/sdk-docs/function/work-in-windows/visual-studio-code/create) 하는 경우에도 타임아웃에 걸릴 수 있습니다.  
개발사에서 뒤끝펑션을 개발할 때 다양한 서드파티 라이브러리를 활용하여 프로그램 자체가 큰 경우,
최초 함수 요청 시 Cold Start 상태로 호출될 때 타임아웃에 의해 실행이 제한될 수 있습니다.  
이 경우 사용하는 라이브러리를 변경하거나 로직을 변경해야 합니다.  
