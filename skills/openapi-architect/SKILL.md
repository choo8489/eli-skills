---
name: openapi-architect
description: 코드나 요구사항에서 OpenAPI 3.1 스펙을 생성하고, API 계약을 검증합니다. API 문서화 시 사용하세요.
argument-hint: [source-or-requirements]
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit
---

# OpenAPI 스펙 설계

대상: $ARGUMENTS

## 프로세스

### 1. API 분석
- 라우트/엔드포인트 자동 탐지 (Express, FastAPI, Gin 등)
- 요청/응답 스키마 추출
- 인증 방식 확인
- 미들웨어 및 검증 로직 파악

### 2. OpenAPI 3.1 스펙 생성

```yaml
openapi: 3.1.0
info:
  title: API 이름
  version: 1.0.0
  description: API 설명

servers:
  - url: http://localhost:3000
    description: 개발 환경

paths:
  /api/users:
    get:
      summary: 사용자 목록 조회
      tags: [Users]
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
      responses:
        '200':
          description: 성공
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserList'
              example:
                users: [{id: "1", name: "홍길동"}]

components:
  schemas:
    User:
      type: object
      required: [id, name, email]
      properties:
        id:
          type: string
          format: uuid
        name:
          type: string
          example: "홍길동"

  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
```

### 3. 포함 사항
- **paths**: 모든 엔드포인트, 메서드, 파라미터
- **schemas**: 요청/응답 모델 (components/schemas)
- **security**: 인증 방식 (JWT, API Key, OAuth2)
- **examples**: 각 엔드포인트의 요청/응답 예시
- **errors**: 공통 에러 응답 (400, 401, 404, 500)
- **tags**: 기능별 그룹핑
- **servers**: 환경별 서버 URL

### 4. 검증
- 스펙과 실제 코드 간 불일치 확인
- 누락된 엔드포인트 식별
- 스키마 참조 무결성 검사

## 규칙
- OpenAPI 3.1 (최신 표준) 사용
- 모든 엔드포인트에 example 포함
- description은 개발자가 읽기 쉽게 작성
- 에러 응답 형식 통일
- 페이지네이션, 필터링 패턴 일관성 유지
