# 制御フロー

## 等価比較

常に`===`と`!==`を使用。

```typescript
// Good
if (a === b) {}
if (a !== b) {}

// Bad
if (a == b) {}
if (a != b) {}
```

### 例外: nullチェック

`null`と`undefined`の同時チェックには`==`を使用可。

```typescript
// OK - null と undefined の両方をチェック
if (value == null) {
  // value は null または undefined
}

// 同等の明示的な書き方
if (value === null || value === undefined) {}
```

## 条件内の代入

条件式内での代入は避ける。

```typescript
// Bad
if (result = getValue()) {}

// Good
const result = getValue();
if (result) {}
```

## switch文

```typescript
// default を必ず含める
switch (status) {
  case 'active':
    activate();
    break;
  case 'inactive':
    deactivate();
    break;
  default:
    throw new Error(`Unknown status: ${status}`);
}
```

## for...of ループ

配列のイテレーションには`for...of`を使用。

```typescript
// Good
for (const item of items) {
  process(item);
}

// Avoid - for...in（フィルタなし）
for (const key in obj) {
  // プロトタイプチェーンのプロパティも含まれる可能性
}

// for...in を使う場合はフィルタ
for (const key in obj) {
  if (Object.prototype.hasOwnProperty.call(obj, key)) {
    process(obj[key]);
  }
}
```

## 例外処理

Errorインスタンスのみをthrow。

```typescript
// Good
throw new Error('Something went wrong');
throw new CustomError('Validation failed', {field: 'email'});

// Bad
throw 'error';
throw 404;
throw {message: 'error'};
```

## try-catch

```typescript
// Good
try {
  await riskyOperation();
} catch (error) {
  if (error instanceof ValidationError) {
    handleValidation(error);
  } else if (error instanceof NetworkError) {
    handleNetwork(error);
  } else {
    throw error;  // 未知のエラーは再throw
  }
}
```
