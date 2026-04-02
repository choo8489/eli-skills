---
name: a11y-auditor
description: WCAG 2.1 AA 기준으로 웹 접근성을 검사합니다. UI 컴포넌트나 페이지의 접근성 점검 시 사용하세요.
user-invocable: true
allowed-tools: Read, Grep, Glob
---

# 접근성 감사 (a11y Audit)

대상: $ARGUMENTS

## 검사 항목

### 1. 인식의 용이성 (Perceivable)

#### 텍스트 대체
- 모든 `<img>`에 의미 있는 `alt` 속성
- 장식용 이미지는 `alt=""` 또는 `role="presentation"`
- SVG 아이콘에 `aria-label` 또는 `<title>`
- 비디오/오디오에 자막/대본

#### 색상 & 대비
- 텍스트 대비 4.5:1 이상 (AA 기준)
- 큰 텍스트(18pt+) 3:1 이상
- 색상만으로 정보 전달하지 않음 (색맹 고려)
- 다크모드에서도 대비 유지

### 2. 운용의 용이성 (Operable)

#### 키보드 접근
- 모든 인터랙티브 요소 Tab으로 접근 가능
- 포커스 순서가 논리적
- 포커스 트랩 없음 (모달 제외)
- 키보드 단축키 충돌 없음
- Skip Navigation 링크

#### 포커스 표시
- 포커스 인디케이터 시각적으로 명확
- `outline: none` 사용 시 대체 스타일 있음
- `:focus-visible` 활용

### 3. 이해의 용이성 (Understandable)

#### 시맨틱 HTML
- 올바른 heading 계층 (h1 → h2 → h3, 건너뛰기 없음)
- `<nav>`, `<main>`, `<aside>`, `<footer>` 랜드마크 사용
- 리스트는 `<ul>`/`<ol>`, 표는 `<table>`
- `<div>` 클릭 핸들러 대신 `<button>` 사용

#### 폼 접근성
- 모든 input에 연결된 `<label>`
- 에러 메시지가 해당 필드와 연결 (`aria-describedby`)
- 필수 필드 표시 (`aria-required`)
- 자동완성 속성 (`autocomplete`)

### 4. 견고성 (Robust)

#### ARIA 사용
- ARIA 속성이 올바르게 사용됨
- `role` 속성 적절성
- `aria-live` 동적 콘텐츠 알림
- `aria-expanded`, `aria-selected` 상태 관리
- 불필요한 ARIA 제거 (네이티브 HTML로 충분한 경우)

### 5. 모션 & 애니메이션
- `prefers-reduced-motion` 미디어 쿼리 대응
- 자동 재생 콘텐츠 정지 가능
- 깜빡임 3회/초 이하

## 출력 형식

```
## 접근성 감사 결과

**WCAG 2.1 AA 준수율**: ~X%

### 위반 사항 (심각도순)
1. 🔴 [WCAG 기준] 파일:라인 - 설명
   - 문제: [구체적 내용]
   - 수정: [코드 수정 방법]

### 경고
1. 🟡 [내용]

### 양호
- ✅ [잘 된 부분]

### 스크린 리더 테스트 포인트
1. [수동 테스트 필요 항목]
```

## 규칙
- 자동 검사 가능한 항목과 수동 확인 필요 항목 구분
- 수정 코드를 구체적으로 제시
- 과도한 ARIA 사용 경고 (네이티브 HTML 우선)
- 한국 웹 접근성 인증(KWCAG) 기준도 참고
