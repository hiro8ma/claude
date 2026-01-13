# 命名規則

## ケース規則

| 識別子 | ケース | 例 |
|--------|--------|-----|
| クラス | UpperCamelCase | `MyClass` |
| インターフェース | UpperCamelCase | `User`（`IUser`は不可） |
| 型エイリアス | UpperCamelCase | `MyType` |
| 列挙型 | UpperCamelCase | `MyEnum` |
| 列挙値 | UpperCamelCase | `MyValue` |
| 関数 | lowerCamelCase | `myFunction` |
| 変数 | lowerCamelCase | `myVariable` |
| パラメータ | lowerCamelCase | `myParam` |
| プロパティ | lowerCamelCase | `myProperty` |
| 定数 | CONSTANT_CASE | `MY_CONSTANT` |

## 定数

モジュールスコープの不変値は`CONSTANT_CASE`を使用。

```typescript
// Good
const MAX_RETRY_COUNT = 3;
const DEFAULT_TIMEOUT_MS = 5000;

// Bad - ローカル変数は定数ケースにしない
function process() {
  const MAX_VALUE = 100;  // Bad
  const maxValue = 100;   // Good
}
```

## 頭字語

頭字語は単語として扱う。

```typescript
// Good
loadHttpUrl()
XMLHttpRequest  // ただし既存APIは例外

// Bad
loadHTTPURL()
```

## インターフェース名

`I`プレフィックスは使用しない（TypeScript標準ライブラリの慣例）。

```typescript
// Good
interface User {}
interface Fetcher {}

// Bad
interface IUser {}
interface IFetcher {}
```

## ファイル名

`camelCase`を使用。コンポーネントは`PascalCase`も可。

```typescript
// Good
utils.ts
stringHelpers.ts
Accordion.tsx  // Reactコンポーネント

// Bad
Utils.ts
string-helpers.ts
```

## 禁止事項

```typescript
// Bad - アンダースコアの先頭/末尾
_privateField
privateField_

// Bad - opt_プレフィックス
function foo(opt_bar?: string) {}

// Good
function foo(bar?: string) {}
```

## 説明的な名前

```typescript
// Good - 意図が明確
const userCount = users.length;
const isAuthenticated = checkAuth();

// Bad - 曖昧
const n = users.length;
const flag = checkAuth();
```
