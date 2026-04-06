# EazyCheck

AI 코딩 도구가 반복하는 실수를 자동으로 막아줍니다.

hooks로 동작해서 토큰을 쓰지 않습니다.

---

## 설치

Python 3.8 이상 필요.

```bash
git clone https://github.com/Giwoong-ryu/eazycheck.git
cd eazycheck

# Mac/Linux
bash install.sh

# Windows PowerShell
.\install.ps1
```

git이 없으면 [ZIP 다운로드](https://github.com/Giwoong-ryu/eazycheck/archive/refs/heads/main.zip).

설치 후 Claude Code 재시작하면 끝.

---

## 뭘 해주나

### API 키 하드코딩 차단

코드에 API 키를 쓰면 저장 자체를 막습니다.

```python
api_key = "sk-abc123..."   # 차단됨
api_key = os.getenv("KEY")  # 통과
```

Google, OpenAI, GitHub, AWS 키 패턴을 감지합니다.

### 반복 실수 경고

AI가 자주 하는 실수 패턴을 `patterns.json`에 쌓아두고, 비슷한 작업을 할 때 미리 경고합니다.

- 파일 안 읽고 수정하려 할 때
- 구버전 AI 모델명 쓰려 할 때
- TypeScript 타입 바꿀 때 사용처 누락

기본 22개 패턴 포함. 쓸수록 늘어납니다.

### /sim

코드 완성 후 `/sim` 입력하면 "3개월 후 + 10배 규모"에서 터질 곳을 찾아줍니다. 이건 토큰을 씁니다.

---

## 구조

설치하면 `~/.claude/`에 이렇게 들어갑니다:

```
hooks/
  eazycheck-gate.py        ← Write/Edit 할 때마다 실행. API 키 차단 + 실수 경고
  eazycheck-smart-gate.py  ← 메시지 보낼 때마다 실행. 관련 패턴 경고

skills/sim/SKILL.md        ← /sim 명령어

state/patterns.json        ← 실수 패턴 DB
```

`settings.json`의 `PreToolUse`와 `UserPromptSubmit` hook으로 등록됩니다. 설치 스크립트가 자동으로 해줍니다.

---

## 토큰

| 기능 | 토큰 |
|------|------|
| API 키 차단 | 0 (Python) |
| 실수 경고 | 0 (Python) |
| 패턴 매칭 경고 | ~50 (매칭 시만) |
| /sim | ~500 (수동 호출) |

---

## 패턴 추가

버그 고치면 AI한테 "패턴에 추가해"라고 하거나 직접 편집:

```json
{
  "id": "짧은-영문-id",
  "symptom": "증상",
  "cause": "원인",
  "solution": "해결법",
  "frequency": "medium"
}
```

`high` 이상이면 hooks가 자동 경고합니다.

---

## 호환

- **Claude Code** — hooks + /sim 전체 동작
- **Antigravity** — hooks + /sim 전체 동작
- Cursor, Codex CLI — SKILL.md만 동작 (hooks 미지원)

---

## 기존 설정 보호

- CLAUDE.md → 끝에 추가 (덮어쓰지 않음)
- patterns.json → 기존 유지 (프리셋 별도 저장)
- settings.json → 기존 hooks 유지하고 추가만
- sim/SKILL.md → 기존 있으면 건너뜀

MIT License
