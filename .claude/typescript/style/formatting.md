# フォーマット

## ファイル構造

ファイルは以下の順序で構成:

1. 著作権/ライセンス情報
2. `@fileoverview` JSDoc（任意）
3. import文
4. 実装

各セクション間は空行1行で区切る。

## インポート

### 種類

```typescript
// モジュールインポート - 大きなAPIに使用
import * as fs from 'fs';

// 名前付きインポート - 頻繁に使用するシンボルに
import {Component, OnInit} from '@angular/core';

// デフォルトインポート - 外部ライブラリで必要な場合のみ
import React from 'react';

// 副作用インポート - ロード時の動作に影響するライブラリに
import 'zone.js';
```

### ルール

- プロジェクト内は相対パスを優先
- 自身のコードでデフォルトエクスポートを使用しない
- 名前付きエクスポートを優先（未宣言インポート時にエラーになるため）

```typescript
// Good - 名前付きエクスポート
export function processData() {}
export class DataProcessor {}

// Bad - デフォルトエクスポート
export default function processData() {}
```

## ブレース

単一文でも常にブレースを使用。

```typescript
// Good
if (condition) {
  doSomething();
}

// Bad
if (condition) doSomething();
if (condition)
  doSomething();
```

## セミコロン

文の終わりには必ずセミコロン。

```typescript
// Good
const x = 1;
doSomething();

// Bad
const x = 1
doSomething()
```

## クラス宣言

セミコロンで終わらせない。

```typescript
// Good
class MyClass {
  property = 'value';
}

// Bad
class MyClass {
  property = 'value';
};
```

## コメント

### JSDoc vs 通常コメント

```typescript
// JSDoc - ユーザー向けドキュメント
/** ユーザーを表すクラス */
class User {}

// 通常コメント - 実装詳細
// この処理は後で最適化する
```

### 複数行コメント

`//`スタイルを使用。

```typescript
// Good
// これは複数行の
// コメントです

// Bad
/* これは複数行の
   コメントです */
```

### デコレータとJSDoc

JSDocはデコレータの前に配置。

```typescript
// Good
/** コンポーネントの説明 */
@Component({...})
class MyComponent {}

// Bad
@Component({...})
/** コンポーネントの説明 */
class MyComponent {}
```
