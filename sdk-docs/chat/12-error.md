---
sidebar_label: 에러 콜백
description: "에러 콜백"
---

# 에러 콜백

채팅 이용 시 발생할 수 있는 에러 콜백 함수 및 Enum 정보입니다.

### 콜백 함수

```csharp
// 채팅 이용 시 발생할 수 있는 에러들이 오는 콜백 함수 입니다.
public void OnError(ERROR_MESSAGE error, object param)
{
    string message = error.ToString();

    switch (error)
    {
        case ERROR_MESSAGE.CHAT_BAN:
            {
                ErrorMessageChatBanParam errorMessageChatBanParam = (ErrorMessageChatBanParam)param;
                if (errorMessageChatBanParam == null) return;

                var banTime = DateTime.Now.AddSeconds(errorMessageChatBanParam.RemainSeconds);

                message = error.ToString() + " : " + banTime.ToString("yyyy-MM-dd HH:mm:ss") + " 까지";
            }
            break;

        case ERROR_MESSAGE.CHANNEL_FULL:
        case ERROR_MESSAGE.INVALID_PASSWORD:
        case ERROR_MESSAGE.ALREADY_CREATED_CHANNEL:
        case ERROR_MESSAGE.CHANNEL_GROUP_TOO_SHORT:
        case ERROR_MESSAGE.CHANNEL_GROUP_TOO_LONG:
        case ERROR_MESSAGE.CHANNEL_NAME_TOO_SHORT:
        case ERROR_MESSAGE.CHANNEL_NAME_TOO_LONG:
        case ERROR_MESSAGE.DUPLICATE_CHANNEL_GROUP:
        case ERROR_MESSAGE.PASSWORD_TOO_LONG:
        case ERROR_MESSAGE.CHANNEL_GROUP_FILTERED:
        case ERROR_MESSAGE.CHANNEL_NAME_FILTERED:
            {
                ErrorMessageChannelParam errorMessageChannelParam = (ErrorMessageChannelParam)param;
                if (errorMessageChannelParam == null) return;

                message = error.ToString() + " : " + errorMessageChannelParam.ChannelGroup + " / " + errorMessageChannelParam.ChannelName + " / " + errorMessageChannelParam.ChannelNumber;
            }
            break;

        default:
            break;
    }
}
```

## ErrorCode(enum)

| Value                     | Description                                                                            | Param                    |
| :------------------------ | :------------------------------------------------------------------------------------- | :----------------------- |
| NOT_AUTHENTICATION      | 인증 되지 않은 사용자 입니다. | NULL                     |
| CHAT_SERVER_FULL          | 채팅 서버 최대 인원 접속 상태 입니다.                                                  | NULL                     |
| WHISPER_OFFLINE           | 귓속말 상대가 오프라인 상태 입니다.                                                    | NULL                     |
| TOO_MANY_REPORT           | 오늘 하루 신고 가능 횟수를 모두 사용 하였습니다.                                       | NULL                     |
| NOT_MY_REPORT             | 자기 자신을 신고 할 수 없습니다.                                                       | NULL                     |
| INVALID_PARAMETER         | 인자 값을 잘못 입력 하였습니다.                                                        | NULL                     |
| CHAT_BAN                  | 채팅 금지 상태 입니다.                                                                 | ErrorMessageChatBanParam |
| DISABLED_CHANNEL          | 채널이 활성화 되어 있지 않습니다.                                                      | NULL                     |
| MESSAGE_TOO_LONG          | 채팅 메시지 길이가 너무 깁니다.                                                        | NULL                     |
| MESSAGE_TOO_SHORT         | 채팅 메시지 길이가 너무 짧습니다.                                                      | NULL                     |
| MESSAGE_FILTERED          | 채팅 메시지가 필터링 되었습니다.                                                       | NULL                     |
| MESSAGE_SPAM              | 채팅 메시지 도배로 인해 메시지가 차단 되었습니다.                                      | NULL                     |
| NOT_NICKNAME              | 닉네임을 설정 하지 않았습니다.                                                         | NULL                     |
| DISABLED_SERVICE          | 해당 서비스(귓속말, 길드채팅등)가 활성화 되어 있지 않은 상태 입니다.                   | NULL                     |
| CHANNEL_FULL              | 현재 채널 인원이 풀 상태 입니다.                                                       | ErrorMessageChannelParam |
| NOT_JOIN_CHANNEL          | 입장 되어 있는 채널이 아닙니다.                                                        | NULL                     |
| ALREADY_CREATED_CHANNEL   | 이미 생성 된 채널 입니다. (채널 생성 시)                                               | ErrorMessageChannelParam |
| DUPLICATE_CHANNEL_GROUP   | 채널 그룹이 이미 존재 합니다. (채널 생성 시)                                           | ErrorMessageChannelParam |
| CHANNEL_GROUP_TOO_LONG    | 채널 그룹이 너무 짧습니다. (채널 생성 시)                                              | ErrorMessageChannelParam |
| CHANNEL_GROUP_TOO_SHORT   | 채널 그룹이 너무 깁니다. (채널 생성 시)                                                | ErrorMessageChannelParam |
| CHANNEL_GROUP_FILTERED    | 채널 그룹이 필터링 되었습니다. (채널 생성 시)                                          | ErrorMessageChannelParam |
| DUPLICATE_CHANNEL_NAME    | 채널 이름이 이미 존재 합니다. (채널 생성 시)                                           | NULL                     |
| CHANNEL_NAME_TOO_LONG     | 채널 이름이 너무 짧습니다. (채널 생성 시)                                              | ErrorMessageChannelParam |
| CHANNEL_NAME_TOO_SHORT    | 채널 이름이 너무 깁니다. (채널 생성 시)                                                | ErrorMessageChannelParam |
| CHANNEL_NAME_FILTERED     | 채널 이름이 필터링 되었습니다. (채널 생성 시)                                          | ErrorMessageChannelParam |
| PASSWORD_TOO_LONG         | 비밀번호를 너무 길게 설정 하였습니다. (채널 생성 시)                                   | ErrorMessageChannelParam |
| INVALID_PASSWORD          | 비밀번호를 잘못 입력 하였습니다. (채널 입장 시)                                        | ErrorMessageChannelParam |
| LIMIT_REPORT_MESSAGE_DAYS | 오래된 채팅 메시지이므로 신고가 불가능합니다.                                          | NULL                     |
| ALREADY_JOIN_CHANNEL      | 이미 입장 한 채널 입니다. 오픈 채널은 같은 채널 그룹 내에서 중복 입장이 불가능 합니다. | NULL                     |
| DUPLICATE_CONNECTION      | 중복 로그인이 발생 되었습니다. | NULL                     |
| NOT_FOUND_BLOCK_URL      | 유저 간 차단 처리 URL 등록이 되어 있지 않습니다. (커스텀 인증 사용 시) | NULL                     |
| FAILED_ADD_BLOCK_PLAYER      | 유저 차단에 실패 하였습니다. | NULL                     |
| ALREADY_BLOCK_PLAYER      | 이미 차단 된 유저 입니다. | NULL                     |
| FAILED_REMOVE_BLOCK_PLAYER      | 유저 차단 해제에 실패 하였습니다. | NULL                     |
| FAILED_UPDATE_AVATAR      | 아바타 업데이트에 실패 하였습니다. | NULL                     |
| FAILED_UPDATE_METADATA      | 메타데이터 업데이트에 실패 하였습니다. | NULL                     |
| FAILED_UPDATE_GAMER_NAME      | 닉네임 변경에 실패 하였습니다. | NULL                     |
| FAILED_UPDATE_LANGUAGE      | 언어 정보 변경에 실패 하였습니다. | NULL                     |
| NOT_BLOCK_USER_WHISPER      | 차단 된 유저에게 귓속말을 보내 실 수 없습니다. | NULL                     |
| BLOCKED_RECIPIENT_WHISPER      | 귓속말 요청 상대가 나를 차단 하였습니다. | NULL                     |

## Param

```csharp
public class ErrorMessageChatBanParam
{
    // 채팅 금지 상태 종료까지 남은 시간 (초)
    public UInt64 RemainSeconds = 0;
}
```

```csharp
public class ErrorMessageChannelParam
{
    // 채널 그룹
    public string ChannelGroup = string.Empty;

    // 채널 이름
    public string ChannelName = string.Empty;

    // 채널 번호
    public UInt64 ChannelNumber = 0;
}
```
