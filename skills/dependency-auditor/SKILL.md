---
name: dependency-auditor
description: 프로젝트 의존성의 보안 취약점, 라이선스, 업데이트 상태를 검사합니다. 의존성 점검 시 사용하세요.
user-invocable: true
allowed-tools: Read, Grep, Glob, Bash
---

# 의존성 감사

대상: $ARGUMENTS

## 검사 항목

### 1. 보안 취약점
- 알려진 CVE가 있는 패키지 식별
- 심각도별 분류 (Critical, High, Medium, Low)
- 취약점이 실제 코드에서 사용되는지 확인
- 패치 가능 여부 및 업그레이드 경로

### 2. 라이선스 호환성
- 각 의존성의 라이선스 확인
- 프로젝트 라이선스와 호환성 검증
- 주의 필요 라이선스 표시:
  - GPL (전염성)
  - AGPL (서버사이드도 전염)
  - SSPL (클라우드 서비스 제한)
  - 라이선스 없음 (저작권 주장 가능)

### 3. 업데이트 상태
- 현재 버전 vs 최신 버전
- Major 업데이트 (Breaking Changes 주의)
- 지원 종료(EOL) 패키지
- 마지막 업데이트 날짜 (유지보수 중단 여부)

### 4. 의존성 건강도
- 직접 의존성 vs 간접(transitive) 의존성 수
- 중복 의존성 (같은 패키지 다른 버전)
- 불필요한 의존성 (사용하지 않는 패키지)
- 번들 사이즈 영향

## 언어별 검사 방법

### Node.js
```bash
npm audit
npx depcheck          # 미사용 의존성
npx license-checker   # 라이선스
```

### Python
```bash
pip audit
safety check
pip-licenses
```

### Go
```bash
govulncheck ./...
go mod tidy           # 미사용 정리
```

## 출력 형식

```
## 의존성 감사 결과

**총 의존성**: N개 (직접: X, 간접: Y)
**취약점**: 🔴 Critical: N / 🟡 High: N / 🟢 OK

### 보안 이슈
1. [패키지@버전] - CVE-XXXX (심각도)
   → 수정: 버전 X.Y.Z으로 업그레이드

### 라이선스 주의
1. [패키지] - GPL-3.0
   → [호환성 설명]

### 업데이트 권장
1. [패키지] 현재 v1.0 → 최신 v3.0 (Major)

### 정리 대상
1. [패키지] - 코드에서 사용되지 않음
```

## 규칙
- `npm audit` / `pip audit` 등 네이티브 도구를 먼저 실행
- 취약점은 실제 영향도를 판단 (사용하지 않는 기능의 취약점은 낮게)
- 업데이트 시 breaking changes 경고
- 한꺼번에 모든 것을 업데이트하지 말고 단계적으로 제안
