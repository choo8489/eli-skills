---
name: storybook-writer
description: UI 컴포넌트의 Storybook 스토리를 CSF3 형식으로 자동 생성합니다. 컴포넌트 문서화 시 사용하세요.
argument-hint: [component-file]
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit
---

# Storybook 스토리 생성기

대상: $ARGUMENTS

## 프로세스

### 1. 컴포넌트 분석
- Props/인터페이스 파악
- 상태 (state) 변화 확인
- 이벤트 핸들러 식별
- 자식 컴포넌트 의존성
- 스타일 변형 (variants)

### 2. CSF3 형식 스토리 생성

```typescript
import type { Meta, StoryObj } from '@storybook/react';
import { fn } from '@storybook/test';
import { Button } from './Button';

const meta = {
  title: 'Components/Button',
  component: Button,
  tags: ['autodocs'],
  parameters: {
    layout: 'centered',
    docs: {
      description: {
        component: '다양한 상황에서 사용되는 기본 버튼 컴포넌트',
      },
    },
  },
  argTypes: {
    variant: {
      control: 'select',
      options: ['primary', 'secondary', 'danger'],
      description: '버튼 스타일 변형',
    },
    size: {
      control: 'radio',
      options: ['sm', 'md', 'lg'],
    },
    disabled: { control: 'boolean' },
    onClick: { action: 'clicked' },
  },
  args: {
    onClick: fn(),
  },
} satisfies Meta<typeof Button>;

export default meta;
type Story = StoryObj<typeof meta>;
```

### 3. 스토리 변형 (Variants)

```typescript
// 기본 스토리
export const Default: Story = {
  args: {
    children: '버튼',
    variant: 'primary',
  },
};

// 모든 변형 보여주기
export const AllVariants: Story = {
  render: () => (
    <div style={{ display: 'flex', gap: '8px' }}>
      <Button variant="primary">Primary</Button>
      <Button variant="secondary">Secondary</Button>
      <Button variant="danger">Danger</Button>
    </div>
  ),
};

// 상태별
export const Disabled: Story = {
  args: { ...Default.args, disabled: true },
};

export const Loading: Story = {
  args: { ...Default.args, loading: true },
};
```

### 4. 인터랙션 테스트 (play 함수)

```typescript
import { expect, userEvent, within } from '@storybook/test';

export const ClickTest: Story = {
  args: Default.args,
  play: async ({ canvasElement, args }) => {
    const canvas = within(canvasElement);
    const button = canvas.getByRole('button');

    await userEvent.click(button);
    await expect(args.onClick).toHaveBeenCalledOnce();
  },
};
```

### 5. 포함할 스토리 목록
- **Default**: 기본 상태
- **AllVariants**: 모든 변형 나란히
- **AllSizes**: 모든 크기 비교
- **WithIcon**: 아이콘 포함 (해당 시)
- **Disabled**: 비활성 상태
- **Loading**: 로딩 상태 (해당 시)
- **Interactive**: play 함수로 동작 테스트
- **Responsive**: 반응형 동작 (해당 시)
- **DarkMode**: 다크모드 (해당 시)

## 규칙
- 프로젝트의 Storybook 버전과 설정을 먼저 확인
- 기존 스토리 파일의 패턴/컨벤션 따르기
- Controls로 모든 props를 조작 가능하게
- 접근성 애드온 (`@storybook/addon-a11y`) 활용
- 스토리 파일 위치: 컴포넌트와 같은 디렉토리
