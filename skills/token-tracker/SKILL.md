---
name: token-tracker
description: Claude API 토큰 사용량과 비용을 분석하고, 최적화 방안을 제시합니다. AI 비용 관리 시 사용하세요.
user-invocable: true
allowed-tools: Read, Grep, Glob, Bash
---

# 토큰 사용량 & 비용 분석

대상: $ARGUMENTS

## 분석 항목

### 1. 토큰 사용량 분석

#### 토큰 종류
- **Input Tokens**: 프롬프트에 보내는 토큰
- **Output Tokens**: Claude가 생성하는 토큰
- **Cache Creation**: 프롬프트 캐시 최초 생성
- **Cache Read**: 캐시 히트 (90% 할인)

#### 모델별 가격 (참고용)
```
Claude Opus:   Input $15/MTok, Output $75/MTok
Claude Sonnet: Input $3/MTok,  Output $15/MTok
Claude Haiku:  Input $0.80/MTok, Output $4/MTok
```

### 2. 비용 최적화 전략

#### 프롬프트 최적화
- 불필요한 컨텍스트 제거
- 시스템 프롬프트 간결하게
- Few-shot 예시 최소화 (필요한 만큼만)
- 구조화된 출력 요청 (JSON 등)

#### 캐싱 활용
```
- 시스템 프롬프트: 캐시 적중률 극대화
- 반복 컨텍스트: prompt caching 사용
- 캐시 TTL: 5분 (자동)
- 캐시 절약: 입력 토큰 비용 90% 감소
```

#### 모델 선택
```
Simple tasks   → Haiku  (빠르고 저렴)
General tasks  → Sonnet (균형)
Complex tasks  → Opus   (최고 품질)
```

#### 배치 처리
- Batch API 사용 시 50% 할인
- 24시간 이내 결과 반환
- 대량 처리에 적합

### 3. 코드 내 사용량 분석

#### 분석 대상
- API 호출 코드에서 토큰 사용 패턴
- 불필요하게 큰 프롬프트
- 반복 호출되는 동일한 요청 (캐싱 기회)
- max_tokens 설정 적절성
- 스트리밍 vs 논스트리밍 선택

#### 최적화 코드 패턴
```typescript
// Before: 매번 전체 컨텍스트 전송
const response = await claude.messages.create({
  model: 'claude-sonnet-4-6-20250514',
  system: longSystemPrompt,  // 매번 재전송
  messages: [...history, newMessage],
});

// After: 캐싱 활용
const response = await claude.messages.create({
  model: 'claude-sonnet-4-6-20250514',
  system: [{
    type: 'text',
    text: longSystemPrompt,
    cache_control: { type: 'ephemeral' }  // 캐시 활성화
  }],
  messages: [...history, newMessage],
});
```

## 출력 형식

```
## 토큰 & 비용 분석

### 현재 사용 패턴
- 일 평균 호출: N회
- 일 평균 토큰: Input Xk, Output Yk
- 월 예상 비용: $Z

### 최적화 기회
1. [기회] — 예상 절감: ~X%
2. ...

### 권장 변경
1. [구체적 코드/설정 변경]

### 최적화 후 예상
- 월 예상 비용: $Z' (X% 절감)
```

## 규칙
- 가격 정보는 변동될 수 있으므로 공식 문서 확인 권장
- 비용만 최적화하지 말고 품질 트레이드오프 함께 제시
- 캐싱은 최소 1024 토큰 이상의 프롬프트에서 효과적
- 보안: API 키가 코드에 하드코딩되어 있으면 경고
