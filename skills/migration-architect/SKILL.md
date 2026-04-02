---
name: migration-architect
description: 안전한 DB 마이그레이션을 계획하고, 롤백 스크립트와 호환성 검증을 수행합니다. 스키마 변경 시 사용하세요.
argument-hint: [migration-description]
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash
---

# DB 마이그레이션 설계

변경 사항: $ARGUMENTS

## 안전성 프레임워크

### 위험도 평가
- **Low**: 컬럼 추가 (nullable), 인덱스 추가
- **Medium**: 컬럼명 변경, 기본값 변경, 인덱스 삭제
- **High**: 컬럼 삭제, 타입 변경, 테이블 삭제
- **Critical**: 대규모 데이터 변환, FK 변경

### Zero-Downtime 마이그레이션 원칙
1. **호환성 유지**: 코드 배포와 마이그레이션은 별도로 안전하게
2. **단계별 진행**: 한 번에 하나의 변경만
3. **롤백 가능**: 모든 변경은 되돌릴 수 있어야 함
4. **락 최소화**: 큰 테이블의 장시간 락 방지

## 마이그레이션 패턴

### 컬럼 추가
```sql
-- Safe: nullable 컬럼 추가 (즉시)
ALTER TABLE users ADD COLUMN phone VARCHAR(20);

-- Safe: 기본값 있는 컬럼 (PG 11+ 즉시, MySQL은 테이블 리빌드)
ALTER TABLE users ADD COLUMN status VARCHAR(20) DEFAULT 'active';
```

### 컬럼 삭제 (3단계)
```
Step 1: 코드에서 해당 컬럼 사용 제거 → 배포
Step 2: 컬럼을 nullable로 변경 (안전 장치)
Step 3: 컬럼 삭제 마이그레이션
```

### 컬럼명 변경 (4단계)
```
Step 1: 새 컬럼 추가
Step 2: 양쪽 컬럼에 쓰기 (dual-write)
Step 3: 기존 데이터 복사, 코드를 새 컬럼으로 전환
Step 4: 기존 컬럼 삭제
```

### 타입 변경 (안전하게)
```
Step 1: 새 타입의 컬럼 추가
Step 2: 트리거로 동기화 또는 dual-write
Step 3: 배치로 기존 데이터 변환
Step 4: 코드 전환 → 기존 컬럼 삭제
```

### 대규모 데이터 마이그레이션
```sql
-- 배치 처리 (락 최소화)
DO $$
DECLARE
  batch_size INT := 1000;
  affected INT;
BEGIN
  LOOP
    UPDATE users
    SET new_col = transform(old_col)
    WHERE new_col IS NULL
    LIMIT batch_size;

    GET DIAGNOSTICS affected = ROW_COUNT;
    EXIT WHEN affected = 0;

    PERFORM pg_sleep(0.1);  -- 부하 조절
  END LOOP;
END $$;
```

## 출력 형식

```
## 마이그레이션 계획

**변경**: [요약]
**위험도**: Low / Medium / High / Critical
**예상 락 시간**: ~Xs
**영향 테이블**: [목록]

### Forward Migration
[SQL 스크립트]

### Rollback Migration
[SQL 스크립트]

### 실행 순서
1. [단계별 실행 계획]

### 검증 쿼리
[마이그레이션 성공 확인용 SQL]

### 주의사항
- [락, 성능, 데이터 정합성 관련]
```

## 규칙
- 롤백 스크립트는 반드시 포함
- 프로덕션 테이블 크기를 고려한 락 시간 추정
- ORM 마이그레이션 도구 사용 시 해당 형식으로도 제공
- 데이터 유실 가능성이 있으면 반드시 경고
- 백업 확인 후 실행 권장
