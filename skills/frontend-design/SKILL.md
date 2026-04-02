---
name: frontend-design
description: 디자인 시스템과 미학 원칙을 기반으로 세련된 UI 컴포넌트를 생성합니다. UI/프론트엔드 작업 시 사용하세요.
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit
---

# 프론트엔드 디자인

요청: $ARGUMENTS

## 디자인 철학

### 핵심 원칙
- **대담하고 의도적인 디자인**: 기본 AI 스타일이 아닌, 개성 있는 디자인
- **시각적 계층 구조**: 크기, 색상, 간격으로 중요도 전달
- **일관된 디자인 토큰**: 색상, 타이포, 간격을 시스템으로 관리
- **접근성 우선**: WCAG 2.1 AA 기준 준수

## 프로세스

### 1. 프로젝트 컨텍스트 파악
- 사용 중인 프레임워크 확인 (React, Vue, Svelte 등)
- CSS 방식 확인 (Tailwind, CSS Modules, styled-components 등)
- 기존 디자인 토큰/테마 확인
- 컴포넌트 라이브러리 확인 (shadcn/ui, MUI, Ant Design 등)

### 2. 디자인 토큰 정의
```
Colors:     primary, secondary, accent, neutral, semantic (success/warning/error)
Typography: heading (h1-h6), body (sm/md/lg), mono
Spacing:    4px 단위 시스템 (4, 8, 12, 16, 24, 32, 48, 64)
Radius:     sm(4), md(8), lg(12), xl(16), full
Shadow:     sm, md, lg (elevation 표현)
```

### 3. 컴포넌트 구현
- 반응형 설계 (mobile-first)
- 다크모드 지원
- 애니메이션은 의미 있는 곳에만 (transition: 150-300ms)
- 인터랙션 피드백 (hover, focus, active, disabled)

### 4. 레이아웃 패턴
- Flexbox/Grid 기반
- 컨테이너 최대폭 설정
- 일관된 간격 시스템 적용
- 콘텐츠 영역과 여백의 비율

## 출력 형식
1. 컴포넌트 코드 (프로젝트 스택에 맞게)
2. 필요한 디자인 토큰/스타일
3. 반응형 breakpoint별 동작 설명
4. 접근성 체크리스트

## 규칙
- 프로젝트의 기존 디자인 시스템을 먼저 확인하고 따르기
- 새 의존성 추가는 최소화
- 시맨틱 HTML 사용 (div 남용 금지)
- 색상 대비 4.5:1 이상 유지
