---
name: env-secrets-manager
description: 환경변수와 시크릿을 관리하고, 누출을 감지하며, 환경별 설정을 검증합니다. 설정 관리나 보안 점검 시 사용하세요.
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit
---

# 환경변수 & 시크릿 관리

대상: $ARGUMENTS

## 기능

### 1. 시크릿 누출 탐지
코드베이스에서 하드코딩된 시크릿을 검색합니다.

#### 탐지 패턴
- API 키: `sk-`, `pk_`, `api_key`, `apiKey`
- 토큰: `token`, `bearer`, `jwt`
- 비밀번호: `password`, `passwd`, `secret`
- 연결 문자열: `mongodb://`, `postgres://`, `mysql://`
- AWS: `AKIA`, `aws_secret`
- 개인키: `-----BEGIN RSA PRIVATE KEY-----`
- Base64 인코딩된 시크릿

#### 검사 위치
- 소스 코드 파일
- 설정 파일 (yaml, json, toml)
- Docker/docker-compose 파일
- CI/CD 파이프라인 파일
- Git 히스토리 (과거 커밋에 노출된 시크릿)

### 2. .env 파일 관리

#### 파일 구조
```
.env                # 로컬 개발용 (gitignore)
.env.example        # 템플릿 (커밋 가능, 값은 비움)
.env.test           # 테스트 환경
.env.production     # 프로덕션 참조용 (실제 값 없이)
```

#### .env.example 생성
```bash
# Database
DATABASE_URL=           # PostgreSQL 연결 URL
DATABASE_POOL_SIZE=10   # 커넥션 풀 크기

# Authentication
JWT_SECRET=             # JWT 서명 키 (최소 32자)
JWT_EXPIRES_IN=1h       # 토큰 만료 시간

# External APIs
STRIPE_SECRET_KEY=      # Stripe API 시크릿 키
SENDGRID_API_KEY=       # SendGrid 메일 API 키

# App Config
NODE_ENV=development
PORT=3000
LOG_LEVEL=info
```

### 3. 환경 간 검증
- `.env.example`과 실제 `.env`의 키 일치 여부
- 프로덕션 필수 변수 누락 확인
- 개발 환경에서 프로덕션 값 사용 경고
- 타입 검증 (숫자, URL, 이메일 등)

### 4. 타입 안전한 환경변수 로딩

#### TypeScript (Zod)
```typescript
import { z } from 'zod';

const envSchema = z.object({
  DATABASE_URL: z.string().url(),
  PORT: z.coerce.number().default(3000),
  NODE_ENV: z.enum(['development', 'production', 'test']),
  JWT_SECRET: z.string().min(32),
});

export const env = envSchema.parse(process.env);
```

#### Python (pydantic)
```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_url: str
    port: int = 3000
    jwt_secret: str

    class Config:
        env_file = ".env"
```

## 출력 형식

```
## 환경변수 점검 결과

### 시크릿 누출 탐지
🔴 발견: N건
1. [파일:라인] - [시크릿 종류] 하드코딩됨
   → 환경변수로 이동: `process.env.XXX`

### .env 검증
- 총 변수: N개
- 누락: [목록]
- 미사용: [목록]

### 권장 .env.example
[생성된 템플릿]

### 타입 안전 로더
[생성된 코드]
```

## 규칙
- 탐지된 시크릿의 실제 값을 출력에 포함하지 않기
- .env 파일이 .gitignore에 포함되어 있는지 확인
- 시크릿 로테이션 필요성 경고 (노출된 경우)
- 환경변수 네이밍: SCREAMING_SNAKE_CASE
