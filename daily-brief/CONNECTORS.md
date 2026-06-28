# Connectors

## 플레이스홀더 동작 방식
플러그인 파일은 특정 제품 대신 `~~카테고리`로 도구를 참조합니다. 사용자는 각 카테고리에 본인이 쓰는 커넥터를 연결하면 됩니다.

## 이 플러그인이 쓰는 커넥터

| 카테고리 | 플레이스홀더 | 옵션 | 필수 여부 |
|---|---|---|---|
| 이메일 | `~~email` | Gmail | 필수 (email-brief, daily-brief) |
| 채팅 | `~~chat` | Slack | 필수 (slack-brief, daily-brief) |
| 캘린더 | `~~calendar` | Google Calendar | 선택 (액션아이템 등록 시) |

> 각 사용자가 본인 계정으로 직접 인증합니다. 제작자의 계정·데이터는 포함되지 않습니다.
> 현재 스킬은 Gmail/Slack MCP 도구 이름(`search_threads`, `slack_read_channel` 등)을 그대로 사용합니다. 다른 이메일/채팅 도구를 쓸 경우 각 SKILL.md의 도구 호출 부분을 해당 커넥터에 맞게 조정하세요.
