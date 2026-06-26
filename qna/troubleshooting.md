# 자주 발생하는 오류와 해결법 — Q&A

## Q. 초기화 시 "앱 ID 또는 클라이언트 키가 올바르지 않습니다" 오류가 납니다.

`TheBackend > Edit Settings`에서 입력한 앱 ID와 클라이언트 키가 뒤끝 콘솔의 값과 일치하는지 확인하세요. 복사 시 앞뒤 공백이 포함되지 않았는지도 확인이 필요합니다.

## Q. 안드로이드 빌드 후 초기화가 실패합니다.

AGP(Android Gradle Plugin) 버전이 8.5 이하인 경우 `mainTemplate.gradle`에 추가 설정이 필요합니다. 채팅 SDK 설치 문서를 참고하세요.

## Q. 채팅 SDK 임포트 후 `Backend.dll` 충돌 오류가 발생합니다.

베이스 SDK와 채팅 SDK는 동일한 `Backend.dll`을 포함합니다. 채팅 SDK 임포트 시 `Backend.dll` 항목의 체크를 해제하고 임포트하세요.

## Q. 뒤끝펑션 호출 시 인증 오류가 납니다.

콘솔에서 뒤끝펑션이 활성화되어 있는지 확인하고, `TheBackend > Edit Settings`의 Function Auth Key 값이 올바르게 입력되어 있는지 확인하세요.

## Q. 랭킹 점수가 업데이트되지 않습니다.

콘솔에서 해당 랭킹 테이블이 생성되어 있고 활성화 상태인지 확인하세요. SDK 호출 전 `Backend.Initialize()`가 성공적으로 완료되었는지도 확인이 필요합니다.

## Q. 더 많은 오류 코드 정보는 어디서 찾나요?

[전체 에러 코드 문서](../sdk-docs/backend-new/02-base/09-all-errors.md)를 참고하세요.
