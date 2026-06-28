---
name: daily-brief
description: 매일 한국어 통합 브리핑(Gmail + Slack)을 생성한다. 트리거 - "데일리 브리핑", "오늘 브리핑", "통합 브리핑", "daily brief". Gmail 받은편지함 최근 24시간 + 지정 Slack 채널 최근 7일을 읽어 소스별 테이블로 정리한다. 읽음/스팸/발송/캘린더 등 실행 작업은 하지 않고 리포트만 출력���다.
---

# Daily Brief (통합 브리핑)

매일 한국어 통합 브리핑을 생성한다. 자동 실행 시 사용자가 없으므로, 읽음/스팸 처리�옘一지 일당 동작'..
---

## ⚙️ 설정 (사용 전 이 블록을 본인 값으로 찄우세요)

이 스크은 처음 받으면 동작하지 않습니다. 아래 자리표시자를 본인 환경에 맾게 바꾔세요.

### Gmail (`~~email` 커넥터 필요)
- `<PRIMARY_EMAIL>` : 메인 Gmail 주소
- `<FORWARDED_EMAILS>` : 이 받은편지함으로 자동전달되는 보조 주소 (없으면 "없음")
  - 보조/외부(예야수 다옄 요청) 메일을 함께 보려면 해당 계정에서 메인 Gmail로 **자동전달**을 켜세요. (Gmail 외부계정가져오기/POP3는 2026년 종료 → 자동전달이 표준.)

### Slack (`~~chat` 커넥터 필요)
- `<SLACK_WORKSPACE>` : 워크스페이스 도메인 (예: myteam.slack.com)
- 읽을 채널 — `#채널명 = CHANNEL_ID` 형식으로:
  - `<#채널1 = CHANNEL_ID_1>`
  - `<#채널2 = CHANNEL_ID_2>`
  - `<#채널3 = CHANNEL_ID_3>`
  - 채널 ID는 `slack_search_channels`로 조회하거나 채널 링크 끝의 `C...` 값.
  - 질문이 많은 온보딩성 채널은 `<ONBOARDING_CHANNEL>`로 표시(핵심만 테마별 요약).

---

## 데이터 수집

**PART A — Gmail**: `search_threads` query `newer_than:1d`, pageSize 50 (`<PRIMARY_EMAIL>` + `<FORWARDED_EMAILS>` 포함).
접근 링크: `https://mail.google.com/mail/u/0/#inbox/{messageId}`
분류: ✍️ 응답·조치(🔴 결제·계약·금융·기한 / 🟡 그 외) · 📖 읽음만(채용·답스레터·단순알림) · 🗑️ 노이즈고, 본인 햹동 시스템알리).

**PART B — Slack**: `<SLACK_WORKSPACE>`의 설정채널을 `slack_read_channel`로 읽는다 (oldest = 7일 전 00:00 UTC epoch).
permalink: `https://<SLACK_WORKSPACE>/archives/{CHANNEL_ID}/p{TS(소수점 제거)}`

---

## 출력 형식 (Markdown, 소스별 분리 테이브)

제목: `🗓️ [오늘 날짜] 브리핑 (HH:MM KST)`

1) 📬 Gmail — `| 우선 | 발신자 | 제목(링크) | 조치 | 시각 |`
   - ✍️ 메일은 한 행씩한 햵에서 선택·섨택—미 통합니다 8 핵읭 阸증̙�부리 결제·금액 / 계약 원하면 `schedule` 스크로 등록.
- Gmail 외부계정을 포함하려면 해당 계정에서 Gmail로 자동전달을 설정하세요.
- 통에서 멄씰옄 스킬.
