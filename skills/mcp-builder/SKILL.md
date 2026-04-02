---
name: mcp-builder
description: Model Context Protocol(MCP) 서버를 설계하고 구현합니다. AI 도구 연동 서버를 만들 때 사용하세요.
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash
---

# MCP 서버 빌더

요구사항: $ARGUMENTS

## MCP 개요

MCP(Model Context Protocol)는 AI 모델이 외부 도구와 데이터에 접근하기 위한 표준 프로토콜입니다.

### 핵심 개념
- **Tool**: AI가 호출할 수 있는 함수 (검색, 계산, API 호출 등)
- **Resource**: AI가 읽을 수 있는 데이터 소스 (파일, DB, API 등)
- **Prompt**: 사전 정의된 프롬프트 템플릿

## 구현 프로세스

### 1. 설계
- 제공할 Tool 목록과 파라미터 정의
- Resource 스키마 정의
- 인증/인가 방식 결정
- 에러 핸들링 전략

### 2. 프로젝트 구조

#### TypeScript (권장)
```
my-mcp-server/
├── src/
│   ├── index.ts          # 서버 진입점
│   ├── tools/            # Tool 구현
│   ├── resources/        # Resource 구현
│   └── utils/            # 유틸리티
├── package.json
└── tsconfig.json
```

#### Python
```
my-mcp-server/
├── src/
│   ├── server.py         # 서버 진입점
│   ├── tools/            # Tool 구현
│   └── resources/        # Resource 구현
├── pyproject.toml
└── README.md
```

### 3. Tool 정의 패턴
```typescript
{
  name: "tool-name",
  description: "명확한 설명 - AI가 언제 사용할지 판단하는 기준",
  inputSchema: {
    type: "object",
    properties: {
      param1: { type: "string", description: "설명" }
    },
    required: ["param1"]
  }
}
```

### 4. 테스트
- MCP Inspector로 Tool/Resource 동작 확인
- 에러 케이스 테스트 (잘못된 입력, 타임아웃)
- Claude Code에서 실제 연동 테스트

## 출력 형식

```
## MCP 서버 설계

**이름**: [서버 이름]
**목적**: [한줄 설명]

### Tools
1. [tool-name] - 설명
   - 입력: { params }
   - 출력: { result }

### Resources
1. [resource-uri] - 설명

### 설정 (claude_desktop_config.json)
[설정 예시]
```

이후 실제 코드를 생성합니다.

## 규칙
- Tool 이름과 설명을 명확하게 (AI가 판단하는 기준)
- inputSchema 검증을 철저히
- 에러 메시지는 AI가 이해할 수 있도록 구체적으로
- 장시간 작업은 프로그레스 알림 포함
- 민감 정보는 환경변수로 처리
