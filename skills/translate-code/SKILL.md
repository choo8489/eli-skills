---
name: translate-code
description: 코드를 한 프로그래밍 언어에서 다른 언어로 변환합니다. 언어 간 코드 변환이 필요할 때 사용하세요.
argument-hint: [source-file] [target-language]
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit
---

# 코드 번역

대상: $ARGUMENTS

## 번역 프로세스

### 1. 원본 코드 분석
- 원본 언어 자동 감지 (또는 사용자 지정)
- 코드의 기능과 구조 파악
- 사용된 라이브러리/프레임워크 확인

### 2. 번역 전략

#### 직접 대응
- 양쪽 언어에 동일한 개념이 있는 경우 1:1 변환
- 예: `for` 루프, `if` 문, 기본 자료구조

#### 관용적 변환 (Idiomatic Translation)
- 대상 언어의 관용적 패턴으로 변환
- 예: Python의 list comprehension → JS의 map/filter
- 예: JS의 Promise → Python의 async/await
- 예: Java의 Stream → Kotlin의 sequence

#### 라이브러리 매핑
- 원본의 라이브러리를 대상 언어의 동등한 라이브러리로 매핑
- 예: Express (JS) → FastAPI (Python) → Gin (Go)
- 대응하는 라이브러리가 없으면 명시

### 3. 타입 시스템 변환
- 정적 → 동적: 타입 정보를 주석이나 docstring으로 보존
- 동적 → 정적: 적절한 타입 추론 및 명시
- Nullable 처리: 각 언어의 null safety 패턴 적용

## 출력 형식

```
## 코드 번역

**원본**: [언어] → **대상**: [언어]

### 변환된 코드
[코드]

### 주요 변환 사항
1. [변환 내용과 이유]

### 주의사항
- [동작 차이가 있을 수 있는 부분]
```

## 규칙
- 기계적 번역이 아닌 대상 언어답게 작성
- 원본의 주석도 번역
- 동작이 달라질 수 있는 부분은 반드시 명시
- 에러 핸들링은 대상 언어의 관용적 패턴으로
