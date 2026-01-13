# 型システム

## 型推論

自明な型は推論に任せる。

```typescript
// Good - 推論で十分
const name = 'Alice';
const count = 42;
const items = ['a', 'b', 'c'];

// 不要な型注釈
const name: string = 'Alice';
const count: number = 42;
```

## 明示的な型注釈

複雑または非自明な型は明示。

```typescript
// Good - 戻り値の型を明示
function processItems(items: Item[]): ProcessedItem[] {
  return items.map(transform);
}

// Good - 複雑なオブジェクト
const config: ServerConfig = {
  port: 3000,
  host: 'localhost',
  ssl: false,
};
```

## interface vs type

### 使い分け

| 用途 | 推奨 |
|------|------|
| オブジェクト構造 | `interface` |
| ユニオン/交差型 | `type` |
| `extends`/`implements` | `interface` |

```typescript
// interface - オブジェクト構造、拡張可能
interface User {
  id: string;
  name: string;
}

interface Admin extends User {
  role: 'admin';
}

// type - ユニオン、交差型
type Status = 'pending' | 'active' | 'inactive';
type UserOrGuest = User | Guest;
type UserWithMeta = User & { createdAt: Date };
```

## 配列型

```typescript
// シンプルな型は T[]
const numbers: number[] = [1, 2, 3];
const strings: string[] = ['a', 'b'];

// ネストした複雑な型は Array<T>
const matrix: Array<Array<number>> = [[1, 2], [3, 4]];
const items: Array<Map<string, Item>> = [];
```

## Null / Undefined

### 使い分け

- **undefined**: 一般的なデフォルト
- **null**: API規約やNode.jsコールバック等で必要な場合

```typescript
// 戻り値の設計 - 構造化を検討
// Good - 明示的な構造
function validate(input: string): {valid: boolean; value?: string} {
  if (input.length > 0) {
    return {valid: true, value: input};
  }
  return {valid: false};
}

// Avoid - null/undefined を直接返す
function validate(input: string): string | null {
  return input.length > 0 ? input : null;
}
```

### オプショナルフィールド

`|undefined`より`?`を優先。

```typescript
// Good
interface User {
  name: string;
  nickname?: string;
}

// Avoid
interface User {
  name: string;
  nickname: string | undefined;
}
```

### Nullチェック

`== null`で両方を同時チェック可能。

```typescript
// null と undefined の両方をチェック
if (value == null) {
  // value は null または undefined
}

// オブジェクトは truthy チェック
if (user) {
  console.log(user.name);
}
```

### 使用箇所での追加

```typescript
// Good - 使用箇所で追加
function find(id: string): User | null {
  return users.get(id) ?? null;
}

// Bad - 型エイリアスに含める
type MaybeUser = User | null;
```

## any を避ける

`any`の代わりに`unknown`を使用。

```typescript
// Good
function parse(json: string): unknown {
  return JSON.parse(json);
}

// 使用時に型ガード
function process(data: unknown): void {
  if (typeof data === 'string') {
    console.log(data.toUpperCase());
  }
}

// Bad
function parse(json: string): any {
  return JSON.parse(json);
}
```

## 複雑な型を避ける

マップ型や条件型は最小限に。

```typescript
// Bad - 過度に複雑
type DeepPartial<T> = {
  [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P];
};

// Good - 必要な場所で明示的に定義
interface UserUpdate {
  name?: string;
  email?: string;
}
```

少しの繰り返しは、複雑な型表現の長期的コストより安い。
