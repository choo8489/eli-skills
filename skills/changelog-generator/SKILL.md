---
name: changelog-generator
description: Git 커밋 히스토리를 분석하여 카테고리별 체인지로그와 릴리즈 노트를 자동 생성합니다.
argument-hint: [version-or-date-range]
user-invocable: true
allowed-tools: Read, Grep, Glob, Bash
---

# 체인지로그 생성기

범위: $ARGUMENTS

## 프로세스

### 1. 커밋 수집
```bash
# 버전 간 커밋
git log v1.0.0..v1.1.0 --oneline --no-merges

# 날짜 범위
git log --since="2024-01-01" --until="2024-02-01" --oneline

# 마지막 릴리즈 이후
git log $(git describe --tags --abbrev=0)..HEAD --oneline
```

### 2. 커밋 분류

#### 카테고리
- **Features** — 새로운 기능 (`feat:`)
- **Bug Fixes** — 버그 수정 (`fix:`)
- **Performance** — 성능 개선 (`perf:`)
- **Security** — 보안 패치 (`security:`)
- **Breaking Changes** — 호환성 깨지는 변경 (`BREAKING CHANGE`)
- **Improvements** — 기존 기능 개선 (`improve:`, `enhance:`)
- **Documentation** — 문서 변경 (`docs:`)
- **Internal** — 리팩토링, 테스트, CI 등 (사용자 대상 노트에서 제외)

### 3. 메시지 가공
- 기술적 커밋 메시지를 사용자 친화적으로 재작성
- 관련 이슈/PR 번호 링크 연결
- Breaking Change는 마이그레이션 가이드 포함

### 4. 버전 결정 (Semantic Versioning)
- Breaking Change → Major (x.0.0)
- New Feature → Minor (0.x.0)
- Bug Fix → Patch (0.0.x)

## 출력 형식

### 개발자용 (CHANGELOG.md)
```markdown
## [1.2.0] - 2024-03-15

### Breaking Changes
- `auth.login()` 파라미터가 객체 형태로 변경 (#123)
  - 마이그레이션: `login(email, pw)` → `login({ email, password })`

### Features
- 소셜 로그인 (Google, GitHub) 지원 추가 (#145)
- 대시보드에 실시간 알림 기능 추가 (#152)

### Bug Fixes
- 로그인 세션이 15분 후 만료되는 문제 수정 (#140)

### Performance
- 메인 페이지 로딩 속도 40% 개선 (#148)
```

### 사용자용 (릴리즈 노트)
```markdown
# v1.2.0 릴리즈 노트

## 새로운 기능
- Google, GitHub 계정으로 간편 로그인이 가능합니다
- 대시보드에서 실시간 알림을 확인할 수 있습니다

## 개선사항
- 메인 페이지가 더 빠르게 로딩됩니다

## 버그 수정
- 로그인이 자동 해제되는 문제를 수정했습니다

## 주의사항
- 로그인 API를 사용하시는 분은 마이그레이션 가이드를 확인해주세요
```

## 규칙
- 내부 변경(refactor, test, ci)은 사용자용에서 제외
- Breaking Change는 항상 최상단에 배치
- 이전 CHANGELOG.md가 있으면 형식을 따름
- Keep a Changelog (keepachangelog.com) 형식 권장
