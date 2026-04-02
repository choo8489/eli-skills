---
name: doc-gen
description: API 문서, JSDoc/docstring, 타입 정의 등의 문서를 자동 생성합니다. 문서화가 필요할 때 사용하세요.
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit
---

# 문서 생성

대상: $ARGUMENTS

## 문서 생성 프로세스

### 1. 대상 분석
- 코드를 읽고 기능, 파라미터, 반환값 파악
- 기존 문서 스타일 확인 (프로젝트 컨벤션 따르기)
- 사용되는 언어에 맞는 문서 형식 선택

### 2. 문서 형식 (언어별)

#### JavaScript/TypeScript
```js
/**
 * 함수 설명
 * @param {string} name - 파라미터 설명
 * @returns {Promise<Result>} 반환값 설명
 * @throws {Error} 예외 상황 설명
 * @example
 * const result = await myFunction('test');
 */
```

#### Python
```python
def my_function(name: str) -> Result:
    """함수 설명.

    Args:
        name: 파라미터 설명

    Returns:
        반환값 설명

    Raises:
        ValueError: 예외 상황 설명

    Example:
        >>> result = my_function('test')
    """
```

#### Go
```go
// MyFunction 함수 설명.
//
// Parameters:
//   - name: 파라미터 설명
//
// Returns Result or error.
func MyFunction(name string) (Result, error) {
```

### 3. API 문서 (해당하는 경우)
- 엔드포인트 URL 및 메서드
- 요청/응답 스키마
- 인증 요구사항
- 사용 예시 (curl 또는 코드)

## 규칙
- 프로젝트의 기존 문서 스타일을 따르기
- "무엇"보다 "왜"를 설명
- 예제 코드를 반드시 포함
- 자명한 것은 문서화하지 않기 (예: `getName`은 이름을 반환합니다)
