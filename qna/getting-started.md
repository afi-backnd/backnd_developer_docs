# TheBackend 시작하기 — Q&A

## Q. TheBackend(뒤끝)가 무엇인가요?

TheBackend(뒤끝)는 게임 개발사를 위한 BaaS(Backend as a Service)입니다. 플레이어 인증, 데이터 저장, 랭킹, 채팅, 길드, 쿠폰, 푸시 알림, 게임 로그 등 게임 서버에서 필요한 기능을 별도 서버 개발 없이 SDK 연동만으로 사용할 수 있습니다.

## Q. 어떤 플랫폼을 지원하나요?

주로 Unity 게임 엔진을 지원하며, Unreal Engine용 채팅 SDK도 별도로 제공합니다.

## Q. 시작하려면 무엇을 해야 하나요?

1. [뒤끝 콘솔](https://console.thebackend.io)에서 회원가입 및 프로젝트 생성
2. 앱 ID와 클라이언트 키 발급
3. Unity Package Manager 또는 `.unitypackage` 파일로 SDK 임포트
4. `TheBackend > Edit Settings`에서 앱 ID, 클라이언트 키 입력
5. 게임 초기화 코드 작성: `Backend.Initialize()` 호출

자세한 내용은 [시작 가이드](../guide/getting-started/)를 참고하세요.

## Q. 무료로 사용할 수 있나요?

요금제에 대한 최신 정보는 공식 사이트(https://thebackend.io)에서 확인하세요.

## Q. SDK를 Unity에 임포트할 때 주의할 점이 있나요?

베이스 SDK와 채팅 SDK를 함께 사용하는 경우, 채팅 SDK 임포트 시 `Backend.dll`이 덮어씌워지지 않도록 해당 파일의 체크를 해제해야 합니다. 자세한 내용은 [베이스 SDK와 함께 사용하는 경우](../sdk-docs/chat/install/with-base-sdk) 문서를 참고하세요.

## Q. 로컬 개발 환경에서 문서 사이트를 실행하는 방법은?

```bash
npm install
npm run start:ko   # 한국어
npm run start:en   # 영어
```
