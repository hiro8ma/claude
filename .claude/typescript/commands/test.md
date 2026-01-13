# テストコマンド

## Jest

### インストール

```bash
npm install --save-dev jest @types/jest ts-jest
```

### 設定 (jest.config.js)

```javascript
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  roots: ['<rootDir>/src'],
  testMatch: ['**/*.test.ts', '**/*.spec.ts'],
  collectCoverageFrom: ['src/**/*.ts', '!src/**/*.d.ts'],
};
```

### コマンド

```bash
# 全テスト実行
jest

# ウォッチモード
jest --watch

# カバレッジ付き
jest --coverage

# 特定ファイル
jest path/to/file.test.ts

# 特定パターン
jest --testNamePattern="should handle"
```

## Vitest

### インストール

```bash
npm install --save-dev vitest
```

### 設定 (vitest.config.ts)

```typescript
import {defineConfig} from 'vitest/config';

export default defineConfig({
  test: {
    globals: true,
    environment: 'node',
    include: ['src/**/*.{test,spec}.ts'],
    coverage: {
      provider: 'v8',
      reporter: ['text', 'html'],
    },
  },
});
```

### コマンド

```bash
# 全テスト実行
vitest run

# ウォッチモード
vitest

# カバレッジ付き
vitest run --coverage

# UI
vitest --ui
```

## package.json スクリプト例

```json
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "test:ci": "jest --ci --coverage --reporters=default --reporters=jest-junit"
  }
}
```

## テストファイル構造

```
src/
├── utils/
│   ├── format.ts
│   └── format.test.ts      # 同じディレクトリ
└── __tests__/              # または専用ディレクトリ
    └── integration/
        └── api.test.ts
```

## テスト例

```typescript
import {formatDate} from './format';

describe('formatDate', () => {
  it('should format date correctly', () => {
    const date = new Date('2024-01-15');
    expect(formatDate(date)).toBe('2024-01-15');
  });

  it('should handle invalid date', () => {
    expect(() => formatDate(null as any)).toThrow();
  });
});
```
