# eli-skills

Claude Code 커스텀 스킬 모음집 — 개발 생산성을 높여주는 **35개**의 슬래시 커맨드

## 설치

```bash
# 개인용 (모든 프로젝트에서 사용)
cp -r skills/* ~/.claude/skills/

# 특정 프로젝트용
cp -r skills/* /your/project/.claude/skills/
```

설치 후 Claude Code에서 `/스킬이름`으로 바로 사용 가능합니다.

---

## 스킬 목록

### 코드 작성 & 품질

| 명령어 | 설명 |
|--------|------|
| `/code-review` | 코드 품질, 버그, 성능, 가독성 리뷰 |
| `/refactor` | 클린 코드 원칙 적용 리팩토링 |
| `/test-gen` | 함수/모듈 테스트 코드 자동 생성 |
| `/doc-gen` | JSDoc, docstring, API 문서 자동 생성 |
| `/complexity-analyzer` | 순환 복잡도, 기술 부채 측정 & 리팩토링 우선순위 |
| `/storybook-writer` | Storybook CSF3 스토리 자동 생성 |

### 타입 & 스키마

| 명령어 | 설명 |
|--------|------|
| `/type-generator` | JSON/Prisma/API → TypeScript 타입 & Zod 스키마 생성 |
| `/openapi-architect` | 코드/요구사항에서 OpenAPI 3.1 스펙 생성 |
| `/graphql-designer` | GraphQL 스키마 설계 (Federation, DataLoader, 페이지네이션) |
| `/regex-builder` | 자연어 → 정규식 변환 & 정규식 해석 |

### 이해 & 학습

| 명령어 | 설명 |
|--------|------|
| `/explain-code` | 비유, 다이어그램으로 코드를 쉽게 설명 |
| `/codebase-to-course` | 코드베이스를 온보딩/학습 자료로 변환 |

### 디버깅 & 성능

| 명령어 | 설명 |
|--------|------|
| `/debug` | 에러 원인 분석 및 해결 방법 제시 |
| `/performance-profiler` | 성능 병목 분석 및 최적화 제안 |
| `/sql-optimizer` | SQL 쿼리 분석 & EXPLAIN 기반 최적화 |
| `/log-analyzer` | 로그 파싱, 에러 패턴 식별, 이상 징후 분석 |

### 보안

| 명령어 | 설명 |
|--------|------|
| `/security-check` | OWASP Top 10 기준 보안 취약점 검사 |
| `/dependency-auditor` | 의존성 취약점, 라이선스, 업데이트 점검 |
| `/env-secrets-manager` | 환경변수 관리, 시크릿 누출 감지, 환경별 검증 |

### 아키텍처 & 설계

| 명령어 | 설명 |
|--------|------|
| `/architecture-review` | 확장성, 장애 대응, 비용, 보안 아키텍처 리뷰 |
| `/db-schema-designer` | 요구사항 → 스키마, 마이그레이션, ERD 설계 |
| `/migration-architect` | 안전한 DB 마이그레이션 계획 & 롤백 스크립트 |
| `/frontend-design` | 디자인 시스템 기반 UI 컴포넌트 생성 |
| `/monorepo-navigator` | Turborepo/Nx/pnpm 워크스페이스 탐색 & 영향 분석 |

### 접근성

| 명령어 | 설명 |
|--------|------|
| `/a11y-auditor` | WCAG 2.1 AA 기준 웹 접근성 검사 |

### Git & 워크플로우

| 명령어 | 설명 |
|--------|------|
| `/quick-commit` | 변경사항 분석 후 커밋 메시지 자동 생성 |
| `/pr-summary` | PR 제목 및 본문 자동 작성 |
| `/git-worktree` | 병렬 브랜치 작업 (워크트리 관리) |
| `/changelog-generator` | Git 커밋 → 카테고리별 릴리즈 노트 생성 |

### 운영 & SRE

| 명령어 | 설명 |
|--------|------|
| `/incident-commander` | 장애 대응 가이드 & 포스트모템 문서 작성 |
| `/token-tracker` | Claude API 토큰 사용량/비용 분석 & 최적화 |

### 도구 & 생성

| 명령어 | 설명 |
|--------|------|
| `/project-bootstrap` | 새 프로젝트 보일러플레이트 + CI/CD 한번에 세팅 |
| `/mcp-builder` | MCP(Model Context Protocol) 서버 설계 및 구현 |
| `/translate-code` | 프로그래밍 언어 간 코드 변환 |

### 콘텐츠

| 명령어 | 설명 |
|--------|------|
| `/voice-dna` | 글쓰기 스타일 추출 및 톤앤매너 적용 |

---

## 프로젝트 구조

```
eli-skills/
├── README.md
└── skills/
    ├── a11y-auditor/SKILL.md
    ├── architecture-review/SKILL.md
    ├── changelog-generator/SKILL.md
    ├── codebase-to-course/SKILL.md
    ├── code-review/SKILL.md
    ├── complexity-analyzer/SKILL.md
    ├── db-schema-designer/SKILL.md
    ├── debug/SKILL.md
    ├── dependency-auditor/SKILL.md
    ├── doc-gen/SKILL.md
    ├── env-secrets-manager/SKILL.md
    ├── explain-code/SKILL.md
    ├── frontend-design/SKILL.md
    ├── git-worktree/SKILL.md
    ├── graphql-designer/SKILL.md
    ├── incident-commander/SKILL.md
    ├── log-analyzer/SKILL.md
    ├── mcp-builder/SKILL.md
    ├── migration-architect/SKILL.md
    ├── monorepo-navigator/SKILL.md
    ├── openapi-architect/SKILL.md
    ├── performance-profiler/SKILL.md
    ├── pr-summary/SKILL.md
    ├── project-bootstrap/SKILL.md
    ├── quick-commit/SKILL.md
    ├── refactor/SKILL.md
    ├── regex-builder/SKILL.md
    ├── security-check/SKILL.md
    ├── sql-optimizer/SKILL.md
    ├── storybook-writer/SKILL.md
    ├── test-gen/SKILL.md
    ├── token-tracker/SKILL.md
    ├── translate-code/SKILL.md
    ├── type-generator/SKILL.md
    └── voice-dna/SKILL.md
```

## 커스텀 스킬 만들기

`skills/` 안에 새 폴더를 만들고 `SKILL.md`를 작성하면 됩니다:

```yaml
---
name: my-skill
description: 언제 이 스킬을 사용할지 설명 (250자 이내)
user-invocable: true
allowed-tools: Read, Grep, Glob
---

# 스킬 제목

여기에 Claude가 따를 지침을 마크다운으로 작성
```

### 주요 옵션

| 필드 | 설명 |
|------|------|
| `name` | 스킬 ID (소문자, 하이픈) |
| `description` | 스킬 설명 — Claude가 자동 호출 여부를 판단하는 기준 |
| `user-invocable` | `true`: `/명령어`로 호출 가능 |
| `disable-model-invocation` | `true`: 수동 호출만 가능 (배포, 커밋 등) |
| `allowed-tools` | 승인 없이 사용 가능한 도구 |
| `argument-hint` | 자동완성 시 표시할 인수 힌트 |

## 라이선스

MIT
