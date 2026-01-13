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

構造的な型には`interface`を使用。

```typescript
// Good - interface
interface User {
  id: string;
  name: string;
}

// type は複合型やユニオンに使用
type Status = 'pending' | 'active' | 'inactive';
type UserOrGuest = User | Guest;
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

使用箇所でのみnull/undefinedを追加。

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
