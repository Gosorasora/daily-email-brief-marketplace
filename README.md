# Sora Plugins — Cowork Plugin Marketplace

Cowork / Claude Code 에서 사용할 수 있는 플러그인 모음입니다.

## 포함된 플러그인

| 이름 | 설명 |
|---|---|
| [`daily-email-brief`](./daily-email-brief) | Gmail 최근 24시간 메일을 중요/참고/노이즈로 분류해 한국어로 요약 |

---

## 설치 방법 (사용자용)

Cowork 또는 Claude Code에서 아래 명령을 입력하세요.

**1. 마켓플레이스 추가**

```
/plugin marketplace add <GitHub 사용자명>/<리포 이름>
```

예시:

```
/plugin marketplace add sora/daily-email-brief-marketplace
```

**2. 플러그인 설치**

```
/plugin install daily-email-brief@sora-plugins
```

**3. Gmail 커넥터 연결** — 설치 후 본인 Gmail 계정을 연결하면 바로 사용 가능합니다.

**4. (선택) 최신 버전으로 업데이트**

```
/plugin marketplace update sora-plugins
```

---

## 배포자 안내 (Sora 본인용)

이 폴더를 GitHub 리포에 그대로 푸시하면 바로 공유 가능한 마켓플레이스가 됩니다.

```bash
cd daily-email-brief-marketplace
git init
git add .
git commit -m "Initial marketplace: daily-email-brief v0.1.0"
git branch -M main
git remote add origin https://github.com/<본인-GitHub-계정>/daily-email-brief-marketplace.git
git push -u origin main
```

리포 URL(`https://github.com/<계정>/daily-email-brief-marketplace`)을 공유하면 받는 사람은 위의 설치 명령으로 사용 가능합니다.

### 플러그인 업데이트 시

1. `daily-email-brief/.claude-plugin/plugin.json` 의 `version` 올리기
2. `.claude-plugin/marketplace.json` 의 해당 플러그인 `version` 도 같은 값으로 올리기
3. 변경사항 커밋 후 GitHub에 푸시
4. 사용자는 `/plugin marketplace update sora-plugins` 로 반영

### 플러그인 추가 시

새 플러그인 폴더를 리포에 추가하고 `.claude-plugin/marketplace.json` 의 `plugins` 배열에 항목만 하나 더 넣으면 됩니다.
