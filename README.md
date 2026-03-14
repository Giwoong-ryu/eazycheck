# EazyCheck

코드를 만들기 전에 과거 실수를 경고하고, 만든 후에 미래 문제를 시뮬레이션합니다.
CLAUDE.md 파일 하나로 설치 — 별도 명령어 없이 자동 작동합니다.

---

## 작동 방식

```
[코드 작성 전]                      [코드 작성 후]
GATE: 과거 실수 패턴 경고      ->   sim: 자동 시뮬레이션으로 잠재 문제 선제 탐지
토론: 설계 충돌 사전 검토      ->   Check: pre-commit / CI / E2E 자동 검증
```

3개 이상 파일을 건드릴 때만 토론과 sim이 작동합니다. 1-2개 파일은 바로 코드로 넘어갑니다.
불필요한 토큰 낭비 없이 필요한 순간에만 개입합니다.

---

## 설치

`~/.claude/` 폴더에 파일을 복사합니다:

```bash
git clone https://github.com/Giwoong-ryu/eazycheck.git

cp eazycheck/CLAUDE.md ~/.claude/CLAUDE.md
cp -r eazycheck/rules/ ~/.claude/rules/
cp -r eazycheck/skills/ ~/.claude/skills/
cp -r eazycheck/agents/ ~/.claude/agents/
cp -r eazycheck/state/ ~/.claude/state/
```

> 기존 CLAUDE.md가 있다면, EazyCheck 내용을 기존 파일 끝에 붙여넣으세요.

---

## 기능

### GATE - 같은 실수 두 번 하지 않기
코드 작성 전 `patterns.json`을 열어 과거에 발생한 실수와 대조합니다.
관련 패턴이 있으면 코드 생성 전에 경고를 출력합니다.

### 토론 - 코드 짜기 전에 설계 충돌 잡기
3개 이상 파일을 변경할 때, architect와 code-reviewer가 병렬로 설계를 검토합니다.
코드를 다 짜고 나서 뒤집는 대신, 시작 전에 이런 케이스를 미리 걸러냅니다.
- 변경 하나가 다른 파일을 조용히 망가뜨리는 경우
- 지금은 작동하지만 기능이 2-3개 더 붙으면 전부 다시 짜야 하는 구조
- 빠른 것 같아 보이지만 나중에 병목이 될 설계

### sim - 완성된 코드로 미래 시뮬레이션
코드 작성 직후 자동 실행됩니다. 실제 코드를 읽고 시간축 시나리오를 돌려
"지금은 괜찮지만 나중에 문제가 될" 부분을 미리 잡아냅니다.
`/sim` 명령으로 언제든 수동 실행도 가능합니다.

### Check - 도구로 기술적 결함 검출
pre-commit, CI, E2E 테스트를 순서대로 실행합니다.
API 키 노출, import 누락, 하드코딩 패턴을 자동으로 잡아냅니다.

---

## 쓸수록 똑똑해집니다

버그를 해결할 때마다 Claude가 `state/patterns.json`에 패턴을 자동으로 기록합니다.
다음 세션부터 같은 실수가 반복되기 전에 경고합니다.

기본 패턴 8개 포함 (API 키 노출, 확장자 하드코딩, import 누락 등)

---

## 파일 구조

```
~/.claude/
├── CLAUDE.md              <- 핵심 규칙 (세션 시작 시 자동 로드)
├── rules/
│   └── eazycheck.md       <- 상세 흐름
├── skills/
│   └── sim/SKILL.md       <- /sim 시나리오 스킬
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

---

## License

MIT
