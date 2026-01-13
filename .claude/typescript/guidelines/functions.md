# 関数

## 関数宣言 vs 式

名前付き関数には関数宣言を使用。

```typescript
// Good - 関数宣言
function processData(data: Data): Result {
  return transform(data);
}

// Avoid - 関数式
const processData = function(data: Data): Result {
  return transform(data);
};
```

## アロー関数

コールバックにはアロー関数を使用。

```typescript
// Good
items.map((item) => item.value);
items.filter((item) => item.isActive);

// Avoid
items.map(function(item) {
  return item.value;
});
```

## Rest パラメータ

`arguments`の代わりに Rest パラメータを使用。

```typescript
// Good
function sum(...numbers: number[]): number {
  return numbers.reduce((a, b) => a + b, 0);
}

// Bad
function sum(): number {
  return Array.from(arguments).reduce((a, b) => a + b, 0);
}
```

## デフォルトパラメータ

```typescript
// Good
function greet(name: string = 'World'): string {
  return `Hello, ${name}!`;
}

// Bad
function greet(name?: string): string {
  name = name || 'World';
  return `Hello, ${name}!`;
}
```

## パラメータの分割代入

```typescript
// Good - 明確
function process({id, name, options}: ProcessParams): void {
  // ...
}

// 使用時
process({id: 1, name: 'test', options: {}});
```
