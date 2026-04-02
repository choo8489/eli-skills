---
name: monorepo-navigator
description: Turborepo, Nx, pnpm 워크스페이스를 탐색하고 패키지 간 의존성과 변경 영향을 분석합니다.
user-invocable: true
allowed-tools: Read, Grep, Glob, Bash
---

# 모노레포 네비게이터

작업: $ARGUMENTS

## 프로세스

### 1. 워크스페이스 감지
자동으로 모노레포 도구를 식별합니다:
- **Turborepo**: `turbo.json`
- **Nx**: `nx.json`, `project.json`
- **pnpm**: `pnpm-workspace.yaml`
- **npm workspaces**: `package.json > workspaces`
- **yarn workspaces**: `package.json > workspaces`
- **Lerna**: `lerna.json`

### 2. 패키지 매핑

```
## 워크스페이스 구조

apps/
├── web          → @company/web (Next.js)
├── api          → @company/api (Express)
└── mobile       → @company/mobile (React Native)

packages/
├── ui           → @company/ui (공유 컴포넌트)
├── config       → @company/config (ESLint, TS 설정)
├── utils        → @company/utils (유틸리티)
└── types        → @company/types (공유 타입)
```

### 3. 의존성 그래프

```
@company/web
  ├── @company/ui
  │   ├── @company/types
  │   └── @company/utils
  ├── @company/utils
  └── @company/types

@company/api
  ├── @company/utils
  └── @company/types
```

### 4. 영향도 분석 (Impact Analysis)

특정 패키지가 변경될 때 영향받는 범위:

```bash
# Turborepo
npx turbo run build --filter=@company/ui... --dry-run

# Nx
npx nx affected --target=build --base=main

# pnpm
pnpm -r --filter @company/ui... run build
```

```
@company/ui 변경 시 영향:
→ @company/web (직접 의존)
→ @company/mobile (직접 의존)
→ CI: web-build, mobile-build 재실행 필요
→ 테스트: web-test, mobile-test, ui-test 실행 필요
```

### 5. 일반적인 작업

#### 새 패키지 추가
```bash
mkdir packages/new-package
# package.json, tsconfig.json 생성
# 워크스페이스 설정에 등록
# 다른 패키지에서 의존성 추가
```

#### 패키지 간 의존성 추가
```bash
# pnpm
pnpm --filter @company/web add @company/ui

# npm workspaces
npm install @company/ui --workspace=apps/web
```

#### 특정 패키지만 빌드/테스트
```bash
# Turborepo
npx turbo run test --filter=@company/ui

# Nx
npx nx test @company/ui

# pnpm
pnpm --filter @company/ui test
```

## 출력 형식

```
## 모노레포 분석

**도구**: [Turborepo/Nx/pnpm]
**패키지 수**: N개 (apps: X, packages: Y)

### 패키지 맵
[구조 다이어그램]

### 의존성 그래프
[ASCII 그래프]

### 변경 영향 분석
[영향받는 패키지 목록]

### 권장 명령어
[상황에 맞는 빌드/테스트 명령]
```

## 규칙
- 워크스페이스 도구의 공식 명령어 사용
- 순환 의존성 발견 시 즉시 경고
- 불필요한 의존성 (사용하지 않는 패키지 import) 식별
- 버전 일관성 확인 (같은 외부 패키지의 다른 버전)
