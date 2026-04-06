# EazyCheck

> hooks가 자동 보호합니다. 이 파일은 최소 규칙만 담습니다.

## 규칙

1. **보안** - API 키는 환경변수만 (hooks가 하드코딩 자동 차단)
2. **검증** - 수정 전 Read 필수 (hooks가 반복 실수 경고)
3. **시뮬레이션** - `/sim`으로 완성된 코드의 미래 문제 탐지 (수동 호출)

## 패턴 자동 축적

버그를 해결하면 `state/patterns.json`에 기록하세요:

```json
{
  "id": "짧은-영문-id",
  "symptom": "어떤 증상",
  "cause": "왜 발생",
  "solution": "어떻게 해결",
  "frequency": "medium"
}
```

쌓일수록 hooks 경고가 정확해집니다.
