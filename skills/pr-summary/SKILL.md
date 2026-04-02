---
name: pr-summary
description: Git 변경사항을 분석하여 PR 제목과 본문을 자동 생성합니다. PR 작성 시 사용하세요.
user-invocable: true
allowed-tools: Read, Grep, Glob, Bash
---

# PR 요약 생성

컨텍스트: $ARGUMENTS

## 프로세스

### 1. 변경사항 수집
- `git diff` 및 `git log`으로 변경 내용 파악
- 변경된 파일 목록과 각 파일의 변경 유형 확인
- 커밋 메시지들을 참고

### 2. PR 작성

#### 제목
- 70자 이내
- 변경의 핵심을 한 문장으로
- 형식: `[type]: 변경 내용` (feat, fix, refactor, docs, test, chore)

#### 본문

```markdown
## Summary
- 변경 내용 요약 (1-3 bullet points)
- 왜 이 변경이 필요한지

## Changes
- 주요 변경사항 목록
- 파일별 또는 기능별로 정리

## Test Plan
- [ ] 테스트 방법 1
- [ ] 테스트 방법 2

## Notes (선택)
- 리뷰어가 알아야 할 사항
- 관련 이슈 링크
```

## 규칙
- 커밋 메시지를 그대로 나열하지 말고 의미 단위로 재구성
- 기술적 변경사항과 비즈니스 영향을 모두 포함
- 스크린샷이 필요한 UI 변경은 언급
