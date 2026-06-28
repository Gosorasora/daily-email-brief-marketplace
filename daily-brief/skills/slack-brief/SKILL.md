---
name: slack-brief
description: 지정한 Slack 워크스페이스의 채널들을 최근 7일 기준으로 읽어 채널별 최신순으로 요약한다. 각 항목에 메시지 permalink를 붙이고, 중복은 처음만 본문·이후는 리마인더로 표시하며, 온보딩성 채널은 테마별로 묶고 마감 임박 액션을 따로 정리한다. 트리거 - "슬랙 요약", "슬랙 브리핑", "채널 요약", "slack brief", "slack summary".
---

# slack-brief

## ⚙️ 설정 (사용 전 채우기)
- `<SLACK_WORKSPACE>` : 워크스페이스 도메인 (예: myteam.slack.com)
- 대상 채널 — `#채널명 = CHANNEL_ID` 형식으로 (개수 자유):
  - `<#채널1 = CHANNEL_ID_1>`
  - `<#채널2 = CHANNEL_ID_2>`
  - `<#채널3 = CHANNEL_ID_3>`
- `<ONBOARDING_CHANNEL>` : 질문이 많아 테마별로 묶을 채널(없으면 생략)
- `~~chat` 커넥터(Slack MCP) 연결 필요 (`slack_search_channels`, `slack_read_channel`, `slack_read_thread`).
  - 채널 ID 모르면 `slack_search_channels`로 먼저 조회. 미연결 시 레지스트리에서 Slack 검색 → Connect 카드.

## 기간
- 실행 시점 기준 최근 7일. `slack_read_channel`의 `oldest`에 7일 전 00:00 UTC epoch 초.
  - 계산: `date -u -d '7 days ago 00:00:00' +%s`
- `limit=100`, 초과 시 `cursor` 페이지네이션.

## 처리 규칙
1. **중복 제거**: 동일 내용이 여러 번/여러 채널 → 처음만 본문, 이후 `🔁 리마인더`.
2. **정렬**: 채널별 최신순(newest first).
3. **온보딩성 채널**: 테마별로 묶음 — 예) 코어팀 / 이벤트·리포팅 / 승인·연결 대기 / 기타.
4. 마감·액션 아이템 별도 강조.

## permalink
- `https://<SLACK_WORKSPACE>/archives/{CHANNEL_ID}/p{TS}`
- {TS} = 메시지 `Message TS`에서 소수점 제거. 예 1782512696.844859 → p1782512696844859.

## 액션아이템 (번호 출력 + 승인 시에만 캘린더 등록)
- 마감 있는 공지를 번호 매겨 출력만. 자동 등록 금지.
- "①③ 추가해" 식 지시 시에만 `~~calendar`(create_event)로 등록(DESCRIPTION에 Slack permalink). "2번 삭제해" → delete_event. 미연결 시 `.ics`.

## 출력 형식
```
## 💬 [날짜] Slack 요약 (최근 7일)
### 📢 #[채널1]
- [제목/요약](permalink) — 날짜 시각, 작성자
### 💬 #[채널2]
...
### 🎓 #[온보딩채널] (테마별)
...
### ⏰ 마감 임박 액션 (번호만)
1. [제목] — [마감]
```

## 규칙
- 언어 한국어, 간결·사실 위주. 추측 금지.
- 7일 내 신규 0건 채널은 "신규 메시지 없음" 한 줄.
