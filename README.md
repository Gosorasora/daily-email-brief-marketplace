# Sora Plugins — Cowork 플러그인 마켓플레이스

Cowork / Claude Code 에서 사용할 수 있는 플러그인 모음입니다.

## 포함된 플러그인

| 이름 | 설명 |
|---|---|
| **데일리브리프 (daily-brief)** | Gmail+Slack 통합 브리핑, Gmail 24시간 요약, Slack 채널 7일 요약 (스킬 3종, 읽기 전용) |

데일리브리프에 포함된 스킬:
- `daily-brief` — Gmail(24h) + Slack(7d) 통합 브리핑
- `email-brief` — Gmail 받은편지함 24시간 요약
- `slack-brief` — Slack 채널 최근 7일 요약

## 설치 방법 (사용자용)

Cowork 또는 Claude Code에서 아래 명령을 입력하세요.

1. 마켓플레이스 추가
   ```
   /plugin marketplace add Gosorasora/daily-email-brief-marketplace
   ```
2. 플러그인 설치
   ```
   /plugin install daily-brief@sora
   ```
3. 커넥터 연결 — 본인 Gmail·Slack 계정을 연결 (캘린더는 선택). 자세한 내용은 `daily-brief/CONNECTORS.md` 참고.
4. 설정값 입력 — 각 SKILL.md 상단 `⚙️ 설정` 블록의 `<...>` 자리표시자(메일 주소, 슬랙 워크스페이스/채널 ID)를 본인 값으로 교체.
5. (선택) 최신 버전으로 업데이트
   ```
   /plugin marketplace update sora
   ```

## 배포자 안내 (Sora 본인용)

플러그인 업데이트 시
- `daily-brief/.claude-plugin/plugin.json` 의 version 올리기
- `.claude-plugin/marketplace.json` 의 해당 플러그인 version 도 같은 값으로 올리기
- 변경사항 커밋 후 GitHub에 푸시
- 사용자는 `/plugin marketplace update sora` 로 반영

## 참고
- Gmail 외부계정 가져오기(POP3)는 2026년 종료되었습니다. 네이버 등 외부 메일을 함께 보려면 해당 계정에서 Gmail로 **자동전달**을 설정하세요.
- 커넥터 인증·스케줄·개인 데이터는 공유되지 않습니다. 각자 본인 환경에서 설정합니다.
