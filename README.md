# TheBackend Developer Docs

TheBackend(뒤끝)은 게임 개발사를 위한 **BaaS(Backend as a Service)**입니다.
Unity 및 Unreal Engine 게임에 서버 기능을 코드 몇 줄로 연동할 수 있도록 SDK와 API를 제공합니다.

공식 문서 사이트: **https://docs.thebackend.io**

---

## 이 레포에 대해

이 레포는 공식 문서 사이트의 원본 마크다운 소스를 공개한 것입니다.
AI agent, LLM, RAG 시스템이 TheBackend 문서를 쉽게 읽고 활용할 수 있도록 공개되었습니다.

> AI 도구가 이 레포를 탐색하는 경우 [`llms.txt`](./llms.txt)를 먼저 읽으세요.

---

## 문서 구조

```
sdk-docs/
├── backend-new/     # 베이스 SDK — 플레이어 데이터, 랭킹, 길드, 쿠폰, 푸시, 게임 로그, 확률 등
├── chat/            # 채팅 SDK — 채널, 메시지, 번역, 신고
├── function/        # 뒤끝펑션 — 서버리스 커스텀 함수
├── world/           # 월드 — 멀티플레이어 기능
├── database/        # 데이터베이스 — 게임 데이터 저장
└── unreal-chat/     # 언리얼 채팅 SDK

api-docs/
└── chat/            # 채팅 REST API 레퍼런스

guide/
├── getting-started/ # 뒤끝 시작하기 (가입 → SDK 연동)
└── console-guide/   # 콘솔 사용 가이드

update-note/         # 모듈별 릴리즈 노트
faq/                 # 자주 묻는 질문
qna/                 # Q&A 형식 문서 (AI agent용)
```

---

## 주요 기능 모듈

| 모듈 | 설명 | 경로 |
|---|---|---|
| 베이스 SDK | 플레이어 인증, 데이터 저장, 랭킹, 길드, 쿠폰, 푸시, 게임 로그, 확률 | `sdk-docs/backend-new/` |
| 채팅 SDK | Unity 실시간 채팅 (채널, 메시지, 번역) | `sdk-docs/chat/` |
| 뒤끝펑션 | 서버리스 함수 — 커스텀 서버 로직 클라우드 실행 | `sdk-docs/function/` |
| 월드 | 멀티플레이어 월드 기능 | `sdk-docs/world/` |
| 데이터베이스 | 게임 데이터 저장 및 조회 | `sdk-docs/database/` |
| 언리얼 채팅 | Unreal Engine 실시간 채팅 | `sdk-docs/unreal-chat/` |
| 채팅 API | REST API 레퍼런스 | `api-docs/chat/` |

---

## 빌드 상태

- Live: ![](https://codebuild.ap-northeast-2.amazonaws.com/badges?uuid=eyJlbmNyeXB0ZWREYXRhIjoiYXZ4MGJ4NnplcDhMWE9QaXBXTk9mMTVuTW5mdmtMdUhoQ1YwU2hQMjJ6enpaYXNNalpSUFozN29FTzNpaVJha2w5Tlc3UU1DRzljVmlPdDFDWVNlNHlBPSIsIml2UGFyYW1ldGVyU3BlYyI6Ik4rNEtubURpcE1PSWgyYmMiLCJtYXRlcmlhbFNldFNlcmlhbCI6MX0%3D&branch=master)
- Alpha: ![](https://codebuild.ap-northeast-2.amazonaws.com/badges?uuid=eyJlbmNyeXB0ZWREYXRhIjoiOEhjZ01Vc011VDFlRlpGd1RxcEJ3emc4TnJ6VW8veWQwVVB0YzBZK1REQ09xU1hpVUluQ3Uyd3lJRitPa3R5NXlVMWE1eHRBN2FUVzZrT05aUnpRSzVJPSIsIml2UGFyYW1ldGVyU3BlYyI6IjNsY1kyc29qK1RaUEZldm4iLCJtYXRlcmlhbFNldFNlcmlhbCI6MX0%3D&branch=alpha)

---

## 문서 업데이트

이 레포는 공식 배포 파이프라인에 의해 자동으로 동기화됩니다.
