---
name: type-generator
description: JSON, Prisma, API 응답 등에서 TypeScript 타입과 Zod 검증 스키마를 자동 생성합니다. 타입 정의가 필요할 때 사용하세요.
argument-hint: [source-file-or-json]
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit
---

# 타입 & 스키마 생성기

소스: $ARGUMENTS

## 지원 소스

### 입력
- JSON 예시 데이터
- Prisma 스키마 파일
- OpenAPI/Swagger 스펙
- API 응답 샘플
- 기존 TypeScript 인터페이스
- SQL CREATE TABLE 문

### 출력
- TypeScript interface/type
- Zod 검증 스키마
- 런타임 타입 가드

## 생성 프로세스

### 1. 소스 분석
- 데이터 구조 파악 (필드, 타입, 중첩)
- nullable/optional 필드 식별
- 배열, 유니온 타입 감지
- 날짜, 열거형 등 특수 타입 식별

### 2. TypeScript 타입 생성
```typescript
// 기본 타입
interface User {
  id: string;
  name: string;
  email: string;
  role: 'admin' | 'user';
  createdAt: Date;
  profile?: UserProfile;  // optional
}
```

### 3. Zod 스키마 생성
```typescript
// Create 스키마 (id, createdAt 제외)
const createUserSchema = z.object({
  name: z.string().min(1).max(100),
  email: z.string().email(),
  role: z.enum(['admin', 'user']),
  profile: userProfileSchema.optional(),
});

// Update 스키마 (모두 optional)
const updateUserSchema = createUserSchema.partial();

// 타입 추론
type CreateUser = z.infer<typeof createUserSchema>;
type UpdateUser = z.infer<typeof updateUserSchema>;
```

### 4. 스마트 변환 규칙
- `DateTime` → `z.coerce.date()`
- `@default` 필드 → Create에서 optional
- `@id` 필드 → Create/Update에서 제외
- `@updatedAt` → 자동 관리로 제외
- Enum → `z.enum([...])` 또는 `z.nativeEnum()`
- 관계 필드 → `z.object({ connect: ... })` 또는 `z.object({ create: ... })`

## 출력 형식

```
## 생성된 타입

### TypeScript
[interface/type 코드]

### Zod 스키마
[create/update/response 스키마]

### 사용 예시
[import 및 사용 방법]
```

## 규칙
- 프로젝트의 기존 타입 컨벤션 따르기
- 불필요한 `any` 타입 사용 금지
- 검증 메시지는 사용자 친화적으로
- re-export를 통한 타입 barrel 파일 제안
