# 型定義ファイル (.d.ts)

## 一般的な型のルール

### プリミティブ型

ラッパー型は使用しない。

```typescript
// Bad
function reverse(s: String): String;
function count(n: Number): Number;

// Good
function reverse(s: string): string;
function count(n: number): number;
```

### ジェネリクス

使用しない型パラメータは定義しない。

```typescript
// Bad - T が使われていない
declare function parse<T>(json: string): unknown;

// Good
declare function parse(json: string): unknown;
```

## コールバック

### 戻り値型

コールバックの戻り値は`void`を使用（`any`は禁止）。

```typescript
// Bad - any は戻り値を誤って使用できる
declare function process(callback: () => any): void;

// Good - void で安全
declare function process(callback: () => void): void;
```

### パラメータ

コールバックのパラメータはオプショナルにしない。

```typescript
// Bad
interface Fetcher {
  getObject(done: (data: unknown, elapsedTime?: number) => void): void;
}

// Good - 呼び出し側はパラメータを無視できる
interface Fetcher {
  getObject(done: (data: unknown, elapsedTime: number) => void): void;
}
```

## 関数オーバーロード

### 順序

**具体的 → 一般的** の順に定義（最初にマッチしたものが選択される）。

```typescript
// Bad - 一般的な定義が先
declare function fn(x: unknown): unknown;
declare function fn(x: HTMLElement): number;
declare function fn(x: HTMLDivElement): string;

const elem: HTMLDivElement = document.createElement('div');
const x = fn(elem);  // x: unknown （意図しない結果）

// Good - 具体的な定義が先
declare function fn(x: HTMLDivElement): string;
declare function fn(x: HTMLElement): number;
declare function fn(x: unknown): unknown;

const elem: HTMLDivElement = document.createElement('div');
const x = fn(elem);  // x: string ✓
```

### オプショナルパラメータで統合

戻り値型が同じオーバーロードは統合する。

```typescript
// Bad - 冗長
interface Example {
  diff(one: string): number;
  diff(one: string, two: string): number;
  diff(one: string, two: string, three: boolean): number;
}

// Good - オプショナルで統合
interface Example {
  diff(one: string, two?: string, three?: boolean): number;
}
```

### ユニオン型で統合

引数の型のみ異なるオーバーロードはユニオン型で統合。

```typescript
// Bad - オーバーロード分割
interface Moment {
  utcOffset(): number;
  utcOffset(b: number): Moment;
  utcOffset(b: string): Moment;
}

// Good - ユニオン型
interface Moment {
  utcOffset(): number;
  utcOffset(b: number | string): Moment;
}
```

値の透過的な受け渡しが可能になる:

```typescript
function setOffset(x: number | string) {
  // ユニオン型なら直接渡せる
  return moment().utcOffset(x);
}
```

## まとめ

| カテゴリ | ルール |
|----------|--------|
| 基本型 | `String` → `string`、`Number` → `number` |
| コールバック戻り値 | `void`を使用（`any`禁止） |
| コールバックパラメータ | 必須で定義（呼び出し側で無視可） |
| オーバーロード順序 | 具体的 → 一般的 |
| パラメータ統合 | オプショナルパラメータを使用 |
| 型の多様化 | ユニオン型を使用 |
