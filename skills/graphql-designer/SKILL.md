---
name: graphql-designer
description: 프로덕션 수준의 GraphQL 스키마를 설계합니다. Federation, DataLoader, 페이지네이션 패턴을 포함합니다.
argument-hint: [requirements]
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit
---

# GraphQL 스키마 설계

요구사항: $ARGUMENTS

## 설계 원칙

### 타입 설계
- 도메인 모델을 정확히 반영하는 타입
- Input 타입과 Output 타입 분리
- Custom Scalar (DateTime, URL, Email 등)
- Enum으로 제한된 선택지 표현
- Interface/Union으로 다형성 표현

### 네이밍 컨벤션
- 타입: PascalCase (`User`, `PostConnection`)
- 필드: camelCase (`createdAt`, `firstName`)
- Enum 값: SCREAMING_SNAKE (`ADMIN`, `ACTIVE`)
- Input: `{Action}{Type}Input` (`CreateUserInput`)
- Mutation: `{action}{Type}` (`createUser`, `deletePost`)

## 스키마 구조

### Query 설계
```graphql
type Query {
  # 단일 조회 (nullable - 없으면 null)
  user(id: ID!): User

  # 목록 조회 (Relay 커서 페이지네이션)
  users(
    first: Int
    after: String
    filter: UserFilter
    orderBy: UserOrderBy
  ): UserConnection!
}
```

### Mutation 설계
```graphql
type Mutation {
  createUser(input: CreateUserInput!): CreateUserPayload!
  updateUser(id: ID!, input: UpdateUserInput!): UpdateUserPayload!
  deleteUser(id: ID!): DeleteUserPayload!
}

# Payload 패턴 (에러 처리 포함)
type CreateUserPayload {
  user: User
  errors: [UserError!]!
}
```

### Relay 커서 페이지네이션
```graphql
type UserConnection {
  edges: [UserEdge!]!
  pageInfo: PageInfo!
  totalCount: Int!
}

type UserEdge {
  node: User!
  cursor: String!
}

type PageInfo {
  hasNextPage: Boolean!
  hasPreviousPage: Boolean!
  startCursor: String
  endCursor: String
}
```

### N+1 방지 (DataLoader)
```
# 각 관계 필드에 DataLoader 패턴 적용
User.posts → PostLoader.loadMany(userIds)
Post.author → UserLoader.load(authorId)
```

## 고급 패턴

### Subscription
```graphql
type Subscription {
  userCreated: User!
  messageReceived(channelId: ID!): Message!
}
```

### Federation (마이크로서비스)
```graphql
# User 서비스
type User @key(fields: "id") {
  id: ID!
  name: String!
}

# Post 서비스 (User 확장)
extend type User @key(fields: "id") {
  id: ID! @external
  posts: [Post!]!
}
```

### 쿼리 복잡도 제한
```
# 깊이 제한: maxDepth(10)
# 복잡도 제한: maxComplexity(1000)
# Rate Limiting: 분당 100회
```

## 출력 형식

```
## GraphQL 스키마

### 타입 정의
[schema.graphql]

### Resolver 구조
[resolver 매핑]

### DataLoader 설정
[N+1 방지 로더]

### 보안 설정
[인증, 복잡도 제한, rate limiting]
```

## 규칙
- nullable vs non-null을 신중하게 결정
- 목록은 항상 Connection 패턴 사용
- Mutation은 항상 Payload 타입 반환
- 민감 필드는 directive로 접근 제어
- 스키마 변경 시 하위 호환성 유지
