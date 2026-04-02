---
name: git-worktree
description: Git worktree를 활용한 병렬 브랜치 작업을 관리합니다. 동시에 여러 브랜치 작업이 필요할 때 사용하세요.
user-invocable: true
disable-model-invocation: true
allowed-tools: Bash
---

# Git Worktree 관리

작업: $ARGUMENTS

## 개념
Git Worktree는 하나의 레포에서 여러 브랜치를 동시에 체크아웃할 수 있는 기능입니다.
`git stash`나 브랜치 전환 없이 병렬 작업이 가능합니다.

## 주요 명령어

### 새 워크트리 생성
```bash
# 기존 브랜치로 워크트리 생성
git worktree add ../project-hotfix hotfix/urgent-bug

# 새 브랜치와 함께 워크트리 생성
git worktree add -b feature/new-thing ../project-feature main
```

### 워크트리 목록 확인
```bash
git worktree list
```

### 워크트리 삭제
```bash
git worktree remove ../project-hotfix
# 또는 강제 삭제
git worktree remove --force ../project-hotfix
```

### 정리 (삭제된 디렉토리 정리)
```bash
git worktree prune
```

## 워크플로우

### 1. 긴급 버그 수정 (현재 기능 개발 중)
```
현재: feature/dashboard 작업 중
긴급: production 버그 발생!

1. git worktree add ../project-hotfix -b hotfix/critical main
2. cd ../project-hotfix
3. [버그 수정 & 커밋 & PR]
4. cd ../project (원래 작업으로 복귀)
5. git worktree remove ../project-hotfix
```

### 2. 코드 리뷰 (작업 중단 없이)
```
1. git fetch origin
2. git worktree add ../project-review origin/pr-branch
3. cd ../project-review
4. [코드 리뷰]
5. git worktree remove ../project-review
```

### 3. 여러 기능 병렬 개발
```
git worktree add ../project-auth feature/auth
git worktree add ../project-api feature/api-v2
git worktree add ../project-ui feature/new-ui
```

## 규칙
- 워크트리 디렉토리는 프로젝트 루트의 형제 디렉토리로 생성
- 네이밍: `../project-{purpose}` 형식 권장
- 작업 완료 후 반드시 워크트리 삭제
- `git worktree list`로 현재 상태 먼저 확인
