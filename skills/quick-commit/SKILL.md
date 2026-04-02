---
name: quick-commit
description: 변경사항을 분석하여 컨벤셔널 커밋 메시지를 자동 생성하고 커밋합니다.
user-invocable: true
disable-model-invocation: true
allowed-tools: Bash
---

# 빠른 커밋

메시지 힌트: $ARGUMENTS

## 프로세스

### 1. 변경사항 분석
- `git status`로 변경된 파일 확인
- `git diff`로 실제 변경 내용 파악
- `git log --oneline -5`로 최근 커밋 스타일 확인

### 2. 커밋 메시지 작성

#### Conventional Commits 형식
```
<type>(<scope>): <subject>

<body>
```

#### 타입
- `feat`: 새로운 기능
- `fix`: 버그 수정
- `refactor`: 리팩토링 (기능 변경 없음)
- `docs`: 문서 변경
- `test`: 테스트 추가/수정
- `chore`: 빌드, 설정 등 기타
- `style`: 코드 스타일 변경 (포매팅 등)
- `perf`: 성능 개선

#### 규칙
- subject는 50자 이내, 명령형으로 작성
- body는 72자에서 줄바꿈
- "왜" 변경했는지를 중심으로 작성
- 프로젝트의 기존 커밋 스타일을 따름

### 3. 커밋 실행
- 사용자에게 메시지를 보여주고 확인 후 커밋
- `.env`, credentials 등 민감 파일은 제외

## 규칙
- 커밋 전에 반드시 변경사항을 사용자에게 보여주기
- 민감 파일이 포함되어 있으면 경고
- `git add -A` 대신 관련 파일만 선택적으로 추가
