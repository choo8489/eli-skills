---
name: db-schema-designer
description: 요구사항을 기반으로 DB 스키마, 마이그레이션, ERD를 설계합니다. 데이터베이스 설계 시 사용하세요.
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash
---

# DB 스키마 설계

요구사항: $ARGUMENTS

## 설계 프로세스

### 1. 요구사항 분석
- 도메인 엔티티 식별
- 엔티티 간 관계 파악 (1:1, 1:N, N:M)
- 필수/선택 필드 구분
- 조회 패턴 예측 (읽기 vs 쓰기 비율)

### 2. 스키마 설계

#### 정규화 원칙
- 기본: 3NF까지 정규화
- 읽기 성능이 중요하면 선택적 비정규화
- 비정규화 시 사유 명시

#### 네이밍 컨벤션
- 테이블: snake_case, 복수형 (users, orders)
- 컬럼: snake_case (created_at, user_id)
- 인덱스: idx_{table}_{columns}
- FK: fk_{table}_{ref_table}

#### 필수 컬럼
- `id`: Primary Key (UUID 또는 auto-increment)
- `created_at`: 생성 시각
- `updated_at`: 수정 시각
- 소프트 삭제 시 `deleted_at`

### 3. 인덱스 설계
- WHERE 절에 자주 사용되는 컬럼
- JOIN에 사용되는 FK 컬럼
- ORDER BY에 사용되는 컬럼
- 복합 인덱스 순서 최적화 (선택도 높은 것 먼저)

### 4. 보안
- Row Level Security (RLS) 정책
- 민감 데이터 암호화 컬럼 표시
- 감사 로그 테이블 설계

## 출력 형식

```
## DB 스키마

### ERD (ASCII)
[테이블 관계 다이어그램]

### 테이블 정의
[CREATE TABLE 문]

### 인덱스
[CREATE INDEX 문]

### 마이그레이션
[UP/DOWN 마이그레이션 SQL]

### 시드 데이터
[INSERT 문 - 테스트용 데이터]
```

## 규칙
- 프로젝트에서 사용하는 DB 엔진에 맞는 SQL 문법 사용
- ORM을 쓴다면 ORM 마이그레이션 형식으로도 제공
- 롤백 가능한 마이그레이션 작성
- 대량 데이터 시 파티셔닝 고려
