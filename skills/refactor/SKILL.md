---
name: refactor
description: 코드 구조를 개선하고 클린 코드 원칙을 적용합니다. 리팩토링이 필요할 때 사용하세요.
user-invocable: true
allowed-tools: Read, Grep, Glob, Edit
---

# 리팩토링

대상: $ARGUMENTS

## 리팩토링 원칙

### 동작을 바꾸지 않는다
- 기존 테스트가 모두 통과해야 함
- 외부 인터페이스(API, 함수 시그니처)는 가능한 유지
- 한 번에 하나의 리팩토링만 적용

### 점진적으로 진행한다
- 작은 단위로 변경
- 각 단계가 독립적으로 동작 가능해야 함

## 리팩토링 패턴

### 함수/메서드 레벨
- **Extract Function**: 긴 함수를 의미 단위로 분리
- **Inline Function**: 불필요한 추상화 제거
- **Rename**: 의도가 명확한 이름으로 변경
- **Simplify Conditional**: 복잡한 조건문 단순화

### 클래스/모듈 레벨
- **Extract Class**: 책임이 많은 클래스 분리
- **Move Method**: 적절한 위치로 이동
- **Replace Inheritance with Composition**: 상속 대신 합성

### 데이터 레벨
- **Replace Magic Number**: 상수로 추출
- **Encapsulate Field**: 직접 접근 대신 메서드 사용
- **Replace Temp with Query**: 임시 변수를 함수로 대체

## 출력 형식

```
## 리팩토링 계획

**목표**: [개선하려는 점]
**범위**: [영향받는 파일/함수]
**위험도**: 낮음 / 중간 / 높음

### 변경 사항
1. [변경 내용] - [이유]
2. ...

### 변경 전/후 비교
[코드 비교]
```

## 규칙
- 리팩토링과 기능 추가를 동시에 하지 않기
- 테스트가 없는 코드는 테스트 추가 먼저 제안
- 과도한 추상화 금지 (3회 이상 반복될 때만 추출)
