---
sidebar_label: 프로젝트 생성
---

# Visual Studio 프로젝트 생성

## Visual Studio에서 뒤끝펑션 프로젝트 템플릿을 이용하여 새 프로젝트 생성하기

아래 설명은 Visual Studio 2019 버전을 기준입니다.  
프로젝트 생성 부분의 UI를 제외하고는 Visual Studio 2017 버전과 2019 이상 버전 모두 동일합니다.  

---
####
1. BackendFunction 템플릿을 선택해 새 프로젝트를 생성합니다.  

시작하기 문서에서 뒤끝펑션 프로젝트 템플릿을 설치해야 새 프로젝트를 생성할 때 아래 그림과 같이 BackendFunction 템플릿이 표시됩니다.  

![image](https://developer.thebackend.io/static/img/bfunc/create/1.PNG)

---
####
2. 프로젝트 속성에서 **어셈블리 이름 및 네임스페이스를 BackendFunction으로 수정**합니다.  

> 대상 프레임워크, 출력 형식 등의 설정은 모두 프로젝트 생성 후 설정되어 있는 **기본 값**을 사용해야 합니다.  

- 대상 프레임워크 : .NET 8.0
- 출력 형식 : 클래스 라이브러리

![image](https://developer.thebackend.io/static/img/bfunc/create/2.png)

![image](https://developer.thebackend.io/static/img/bfunc/create/3.PNG)

---
####
3. 네임스페이스, 클래스명, 함수명을 변경합니다.  

뒤끝펑션에서 제공하는 템플릿으로 프로젝트를 생성하면 네임스페이스와 클래스명은 개발사에서 생성한 프로젝트 명으로 변경될 수 있습니다.  
이 부분을 아래와 같이 변경해야 합니다.  

- 네임스페이스 : BackendFunction
- 클래스명 : BFunc
- BFunc에 존재하는 Function 함수 형태
   ```js
   public Stream Function(Stream stream, ILambdaContext context)
   ```
  > 개발사에서 제작한 뒤끝펑션을 뒤끝 SDK에서 호출하면 **BackendFunction.BackendFunction.BFunc.Function**이 호출되게 됩니다.  

![image](/img/docs/guide/function/setting/create-template-sample.png)
