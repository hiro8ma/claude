# クラス

## readonly修飾子

変更されないプロパティには`readonly`を使用。

```typescript
// Good
class User {
  readonly id: string;
  readonly createdAt: Date;
  name: string;

  constructor(id: string, name: string) {
    this.id = id;
    this.name = name;
    this.createdAt = new Date();
  }
}
```

## パラメータプロパティ

コンストラクタで直接プロパティを宣言。

```typescript
// Good - パラメータプロパティ
class User {
  constructor(
    readonly id: string,
    private name: string,
    public email: string,
  ) {}
}

// 冗長な書き方
class User {
  readonly id: string;
  private name: string;
  public email: string;

  constructor(id: string, name: string, email: string) {
    this.id = id;
    this.name = name;
    this.email = email;
  }
}
```

## プライベートフィールド

`#`構文は避け、`private`修飾子を使用。

```typescript
// Good
class MyClass {
  private secret: string;
}

// Avoid
class MyClass {
  #secret: string;
}
```

## 静的メソッド

静的メソッド内で`this`を参照しない。

```typescript
// Good
class Calculator {
  static add(a: number, b: number): number {
    return a + b;
  }
}

// Bad
class Calculator {
  static instance = new Calculator();

  static getInstance(): Calculator {
    return this.instance;  // 避ける
  }
}
```

## ゲッター/セッター

副作用を持たせない。

```typescript
// Good
class User {
  private _name: string;

  get name(): string {
    return this._name;
  }

  set name(value: string) {
    this._name = value;
  }
}

// Bad - 副作用
get name(): string {
  this.accessCount++;  // 副作用
  return this._name;
}
```
