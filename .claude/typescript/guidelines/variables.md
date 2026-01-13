# 変数

## const / let

常に`const`または`let`を使用。`var`は禁止。

```typescript
// Good
const immutable = 'value';
let mutable = 'value';

// Bad
var legacy = 'value';
```

## 宣言

1つの文で1つの変数を宣言。

```typescript
// Good
const a = 1;
const b = 2;

// Bad
const a = 1, b = 2;
```

## 配列

`Array()`コンストラクタは使用しない。

```typescript
// Good
const arr: string[] = [];
const arr = ['a', 'b', 'c'];

// Bad
const arr = new Array<string>();
const arr = Array('a', 'b', 'c');
```

## オブジェクト

リテラル構文を使用。

```typescript
// Good
const obj = {};
const obj = {key: 'value'};

// Bad
const obj = new Object();
```

## スプレッド構文

配列とオブジェクトのコピー・マージに使用。

```typescript
// 配列のコピー
const copy = [...original];

// 配列のマージ
const merged = [...arr1, ...arr2];

// オブジェクトのマージ
const merged = {...obj1, ...obj2};
```
