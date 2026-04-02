---
name: security-check
description: OWASP Top 10 기준으로 코드의 보안 취약점을 검사합니다. 보안 점검이 필요할 때 사용하세요.
user-invocable: true
allowed-tools: Read, Grep, Glob
---

# 보안 점검

대상: $ARGUMENTS

## 검사 항목 (OWASP Top 10 기반)

### 1. 인젝션 (Injection)
- SQL 인젝션: 파라미터화 쿼리 사용 여부
- Command 인젝션: 쉘 명령 조합 시 이스케이핑
- XSS: 사용자 입력의 HTML 이스케이핑
- Template 인젝션: 서버사이드 템플릿 안전성

### 2. 인증 및 세션 관리
- 비밀번호 해싱 (bcrypt/argon2 사용 여부)
- 세션 토큰 안전성
- JWT 검증 로직
- 브루트포스 방어 (rate limiting)

### 3. 민감 데이터 노출
- 하드코딩된 시크릿 (API 키, 비밀번호)
- 로그에 민감 정보 출력
- 에러 메시지에 내부 정보 노출
- HTTPS 사용 여부

### 4. 접근 제어
- 권한 검사 누락 (IDOR)
- 수평/수직 권한 상승 가능성
- API 엔드포인트 인가 처리

### 5. 보안 설정
- CORS 설정 적절성
- CSP 헤더 설정
- 의존성 취약점 (알려진 CVE)
- 디버그 모드 비활성화

### 6. 기타
- Path Traversal
- SSRF (Server-Side Request Forgery)
- Prototype Pollution (JavaScript)
- Deserialization 취약점

## 출력 형식

```
## 보안 점검 결과

**전체 등급**: 🔴 위험 / 🟡 주의 / 🟢 양호

### 발견된 취약점
1. [심각도] 파일:라인 - 취약점 설명
   - 위험: [악용 시나리오]
   - 수정: [구체적인 수정 방법]

### 권장 사항
- [추가 보안 강화 제안]
```

## 규칙
- 오탐(false positive)을 줄이기 위해 컨텍스트를 충분히 확인
- 각 취약점에 구체적인 수정 코드를 제시
- 심각도 순으로 정렬 (Critical > High > Medium > Low)
