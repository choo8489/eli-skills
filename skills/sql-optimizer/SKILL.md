---
name: sql-optimizer
description: SQL 쿼리를 분석하고 EXPLAIN 기반으로 성능 최적화 및 인덱스를 제안합니다. 쿼리 성능 튜닝 시 사용하세요.
argument-hint: [query-or-file]
user-invocable: true
allowed-tools: Read, Grep, Glob, Bash
---

# SQL 쿼리 최적화

대상: $ARGUMENTS

## 분석 프로세스

### 1. 쿼리 이해
- 쿼리의 의도 파악
- 관련 테이블/인덱스 확인
- 예상 데이터 규모 파악

### 2. 문제 패턴 식별

#### 성능 킬러
- **SELECT ***: 필요한 컬럼만 명시
- **N+1 쿼리**: JOIN으로 통합 또는 배치 조회
- **서브쿼리 남용**: JOIN이나 CTE로 변환
- **LIKE '%keyword%'**: Full-text search 고려
- **OR 조건 다수**: UNION ALL로 분리
- **함수 래핑 컬럼**: `WHERE YEAR(date) = 2024` → 범위 조건으로

#### 인덱스 관련
- Sequential Scan (Full Table Scan) 발생 지점
- 복합 인덱스 순서 최적화 (선택도 높은 컬럼 먼저)
- Covering Index 가능 여부
- 인덱스 과다 (INSERT/UPDATE 성능 저하)

#### 조인 최적화
- 조인 순서 (작은 테이블 먼저)
- 조인 타입 (Nested Loop vs Hash Join vs Merge Join)
- 불필요한 조인 제거

### 3. EXPLAIN 분석 (제공된 경우)
```sql
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT)
SELECT ...
```

#### 주요 확인 포인트
- Seq Scan vs Index Scan
- Sort 비용 (외부 정렬 여부)
- Hash Join 메모리 사용량
- Rows Estimated vs Actual (통계 정확도)
- Buffer Hit Rate (캐시 효율)

### 4. 최적화 제안

#### 쿼리 리라이트
```sql
-- Before
SELECT * FROM orders WHERE YEAR(created_at) = 2024;

-- After (인덱스 활용 가능)
SELECT id, user_id, total
FROM orders
WHERE created_at >= '2024-01-01'
  AND created_at < '2025-01-01';
```

#### 인덱스 추가
```sql
-- 복합 인덱스 (WHERE + ORDER BY 커버)
CREATE INDEX idx_orders_user_created
ON orders (user_id, created_at DESC);
```

## 출력 형식

```
## SQL 최적화 결과

**원본 쿼리 문제**: [핵심 문제 한줄]
**예상 개선**: ~Nx 빠름

### 최적화된 쿼리
[수정된 SQL]

### 변경 사항
1. [변경 내용] — [이유]

### 권장 인덱스
[CREATE INDEX 문]

### EXPLAIN 비교 (가능한 경우)
Before: Seq Scan, cost=... rows=...
After:  Index Scan, cost=... rows=...
```

## DB 엔진별 참고
- **PostgreSQL**: CTE는 optimization fence 주의 (v12+에서 개선)
- **MySQL**: 서브쿼리 최적화가 약함, JOIN 선호
- **SQLite**: 인덱스 전략이 제한적, 쿼리 단순화 우선

## 규칙
- 최적화 전후 결과가 동일한지 반드시 확인
- 인덱스 추가 시 쓰기 성능 트레이드오프 언급
- 데이터 규모에 따라 효과가 달라짐을 명시
- 가능하면 EXPLAIN 실행 결과를 요청
