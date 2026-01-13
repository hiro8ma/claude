# 禁止事項

## ラッパー型

プリミティブのラッパー型は使用しない。

```typescript
// Bad
const str: String = 'hello';
const num: Number = 42;
const bool: Boolean = true;

// Good
const str: string = 'hello';
const num: number = 42;
const bool: boolean = true;
```

## eval / Function

動的コード評価は禁止。

```typescript
// Bad
eval('console.log("hello")');
const fn = new Function('a', 'b', 'return a + b');

// Good - 別のアプローチを検討
```

## with文

使用禁止。

```typescript
// Bad
with (obj) {
  console.log(property);
}

// Good
console.log(obj.property);
```

## namespace

モジュールを使用。

```typescript
// Bad
namespace MyNamespace {
  export function foo() {}
}

// Good
export function foo() {}
```

## const enum

使用しない。

```typescript
// Bad
const enum Status {
  Active,
  Inactive,
}

// Good
enum Status {
  Active,
  Inactive,
}

// または
const Status = {
  Active: 'active',
  Inactive: 'inactive',
} as const;
```

## プロトタイプの変更

組み込みオブジェクトのプロトタイプを変更しない。

```typescript
// Bad
Array.prototype.myMethod = function() {};
String.prototype.toTitleCase = function() {};

// Good - ユーティリティ関数として実装
function toTitleCase(str: string): string {}
```

## debugger

本番コードには含めない。

```typescript
// Bad - 本番コード内
function process() {
  debugger;
  // ...
}

// デバッグ時のみ使用し、コミット前に削除
```

## 非標準機能

ECMAScript標準外の機能は使用しない。

## @ts-ignore

原則禁止。テストでの例外的使用のみ許可。

```typescript
// Bad
// @ts-ignore
const value: string = 42;

// Good - 型を正しく修正
const value: number = 42;
```
