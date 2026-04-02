---
name: project-bootstrap
description: 프로젝트 초기 세팅을 자동 생성합니다. 새 프로젝트 시작 시 보일러플레이트, CI/CD, 린트 설정 등을 한번에 구성하세요.
argument-hint: [stack] [project-name]
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash
---

# 프로젝트 부트스트랩

요구사항: $ARGUMENTS

## 프로세스

### 1. 스택 확인
사용자가 지정하지 않은 항목은 물어보기:
- **언어/런타임**: Node.js, Python, Go, Rust 등
- **프레임워크**: Next.js, FastAPI, Gin 등
- **DB**: PostgreSQL, MySQL, MongoDB, SQLite 등
- **패키지 매니저**: npm, pnpm, yarn, pip, poetry 등
- **테스트**: Jest, Vitest, pytest, Go test 등
- **배포 환경**: Vercel, AWS, Docker, Fly.io 등

### 2. 프로젝트 구조 생성

#### 공통 파일
```
project/
├── .gitignore            # 언어별 최적화
├── .env.example          # 환경변수 템플릿 (.env는 gitignore)
├── README.md             # 프로젝트 설명, 실행 방법
├── LICENSE               # 라이선스 (사용자 선택)
└── .github/
    └── workflows/
        └── ci.yml        # GitHub Actions CI
```

#### 개발 도구
```
├── .editorconfig         # 에디터 설정 통일
├── .prettierrc           # 코드 포맷터 (해당 시)
├── eslint.config.js      # 린터 (해당 시)
├── .husky/               # Git hooks (해당 시)
│   └── pre-commit
└── lint-staged.config.js # 커밋 시 린트 (해당 시)
```

#### Docker (선택)
```
├── Dockerfile
├── docker-compose.yml
└── .dockerignore
```

### 3. 설정 내용

#### CI/CD (GitHub Actions)
- 린트 체크
- 테스트 실행
- 빌드 확인
- (선택) 자동 배포

#### Git Hooks
- pre-commit: 린트 + 포맷팅
- commit-msg: 커밋 메시지 형식 검증

#### 환경 변수
- `.env.example`에 필요한 변수 목록
- `.env`는 `.gitignore`에 포함
- 설명 주석 포함

### 4. 초기 코드
- 최소한의 Hello World / Health Check 엔드포인트
- 기본 폴더 구조 (src, tests, docs 등)
- 첫 번째 테스트 파일

## 규칙
- 사용자에게 모든 선택을 강요하지 않고 합리적인 기본값 사용
- 불필요한 설정 파일은 생성하지 않기
- 각 파일에 간단한 주석으로 용도 설명
- 생성 후 실행 방법을 안내
