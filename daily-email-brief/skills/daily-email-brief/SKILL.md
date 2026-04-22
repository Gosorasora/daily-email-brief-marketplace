---
name: daily-email-brief
description: Summarize Gmail inbox from the last 24 hours in Korean, grouped into important/reference/noise buckets. Triggers on "오늘 메일 요약", "이메일 요약해줘", "받은편지함 정리", "오늘 메일 중요한거", "daily email summary", "inbox brief", or similar daily inbox digest requests. Handles a primary Gmail plus auto-forwarded or POP3-synced secondary accounts in the same inbox.
---

# Daily Email Brief (Korean)

Produce a concise Korean-language summary of the user's Gmail inbox for the last 24 hours. Group messages into three buckets (중요 / 참고 / 노이즈) and deliver a tight, scannable digest.

## When to trigger

Activate for any request like:

- "오늘 메일 요약 해줘"
- "이메일 중요한거 요약"
- "받은편지함 정리해줘"
- "오늘 온 이메일 브리핑"
- "daily email brief / inbox summary"

Also activate automatically when invoked as a scheduled task.

## Requirements

The user must have a Gmail connector installed that exposes these tools:

- `search_threads` — list threads with a Gmail query
- `get_thread` — fetch full thread content

If Gmail tools are not available, tell the user to connect a Gmail connector first and stop.

## Workflow

### 1. Fetch threads

Call `search_threads` with:

- `query`: `newer_than:1d`
- `pageSize`: `50`

If the user asks for a different window (예: "지난 3일", "이번 주"), translate to Gmail's `newer_than:Nd` syntax. Default is 1 day.

### 2. Classify every thread

For each message, read `sender`, `subject`, `snippet`, `date`, `toRecipients` and sort into one of three buckets:

**🔴 중요 / 액션 필요**

- Emails from real people (non-automated senders)
- Work emails that need a reply
- Payments, orders, shipping, contracts, taxes, finance alerts
- Important notices from services the user actively uses / subscribed to
- Interviews and scheduling coordination
- Any email with an explicit deadline

**🟡 참고 (확인 권장)**

- Job alerts (LinkedIn auto-recommendations included)
- Newsletters the user subscribed to where the body has real value
- System/security notifications that were NOT triggered by the user's own action

**🗑️ 노이즈 (제외)**

- Subjects containing `[AD]`, `광고`, `프로모션`, `Sale`, `Deal`
- Pure marketing push
- System notifications about actions the user just did (2FA codes, password changes, verification codes they requested)

Noise is not summarized. Only the count is shown.

### 3. Expand important threads when needed

If a 🔴 important email's snippet is too thin to extract the core action, call `get_thread` with `messageFormat: "FULL_CONTENT"` and distill the body into 1–2 sentences.

For 🟡 reference emails, the snippet alone is enough — one-line summary only.

### 4. Output format

Use this Markdown shape exactly. Times are in KST (UTC+9); convert from the API's UTC timestamps.

```
## 📬 [YYYY-MM-DD] 이메일 요약 ([실행 시간])

총 N건 (중요 X / 참고 Y / 노이즈 Z)

### 🔴 중요/액션 필요 (X건)
- **[보낸이]** — [제목]: [핵심 내용 한두 문장]. _([HH:MM])_
  ...

### 🟡 참고 (Y건)
- **[보낸이]** — [제목] _([HH:MM])_
  ...

### 🗑️ 노이즈 Z건 (광고·자동 알림 등) — 생략

---
💡 [있을 때만] 오늘 특별히 챙겨야 할 일: ...
```

### 5. Edge cases

- If the search returns 0 threads: respond only with `최근 24시간 내 새 메일이 없습니다.` — nothing else.
- If all threads fall into the noise bucket: show the header + noise count, skip the 🔴/🟡 sections.
- Never invent content that is not in the snippet or thread body.
- Never add greetings, meta commentary, or explanations outside the digest itself.

## Style rules

- **Language**: Korean
- **Tone**: 간결하고 사실 위주. 과장 금지.
- **Boldness**: 보낸이만 `**bold**`, 제목은 평문
- **Length**: 각 🔴 항목은 2문장 이하, 🟡 항목은 1줄
- **Action callout**: 기한이 명시된 중요 메일이 있으면 맨 아래 `💡` 박스에 "오늘/내일까지 해야 할 일"로 한 번 더 강조

## Running as a scheduled task

This skill is designed to work both interactively and as an automated scheduled task (예: 매일 오전 8시 자동 실행). When run on a schedule, the user is not present — make reasonable defaults, never ask clarifying questions, and just produce the digest.
