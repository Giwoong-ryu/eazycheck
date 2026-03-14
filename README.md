# EazyCheck

AI 코딩 에이전트가 코드 만들기 전에 체크하고, 만든 후에 다시 체크합니다.

---

## 작동 방식

```
[코드 만들기 전]                    [코드 만든 후]
GATE: 과거 실수 확인          ->    sim: 미래 문제 시뮬레이션
토론: 설계 검토 (3+ 파일)     ->    Check: 도구 검증
```

설치하면 자동으로 작동합니다. 별도 명령어가 필요 없습니다.

---

## 설치

`~/.claude/` 폴더에 파일을 복사합니다:

```bash
# 1. 클론
git clone https://github.com/eazypick/eazycheck.git

# 2. 파일 복사
cp eazycheck/CLAUDE.md ~/.claude/CLAUDE.md
cp -r eazycheck/rules/ ~/.claude/rules/
cp -r eazycheck/skills/ ~/.claude/skills/
cp -r eazycheck/agents/ ~/.claude/agents/
cp -r eazycheck/state/ ~/.claude/state/
```

> 기존 CLAUDE.md가 있다면, EazyCheck 내용을 기존 파일에 붙여넣으세요.

---

## 기능

### GATE - 과거 실수 방지
코드 작성 전에 `patterns.json`을 확인하여 과거에 발생한 실수를 미리 경고합니다.

### 토론 - 설계 검토
3개 이상 파일을 변경할 때, architect와 code-reviewer가 자동으로 설계를 검토합니다.
- 기존 시스템과 충돌하는가?
- 이 설계가 6개월 후에도 유효한가?
- 규모 10배 시 버티는가?

### sim - 미래 시뮬레이션
코드 작성 후, 미래에 발생할 수 있는 문제를 시나리오로 탐색합니다.
`/sim` 명령으로 수동 실행도 가능합니다.

### Check - 도구 검증
pre-commit, CI, E2E 테스트로 기술적 결함을 자동 검출합니다.

---

## 쓸수록 똑똑해집니다

버그를 해결할 때마다 Claude가 `state/patterns.json`에 자동으로 패턴을 기록합니다.
다음 세션에서 같은 실수를 사전에 경고합니다.

기본 제공 패턴: 8개 (API 키 노출, 확장자 하드코딩, import 누락 등)

---

## 파일 구조

```
~/.claude/
├── CLAUDE.md              <- 핵심 규칙 (자동 로드)
├── rules/
│   └── eazycheck.md       <- 상세 흐름
├── skills/
│   └── sim/SKILL.md       <- /sim 시나리오 시뮬레이션
├── agents/
│   └── eazycheck-gate.md  <- GATE 검증 에이전트
└── state/
    ├── patterns.json      <- 실수 패턴 DB (자동 축적)
    └── routing.json       <- 라우팅 규칙
```

---

## 호환성

SKILL.md 표준을 따르므로 다음 도구에서 작동합니다:
- Claude Code
- OpenAI Codex CLI
- Gemini CLI
- Cursor
- Antigravity
- 기타 SKILL.md 지원 도구

---

## License

MIT
